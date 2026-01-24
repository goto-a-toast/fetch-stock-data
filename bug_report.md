# バグ検証レポート

## 日付: 2026-01-24

## 修正済みのバグ

### 1. ✅ TypeError修正（コミット 5620371）

**場所**: セル6, セル8, セル12

**問題**: yfinanceが複数銘柄を取得した場合、カラム名がMultiIndex（タプル）になることがあり、`str.join()` でTypeErrorが発生する

**修正内容**:
- セル6: `', '.join(stock_data.columns)` → `', '.join(map(str, stock_data.columns))`
- セル8: `list(stock_data_jp.columns)` → `[str(col) for col in stock_data_jp.columns]`
- セル12: `', '.join(stock_data_jp.columns)` → `', '.join(map(str, stock_data_jp.columns))`

**検証結果**: ✅ **正しく修正されています**

---

### 2. ✅ 重大なバグ修正（コミット 994ec1c）

**問題**: データが取得できない場合でも処理が続行され、空のCSVファイルがダウンロードされる

**修正内容**:
1. 入力値の検証を追加（日付形式、銘柄コード、interval）
2. データダウンロードの例外処理を追加
3. データが空の場合に `ValueError` を発生させて処理を中断
4. カラム構造の検証を追加
5. ファイル操作のエラーハンドリングを追加

**検証結果**: ✅ **正しく修正されています**

---

## 🐛 新たに発見されたバグ

### 3. ❌ ファイル名とコンテンツの不整合

**場所**: セル8とセル12の間の整合性

**重要度**: 中

**問題の詳細**:

セル8でカラム名の日本語化が失敗した場合（カラム数が6でない、またはカラム名が期待と異なる場合）、`stock_data_jp` のカラムは英語のままですが、セル12では常に以下のファイル名が使用されます:

```python
filename_jp = f"{ticker_symbol}_{start_date}_to_{end_date}_日本語.csv"
```

**結果**: 英語のカラム名を持つCSVファイルが「〜日本語.csv」という名前で保存される

**再現手順**:
1. yfinanceのAPIが変更され、カラム数が6でない、またはカラム名が異なるデータが返される
2. セル8で警告が表示され、日本語化がスキップされる
3. セル12を実行すると、英語カラム名のファイルが「〜日本語.csv」という名前で保存される

**影響**:
- ユーザーがファイル名から日本語カラムだと誤解する
- ファイルの内容とファイル名が一致しない

**推奨される修正方法**:

セル12でカラムが実際に日本語化されているかをチェックし、ファイル名を動的に決定する:

```python
# カラムが日本語化されているかチェック
is_japanese = '始値' in [str(col) for col in stock_data_jp.columns]

if is_japanese:
    filename_jp = f"{ticker_symbol}_{start_date}_to_{end_date}_日本語.csv"
else:
    filename_jp = f"{ticker_symbol}_{start_date}_to_{end_date}_copy.csv"
    print("⚠️ カラムが日本語化されていないため、ファイル名を調整しました")
```

または、より明確にフラグを使用する方法:

セル8の最後に以下を追加:
```python
# 日本語化が成功したかどうかのフラグ
columns_converted_to_japanese = False

# ...（カラム変換のコード）...

if actual_cols == expected_cols:
    stock_data_jp.columns = ['始値', '高値', '安値', '終値', '調整後終値', '出来高']
    columns_converted_to_japanese = True  # フラグをセット
    print("✓ カラム名を日本語に変換しました")
```

セル12:
```python
# ファイル名を動的に決定
if columns_converted_to_japanese:
    filename_jp = f"{ticker_symbol}_{start_date}_to_{end_date}_日本語.csv"
else:
    filename_jp = f"{ticker_symbol}_{start_date}_to_{end_date}_copy.csv"
```

---

## まとめ

### 修正済み: 2件
- ✅ TypeError修正（カラム名がタプルの場合）
- ✅ 空データダウンロード防止の重大なバグ修正

### 新規発見: 1件
- ❌ ファイル名とコンテンツの不整合（日本語化失敗時のファイル名問題）

### 推奨アクション
1. ファイル名とコンテンツの不整合バグを修正
2. 修正後、再度テストを実行して確認
