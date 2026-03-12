# LAÉSSE 取扱店一覧ページ

## プロジェクト概要
LAÉSSE（化粧品メーカー）の取扱店一覧を表示するランディングページ。
代理店系（エキスパート・認定・代理店・取次店）と取扱店系（プレステージ・アドバンス・ベーシック・スターター）の全822店舗情報を掲載し、紹介URLを通じて購入導線を提供する。

## Notion プロジェクトページ
- URL: https://www.notion.so/31eaa962599881bb8825e60af85275fd
- 「Notion更新して」と言われたらこのページの進捗ログを更新すること

## GitHub
- リポジトリ: https://github.com/Shuhei0910/laesse-stores.git

## 技術スタック
- HTML / CSS / JavaScript（バニラ、フレームワークなし）
- 単一ファイル構成（`index.html`）
- Google Fonts（Cormorant Garamond）
- レスポンシブ対応

## ディレクトリ構成
```
laesse.stores.260225/
├── index.html                      # メインページ（CSS/JSすべて内包）
├── images/                         # ヒーロー画像など
├── LAÉSSE取扱店一覧_確認用.xlsx      # クライアント確認用Excel（全942件＋個人名除外822件）
└── CLAUDE.md                       # このファイル
```

## 重要な注意点

### 紹介URL（最重要）
- `firstSalonUrl` / `firstPublicUrl` は各店舗固有のURL
- **絶対に間違えてはいけない** — 各店舗の売上に直結する
- フォーマット: `https://member.laesse0715.com/{ID}` / `https://member.laesse0715.com/U{ID}`
- 更新時はExcelのデータとIDベースで厳密マッチングすること

### 店舗データ構造
```javascript
{
  name: '店舗名',
  yomi: 'よみがな',
  prefecture: '都道府県',
  address: '住所',
  mapUrl: 'Google Maps URL',
  rank: 'expert|certified|agency|retailer|prestige|advance|basic|starter',
  area: '地方ブロック（北海道/東北/関東/中部/近畿/中国/四国/九州・沖縄）',
  products: ['cosmetics', 'pinktone', 'jewelry_peel', 'photo'],
  firstSalonUrl: '初回サロン紹介URL',
  firstPublicUrl: '初回一般紹介URL',
  secondSalonUrl: '2回目サロンURL（共通）',
  secondPublicUrl: '2回目一般URL（共通）',
  instagram: 'InstagramURL',
  hotpepper: 'HotPepper/HPのURL',
}
```

### 店舗数内訳（822件）
- 代理店系: エキスパート26 / 認定6 / 代理店49 / 取次店9
- 取扱店系: プレステージ55 / アドバンス154 / ベーシック469 / スターター54
- 個人名の店舗は除外済み（120件）

### データソース
- 代理店情報はExcelファイル（`代理店URL(2).xlsx`など）で管理されている
- Excel列の「Instagram」と「ホットペッパー/HP」が入れ違いの店舗が存在するので注意（URLのドメインで判別する）
- rankの対応:
  - `エキスパート代理店` → `expert`
  - `認定代理店` → `certified`
  - `代理店` → `agency`
  - `非代理店` → `retailer`（表示名は「取次店」）
  - `取扱店/ プレステージサロン` → `prestige`
  - `取扱店/ アドバンスサロン` → `advance`
  - `取扱店/ ベーシックサロン` → `basic`
  - `取扱店/ スターターサロン` → `starter`
- 取扱店系は紹介URLがないため、モーダルでは「店舗に直接お問い合わせください」と表示

### クライアント確認用Excel
- `LAÉSSE取扱店一覧_確認用.xlsx` にまとめ版あり
- シート1: 全942件（個人名含む）
- シート2: 個人名除外822件（サイト掲載対象）
- クライアント記入欄（掲載可否・掲載名・URL・備考）は黄色背景
- No.列は `=ROW()-1` で行削除時に自動連番振り直し

### アポストロフィ問題（要注意・過去に障害発生）
- `Nail Salon Rin's` や `Nm'Rose` など、名前にアポストロフィを含む店舗はJavaScript内で `\'` エスケープが必要
- `Men's Daryle` は右シングルクォート（U+2019 `'`）を使用しており、JS的には問題なかったが、スクリプトで通常のアポストロフィ（U+0027 `'`）に変換してしまいエスケープ漏れが発生 → **全店舗が表示不能になる障害を引き起こした**
- **教訓: 右シングルクォート（U+2019）はJSのシングルクォート文字列内で安全。むやみに変換しないこと。変換する場合は必ず `\'` にエスケープすること**
- **必ず `node --check` でJS構文チェックしてからプッシュすること**

### デプロイ
- GitHub Pages: https://shuhei0910.github.io/laesse-stores/
- mainブランチへのプッシュで自動デプロイ
- 反映後もブラウザキャッシュが残る場合あり（Cmd+Shift+Rで確認）

### フィルター機能
- **種別フィルター**: 2段構え（代理店系/取扱店系 → 詳細種別ボタン）
- **エリアフィルター**: 地方ブロック8つ（件数表示付き）
- **商品フィルター**: 化粧品/ピンクトーン/jewelry peel研修/フォト卸
- **キーワード検索**: 店舗名・よみがなで検索

## 更新履歴
- 2026-03-10: 全822店舗に拡張（92件→822件）、取扱店系4カテゴリ追加、非代理店→取次店に名称変更、種別フィルター2段構え化、エリアフィルター地方ブロック化、URL無し店舗の専用UI追加、個人名120件除外、クライアント確認用Excelまとめ版作成
- 2026-03-10: 非代理店9店舗の紹介URL追加、全67店舗にInstagram/HotPepperリンク追加、Men's Daryleのクォート修正、アポストロフィ変換による全店舗表示不能障害→修正
