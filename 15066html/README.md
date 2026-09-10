# 商品詳細ページ テンプレート（チーム向け）

もらため商品詳細 HTML のテンプレートです。案件ごとにこのディレクトリを使い、プロジェクト ID に合わせて編集してください。

開発は **Live Server** だけで行います。`npm` もビルドツールも不要です。

> **フロントエンド未経験の方へ**
> まず **[はじめてガイド（これだけでOK）](./はじめてガイド.md)** だけ読んで進めてください。本 README はくわしい説明用です。

---

## 0. 入手と整理（ZIP）

GitHub から ZIP をダウンロードして解凍したら、**作業に不要なものは削除**してから始めます（フォルダが見通しよくなります）。

> **コツ:** 毎回 GitHub から取り直すより、自分用にカスタマイズした `00000html` をコピーして次の案件に使う方が楽です。また `dist/00000.html` を編集用にし、参照用は `dist/reference.html`（プロジェクト ID なし）として複製すると、部品のコピーがしやすいです。くわしくは [はじめてガイド「使い方のコツ」](./はじめてガイド.md#使い方のコツおすすめ) を参照。

### 残すもの（これ以外は削除）

リポジトリ直下に残すのは **`00000html` フォルダだけ** です。その中身は次を残します。

```
00000html/
├── はじめてガイド.md   # 未経験者向け（これだけでOK）
├── README.md           # チーム向け手順（くわしい説明）
├── dist/               # HTML・画像・スクリプト・CSS（納品・プレビュー。ここを直接編集する）
└── .vscode/            # Live Server 用設定
```

| 残すもの | 役割 |
| -------- | ---- |
| `はじめてガイド.md` | 未経験者向けの最短手順 |
| `README.md` | チーム向けのくわしい手順 |
| `dist/` | 編集・納品する成果物（HTML と CSS を直接編集する） |
| `.vscode/` | Live Server 用設定 |

### 削除してよいもの（例）

ZIP に含まれている次のようなものは **案件作業では使いません**。削除して構いません。

- ルートの `README.md`（作成者向け）
- `package.json` / `package-lock.json`（npm・作成者用）
- `.github/` / `.cursor/` / ルートの `.vscode/`
- `.prettierrc.json` / `.stylelintrc.json` / `eslint.config.mjs` / `.gitignore`

整理後は `00000html`（またはリネーム後の `15403html` など）だけを VS Code / Cursor で開いて作業します。

**チームが主に触るのは次です。**

- `dist/` … HTML・画像・スクリプト・CSS

---

## 1. はじめに（必須）

作業開始時に、プレースホルダ `00000` を **実際のプロジェクト ID** に置き換えます。クラス名の `p00000` も文字列中の `00000` を含むため、同じ置換で `p15403` になります。

例: プロジェクト ID が `15403` の場合

| 変換前                 | 変換後（一括置換の結果） |
| ---------------------- | ------------------------ |
| `00000`                | `15403`                  |
| `p00000`               | `p15403`                 |
| フォルダ名 `00000html` | `15403html`（手動リネーム） |
| `00000.html`           | `15403.html`（手動リネーム） |

### VS Code / Cursor での一括置換

1. テンプレートフォルダ（例: `00000html`）を開く
2. サイドバーで対象フォルダを選択した状態で、検索パネルを開く（`Cmd + Shift + F` / `Ctrl + Shift + F`）
3. 「ファイルで置換」で次を実行する

```
00000  →  15403
```

> **注意**
>
> - 置換範囲は `00000html`（またはコピー後のフォルダ）内に限定する
> - HTML / CSS / `.vscode/settings.json` など、拡張子を問わずすべて置換する
> - 商品 URL など、プロジェクト ID とは無関係な `00000` を含む文字列（例: `000000000177`）は意図せず変わることがある。置換後に目視で確認する

### フォルダ・ファイル名の変更

置換後、次もリネームします。

```
00000html/                 →  15403html/
00000html/dist/00000.html  →  15403html/dist/15403.html
```

---

## 2. ディレクトリ構成（チームが使う範囲）

```
00000html/
├── dist/                 # 納品・プレビュー用（ここを直接編集する）
│   ├── 00000.html
│   ├── images/
│   ├── scripts/
│   └── styles/
│       └── style.css     # CSS 本体（直接編集する。ビルドで生成されるものではない）
└── .vscode/              # Live Server 用設定
```

| ディレクトリ  | 役割 |
| ------------- | ---- |
| `dist/`       | 納品・プレビュー用一式。HTML・画像・スクリプト・CSS をすべてここで直接編集する |
| `.vscode/`    | Live Server 用の自動設定 |

`dist/styles/style.css` は Sass などのビルドを経由せず、このファイル自体を直接編集します。中は [Modern BEM コーディング規約 (Draft)](https://github.com/YoshinoriKanno/doc-modern-bem) に合わせて `Foundation` / `Layout` / `Blocks` / `Pages` の見出しコメントで区切ってあります。

```css
/* ==========================================================================
   Blocks — button
   ========================================================================== */
.p00000 .p00000-button {
  ...
}
```

| コメント区分   | 役割 |
| -------------- | ---- |
| `Foundation`   | リセット・フォント読み込みなど、BEM の階層に属さない共通基盤 |
| `Layout`       | 外側の余白や配置など、コンテキスト依存のスタイル専用（本テンプレでは container / spacing / visibility / typography） |
| `Blocks`       | 再利用する UI コンポーネント（Block）。1 コメント区分 = 1 Block。Element / Modifier / State も同じ区分内に書く |
| `Pages`        | この案件専用の組み立て用 Block（`.p00000-project`）。本テンプレでは `Pages — project` 区分 |

> Element 単位でコメント区分を切らない。`Pages — project` に他 Block の定義を同居させない。親から子 Block を直接スタイリングする場所も作らない。詳細は上記規約を参照。

---

## 3. Modern BEM とクラス命名

命名規則・State の扱い・アンチパターンなどの詳細は、[Modern BEM コーディング規約 (Draft)](https://github.com/YoshinoriKanno/doc-modern-bem) を参照してください。以下は本テンプレート固有の補足です。

プレフィックス `p00000`（置換後は `p15403` など）は、サイト全体 CSS との衝突回避用です。Block 名の前に付けます。

| 種類         | 命名規則                         | 例                                           |
| ------------ | --------------------------------- | -------------------------------------------- |
| **Block**    | `p00000-{block}`                 | `p00000-box`, `p00000-section`               |
| **Element**  | Block 名 + `__` + 要素名         | `p00000-allergy__heading`                    |
| **Modifier** | Block / Element + `--` + 修飾    | `p00000-section--1`, `p00000-indent--11`     |
| **State**    | `is-*` または `data-*`（動的）   | `is-open`（必要時）                          |

### どこを編集するか

- **再利用する UI を追加・変更** → `dist/styles/style.css` の `Blocks` セクションに追記する
- **この案件だけの構成・余白・配置** → `dist/styles/style.css` の `Pages — project` セクション（`.p00000-project` / Element）に書く
- **余白コンテナ・表示切替・フォント指定** → `Layout` セクションを使う
- **共通 Block として切り出せそうな UI** → `Pages — project` ではなく `Blocks` セクションに置く

> Block 同士を親から直接スタイリングしない（例: `.p00000-project .p00000-button { }` は不可）。配置用 Element（`p00000-project__section` や `line-up__card-action` など）に余白を持たせる。

### Pages — project セクションの書き方

案件専用スタイルは `dist/styles/style.css` の `Pages — project` セクションにまとめます。

#### まず読む

- ボタン・見出し・商品一覧など「他の案件でも使いそうな部品」→ `Blocks` セクションに追記する（例: `.p00000-button`）
- 「この案件のこのページにしか出てこない」見出し帯・特集・レシピ枠など → `Pages — project` セクションに書く

#### 書き方の基本

1. HTML の `<article class="p00000-main p00000-project">` が土台
2. セクションごとに Element クラスを付ける（アンダースコア 2 つ `__`）
3. スタイルは `.p00000-project__名前` に書く（`Pages — project` セクション内で完結）

HTML 例:

```html
<div class="p00000-project__feature">...</div>
<div class="p00000-project__recipes">...</div>
<div class="p00000-project__comments">...</div>
```

CSS 例（`Pages — project` セクションに追加していく）:

```css
.p00000 .p00000-project__feature {
  /* ... */
}
.p00000 .p00000-project__recipes {
  /* ... */
}
.p00000 .p00000-project__comments {
  /* ... */
}
```

#### やってはいけないこと

他の部品をここから直接いじらない。

```css
/* NG */
.p00000 .p00000-project .p00000-button {
  margin-top: 24px;
}

/* OK: 余白用の枠（Element）を用意する */
.p00000 .p00000-project__action {
  margin-top: 24px;
}
```

```html
<div class="p00000-project__action">...</div>
```

`.p00000-feature` のように `project` を外した別名をこのセクションに増やさない（1 セクション = 1 Block。`.p00000-project` とその `__` 要素だけにする）。

### 主な Block / Layout / Page クラス

| クラス | 用途 |
| ------ | ---- |
| `p00000-main` | 商品詳細ページの共通シェル（フォント・枠など） |
| `p00000-project` | この案件専用のページ Block（`Pages — project` セクション） |
| `p00000-section` | セクション Block |
| `p00000-delivering-items` | 今回お届けする商品一覧 |
| `p00000-line-up` | 商品ラインナップ |
| `p00000-box` | 白ボックス |
| `p00000-heading` | 共通見出し |
| `p00000-banner` / `p00000-banner__item` | バナー |
| `p00000-balloon` | 下向き三角付きのキャッチコピーバルーン |
| `p00000-item-label` | 医薬部外品などの商品区分ラベル |
| `p00000-item-spec` | 内容量・価格などの商品スペック（`dl` / `dt` / `dd`） |
| `p00000-item-spec--horizontal` | 上記の PC 横並び版（640px 以上で flex 横並び） |
| `p00000-item-spec__row--cell` | 横並び版の行（`--horizontal` と併用） |
| `p00000-item-spec__term--slash` | 項目名の後に `／` を表示（横並び版で使用） |
| `p00000-allergy` | アレルギー情報 |
| `p00000-accordion-details` | 開閉アコーディオン |
| `p00000-youtube` | YouTube 埋め込み |
| `p00000-cta-x` | X（旧 Twitter）投稿 CTA |
| `p00000-button` | 共通ボタン |
| `p00000-container` | 余白コンテナ（Layout） |
| `p00000-mt-2` など | 余白ユーティリティ（Layout・下表参照） |
| `p00000-font-noto-serif-jp` など | フォント指定（Layout） |
| `p00000-hidden` / `pc:p00000-hidden` | 表示切替（Layout） |

> HTML に出てくる `p00000-detail-container` / `p00000-mv` はサイト側ラッパー用で、本テンプレの CSS Block ではありません。

### 余白ユーティリティ（`Layout — spacing utilities` セクション）

[Tailwind CSS](https://tailwindcss.com/docs/margin) 風の命名です。`dist/styles/style.css` の `Layout — spacing utilities` セクションに定義されています。

形式: `p00000-{種類}{方向?}-{サイズ}`

| 種類 | 意味 |
| ---- | ---- |
| `m` | margin |
| `p` | padding |

| 方向 | 意味 |
| ---- | ---- |
| （なし） | 四方向 |
| `x` | 左右 |
| `y` | 上下 |
| `t` | 上 |
| `r` | 右 |
| `b` | 下 |
| `l` | 左 |

| サイズ | 値 |
| ------ | -- |
| `2` | `0.5rem`（8px 相当） |
| `4` | `1rem`（16px 相当） |
| `8` | `2rem`（32px 相当） |

例:

```html
<p class="p00000-item-label p00000-mt-2">医薬部外品</p>
<dl class="p00000-item-spec p00000-mt-8">...</dl>
<div class="p00000-px-4 p00000-py-2">...</div>
```

利用可能なクラス（いずれも `p00000-` 付き）:

- margin: `m-2/4/8` `mx-2/4/8` `my-2/4/8` `mt-2/4/8` `mr-2/4/8` `mb-2/4/8` `ml-2/4/8`
- padding: `p-2/4/8` `px-2/4/8` `py-2/4/8` `pt-2/4/8` `pr-2/4/8` `pb-2/4/8` `pl-2/4/8`

---

## 4. 開発環境（Live Server）

### 推奨拡張機能

- Live Server（プレビュー用）

### プレビュー（Live Server）

1. Live Server で `dist/00000.html`（置換後は `dist/15403.html` など）を開く
2. 開発中はローカル CSS を読み込む

```html
<link href="./styles/style.css" rel="stylesheet" type="text/css" />
```

`dist/styles/style.css` を保存すると、Live Server が自動でブラウザをリロードします（ビルドや監視は不要です）。

公開用に CDN へ切り替える場合は、ローカルをコメントアウトし、本番 URL を有効にします。

```html
<!-- <link href="./styles/style.css" rel="stylesheet" type="text/css" /> -->
<link href="https://image.moratame.net/images/detail/15403/styles/style.css" rel="stylesheet" type="text/css" />
```

### アコーディオン（使う場合）

`p00000-accordion-details` を使う場合は、JS も CSS と同様にローカル / CDN の切り替えが必要です。

1. `dist/scripts/accordion.2.js` を本番の画像サーバへアップロードする
   （例: `https://image.moratame.net/images/detail/15403/scripts/accordion.2.js`）
2. HTML 末尾近くの script を、開発用ローカルから CDN へ切り替える

開発中（ローカル）:

```html
<script src="./scripts/accordion.2.js"></script>
<!-- <script src="https://image.moratame.net/images/detail/15403/scripts/accordion.2.js"></script> -->
```

公開時（CDN）:

```html
<!-- <script src="./scripts/accordion.2.js"></script> -->
<script src="https://image.moratame.net/images/detail/15403/scripts/accordion.2.js"></script>
```

> アコーディオンを使わない案件では、該当の HTML・script タグを削除して構いません。一括置換後は URL 内のプロジェクト ID も確認してください。

---

## 5. 作業の流れ（チェックリスト）

1. [ ] GitHub から ZIP を入手し、`00000html` 以外を削除して作業場所に置く
2. [ ] `00000` → `XXXXX` を一括置換する（`p00000` も同時に `pXXXXX` になる）
3. [ ] フォルダ名・HTML ファイル名をリネームする
4. [ ] Live Server で `dist/*.html` を開く
5. [ ] `dist/*.html` と `dist/styles/style.css` を中心にコーディングする
6. [ ] 画像は `dist/images/`（または CDN パス）に配置する
7. [ ] 公開前に CSS のリンク先（ローカル / CDN）を確認する
8. [ ] アコーディオン利用時は `accordion.2.js` をアップロードし、script のコメントを切り替える

---

## 6. マークアップの例

```html
<article class="p00000-main p00000-project">
  <section class="p00000-section p00000-section--1">
    <div class="p00000-container">
      <div class="p00000-box">
        <h3 class="p00000-heading">見出し</h3>
        <p class="p00000-font-noto-serif-jp">明朝体のテキスト</p>
      </div>
    </div>
  </section>
</article>
```

### ぶら下がりインデント（ご注意点など）

先頭記号（・ ※ ● など）を 1 文字分、番号付き（※1 ＊1 など）を 2 文字分ずらします。

```html
<p class="p00000-indent p00000-indent--11">※すべての菌を取り除くわけではありません。</p>
<p class="p00000-indent p00000-indent--07">・1日の摂取目安量を守ってください。</p>
<p class="p00000-indent p00000-indent--11">●乳幼児・小児の手の届かない所に置いてください。</p>
<p class="p00000-indent p00000-indent--22">※1 食物アレルギーの方は原材料名をご確認ください。</p>
<p class="p00000-indent p00000-indent--19">＊1 原材料の特性により色等が変化することがあります。</p>
```

| クラス | 用途 |
|--------|------|
| `p00000-indent` | ぶら下がりインデントのベース（必須） |
| `p00000-indent--07` | 0.7em（・ など） |
| `p00000-indent--10` | 1em |
| `p00000-indent--11` | 1.1em（※ ● など） |
| `p00000-indent--19` | 1.9em（＊1 など） |
| `p00000-indent--22` | 2.2em（※1 など） |

### 文中の上付き注釈（米印など）

```html
<p>除菌<sup class="p00000-sup">※</sup>し、新たなニオイの発生を防ぎます。</p>
<p class="p00000-indent p00000-indent--11">※すべての菌を取り除くわけではありません。</p>
```

---

## 7. 補足

### 文字化け対策

特殊文字は実体参照を使います。

```
® → &#174;
```
