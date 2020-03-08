---
title: github-hexo-notes-3
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




## References
- [禾七博客 - jekyll-hexo-hugo 互相遷移時関於永久鏈接的問題(中国語)](https://leay.net/2019/09/23/jekyll-hexo-hugo/)
