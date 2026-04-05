# Fendi Price Tracker

[![Bright Data](https://img.shields.io/badge/Powered%20by-Bright%20Data-blue?style=flat-square)](https://brightdata.jp)
[![Fendi Price Tracker](https://img.shields.io/badge/Fendi%20Price%20Tracker-Managed%20Solution-orange?style=flat-square)](https://brightdata.jp/products/insights/price-tracker/fendi)
[![Python](https://img.shields.io/badge/Python-3.9%2B-yellow?style=flat-square)](https://python.org)

[![Bright Insights Price Tracker](https://raw.githubusercontent.com/danielshashko/bright-insights-assets/main/price-tracker-hero-v2.png)](https://brightdata.jp/products/insights/price-tracker/fendi)

イタリアのラグジュアリーファッションブランドであるFendiの価格をリアルタイムで追跡します。開始方法は2つあります。**フルマネージド**のインテリジェンスプラットフォーム、または独自のpipelineを構築するための**セルフサービスAPI**です。

---

## オプション1: Bright Insights - AI搭載の価格トラッキング（推奨）

**[Bright Insights](https://brightdata.jp/products/insights/price-tracker/fendi)** は、Bright Dataのフルマネージドなリテールインテリジェンスプラットフォームです。scraperの構築やインフラの保守は不要で、構造化された分析対応の価格データをダッシュボード、data feed、またはBIツールにそのまま配信できます。

**チームがBright Insightsを選ぶ理由:**
- 🚀 **セットアップ不要** - すぐに使えるダッシュボードとdata feedで数分で利用開始
- 🤖 **AIによる推奨** - 会話型AIアシスタントが数百万のデータポイントを即座に実用的なinsightへ変換
- ⚡ **リアルタイム監視** - 1時間ごとから日次までの更新頻度と即時アラート（email、Slack、webhook）
- 🌍 **無制限のスケール** - あらゆるwebsite、あらゆる地域、あらゆる更新頻度に対応
- 🔗 **プラグアンドプレイ統合** - AWS、GCP、Databricks、Snowflakeなど
- 🛡️ **フルマネージド** - schema変更、site更新、データ品質をBright Dataが自動で処理

**主なユースケース:**
- ✅ Fendiの**希少品・限定品の価格**を監視
- ✅ サイズやカラーごとの**在庫状況**をリアルタイムで追跡
- ✅ リテール価格に対する**リセールプレミアム**を検証
- ✅ MAPポリシー準拠を監視し、価格違反を検出
- ✅ 競合のプロモーションと販促動向を追跡
- ✅ クリーンで正規化されたデータを動的価格設定アルゴリズムやAIモデルに直接投入

> **月額$250から - [お見積もりはこちら →](https://brightdata.jp/products/insights/price-tracker/fendi)**

---

## オプション2: Web Scraper APIによるセルフサービス

独自のpipelineを構築したいですか？ Bright Dataの**Web Scraper API**なら、proxyやscrapingインフラを管理することなく、Fendiの商品データ（価格、在庫状況、レビューなど）にプログラムからアクセスできます。

### 前提条件

- Python 3.9以上
- [Bright Data account](https://brightdata.jp)（無料トライアルあり）
- Bright Dataの**API token**（[取得方法](https://docs.brightdata.jp/general/account/account-settings#api-token)）
- Fendi商品用のBright Data **Web Scraper ID**（[Web Scrapers control panelで確認](https://brightdata.jp/cp/scrapers)）

### セットアップ

1. **このrepositoryをclone**

   ```bash
   git clone https://github.com/bright-jp/fendi-price-tracker.git
   cd fendi-price-tracker
   ```

2. **依存関係をインストール**

   ```bash
   pip install -r requirements.txt
   ```

3. **認証情報を設定**

   `.env.example` を `.env` にコピーし、値を入力します:

   ```bash
   cp .env.example .env
   ```

   ```env
   BRIGHTDATA_API_TOKEN=your_api_token_here
   BRIGHTDATA_DATASET_ID=your_dataset_id_here
   ```

   > **Web Scraper IDの確認方法**
   > [Bright Data Control Panel](https://brightdata.jp/cp/scrapers) にログインし、**Web Scrapers** に移動して、
   > 「Fendi」を検索し、Web Scraper ID（形式: `gd_xxxxxxxxxxxx`）をコピーしてください。

---

## 使用方法

### 1. URLで特定の商品を追跡

Fendiの商品URLのリストを渡して、構造化された価格データを取得します:

```python
from price_tracker import track_prices

urls = [
    "https://www.fendi.com/en/products/sample-product-123456",
    # Add more product URLs here
]

results = track_prices(urls)
for item in results:
    print(f"{item.get('title')} - {item.get('final_price', item.get('price'))} {item.get('currency', '')}")
```

または直接実行:

```bash
python price_tracker.py
```

### 2. キーワードで商品を検索

キーワード検索に一致する商品を見つけます:

```python
from price_tracker import discover_by_keyword

results = discover_by_keyword("laptop", limit=50)
```

### 3. カテゴリURLで商品を閲覧

Fendiのカテゴリページからすべての商品を収集します:

```python
from price_tracker import discover_by_category

results = discover_by_category(
    "https://fendi.com/category/example",
    limit=100,
)
```

---

## 出力フィールド

各結果レコードには次のフィールドが含まれます:

| Field | Description |
|-------|-------------|
| `url` | 商品ページURL |
| `title` | 商品名 |
| `brand` | ブランド/メゾン |
| `price` | 小売価格 |
| `currency` | 通貨コード |
| `in_stock` | 在庫状況 |
| `color` | カラー |
| `size` | 利用可能なサイズ |
| `material` | 使用素材 |
| `sku` | SKU / 参照番号 |
| `images` | 商品画像 |
| `description` | 商品説明 |
| `timestamp` | 収集タイムスタンプ |

### 出力例

```json
[
  {
    "url": "https://www.fendi.com/en/products/sample-product-123456",
    "title": "Example Product Name",
    "brand": "Example Brand",
    "initial_price": 59.99,
    "final_price": 44.99,
    "currency": "USD",
    "discount": "25%",
    "in_stock": true,
    "rating": 4.5,
    "reviews_count": 1234,
    "images": ["https://fendi.com/images/product1.jpg"],
    "description": "Product description text...",
    "timestamp": "2025-01-15T10:30:00Z"
  }
]
```

---

## 高度なオプション

`trigger_collection()` 関数は、データ収集を制御するためのオプションパラメータを受け付けます:

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `limit` | integer | - | 返されるレコードの最大数 |
| `include_errors` | boolean | `true` | 結果にエラーレポートを含める |
| `notify` | string (URL) | - | snapshotの準備完了時に呼び出されるwebhook URL |
| `format` | string | `json` | 出力形式: `json`、`csv`、または `ndjson` |

オプション付きの例:

```python
from price_tracker import trigger_collection, get_results

inputs = [{"url": "https://www.fendi.com/en/products/sample-product-123456"}]
snapshot_id = trigger_collection(inputs, limit=200, notify="https://your-webhook.com/hook")
results = get_results(snapshot_id)
```

---

## リソース

- 🌟 [Fendi Price Tracker - Bright Insights (Managed)](https://brightdata.jp/products/insights/price-tracker/fendi)
- 🔧 [Fendi Scraper API](https://brightdata.jp/products/web-scraper/fendi)
- 📖 [Bright Data Web Scraper API Documentation](https://docs.brightdata.jp/scraping-automation/web-scraper-api/overview)
- 🗄️ [Web Scrapers Control Panel](https://brightdata.jp/cp/scrapers)
- 🔑 [How to get an API token](https://docs.brightdata.jp/general/account/account-settings#api-token)
- 🌐 [Bright Data Homepage](https://brightdata.jp)

---

*[Bright Data](https://brightdata.jp) により構築 - 業界をリードするwebデータプラットフォーム。*