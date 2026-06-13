# キヴォトスポータル — プロジェクトコンテキスト

> **新しいチャットでこのファイルを読み込むことで作業をスムーズに再開できます。**
> 最終更新: 2026-06-14（このセッションの全変更を反映済み）

---

## 1. プロジェクト概要

| 項目 | 内容 |
|---|---|
| サイト名 | **キヴォトスポータル** |
| ジャンル | ブルーアーカイブ（Blue Archive）非公式ファンサイト |
| 種別 | 静的サイト（ビルドステップなし） |
| ローカルパス | `C:\Users\vorw0\Documents\Cursor\キヴォトスポータル` |
| GitHub | `https://github.com/KOVO-cyber/kivotos-portal`（public） |
| 本番 URL | Cloudflare Pages（Connect to Git → kivotos-portal）※接続設定済み |
| 免責事項 | Yostar / NAT Games とは無関係の非公式ファンサイト |
| 素材参照元 | `C:\Users\vorw0\Documents\Cursor\G-CHIVE「ブルーアーカイブ」` |

---

## 2. 技術スタック

- **フロントエンド**: HTML + CSS + Vanilla JS のみ（フレームワーク・ビルドツールなし）
- **データ**: JSON ファイルを `fetch()` で動的ロード
- **ホスティング**: Cloudflare Pages（Git 連携、Build settings = None / 静的サイト）
- **バージョン管理**: Git / GitHub（ブランチ: `master`）
- **収益化（未実装）**: Google AdSense（`index.html` にコメントアウト済み）・Amazon Associates

---

## 3. ファイル構成

```
キヴォトスポータル/
├── index.html                  # ホーム（ダッシュボード）
├── archive/
│   └── index.html              # 生徒一覧（メインページ）
├── timeline/
│   └── index.html              # イベント年表
├── assets/
│   ├── data/
│   │   ├── characters.json     # 生徒データ（259キャラ）
│   │   ├── events.json         # 現在のイベント情報（プレースホルダー）
│   │   ├── timeline.json       # 年表データ（プレースホルダー）
│   │   └── affiliates.json     # アフィリエイト商品（プレースホルダー）
│   ├── icons/                  # 生徒アイコン画像（PNG）※ Git 管理対象
│   └── schools/                # 学園ロゴ画像（PNG）※ Git 管理対象
├── _headers                    # Cloudflare Pages キャッシュ設定
├── _redirects                  # Cloudflare Pages クリーン URL
├── .gitignore
└── CONTEXT.md                  # ← このファイル
```

---

## 4. デザインシステム

### テーマ
ブルーアーカイブ公式 UI を参考にした**ライトテーマ**（青空系グラデーション背景）。
ヘッダーのみダークネイビー（BA 公式の特徴）。

### カラーパレット（CSS 変数）

```css
/* 全ページ共通 */
--text:   #1e3a5f;   /* 本文テキスト（ダークネイビー） */
--muted:  #5a7a9a;   /* サブテキスト（ブルーグレー） */
--border: #aecde8;   /* ボーダー */
--navy:   #0e2040;   /* 最深ネイビー（タイトル等） */
--blue:   #1a5fa8;   /* メインブルー */
--sky:    #3b9bd8;   /* アクセントスカイブルー */
--card:   rgba(240,249,255,.97);  /* カード背景 */
--on-dark: rgba(210,235,255,.92); /* ダークヘッダー上のテキスト */
```

### 背景（全ページ統一）
```css
background: linear-gradient(155deg, #d6eaff 0%, #eaf5ff 45%, #d8edf7 100%);
background-attachment: fixed;
```

### ヘッダー（全ページ統一）
```css
background: rgba(10,24,52,.93);
border-bottom: 1px solid rgba(80,150,220,.3);
backdrop-filter: blur(10px);
```
ロゴ HTML:
```html
<div class="logo-mark">K</div>
<span class="logo-text">キヴォトス<span>ポータル</span></span>
```

---

## 5. 各ページの仕様

### 5-1. ホーム（`index.html`）

- ポータルタイトル（`キヴォトスポータル`）・サブタイトル表示
- **イベント速報バー**: `events.json` を fetch して現在のガチャ・イベント・総力戦を表示
- **3カラム ダッシュボード**:
  - 左: 公式情報フィード（プレースホルダー）
  - 中: ピックアップキャラ（`characters.json` の `goodsUrl` があるキャラ）
  - 右: アフィリエイト枠（`affiliates.json`）
- ナビ: ホーム / **生徒一覧** / イベント年表

### 5-2. 生徒一覧（`archive/index.html`）← **メインページ**

#### 表示仕様
- **表示順**: 五十音順（カタカナ・ひらがな → 漢字の順）
- **キャラ名**: 中央揃え、長名は自動縮小（7px まで）
- **学園名**: カード内に表示しない
- **学園アイコン**: カード右上に PNG バッジ（トグルで ON/OFF）
- **カード下部ボーダー**: 学園ごとに色分け

#### フィルター仕様（**複数選択可・AND 結合**）

| フィルター | ラベル | 場所 | 選択方式 |
|---|---|---|---|
| 学園 | 学園 | 常時表示 | 複数選択可 |
| 学園アイコン表示 | 学園アイコン | 右上トグル | ON/OFF |
| バージョン | バージョン | 詳細フィルタ（動的生成） | 複数選択可 |
| 役割 | 役割 | 詳細フィルタ | 複数選択可 |
| ポジション | ポジション | 詳細フィルタ | 複数選択可 |
| ユニットタイプ | STRIKER / SPECIAL | 詳細フィルタ | 複数選択可 |
| 攻撃タイプ | 攻撃タイプ | 詳細フィルタ | 複数選択可 |
| 防御タイプ | 防御タイプ | 詳細フィルタ | 複数選択可 |
| 武器タイプ | 武器タイプ | 詳細フィルタ | 複数選択可 |
| 名前検索 | （検索ボックス） | フィルターバー上部 | テキスト入力 |

- **「↺ フィルタをリセット」ボタン**: 詳細フィルタ末尾に設置、全フィルタ＋検索を一括クリア
- **フィルター状態管理**: `Map<string, Set<string>>`（空 Set = 絞り込みなし）
- **フィルター論理**: グループ内 OR、グループ間 AND

#### バージョン抽出ロジック
```js
function getVersion(name) {
  const m = name.match(/（([^）]+)）/);
  if (!m) return '通常';
  const v = m[1];
  return v.startsWith('スタイル') ? '臨戦' : v;
}
```

#### 五十音ソート
```js
function isKanaFirst(name) { return /^[ぁ-んァ-ヶー]/.test(name); }
allChars.sort((a, b) => {
  const ag = isKanaFirst(a.name) ? 0 : 1;
  const bg = isKanaFirst(b.name) ? 0 : 1;
  if (ag !== bg) return ag - bg;
  return a.name.localeCompare(b.name, 'ja');
});
```

### 5-3. イベント年表（`timeline/index.html`）

- `timeline.json` を fetch して年ごとにグループ表示
- タグ種別: `main`（イベント）/ `collab`（コラボ）/ `total`（総力戦）/ `main-story`（メインストーリー）
- 各イベントに PV リンク・グッズリンクを設置可能

---

## 6. データ構造

### `characters.json` — 1エントリの形式

```json
{
  "name": "アリス",
  "school": "ミレニアム",
  "role": "アタッカー",
  "image": "アリス_icon.png",
  "weaponType": "RG",
  "unitType": "STRIKER",
  "position": "BACK",
  "attackType": "神秘",
  "defenseType": "特殊装甲",
  "page": "#",
  "goodsUrl": ""
}
```

| フィールド | 値の例 |
|---|---|
| `name` | `"アリス"` / `"アリス（メイド）"` ← （）内がバージョン |
| `school` | `"アビドス"` / `"ゲヘナ"` / `"トリニティ"` / `"ミレニアム"` / `"山海経"` / `"百鬼夜行"` / `"アリウス"` / `"SRT"` / `"レッドウィンター"` / `"ヴァルキューレ"` / `"ワイルドハント"` / `"ハイランダー"` / `"その他"` |
| `role` | `"タンク"` / `"アタッカー"` / `"ヒーラー"` / `"サポート"` / `"スペシャル"` |
| `weaponType` | `"SG"` / `"SMG"` / `"AR"` / `"HG"` / `"SR"` / `"RG"` / `"MG"` / `"GL"` / `"RL"` / `"MT"` / `"FT"` |
| `unitType` | `"STRIKER"` / `"SPECIAL"` |
| `position` | `"FRONT"` / `"MIDDLE"` / `"BACK"` |
| `attackType` | `"爆発"` / `"貫通"` / `"神秘"` / `"振動"` |
| `defenseType` | `"軽装備"` / `"重装甲"` / `"特殊装甲"` / `"弾力装甲"` / `"複合装甲"` |
| `page` | URL or `"#"` ← 個別ページ未作成のため全て `"#"` |
| `goodsUrl` | Amazon アフィリエイト URL or `""` |

**総数**: 259キャラクター

> **注意**: G-CHIVE の `role` は `"サポーター"` / `"T.S"` だが、`characters.json` では `"サポート"` / `"スペシャル"` に統一済み。

### `events.json`（現在プレースホルダー）
```json
[{ "type": "gacha|event|total", "name": "名称", "startDate": "YYYY-MM-DD", "endDate": "YYYY-MM-DD", "url": "" }]
```

### `timeline.json`（現在プレースホルダー）
```json
[{ "year": "2021", "events": [{ "name": "...", "type": "main|collab|total|main-story", "date": "2021/02", "desc": "...", "pvUrl": "", "goodsUrl": "" }] }]
```

### `affiliates.json`（現在プレースホルダー）
```json
[{ "name": "商品名", "thumb": "画像URL", "type": "amazon|amiami", "url": "アフィURL" }]
```

---

## 7. 学園カラー・アイコンマッピング

```js
const SCHOOL_CLASS = {
  'アビドス':'abydos',    // border: #EA580C（オレンジ）
  'ゲヘナ':'gehenna',     // border: #B91C1C（赤）
  'トリニティ':'trinity', // border: #64748B（グレー）
  'ミレニアム':'millennium', // border: #2563EB（青）
  '山海経':'shanhai',     // border: #6D28D9（紫）
  '百鬼夜行':'hyakki',   // border: #DB2777（ピンク）
  'アリウス':'arius',     // border: #881337（深紅）
  'SRT':'srt',            // border: #0D9488（ティール）
  'レッドウィンター':'redwinter', // border: #DC2626（赤）
  'ヴァルキューレ':'valkyrie',    // border: #7C3AED（紫）
  'ワイルドハント':'wildhunt',    // border: #059669（緑）
  'ハイランダー':'highlander',    // border: #D97706（琥珀）
  'その他':'other',       // border: #64748B（グレー）
};
// アイコンパス: ../assets/schools/{学園名}.png
```

---

## 8. Git / デプロイ

```
GitHub リポジトリ  : https://github.com/KOVO-cyber/kivotos-portal
GitHub ユーザー   : KOVO-cyber
git user.email    : 293411492+KOVO-cyber@users.noreply.github.com
ブランチ          : master
デプロイ先        : Cloudflare Pages（Connect to Git → kivotos-portal）
```

### 通常の更新フロー
```powershell
cd "C:\Users\vorw0\Documents\Cursor\キヴォトスポータル"
git add -A
git commit -m "作業内容の説明"
git push
# → Cloudflare Pages が自動デプロイ
```

> **注意**: Cursor の Shell ツールが `no exit status` で応答しない場合は、上記コマンドをターミナルに直接貼り付けて実行する。

---

## 9. 完了済み作業

- [x] プロジェクト基本構成（3ページ HTML + JSON）
- [x] ブルアカ風ライトテーマ（空色グラデ背景・全ページ統一）
- [x] ダークネイビーヘッダー（`K` ロゴマーク + 青テキスト）統一
- [x] サイト名「キヴォトスポータル」日本語統一（全ナビ・タイトル）
- [x] ページ名「生徒アーカイブ」→「生徒一覧」統一
- [x] `characters.json` に 259 キャラクターデータ投入
- [x] 生徒一覧: 五十音順ソート（カタカナ→漢字）
- [x] 生徒一覧: キャラ名中央揃え・長名自動縮小（7px まで）
- [x] 生徒一覧: 学園名をカードから削除
- [x] 生徒一覧: 名前検索バー（リアルタイム絞り込み）
- [x] 生徒一覧: フィルター複数選択対応（`Map<string, Set>`）
- [x] 生徒一覧: 「↺ フィルタをリセット」ボタン
- [x] 生徒一覧: 詳細フィルタ G-CHIVE 風グリッドレイアウト（ラベル固定幅）
- [x] 生徒一覧: フィルター追加（ポジション・防御タイプ）
- [x] 生徒一覧: フィルターラベル正式化（役割 / STRIKER・SPECIAL / 攻撃タイプ / 防御タイプ / 武器タイプ）
- [x] 学園アイコントグル（ON/OFF で再レンダリング）
- [x] GitHub リポジトリ作成・プッシュ済み
- [x] Cloudflare Pages 接続設定済み
- [x] 不要ファイル削除（build_chars.py / setup_assets.ps1）

---

## 10. 未着手・今後の作業候補

### 高優先度（データ整備）
- [ ] `events.json` を最新イベントデータに更新（現在プレースホルダー）
- [ ] `timeline.json` に実際の年表データを入力
- [ ] `affiliates.json` に実際のアフィリエイトリンクを入力
- [ ] `characters.json` の `goodsUrl` に実際のアフィリエイトリンクを入力
- [ ] `characters.json` の `page`（全て `"#"`）に詳細ページ URL を設定

### 中優先度（機能拡充）
- [ ] 各キャラクターの個別詳細ページ（`/archive/{名前}/index.html`）
- [ ] Google AdSense 審査申請・コード有効化
- [ ] イベント速報の定期更新フロー整備
- [ ] スマートフォン向けレスポンシブ強化

### 低優先度
- [ ] OGP メタタグ（SNS シェア用画像・説明）
- [ ] サイトマップ / robots.txt（SEO）
- [ ] Amazon Associates リンク生成ワークフロー

---

## 11. 注意事項

| 項目 | 内容 |
|---|---|
| アイコン画像 | `仮icon` サフィックスあり（例: `アコ_仮icon.png`）。存在しない場合は `★` プレースホルダー表示 |
| G-CHIVE の role 表記 | G-CHIVE では `"サポーター"` / `"T.S"` だが当サイトは `"サポート"` / `"スペシャル"` |
| G-CHIVE の defenseType | `"弾力装甲"` が存在する。当サイトの `characters.json` には一部未反映の可能性あり |
| PowerShell 日本語パス | `.ps1` は CP932 問題があるのでコマンドは直接ターミナルに貼り付けて実行 |
| Shell ツール無応答 | Cursor の Shell ツールが `no exit status` になる場合はユーザーが手動実行 |
| メールアドレス保護 | GitHub noreply アドレス設定済み |

---

## 12. 新チャットで作業再開する際の指示

このファイル（`CONTEXT.md`）を添付またはドラッグ＆ドロップして：

```
CONTEXT.md を読み込みました。
キヴォトスポータルの作業を再開します。
今日やりたいことは〇〇です。
```
