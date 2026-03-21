# Yuiel ナイトブラ — BASE設置ガイド

## BASEでの設置方法

BASEでは「HTML編集 App」を使ってカスタムHTMLを設置します。

---

## 前提

- BASEアカウントにログイン済み
- 「HTML編集」App がインストール済み
  - 未インストールの場合: BASE管理画面 → Apps → 「HTML編集」で検索 → インストール

---

## 手順1: HTML編集Appでテーマを編集

1. BASE管理画面 → **デザイン** → **HTML編集**
2. 現在のテーマを選択 → **編集**
3. 以下のファイルが表示される:
   - `top.html` ← **index.htmlの内容をここに**
   - `item.html` ← **product.htmlの内容をここに**
   - `style.css`
   - その他テンプレート

---

## 手順2: 画像のアップロード

BASEでは画像を直接ホスティングできないため、以下の方法で対応:

### 方法A: BASE商品画像として登録（推奨）
1. BASE管理画面 → **商品管理** → 商品を選択
2. 商品画像として各カラーの写真をアップロード
3. アップロード後、画像URLを右クリック → URLをコピー
4. HTMLの `images/xxx.webp` を取得したURLに置換

### 方法B: 外部ホスティング
1. 画像をCloudflare R2、Imgur、またはGoogle Driveにアップロード
2. 公開URLを取得
3. HTMLの `images/xxx.webp` をURLに置換

### 画像URL一括置換リスト

HTMLの以下のパスを、実際の画像URLに置換してください:

```
images/hero-main.webp       → [ヒーローメイン画像URL]
images/philosophy-main.webp → [フィロソフィーメイン画像URL]
images/philosophy-sub1.webp → [フィロソフィーサブ1画像URL]
images/philosophy-sub2.webp → [フィロソフィーサブ2画像URL]
images/feature-01.webp      → [Feature01画像URL]
images/feature-02.webp      → [Feature02画像URL]
images/color-white.webp     → [ホワイト画像URL]
images/color-beige.webp     → [ベージュ画像URL]
images/color-khaki.webp     → [カーキ画像URL]
images/color-black.webp     → [ブラック画像URL]
images/product-khaki.webp   → [商品カードカーキ画像URL]
images/product-black.webp   → [商品カードブラック画像URL]
images/product-beige.webp   → [商品カードベージュ画像URL]
images/gallery-front.webp   → [ギャラリー正面画像URL]
images/gallery-side.webp    → [ギャラリー横面画像URL]
images/gallery-back.webp    → [ギャラリー背面画像URL]
images/gallery-wearing.webp → [ギャラリー着用画像URL]
images/gallery-material.webp→ [ギャラリー素材画像URL]
```

---

## 手順3: BASEテンプレートタグの対応

BASEのHTML編集では、独自のテンプレートタグを使う必要があります。
現在のHTMLをBASEに貼り付ける際、以下の点を調整:

### カート機能の連携
現在のHTMLの「カートに追加」ボタンは見た目のみです。
BASE上で実際にカート連携するには:

```html
<!-- 現在のボタン -->
<button class="product-card-btn">カートに追加</button>

<!-- BASE対応版に差し替え -->
<form action="/items/[商品ID]/add_cart" method="post">
  <input type="hidden" name="variant_id" value="[バリエーションID]">
  <input type="hidden" name="quantity" value="1">
  <button type="submit" class="product-card-btn">カートに追加</button>
</form>
```

### 商品ページのBASEタグ
product.html の商品情報をBASE動的データに差し替え可能:

```html
<!-- 価格表示 -->
{ItemPrice}

<!-- 商品名 -->
{ItemTitle}

<!-- カートフォーム -->
{AddCartForm}
```

---

## 手順4: ドメイン設定（任意）

独自ドメインを設定する場合:
1. BASE管理画面 → **ショップ設定** → **独自ドメイン**
2. ドメインを入力（例: yuiel.com）
3. DNS設定でCNAMEレコードを追加

---

## 手順5: 公開前チェックリスト

- [ ] 全画像が表示されているか確認
- [ ] モバイル表示の確認（レスポンシブ）
- [ ] カート追加ボタンが正常に動作するか
- [ ] 特定商取引法ページへのリンクが正しいか
- [ ] プライバシーポリシーページが設定されているか
- [ ] OGP画像が正しく表示されるか（SNSシェア用）
- [ ] Google Analytics / Meta Pixel の設置（任意）

---

## 注意事項

- BASEの「HTML編集」は上級者向け機能。テーマの復元ポイントを必ず作成してから編集
- CSSはインラインで記述済み（`<style>`タグ内）のため、別ファイル不要
- JavaScriptもインラインで記述済み
- BASEの無料プランでもHTML編集は利用可能
