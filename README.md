# 株価データ取得プログラム / Stock Price Fetcher

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/goto-a-toast/fetch-stock-data/blob/main/stock_price_fetcher.ipynb)

## 日本語

### 概要

このプロジェクトは、Google Colab上で動作する株価データ取得ツールです。yfinance ライブラリを使用して、日本株・米国株などの株価データを簡単に取得し、CSVファイルとしてダウンロードできます。

### 機能

- 📊 **株価データの取得**: 日本株・米国株など、世界中の株式データを取得可能
- 📅 **期間指定**: 開始日と終了日を自由に設定
- ⏱️ **データ間隔の選択**: 日次、週次、月次など、様々な時間足に対応
- 💾 **CSVダウンロード**: 取得したデータをCSVファイルとして保存・ダウンロード
- 🇯🇵 **日本語対応**: カラム名を日本語に変換したバージョンも出力可能
- 📈 **上昇予想銘柄のレコメンド**: 過去の株価推移からテクニカル指標でスコアリングし、上昇期待度が高い銘柄をランキング表示
- 🤖 **AI予測モデル**: 上位銘柄に対し LSTM / Stacked LSTM / GRU の3モデルを訓練し、MAPE最小のモデルで将来株価を予測

### レコメンド機能（セクション7）

日経225・TOPIX Core30・S&P500・NASDAQ100 の構成銘柄を自動取得し、約1000銘柄を2段階スクリーニングで上昇期待度ランキング化します。

**Stage 1（事前スクリーニング）**: 全銘柄の3ヶ月データを一括取得し、20日リターン・出来高比で上位200銘柄に絞り込み

**Stage 2（深層テクニカル分析）**: 以下のテクニカル指標でスコアリング
- **移動平均線 (SMA20 / SMA50 / SMA200)**: 各線の並びから上昇トレンドを判定
- **ゴールデンクロス**: 短期SMAが中期SMAを上抜けた直近シグナルを検出
- **RSI (14日)**: 売られすぎからの反発・健全な水準を加点、過熱は減点
- **モメンタム**: 直近20日・60日のリターン
- **ボリンジャーバンド**: バンド内位置で過熱感を判定
- **出来高トレンド**: 直近5日平均と20日平均の比較

セクション7冒頭の `use_nikkei225` / `use_topix_core30` / `use_sp500` / `use_nasdaq100` フラグで対象指数を選択できます。

### AI予測モデルによる将来予測（セクション8）

Stage 2 上位銘柄に対し、**3種類のニューラルネットワークから最適モデルを自動選択**して将来N日先の株価を予測します。

| モデル | 構造 | 特徴 |
|---|---|---|
| **LSTM-Simple** | LSTM(32) + Dense(1) | 軽量・高速 |
| **LSTM-Stacked** | LSTM(50) × 3 + Dropout + Dense(1) | 表現力高め |
| **GRU** | GRU(50) × 2 + Dropout + Dense(1) | LSTM の軽量代替 |

各銘柄について3モデルを訓練し、**バリデーション MAPE が最小のモデル**で `NN_FORECAST_DAYS` 日先まで反復予測します。出力は以下のチャートとCSV:
- 銘柄ごとの「実績 / テスト予測 / 将来予測」チャート (`ai_forecast.png`)
- モデル別精度比較バーチャート (`ai_model_comparison.png`)
- AI予測込み拡張ランキング CSV (`recommend_with_ai_YYYYMMDD.csv`)

> ⚠️ **免責事項**: 本機能は過去データに基づくテクニカル分析および機械学習による予測の自動化であり、将来の株価を保証するものではありません。投資判断は自己責任で行ってください。

### 使い方

1. 上記の「Open in Colab」バッジをクリックして、Google Colabでノートブックを開きます
2. セル2で銘柄コード、期間、データ間隔を設定します
3. メニューから「ランタイム」→「すべてのセルを実行」を選択
4. データが自動的にダウンロードされます

### 取得できるデータ

- **始値 (Open)**: 期間の最初の取引価格
- **高値 (High)**: 期間の最高価格
- **安値 (Low)**: 期間の最低価格
- **終値 (Close)**: 期間の最後の取引価格
- **調整後終値 (Adj Close)**: 株式分割や配当を考慮した終値
- **出来高 (Volume)**: 期間の取引量

### 銘柄コードの例

#### 日本株
- トヨタ自動車: `7203.T`
- ソニーグループ: `6758.T`
- ソフトバンクグループ: `9984.T`
- キーエンス: `6861.T`
- 三菱UFJフィナンシャル・グループ: `8306.T`

#### 米国株
- Apple: `AAPL`
- Microsoft: `MSFT`
- Google (Alphabet): `GOOGL`
- Amazon: `AMZN`
- Tesla: `TSLA`

### 必要な環境

- Google Colabアカウント（Googleアカウントがあれば無料で使用可能）
- インターネット接続

### 使用ライブラリ

- `yfinance`: Yahoo Finance APIから株価データを取得
- `pandas`: データの整形と処理
- `google.colab`: ファイルのダウンロード機能

---

## English

### Overview

This is a stock price data fetching tool that runs on Google Colab. Using the yfinance library, you can easily retrieve stock price data for Japanese stocks, US stocks, and more, and download them as CSV files.

### Features

- 📊 **Stock Data Retrieval**: Fetch stock data from around the world, including Japanese and US stocks
- 📅 **Custom Date Range**: Set start and end dates freely
- ⏱️ **Flexible Intervals**: Support for daily, weekly, monthly, and other time intervals
- 💾 **CSV Download**: Save and download retrieved data as CSV files
- 🇯🇵 **Japanese Support**: Option to output CSV with Japanese column names
- 📈 **Bullish Stock Recommender**: Auto-fetches Nikkei 225 / TOPIX Core30 / S&P 500 / NASDAQ-100 constituents (~1000 stocks) and ranks them via 2-stage screening
- 🤖 **AI Forecast**: Trains LSTM / Stacked LSTM / GRU on top stocks, auto-selects the best model by MAPE, and forecasts future prices

### Recommender (Section 7)

**Stage 1 (Pre-screening)**: Bulk-downloads 3-month data for ~1000 tickers and filters by 20-day return + volume ratio to keep the top 200.

**Stage 2 (Deep technical analysis)** scores each candidate using:

- **Moving Averages (SMA20 / SMA50 / SMA200)** — uptrend alignment
- **Golden Cross** — recent short-term SMA crossing above mid-term SMA
- **RSI (14)** — rewards healthy / oversold-rebound zones, penalizes overbought
- **Momentum** — 20-day and 60-day returns
- **Bollinger Bands** — position within the band
- **Volume trend** — recent 5-day vs 20-day average volume

Toggle indices via `use_nikkei225` / `use_topix_core30` / `use_sp500` / `use_nasdaq100` at the top of section 7.

### AI Forecast (Section 8)

For the top-K recommendations from Stage 2, three neural networks are trained and the **best model (lowest validation MAPE)** is auto-selected to forecast N days ahead:

| Model | Architecture |
|---|---|
| **LSTM-Simple** | LSTM(32) + Dense(1) |
| **LSTM-Stacked** | LSTM(50) × 3 + Dropout + Dense(1) |
| **GRU** | GRU(50) × 2 + Dropout + Dense(1) |

Outputs: per-stock forecast chart (`ai_forecast.png`), model-comparison bar chart (`ai_model_comparison.png`), and augmented ranking CSV (`recommend_with_ai_YYYYMMDD.csv`).

> ⚠️ **Disclaimer**: This is automated technical analysis and ML-based forecasting on historical data — it does not guarantee future price movements. Make investment decisions at your own risk.

### How to Use

1. Click the "Open in Colab" badge above to open the notebook in Google Colab
2. In cell 2, configure the ticker symbol, date range, and data interval
3. Select "Runtime" → "Run all" from the menu
4. Data will be automatically downloaded

### Available Data

- **Open**: First trading price of the period
- **High**: Highest price of the period
- **Low**: Lowest price of the period
- **Close**: Last trading price of the period
- **Adj Close**: Adjusted closing price considering stock splits and dividends
- **Volume**: Trading volume of the period

### Example Ticker Symbols

#### Japanese Stocks
- Toyota Motor: `7203.T`
- Sony Group: `6758.T`
- SoftBank Group: `9984.T`
- Keyence: `6861.T`
- Mitsubishi UFJ Financial Group: `8306.T`

#### US Stocks
- Apple: `AAPL`
- Microsoft: `MSFT`
- Google (Alphabet): `GOOGL`
- Amazon: `AMZN`
- Tesla: `TSLA`

### Requirements

- Google Colab account (free with a Google account)
- Internet connection

### Libraries Used

- `yfinance`: Retrieve stock price data from Yahoo Finance API
- `pandas`: Data formatting and processing
- `google.colab`: File download functionality

---

## License

This project is open source and available for personal and educational use.

## Contributing

Feel free to submit issues or pull requests to improve this project.
