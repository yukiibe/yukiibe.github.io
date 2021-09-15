---
layout: post
title:  "Github Pages + Jekyll でブログサイトを作る方法"
date:   2021-09-14 23:57:00 +0900
categories: blog github jekyll
---
こんにちは、若手エンジニアのしまうまです。

この記事では、<br>
「__JekyllをベースにしてGithub Pages上にブログサイトを構築する方法__」を紹介します。

![](/assets/images/pexels-negative-space-92904.png)

実際にこのブログサイトはその方法で構築しています。ぜひ参考にしてみてください。<br>
今回使用した環境・技術は以下の通りです。
1. Windows Subsystem for Linux 2 (WSL2) / Ubuntu
2. Github
3. rbenv / ruby / gem / bundler
4. Jekyll

ちなみに、WSL2の環境でなくても構築は可能です。その場合、Git bashなど他のツールを利用する必要があります。
[Github][github-docs]の公式が用意している[構築手順][jekyll-by-git-bash-github]はGit bashを前提にして書かれています。

※なお、今回の記事はGitの知識やGithubの利用経験があることを前提に書いています。

---

### __目次__
1. [Github Pagesとは](#about-github-pages)
2. [Jekyll（ジキル）とは](#about-jekyll)
3. [Github上でリポジトリを作成する](#create-repository-on-github)
4. [Jekyllをインストールする](#install-jekyll)
5. [Jekyllを使ってサイトを構築する](#build-site-by-jekyll)

---

<a id="about-github-pages"></a>
### 1. __Github Pagesとは__

<a id="about-jekyll"></a>
### 2. __Jekyll（ジキル）とは__

→静的サイトジェネレータの一種
→Jekyllを使用することでブログサイトっぽいテーマを自動で作ってくれる

<a id="create-repository-on-github"></a>
### 3. __Github上でリポジトリを作成する__

まずは、ブログサイトを公開できるようにGithub（Web）上でリポジトリを作成します。<br>
作成したリポジトリに、この後Jekyllで生成するサイト用のソースをアップすることでサイトが公開できるようになります。

作成方法については、以下のGithub公式ドキュメントをご参照ください。<br>
→[サイト用にリポジトリを作成する][create-github-repository-for-site]

ポイントとしては

* リポジトリ名は`<user>.github.io`
* リポジトリの可視性は`public`

です。

<a id="install-jekyll"></a>
### 4. __Jekyllをインストールする__

[Jekyll][jekyll-docs]の公式に[インストール方法][jekyll-install]が記載されていますが、
`apt`を利用したインストール方法はrubyのバージョンや権限周りでうまくいかないようなので、
今回は`rbenv`を利用してrubyをインストールします。

以降の作業はUbuntuのターミナルで行います。（Git bashを使用する場合は、Git bashで行います）

#### __rubyのインストール__

まず、`apt`によってすでにrubyがインストールされている場合はアンインストールを行います

```
sudo apt purge ruby
※私の環境ではruby2.5でした。
```

rbenvのインストール
```
git clone https://github.com/rbenv/rbenv.git ~/.rbenv
git clone https://github.com/rbenv/ruby-build.git ~/.rbenv/plugins/ruby-build
```

rbenvが利用できるようにパスを通します。`.bashrc`に下記を追記
```
export PATH="$HOME/.rbenv/bin:$PATH"
eval "$(rbenv init -)"
```

`.bashrc`の変更を反映
```
source .bashrc
```

インストール可能なバージョンを確認
```
rbenv install -l
```

rubyのインストール
```
rbenv install 2.7.4
```
※依存パッケージ系でエラーが出た場合は、エラー内容にしたがって依存パッケージをインストールします。

rbenvがrubyのどのバージョンを使用するかを指定（これをやらないとrubyコマンドが通りません）
```
rbenv global 2.7.4
```

rubyのインストール確認
```
ruby -v
```

#### __Jekyllのインストール__

gemがインストールされているか確認（rubyと一緒にインストールされています）
```
gem -v
```

gemの最新化
```
gem update
```

Jekyllのインストール
```
gem install jekyll builder
```

Jekyllのインストール確認
```
jekyll -v
```

<a id="build-site-by-jekyll"></a>
### 5. __Jekyllを使ってサイトを構築する__



[github-docs]: https://docs.github.com/ja
[jekyll-by-git-bash-github]: https://docs.github.com/ja/pages/setting-up-a-github-pages-site-with-jekyll/creating-a-github-pages-site-with-jekyll
[create-github-repository-for-site]: https://docs.github.com/ja/pages/getting-started-with-github-pages/creating-a-github-pages-site#creating-a-repository-for-your-site
[jekyll-install]: https://jekyllrb.com/docs/installation/windows/
[jekyll-docs]: https://jekyllrb.com/docs/home
