# LAÉSSE 取扱店一覧ページ

## プロジェクト概要
LAÉSSE（化粧品メーカー）の取扱店一覧を表示するランディングページ。
代理店・認定代理店・エキスパート代理店・非代理店（取扱店）の各店舗情報を掲載し、紹介URLを通じて購入導線を提供する。

## 技術スタック
- HTML / CSS / JavaScript（バニラ、フレームワークなし）
- 単一ファイル構成（`index.html`）
- Google Fonts（Cormorant Garamond）
- レスポンシブ対応

## ディレクトリ構成
```
laesse.stores.260225/
├── index.html        # メインページ（CSS/JSすべて内包）
├── images/           # ヒーロー画像など
└── CLAUDE.md         # このファイル
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
  rank: 'expert|certified|agency|retailer',  // エキスパート|認定|代理店|非代理店
  area: 'エリア',
  products: ['cosmetics', 'pinktone', 'jewelry_peel', 'photo'],
  firstSalonUrl: '初回サロン紹介URL',
  firstPublicUrl: '初回一般紹介URL',
  secondSalonUrl: '2回目サロンURL（共通）',
  secondPublicUrl: '2回目一般URL（共通）',
  instagram: 'InstagramURL',
  hotpepper: 'HotPepper/HPのURL',
}
```

### データソース
- 代理店情報はExcelファイル（`代理店URL(2).xlsx`など）で管理されている
- Excel列の「Instagram」と「ホットペッパー/HP」が入れ違いの店舗が存在するので注意（URLのドメインで判別する）
- rankの対応:
  - `エキスパート代理店` → `expert`
  - `認定代理店` → `certified`
  - `代理店` → `agency`
  - `非代理店` → `retailer`

### アポストロフィ問題
- `Nail Salon Rin's` や `Nm'Rose` など、名前にアポストロフィを含む店舗はJavaScript内で `\'` エスケープが必要
- `Men's Daryle` は右シングルクォート（`'`）を使用していたバグがあり、`\'` に修正済み

## 更新履歴
- 2026-03-10: 非代理店9店舗の紹介URL追加、全67店舗にInstagram/HotPepperリンク追加、Men's Daryleのクォート修正
