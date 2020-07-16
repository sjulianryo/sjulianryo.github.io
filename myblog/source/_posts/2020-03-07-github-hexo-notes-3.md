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

スペースを節約のためにいらないデフォルトで入っていた `source/libs/titillium-web` 
も削除した

## CDN
あとは使う写真をどこに保存しるかだね。普通に HEXO とレポジトリに入れても良いが、
毎回 clone のとき多分絶対死ぬ。
無料で使いやすくてあまり消えてしまう心配がない良い CDN っぽいものないかな…

あった。
[jsDeliver](https://www.jsdelivr.com/) がとってもすばらしい。
使い方も簡単：
- 写真や js/css など CDN で配布したいものを入れる用の Github レポジトリ作成
- ファイルを master ブランチにアップロード（Web 経由で OK）
- あとはルールにしたがって画像の URL を生成（例えばこの記事のサムネイル）：
https://cdn.jsdelivr.net/gh/sjulianryo/blogPics/blogImg/thumb-4-fonts.jpg

(解説)
https://cdn.jsdelivr.net/gh/（ここまで固定） + {github_username} / {repository_name} / {folder_name}（あれば）/ {file_name}

これで jsdelivr の cdn から github のレポジトリにある画像ファイルを
ブログに出すことができるようになった。
原理はなんとなく想像できるが、
詳しいことは気力があれば調べる…
長く使い続けられると良いね！

## Google Analytics
Hueman がデフォルトで設定の場所を用意してくれたので、
単純に `themes/hueman/_config.yml` に ID を記載して終わり


Blog を書くのに最低限の設定はこんぐらいかなと。
あとはゆくゆく思いついたらまた追記していく。
例えば Build-pipeline を作ってみて、
Browser で Github で編集するだけで記事更新できるようにするとか。
いまのところはまず記事をいっぱい書くところだが…


## References
- [禾七博客 - jekyll-hexo-hugo 互相遷移時関於永久鏈接的問題(中国語)](https://leay.net/2019/09/23/jekyll-hexo-hugo/)
- [ChrAlpha 的幻想郷 - jsDelivr | 免費加速図片等網站静態資源(中国語)](https://blog.ichr.me/post/use-jsdelivr-speed-up-static-files-visits/)