# CSS アーキテクチャ（Modern BEM・プレーン CSS）

Sass などのビルドステップは使わない。`{id}html/dist/styles/style.css` が唯一の CSS ソースであり、**このファイルを直接編集する**。コンパイル済みの生成物ではない。

クラス設計は [Modern BEM コーディング規約](https://github.com/YoshinoriKanno/doc-modern-bem)（`foundation` / `layout` / `blocks` / `pages`）に準拠する。かつては SCSS パーシャルでファイル分割していたが、現在は `style.css` 1 ファイルの中を次の見出しコメントで区切って表現する。

```css
/* ==========================================================================
   Blocks — button
   ========================================================================== */
.p{id} .p{id}-button {
  ...
}
```

- `Foundation` — リセット・ベーススタイルとフォント読み込み（`@import` の Google Fonts 含む）。Block ではないため BEM プレフィックスを持たない。
- `Layout` — コンテキスト依存のスタイルのみ：`container`、`spacing`（margin/padding ユーティリティ）、`typography`（フォントユーティリティ）、`visibility`（`p{id}-hidden`、`pc:p{id}-hidden` などのブレークポイントヘルパー）。
- `Blocks` — 再利用する UI Block ごとに 1 コメントセクション（`accordion` / `allergy` / `balloon` / `banner` / `box` / `button` / `cta-x` / `delivering-items` / `heading` / `item-label` / `item-spec` / `line-up` / `main` / `section` / `youtube`）。各 Block の Element / Modifier / State も同じセクション内に書く。
- `Pages — project` — この案件専用のページ組み立て用 Block（`.p{id}-project` とその `__` 要素）のみを扱う単一セクション。他案件でも使い回せそうな UI をここに書かない。

`style.css` 冒頭の `.p{id} { ... }` がルート枠で、`max-width: 640px` のコンテナと `box-sizing: border-box` を持つ。個々のルールはすべて `.p{id} .p{id}-xxx { ... }` の形で `.p{id}` を前置してスコープしている（サイト側の jQuery Mobile / Bootstrap スコープ CSS に詳細度で負けないようにするため）。

## 守るべきルール

- 見出しコメント（`/* ===... Blocks — xxx ... === */`）を消したり、Block をまたいで混在させたりしない。1 セクション = 1 Block を保つ。
- ある Block/ページのセクションから別の Block を直接スタイリングしない（例: `.p{id}-project .p{id}-button {}` は不可）。代わりにページ／Block 側に専用のラッパー Element を追加し、余白やレイアウトのルールはそちらに書く。
- クラス命名: Block は `p{id}-{block}`、Element は `p{id}-{block}__{element}`、Modifier は Block/Element への `--{modifier}` 付与、State は `is-*` / `data-*`。
- ブレークポイントは `640px` 単一（モバイルファースト）。PC 専用の分岐は `pc:p{id}-hidden` / `pc:p{id}-inline` のようなユーティリティ、または `@media (width >= 640px)` を使う。
- SP/PC で画像を出し分ける場合は CSS の背景切り替えではなく `<picture>` タグを使う（詳細は @coding-workflow.md）。
- HTML の本文中の特殊文字は生の Unicode ではなく実体参照を使う（例: `®` → `&#174;`）。後工程での文字化けを避けるため。

## SP のフルードスケーリング（デザインカンプ準拠）

`/coding` で商品概要をコーディングする際のデザインカンプは、実装サイズの2倍解像度（PC 1280px / SP 750px）で提出される（詳細は @coding-workflow.md）。SP のサイズ指定は固定 px ではなく、**750px カンプ上の測定値をそのまま使った `vw` 比例値**で実装する。

```css
/* デザインカンプ（750px）上で 28px の場合 */
font-size: calc(28 / 750 * 100vw);
```

- SP カンプは実装の基準幅 375px の**ちょうど2倍**（750px）で提出されるため、カンプ上の測定値を先に2で割ってから 375 で割る、といった変換は不要。`calc(カンプ上の測定値 / 750 * 100vw)` にそのまま入れればよい。ビューポート幅 375px のときにちょうど実寸（カンプの半分）の px 値になり、320px〜640px の間はウィンドウ幅に比例して滑らかに拡大縮小する。
- 対象は font-size に限らない。margin / padding / width / height / `position: absolute` の座標など、「デザインカンプの比率のまま伸縮すべき」数値すべてに適用する。
- `@media (width >= 640px)` 側では `vw` のフルード値をそのまま使わず、1280px の PC デザインカンプ上の測定値を**2で割った実測 px 値**で**必ず上書きする**。SP でフルード化したプロパティを PC 側で上書きし忘れると、640px 以降も伸び続けて崩れる。
- 逆に PC 側（`@media (width >= 640px)` 内）は `vw` を使わず、カンプ測定値を2で割った実測 px の固定値でピクセルパーフェクトに実装する。SP・PC いずれも「カンプの生の測定値をそのまま px として使わない」ことが重要（SP は `vw` 変換前提の分母が 750、PC は 2 で割った値）。
- `.p{id}` の `max-width: 640px` はあくまで安全上限。実際の PC 表示幅は親要素 `.p{id}-detail-container`（サイト側 CSS 管轄・編集対象外）が 640px に制御する想定のため、この値自体は変更しない。

既存テンプレート（`00000html/`）内のダミー Block の数値は、実在のデザインカンプに基づかない見本のため固定 px のままで問題ない。上記のフルード実装は、`/coding` で実際のデザインカンプを渡された案件のコーディングにのみ適用する。
