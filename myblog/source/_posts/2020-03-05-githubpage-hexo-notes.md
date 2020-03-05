---
title: Github Page + Hexo メモ
date: 2020-03-05 00:18:35
category: [Blog, Hexo]
tags:
---

### Hexo Installation
#### Node.js、hexo-cli 環境構築
Windows は素直に msi を落としてインストールか、`choco install nodejs-lts` か。
Mac の場合は `brew` で、`nodebrew` を使ってもいいし。
その場で検索すればすぐ出てくる。

できたら `hexo-cli` を入れる

```shell command-line
$ npm install hexo-cli -g
```

`hexo` だけ叩いて説明が表示されたら OK でしょう。

> Visual Studio Code の Remote-Container を使う場合

Remote-Container の使い方は公式マニュアル [Developing inside a Container](https://code.visualstudio.com/docs/remote/containers) でチェック。
macOS のユーザフォルダに変な .xxx の隠れフォルダがいっぱいできるのいやだというやや潔癖症(?)的な理由で検索して見つけたけど、使ってみたらわりと便利。
（ただし、性能の良いマシンが必要、配置ファイルもちょこちょこ手入れがいるからだるさはある。わいみたいな会社＋家で 10 台近くマシン持っている人にとっては必須かも？）

前まではコンテナを動かしながら Port Forward できたが、最近もう一回やてみたらメニューが消えてしまった(おれだけ?)ので、devcontainer.jsonで指定する必要がある。

```js devcontainer.json
...
"forwardPorts": [4000],
...
```

### Git 初期化
`{username}.github.io` でレポジトリ作ってしばらく待てれば Github Page が見れるようになる。デフォルトで表示されるのは master ブランチの中身。
Hexo の設定ファイルとかは Hexo ブランチで持ち、 `hexo-deployer-git`で master ブランチでやるのが一番自分的には楽かな。Travis CI 組んでいる人もいるでしょうけどまだ良さがわからないのでいまのところはこれで。

hexo ブランチを作るからブログの初期化まで

```shell command-line
$ git branch hexo
$ git switch hexo
$ hexo init myblog #ここの名前はフォルダ名になる
$ cd myblog
$ npm install
$ npm install hexo-deployer-git -s
```

で、 `hexo s`を叩いて、`http://localhost:4000`が開けば OK。

### hexo-deployer-git でデプロイしてみる
ブログフォルダにある `_config.yml` の中のデプロイ方式を設定

```yml _config.yml
…
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repo: https://github.com/{username}/{username}.github.io #使われるレポジトリ
  branch: master
…
```
編集終わったら `hexo d` で試してみる。
エラーが出てなく、しばらく時間おけば https://{username}.github.io にリリースされているはず。