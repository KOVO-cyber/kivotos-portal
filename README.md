# KIVOTOS PORTAL — キヴォトスポータル

ブルーアーカイブ非公式ファンポータルサイト。

## ディレクトリ構成

```
キヴォトスポータル/
├── index.html              # ホーム（ダッシュボード）
├── archive/
│   └── index.html          # 生徒アーカイブ（学園・ロールフィルタ付き一覧）
├── timeline/
│   └── index.html          # イベント年表
├── assets/
│   ├── bg.jpg              # ★ 要配置：背景画像（G-CHIVEからコピー or 別途用意）
│   ├── data/
│   │   ├── characters.json # キャラ一覧データ（新キャラ追加時に更新）
│   │   ├── events.json     # 現在開催中イベント・ガチャ（月数回更新）
│   │   ├── timeline.json   # イベント年表データ
│   │   └── affiliates.json # アフィリエイト商品（週次更新）
│   └── images/
│       └── characters/     # キャラ画像（filename は characters.json の image フィールドと一致させる）
└── README.md
```

## セットアップ

1. **背景画像を配置する**
   - `assets/bg.jpg` にブルアカの背景画像を配置する
   - G-CHIVE の `assets/bg.jpg` をそのままコピーしてもよい

2. **ローカルプレビュー**
   - Live Server (VS Code/Cursor 拡張) で `index.html` を開く
   - または `python -m http.server 8080` でローカルサーバーを起動

3. **デプロイ**
   - Cloudflare Pages または Vercel に `キヴォトスポータル/` フォルダをそのまま push

## データ更新手順

### イベント情報 (events.json) — 月数回

```jsonc
// type: "gacha" | "event" | "total"
{
  "type": "gacha",
  "name": "ピックアップ：〇〇",
  "startDate": "2025-06-10",
  "endDate": "2025-06-20",
  "url": "https://..."
}
```

### キャラ追加 (characters.json) — 新キャラ実装時

```jsonc
{
  "name": "キャラ名",
  "school": "ミレニアム",       // アビドス|ゲヘナ|トリニティ|ミレニアム|山海経|百鬼夜行|アリウス|SRT
  "role": "タンク",             // タンク|アタッカー|ヒーラー|サポート|スペシャル
  "image": "filename.jpg",      // assets/images/characters/ に配置
  "page": "../archive/xx.html", // 詳細ページ（なければ "#" でOK）
  "goodsUrl": ""
}
```

### アフィリエイト商品 (affiliates.json) — 週次

```jsonc
{
  "name": "商品名",
  "thumb": "画像URL or 空文字",
  "type": "amazon",    // "amazon" | "amiami"
  "url": "アフィリエイトURL（PR）"
}
```

## デザイン仕様

| 要素 | 値 |
|------|----|
| フォント | Hiragino Kaku Gothic ProN → Segoe UI → Yu Gothic UI |
| 背景 | bg.jpg + ブラー4px + オーバーレイ |
| ヘッダーBG | rgba(20,38,64,.92) + backdrop-blur |
| アクセント橙 | #F39801 |
| プライマリ紺 | #1f3b62 |
| 学園カラー | アビドス:#EA580C ゲヘナ:#B91C1C トリニティ:#64748B ミレニアム:#2563EB 山海経:#6D28D9 百鬼夜行:#DB2777 アリウス:#881337 SRT:#0D9488 |

## 免責事項

当サイトは「ブルーアーカイブ -Blue Archive-」の非公式ファンサイトです。
Yostar / NAT Games のいかなる関与も受けていません。
ゲーム内画像・音楽・その他コンテンツの著作権は各権利者に帰属します。
