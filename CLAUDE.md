# 株式会社エスジー コーポレートサイト（sgyk.jp）

横浜の設計施工・特注家具/照明会社「株式会社エスジー」のサイト。
旧サイト（sgline.jp）の内容を、roots-inc.com を参考にした構成・動きで作り直したもの。

## 絶対に守ること

- **HTML と CSS だけ**で作る。JavaScript・ビルドツール・フレームワークは使わない。
  例外: Cloudflare Web Analytics の計測用スクリプトは、Cloudflare Pages が配信時に各ページへ自動で差し込んでいる。
  サイトのファイルには書かない（自分で `<script>` を足さない）。設定は Cloudflare の Pages → sgyk-jp → Metrics → Web Analytics。
- ドメインは **sgyk.jp**、問い合わせメールは **info@sgyk.jp**。旧 `sgline.jp` / `info@sgline.jp` は書かない。
  公開 URL は **https://www.sgyk.jp/**（www 付き）。`canonical` `og:url` `og:image` は `https://www.sgyk.jp/…` で書く。
- 画像は `images/` フォルダのものだけを使う（外部サイトの画像を直リンクしない）。
- スマホで必ず見やすいこと。変更したら幅 375px でも確認し、横スクロールが出ないこと。
- 会社情報・実績の文言は、旧サイトの内容か、依頼者が写真フォルダの名前で指定した内容が元。勝手に創作・追加しない。

## ファイル構成

```
index.html     トップ（HERO / ABOUT / SOLUTION / WORKS 抜粋6件 / HOUSING WORKS / CUSTOM WORKS / CONTACT / ACCESS）
about.html     会社概要・ご挨拶
works.html     STORE WORKS 全37件の一覧 + HOUSING WORKS（#housing）+ CUSTOM WORKS（#custom）
works/         物件ごとの詳細ページ（works/<slug>.html、店舗37枚 + 住宅4枚 house-o / house-s / house-m / house-t）
contact.html   お問い合わせ
css/style.css  全ページ共通のスタイル（これ1枚。先頭に目次あり）
images/        ロゴ（logo.png / logo-name.png）、HERO（hero-1〜4.jpg）、ABOUT（about-2 / about-3.jpg）、
               SOLUTION（solution-1 / 2.jpg）、ACCESS（office.jpg）、about.html 上部（bashamichi-1〜3.jpg）
images/works/  物件の写真（<slug>-1.jpg 〜 <slug>-5.jpg。住宅は house-o-1.jpg など）
images/custom/ CUSTOM WORKS の写真（施設名の slug.jpg。1施設1枚）
```

- ナビは HOME / ABOUT / STORE WORKS / HOUSING WORKS / CUSTOM WORKS / CONTACT の6項目。
  PC のヘッダーは 1 段目にロゴ、2 段目（右寄せ）にメニュー（`.header` が `flex-direction: column`、`.gnav` が `align-self: flex-end`）。
  ロゴとメニューを同じ段に戻さない（依頼者の指示。窮屈に見えるため）。
  メニュー（`.gnav`）は幅 961px でぎりぎり1行に収まっているので、項目を増やすなら 961px 幅で確認する。

- ヘッダー・メニュー（`.drawer`）・CONTACT・フッターは**全ページに同じ HTML をコピー**している。
  1か所直したら**全ページ（ルートの4枚 + works/ の41枚）**を直す。
- `works/` 内のページはリンクと画像のパスがすべて `../` 始まり（`../css/style.css`、`../images/…`、`../index.html`）。
- ページを増やすときは既存ページを複製し、`<title>` `description` `canonical` `og:url`（`https://www.sgyk.jp/…`）と
  ナビの `aria-current="page"` を付け替える。

## 会社情報（変更時は全ページをそろえる）

- 株式会社エスジー（英字表記 SG Co., Ltd.）
- 〒231-0011 神奈川県横浜市中区太田町4-50 馬車道450ビル5F
- TEL 045-305-4223 ／ FAX 045-305-4224 ／ info@sgyk.jp
- メールは `mailto:` リンク（フォームは置かない。サーバー側の処理が無いため）

## デザインの決まり

- ロゴはマーク `images/logo.png` + 社名の文字画像 `images/logo-name.png`（「SG Inc.」、元画像 社名.jpg の白背景を透明にした PNG）。
  文字はテキストではなく画像で置き、マークの上下中央にそろえる（`.logo` の `align-items: center`）。
  ロゴ画像は「SG Inc.」だが、フッターのコピーライトは「© 2026 SG Co., Ltd.」のまま（依頼者の指示があるまで変えない）。
- 色は `css/style.css` 先頭の `:root` 変数で管理。直接色コードを散らさない。
  - 背景 `--bg #e9e9e9` ／ 文字 `--ink #2b2b2b` ／ 濃色面 `--dark #262626` ／ アクセント `--accent #b3955a`（ロゴのゴールド）
- フォント: 英字 Roboto（細い 100/300 と太い 800 の対比）、日本語 Noto Sans JP、手書き風 Mrs Saint Delafield（Google Fonts）。
- 英字見出しは「細字 + 太字」の組み合わせ（例: `SG <b>WORKS</b>`）。
- ボタンは 2 種類: 枠付き `.btn`（濃色面では `.btn--light`）と下線付き `.link-more`。
- ブレークポイントは 960px（タブレット）と 680px（スマホ）。

## 動き（すべて CSS）

- HERO の写真切り替え: `.hero-slides li` + `@keyframes heroFade`（4枚 × 7秒 = 28秒周期）。
  枚数を変えるときは `animation` の秒数、`animation-delay`、keyframes の % を計算し直す。
  写真だけ差し替えるときは `images/hero-N.jpg` を同じ名前で上書きする（3枚目は nanae_016.jpg。1枚目は `.hero` の背景と og:image も兼ねる）。
- トップ ABOUT 欄の写真は `works/club-nanae-1.jpg`、`about-2.jpg`、`about-3.jpg`、`works/belle-vie-grand-salon-1.jpg`。
- about.html 上部の写真切り替え: `.hero-slides--3` + `@keyframes heroFade3`（馬車道の写真 `bashamichi-1〜3.jpg`、3枚 × 6秒 = 18秒周期で繰り返し）。
- ACCESS 欄の会社写真は `images/office.jpg`（元写真 Image1.jpg、ビルのエントランス）。
- 文字のせり上がり: `.hero-title i` / `.page-title i` + `riseUp`。
- スクロールで現れる: 要素に `.reveal`（下から）/ `.reveal-left`（左から）/ `.reveal-img`（画像が左から開く）を付ける。
  `animation-timeline: view()` を使用。非対応ブラウザでは最初から表示される（消えたままにならない）ので、
  `@supports` の囲みを外さないこと。
- メニュー開閉: `#nav-toggle` チェックボックス + `.nav-btn` ラベル + `.drawer`。
  `<input>` → `<label>` → `<nav class="drawer">` の並び順（兄弟セレクタ）を崩さない。
- HOUSING WORKS のタブ: ラジオボタン `#tab-all` `#tab-rc` `#tab-wood` と `li` の `data-type="rc|wood"`。
- CUSTOM WORKS の写真の拡大: チェックボックス `.lb-toggle` + `.lightbox`（`@keyframes lbFade` `lbZoom`）。
  開いている間は `html:has(.lb-toggle:checked)` で後ろのページのスクロールを止める。
- `prefers-reduced-motion: reduce` では動きを止める設定を維持する。

## 物件の詳細ページ（works/<slug>.html）

- 構成は上から: メイン写真（`.page-hero--work`）→ 物件名・場所・カテゴリ（`.detail-head` `.pills`）→
  Date / Location / Category / Work の表（`.spec`）→ 写真を縦に並べる `.gallery` → 前後の物件 `.pager` → CONTACT → フッター。
- 写真は**1物件 3〜5 枚**。旧サイト（sgline.jp）の同じ物件ページで使われていた写真を、掲載順のまま優先して選ぶ。
  `<slug>-1.jpg` が一覧のサムネイルと詳細ページのメイン写真を兼ねる。
- 例外: M.D.B・lasoeur・BALENCIAGA・on A TABLE は手元に写真が 2 枚しか無いため 2 枚。写真が増えたら 3 枚以上にする。
- `.pager` の PREV / NEXT は works.html の並び順（新しい順）の前後の物件。物件を追加・削除したら前後のページの PREV / NEXT も直す。
- HOUSING WORKS（O邸・S邸・M邸・T邸）の詳細ページは `works/house-<頭文字>.html`。表は Date / Category / Structure、
  PREV / NEXT は住宅どうし、中央のボタンは `../works.html#housing`、ナビの `aria-current` は HOUSING WORKS に付ける。
  一覧（works.html と index.html の `.rows`）の行は `<li data-type><a href><img>…<span class="row-view">VIEW MORE</span></a></li>`。
  住宅を追加したら両方の `.rows` に行を足す。元写真は `house追加` フォルダ（フォルダ名が「名前:年:構造」）。

## CUSTOM WORKS（特注家具・照明）

- index.html と works.html の HOUSING WORKS の後ろ（`<section id="custom">`）に同じ内容を置く（トップだけ MORE リンク付き）。詳細ページは無い。
- Furniture（家具）と Lighting（照明）の 2 グループ（`.custom-group`）。写真 1 枚ごとに
  `<li class="reveal"><figure><label for="cw-<slug>" class="custom-img reveal-img"><img …></label><figcaption><span class="custom-name">施設名</span><span class="custom-loc">pref / city</span></figcaption></figure></li>`。
  家具の写真は縦長なので `.custom-grid--furniture`（4:5）、照明は 4:3 で切り抜く。
- 写真をクリックすると拡大表示（写真だけが小さい状態から徐々に大きくなる。どこをクリックしても閉じる）。
  section#custom の末尾（`.wrap` の外）に、写真ごとに `<input type="checkbox" id="cw-<slug>" class="lb-toggle">` と
  `<label for="cw-<slug>" class="lightbox"><img …></label>` の 2 行を置く。写真を足したら、この 2 行も足す。
  この 2 行を `.reveal` などの動きのある要素の中に入れない（拡大表示が画面いっぱいに広がらなくなる）。
  拡大表示でも同じ画像を使うので、`images/custom/` の写真は元の大きさのまま（長辺 1280px まで）保存している。
- **年は表示しない**（依頼者の指示）。施設名と場所は元写真のファイル名「施設名:市区:都道府県」（Finder では「/」区切り）から取り、
  場所はサイトの他の欄と同じ小文字の `pref / city` に並べ替える（例: `notredame:shimonoseki:Yamaguchi` → notredame ／ yamaguchi / shimonoseki）。
  施設名はファイル名の表記のまま（LAZOR GARDEN は STORE WORKS 側の「LAZOR GARDEN SAPPORO」とそろえていない）。
- 元写真は `Custom Works/Furniture` と `Custom Works/Lighting`。並びはファイル名のアルファベット順。写真を足したら index.html と works.html の両方に足す。

## 実績（WORKS）を追加するとき

1. slug（半角英小文字とハイフン）を決め、写真 3〜5 枚を `images/works/<slug>-1.jpg` … の名前で保存する
   （例: `sips -s format jpeg -s formatOptions 78 -Z 1600 元.jpg --out images/works/xxx-1.jpg`）。
2. `works/` の既存ページを複製して `works/<slug>.html` を作り、`<title>` `description` `canonical` `og:url` `og:image`、
   物件名・表・写真・PREV / NEXT を書き換える。
3. `works.html` の `.works-grid` に `<a class="work reveal" href="works/<slug>.html">` を1つ複製して書き換える。
   並びは新しい順。最新なら先頭、過去の物件ならその年の位置に差し込み、前後のページの PREV / NEXT を付け替える。
   日付は `2025.07` 形式（月が不明なら年だけ）、場所は小文字ローマ字 `pref / city`。先頭3件以外の画像には `loading="lazy"`。
4. トップに出す場合は `index.html` の `.works-grid`（新しい順に6件）も入れ替える。
5. `WORKS追加` フォルダの物件は、フォルダ名「物件名:年:都道府県:市区:カテゴリ:内容」（Finder では「/」区切りに見える）の内容をそのまま使う。
   頭文字だけ大文字にそろえる（例: wedding → Wedding、furniture＆Lighting → Furniture & Lighting）。
   カテゴリは旧サイト由来の物件が「Bridal」、追加分が「Wedding」で混在している（依頼者の指示があるまで統一しない）。
   「燔家八風」は読みが未確認のため slug を仮に `happu` としている。
6. 元写真のフォルダ名と旧サイトの記載が食い違う場合（例: フォルダ名は「札幌」だが旧サイトは okinawa / yomitanson）は旧サイトの記載を使う。

## 画像の決まり

- ファイル名は半角英小文字・数字・ハイフンのみ（日本語名・スペース不可）。
- 目安サイズ: HERO 長辺 2000px、物件写真は長辺 1600px（重いものは 1400px・画質 60）、その他 1000〜1600px。1枚 800KB 以下。
- `alt` は内容を書く。飾りの写真は `alt=""`。画面外の画像には `loading="lazy"`。
- 元写真の置き場所: `~/Downloads/100.事務所ホームページ/`（案件ごとのフォルダ。店舗の追加分は `WORKS追加`、住宅は `house追加`）。
  このフォルダは読むだけ。中にファイルを作ったり書き換えたりしない。
- 6枚以上ある物件は、横長の写真を優先して5枚選ぶ（1枚目は横長にする）。

## 確認方法

`python3 -m http.server 8765` をこのフォルダで実行し `http://localhost:8765/` を開く
（`.claude/launch.json` の `site` でも起動できる）。PC 幅と 375px 幅の両方を見る。

## 公開（GitHub と Cloudflare Pages）

- リポジトリは GitHub の `atsunori7/sgyk-jp`（公開）。`main` に push すると Cloudflare Pages（プロジェクト `sgyk-jp`、ビルド設定なし）が自動で更新する。
- 公開 URL は `https://www.sgyk.jp/`（Cloudflare Pages のカスタムドメイン）。`https://sgyk-jp.pages.dev` でも同じ内容が見られる。
- `sgyk.jp`（www なし）は Cloudflare の転送ルール（Rules → Redirect Rules）で `https://www.sgyk.jp/` の同じパスへ 301 転送している。
- アクセス数は Cloudflare の Web Analytics（Pages → sgyk-jp → Metrics → View Web Analytics）で見る。
  sgyk.jp の Analytics（ドメイン側）はボットも含む概数。
- **`css/style.css` を変えたら、全ページ（45枚）の `<link href="…css/style.css?v=20261117">` の `?v=` を新しい日付にする**
  （同じ日の2回目以降は `?v=20261117-2` など）。Cloudflare がブラウザに CSS・画像を 4 時間キャッシュさせる設定
  （`cache-control: max-age=14400`）のため、変えないと前に見た人のブラウザでは「新しい HTML ＋ 古い CSS」になり、配置が崩れる。
  画像を同じ名前で差し替えた場合も最大 4 時間は古い画像が見えることがある（キャッシュを消して再読み込みすれば新しくなる）。
- `_redirects` は Cloudflare Pages 用の設定ファイル。`CLAUDE.md`・`.claude/`・`.gitignore` をサイトから見えなくしている（トップへ転送）。消さないこと。
- sgyk.jp の DNS は Cloudflare で管理している（ネームサーバー fay / luke.ns.cloudflare.com。ドメインの契約は VALUE-DOMAIN のまま）。
  メール（@sgyk.jp）はバリューサーバー（e6.valueserver.jp / 157.7.184.26）で動いており、Cloudflare は通らない。
  DNS を変えるときはメール用のレコード（MX → mail.sgyk.jp、mail の A、SPF・DKIM・DMARC の TXT）を必ず残し、
  メール関係のレコードは「DNS only」のままにする（Proxied にするとメールが止まる）。
