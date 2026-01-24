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
