---
layout: post
title:  "Github Pages + Jekyll でブログサイトを作る方法"
date:   2021-09-16 11:00:00 +0900
permalink: /articles/how-to-build-github-pages-blog-site-with-jekyll
---
こんにちは、若手エンジニアのYUKIです。

この記事では、<br>
「__JekyllでGithub Pages上にブログサイトを構築する方法__」を紹介します。

![](/assets/images/pexels-negative-space-92904.png)

実際にこのブログサイトはその方法で構築しています。ぜひ参考にしてみてください。<br>
今回使用した環境・技術は以下の通りです。
1. Windows Subsystem for Linux 2 (WSL2) / Ubuntu
2. Github
3. rbenv / ruby / gem / bundle
4. Jekyll

ちなみにWSL2の環境でなくても構築は可能です。<br>
その場合、Git bashなど他のツールを利用する必要があります。<br>
Githubの公式ドキュメントが用意している[__こちらの構築手順__][jekyll-by-git-bash-github]はGit bashを前提にして書かれています。

---

### __目次__
1. [__Github Pagesとは__](#about-github-pages)

2. [__Jekyll（ジキル）とは__](#about-jekyll)

3. [__Github上でリポジトリを作成する__](#create-repository-on-github)

4. [__Jekyllをインストールする__](#install-jekyll)

5. [__Jekyllを使ってサイトを構築する__](#build-site-by-jekyll)

6. [__最後に__](#last-words)

---

<a id="about-github-pages"></a>
### 1. __Github Pagesとは__

Github Pagesは、Githubが提供している静的サイトのホスティングサービスです。<br>
Githubのアカウントがあれば、無料かつ簡単にサイトを立ち上げることができるので、<br>
サーバーやドメインなどを用意するコストはかかりません。（任意で独自のドメインを設定することも可能です）

---

<a id="about-jekyll"></a>
### 2. __Jekyll（ジキル）とは__

Jekyll（ジキル）は、静的サイトジェネレーターと呼ばれるサイト構築ツールです。<br>
Jekyllは特にブログサイトを構築・運用するのに適しています。言語はrubyです。

普通ブログサイトを公開しようと思ったら、HTMLやCSSファイルなどを自前で用意しなければなりません。
しかし、Jekyllツールを使用することで、ブログサイトを運用するために必要な機能やテーマを簡単に作成できます。
これによって、ユーザーはHTMLやCSSなどを気にすることなく、ブログ記事の作成に注力することができます。

---

<a id="create-repository-on-github"></a>
### 3. __Github上でリポジトリを作成する__

まずは、ブログサイトを公開できるようにGithub（Web）上でリポジトリを作成します。<br>
作成したリポジトリに、この後Jekyllで生成するサイト用のソースをアップすることでサイトを構築できるようになります。

リポジトリの作成方法については、こちらの[__Githubの公式ドキュメント__][create-github-repository-for-site]をご参照ください。<br>

ポイントとしては

* リポジトリ名は`<user>.github.io` （組織で運用する場合は`<organization>.github.io`）
* リポジトリの可視性は`public`

です。

---

<a id="install-jekyll"></a>
### 4. __Jekyllをインストールする__

こちらの[__Jekyllの公式ドキュメント__][jekyll-install]には事前にrubyをインストールする方法が記載されていますが、
`apt`を利用したインストール方法はrubyのバージョンや権限周りでうまくいかないようなので、
今回は`rbenv`を利用してrubyをインストールします。

以降の作業はUbuntuのターミナルで行います。（Git bashを使用する場合は、Git bashで行います）

#### __rubyのインストール__

まず、`apt`によってすでにrubyがインストールされている場合はアンインストールを行います

```
cd ~
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

---

<a id="build-site-by-jekyll"></a>
### 5. __Jekyllを使ってサイトを構築する__

Githubリポジトリの作成、Jekyllのインストールが完了したら、<br>
次はローカルリポジトリを作成して、そこにJekyllでサイトを構築し、Github上にアップロードします。
基本的には、Githubが用意している[__こちらの構築手順__][build-github-pages-site-with-jekyll]に沿って解説します。

#### __ローカルリポジトリの作成__

※`<user>`の部分は書き換えてください。
```
mkdir <user>.github.io
cd ~/<user>.github.io
git init
```

リポジトリの下にディレクトリを挟みたい場合は
```
mkdir <user>.github.io/docs
cd ~/<user>.github.io/docs
```
※リポジトリの下にディレクトリを作成して、その下にJekyllを構築したい場合は、
今回作成したGithubリポジトリのSettings → Pages の `source` で公開するソースの場所を調整する必要があります。

![](/assets/screenshots/2021-09-16_101702.png)

私の環境でも、`<user>.github.io/docs`の下にJekyllを構築しています。
（リポジトリ直下にJekyllを構築しても特に問題はないと思います）
設定方法については[__こちらの説明__][github-pages-built-source]をご参照ください。

#### __Jekyllでサイトを構築__

サイトを作成したいディレクトリに移動して下記を実行
```
jekyll new --skip-bundle .
```
※bundleは後でインストールします

実行すると、以下のようなファイル群が自動で生成されます
```
ubuntu@Y-IBE:~/yukiibe.github.io/docs$ ls -al
total 36
drwxr-xr-x 3 ubuntu ubuntu 4096 Sep 13 22:43 .
drwxr-xr-x 4 ubuntu ubuntu 4096 Sep 13 17:52 ..
-rw-r--r-- 1 ubuntu ubuntu   56 Sep 13 22:43 .gitignore
-rw-r--r-- 1 ubuntu ubuntu  419 Sep 13 22:43 404.html
-rw-r--r-- 1 ubuntu ubuntu 1125 Sep 13 22:43 Gemfile
-rw-r--r-- 1 ubuntu ubuntu 2080 Sep 13 22:43 _config.yml
drwxr-xr-x 2 ubuntu ubuntu 4096 Sep 13 22:43 _posts
-rw-r--r-- 1 ubuntu ubuntu  539 Sep 13 22:43 about.markdown
-rw-r--r-- 1 ubuntu ubuntu  175 Sep 13 22:43 index.markdown
```

Gemファイルの編集
```
vi Gemfile
```
下記をコメントアウト
```
#gem "jekyll", "~> 4.2.0"
```
「# gem "github-pages"」で始まる行を、[__依存バージョン一覧__][github-pages-dependency-versions]の、<br>
"github-pages"のバージョンを確認して、以下の GITHUB-PAGES-VERSION を書き換えて保存。
```
gem "github-pages", "~> GITHUB-PAGES-VERSION", group: :jekyll_plugins
↓
gem "github-pages", "~> 219", group: :jekyll_plugins
```

bundleのインストール
```
bundle install
```

#### __Github上にアップロード__

コミットしてリモートリポジトリへプッシュ
```
git add .
git commit -m 'Initial GitHub pages site with Jekyll'
```
```
git remote add origin https://github.com/<user>/<user>.github.io.git
git push -u origin master
```

#### __ブログサイトの動作確認__

ブログサイトTopのURLは下記になります。確認してみましょう。
```
https://<user>.github.io
```
下記のようなページが表示されればOKです。
（反映直後は表示されない可能性があるので、その場合は少し時間を置いてから再アクセスします）

![](/assets/screenshots/2021-09-16_103416.png)

---

<a id="last-words"></a>
### 6. __最後に__

#### __テーマやブログ記事について__

Jekyllには他にも複数のテーマが用意されています。今回はデフォルトの`Minima`というテーマを使用しましたが、
[__Githubがサポートしているテーマ__][github-supported-themes]の他、それ以外のテーマも工夫をすれば適用できるようです。
[__詳しくはこちら__][about-jekyll-themes]をご参照ください。

Jekyllの最大のメリットは、HTMLやCSSなど細かいことを気にせずに記事の作成に集中できることだと思っています。
実際に、記事の作成は基本的にすべて`markdown`で記述しています。（この記事もmarkdownのみで再現しています）<br>
そのあたりの記事の作成や運用については、また別の機会に解説できたらなと思います。

今回の内容は以上となります。
本記事が構築の手助けになれば幸いです。

[jekyll-by-git-bash-github]: https://docs.github.com/ja/pages/setting-up-a-github-pages-site-with-jekyll/creating-a-github-pages-site-with-jekyll
[create-github-repository-for-site]: https://docs.github.com/ja/pages/getting-started-with-github-pages/creating-a-github-pages-site#creating-a-repository-for-your-site
[jekyll-install]: https://jekyllrb.com/docs/installation/windows/
[jekyll-docs]: https://jekyllrb.com/docs/home
[build-github-pages-site-with-jekyll]: https://docs.github.com/ja/pages/setting-up-a-github-pages-site-with-jekyll/creating-a-github-pages-site-with-jekyll#creating-your-site
[github-pages-built-source]: https://docs.github.com/ja/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site#choosing-a-publishing-source
[github-pages-dependency-versions]: https://pages.github.com/versions/
[github-supported-themes]: https://pages.github.com/themes/
[about-jekyll-themes]: https://jekyllrb.com/docs/themes/
