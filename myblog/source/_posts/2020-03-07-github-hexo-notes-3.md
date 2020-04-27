---
title: Github + Hexo でブログのメモ(3)
date: 2020-03-07 18:11:22
category: [Blog, Hexo]
tags:
  - fontend
  - git
  - cdn
  - fonts
thumbnail: https://cdn.jsdelivr.net/gh/sjulianryo/blogPics/blogImg/thumb-4-fonts.jpg
---
![](https://cdn.jsdelivr.net/gh/sjulianryo/blogPics/blogImg/thumb-4-fonts.jpg)

メモ(1)とメモ(2)で基本的にブログを書き始められるが、
ここではフォントとか、引っかかったポイントなどをまとめる

## テーマの細かい設定
### カテゴリの階層

カテゴリをグルーピングしたいときは、
リストで記載すれば自動的に同じキーワードを階層でまとめてくれる。

例：
```markdown post.md
categories:
- [Sports, Baseball]
- [MLB, American League, Boston Red Sox]
- [MLB, American League, New York Yankees]
- Rivalries
```

詳細は公式マニュアル [Font-matter](https://hexo.io/docs/front-matter.html) を参照

### ブログ記事の命名法

ブログを引っ越したいとか、別の Framework を使いたいとき、
SEO、コメントなどに影響したくないので、
できるだけ記事のアドレスはそのままにしたい。

命名法は `/year/month/day/title`、
例として `https://example.com/2020/03/01/post-name` のように、
日付を年月日ごとに分けて、最後に記事の名前。

こうすることでアドレスはかぶりにくいし、見え方もすっきりする。

設定はブログフォルダの `_config.yml` でできる

```yml _config.yml
new_post_name: :year-:month-:day-:title.md
permalink: :year/:month/:day/:title/
```

これで `hexo new post` のときにタイトルだけ入れれば
自動的日付もつけてくれるし、生成されるリンクも年月日ごとに分かれる。

### 日本語フォント追加

最初の設定だと中華フォントになるため、テーマの中身をいじって日本語フォントを追加。
テーマによっては場所といじり方が違うので、ここでは Hueman というテーマを例に。

場所は `themes/hueman/source/css/_variables.styl`
Mac の日本語フォント Hiragino Kaku と Windows の日本語フォント Meiryo を追加
個人的には Meiryo ui が好きなので優先順的には Meiryo UI を先にした。

```css _variables.styl
font-sans = "Titillium Web", "Helvetica Neue", Helvetica, Arial, 'Hiragino Kaku Gothic ProN', 'meiryo ui', meiryo, sans-serif
```

> Google Fonts を使う場合

あとあと考えたら、やっぱ環境によってフォントが違うのがいやで、
Google Fonts を導入することにした。

Google Fonts のページ [https://fonts.google.com/](https://fonts.google.com/) にアクセスして、
好きなフォントと太さを選択し、右側に出てくるサイドバーの「Embed」を選ぶことで、
埋め込む css のコートと、利用する際の font-family 表記を生成してくれる。

ここでテーマの設定ファイルに二箇所をいじることが必要になってくる
- 読み込む用の記載をいれる場所
- 上の `_variables.styl` にそれを使うと記載

まずは `/themes/hueman/layout/common/head.ejs` に Google Fonts を読み込む記載を入れる
デフォルトで入っていた `titillium-web` というフォントの記載があったのでそれを参考に

```js head.ejs
<%- css('https://fonts.googleapis.com/css?family=Noto+Sans+JP:400,700&display=swap" rel="stylesheet') %>
```

次に上と同様に `themes/hueman/source/css/_variables.styl` に font-family の設定を追加
```css _variables.styl
font-sans = 'Noto Sans JP', '游ゴシック', YuGothic, 'ヒラギノ角ゴ Pro W3', 'Hiragino Kaku Gothic Pro', Verdana, 'メイリオ', Meiryo, sans-serif
```

スペースを節約のためにいらないデフォルトで入っていた `source/libs/titillium-web` も削除した


## Google Analytics
Hueman がデフォルトで設定の場所を用意してくれたので、単純に `themes/hueman/_config.yml` に ID を記載して終わり


Blog を書くのに最低限の設定はこんぐらいかなと。
あとはゆくゆく思いついたらまた追記していく。
例えば Build-pipeline を作ってみて、Browser で Github で編集するだけで記事更新できるようにするとか。
いまのところはまず記事をいっぱい書くところだが…


## References
- [禾七博客 - jekyll-hexo-hugo 互相遷移時関於永久鏈接的問題(中国語)](https://leay.net/2019/09/23/jekyll-hexo-hugo/)
