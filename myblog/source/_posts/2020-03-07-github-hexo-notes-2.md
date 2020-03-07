---
title: Github Page + Hexo でブログのメモ(2)
date: 2020-03-07 00:07:44
category: [Blog, Hexo]
tags:
  - nodejs
  - fontend
  - git
thumbnail: https://cdn.jsdelivr.net/gh/sjulianryo/blogPics/blogImg/thumb-3-theme.jpg
---
![](https://cdn.jsdelivr.net/gh/sjulianryo/blogPics/blogImg/thumb-3-theme.jpg)

ここでこだわりすぎると疲れてあきらめやすいので、
自分みたいな根性のないやつやめんどくさがり屋は要注意！

## Theme & git submodule

そのままデフォルトの Landscape で行くのも素晴らしいと思うが、
やはりいろいろこだわりたくて。

[https://hexo.io/themes/](https://hexo.io/themes/) 公式のテーマページで色々探った結果、
[hexo-theme-human](https://github.com/ppoffice/hexo-theme-hueman) が一番良いと感じてこれにした。
最近結構チェックしている[マナブ](https://manablog.org)さんのスタイルにも似てて簡潔で写真もあって、
多分そのうち飽きるけどとりあえずこれで。
（もちろん本当は自分で作るのが一番だが…）

![](https://camo.githubusercontent.com/0902c896a283714f7a795338dbfeca67092e9b02/687474703a2f2f70706f66666963652e6769746875622e696f2f6865786f2d7468656d652d6875656d616e2f67616c6c6572792f73637265656e73686f742e6a7067)

### Theme import

blog フォルダの中の themes フォルダに、
テーマのレポジトリを入れれば言い話だが、
いろいろバージョンアップしてくれたときの同期とかを考えると、
やはり git submodule で管理したほうが便利かと。
もっと便利なやり方が見つかったらまた変える。

テーマによってはテーマフォルダにある `_config.yml` をいじることなく、
blog フォルダでレンダリングするやり方もあるらしいけど、
今回使う hueman テーマはどうやら `_config.yml` を自分で作るやり方なので、
一回フォークして自分の github に入れていろいろ変更していく管理にした。

[github repository の Fork の説明](https://help.github.com/en/github/getting-started-with-github/fork-a-repo)

フォークできたら実際に自分の blog に入れる

```shell command-line
$ cd myblog
$ git submodule add https://github.com/{your.username}/hexo-theme-hueman.git themes/hueman
$ cd themes/hueman
$ cp _config.yml.example _config.yml
```

hueman テーマは先に themes/hueman フォルダにある 
`_config.yml.example` をコピーする必要がある
もちろん File explorer や Finder でやっても OK。

あとは myblog にある `_config.yml` のテーマ設定を変更

```yml _config_yml
...
# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
# theme: landscape
theme: hueman
...
```

これで一回 `hexo s` で効果をみて、テーマが変わったら完成。

### submodule の管理(upstream 同期)

submodule のコミットとプッシュはメインレポジトリで
[`--recursive` コマンドでいろいろできる](https://juejin.im/post/5c2e22fcf265da615d72c596) みたいけど、
頭わるい(歳もとった)から個別でいろいろやっている

```shell command-line
$ git clone https://github.com/{username}/{username}.github.io.git
$ cd {username}.github.io.git
$ git submodule init
$ git submodule update
$ cd myblog/themes/hueman
$ git checkout master # or 使っている branch
```

あとは themes/hueman を別の Git レポジトリで管理すれば良い
（ブランチ変更とか、コミットプッシュとか）
毎回 checkout しないといけないのはちょっとめんどいけど…

そしてテーマの作者がいろいろアップデートしてくれて取り入れたいときは
どうも `upstream` というのを取り入れてマージする感じらしい

```shell command-line
$ git remote add upstream git://github.com/ppoffice/hexo-theme-hueman.git
$ git branch -a
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/gh-pages
  remotes/origin/hexo-2.x
  remotes/origin/master
  remotes/origin/site
  remotes/upstream/gh-pages
  remotes/upstream/hexo-2.x
  remotes/upstream/master
  remotes/upstream/site
```

たぶんこんな感じになったら `upstream` としてオリジナルのテーマレポジトリをリンクしている
アップデートの内容を取り入れたいときはそれをマージする

```shell command-line
$ git fetch upstream
$ git checkout master
$ git merge upstream/master
```

Conflict しないようにカスタマイズのときはいろいろ気をつけましょう！

### References

- [GitHubでFork/cloneしたリポジトリを本家リポジトリに追従する
](https://qiita.com/xtetsuji/items/555a1ef19ed21ee42873)

一番わかりやすくてとても助かった！