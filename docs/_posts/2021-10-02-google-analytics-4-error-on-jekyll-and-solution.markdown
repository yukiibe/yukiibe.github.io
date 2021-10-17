---
layout: post
title:  "Google Analytics 4 がJekyllサイトでエラーになる原因と解決方法"
date:   2021-10-02 11:00:00 +0900
permalink: /articles/google-analytics-4-error-on-jekyll-and-solution
---
こんにちは、若手エンジニアのYUKIです。

この記事では、<br>
「__Google Analytics 4 がJekyllサイトでエラーになる原因と解決方法__」について紹介します。

![](/assets/images/pexels-photomix-company-106344.png)

今回は以下のことを前提に説明していきます。
1. Github PagesでJekyllサイトを運用している
2. Jekyllテーマはデフォルトの状態 `theme: minima`
3. Google Analytics 4 を使用している
4. Google Tag Assistantを使用している

---

### __目次__
1. [__JekyllでGoogle Analyticsを設定する従来の方法__](#conventional-method-to-set-google-analytics-with-jekyll)

2. [__正常に動作しない原因__](#reason-why-not-working)

3. [__解決方法__](#solution)

---

<a id="conventional-method-to-set-google-analytics-with-jekyll"></a>
### 1. __JekyllでGoogle Analyticsを設定する従来の方法__

本来、JekyllサイトでGoogle Analyticsの計測IDを設定するのはとても簡単です。<br>
Jekyllフォルダ直下にある`_config.yml`に

```
google_analytics: UA-〇〇〇〇〇〇〇〇
```

というふうに計測IDを設定をしてbuildするだけで、サイトページのGoogle Analyticsが機能するようになります。

しかし、最新のGoogle Analytics 4の計測ID（G-で始まる文字列）をこの方法で設定しても、エラーになってしまいます。

※下記はGoogle Tag Assistantが検出したエラー

![](/assets/screenshots/2021-09-28_181040.png)

---

<a id="reason-why-not-working"></a>
### 2. __正常に動作しない原因__

#### __Google Analytics Javascriptについて__

Google Tag Assistantで以下のようなエラーが検出されました。
```
Invalid or missing web property ID
```
このエラーの詳細を確認したところ、以下のような説明が書かれていました。
```
The Google Analytics web property ID is not properly set within the Google Analytics JavaScript.
The web property ID is used to associate the Google Analytics request to a specific Analytics website profile.
Failing to properly set the Google Analytics web property ID will prevent pageviews from associating to your Analytics website profile.
```

これによると、サイトページの、Google Analyticsを埋め込んで送信するためのJavascriptに、
Google Analyticsの計測IDが正しく設定されていない。ということが書かれています。

下図がそのJavascript部分です。（サイトページの meta タグに記述されています）

![](/assets/screenshots/2021-09-28_220944.png)

#### __Jekyllのデフォルトのテーマが生成するJavascript__

色々と調べたところ、どうやらこのJavascript部分の記述が、Google Analytics 4に対応していないことが原因でした。

Jekyllのデフォルトのテーマ `minima` が自動で生成するJavascriptが、
Google Analytics 4に対応したJavascriptでなければ正常に動作しないみたいです。

そのため、`_config.yml`でテーマを`theme: minima`と指定している限りはこの問題は解決できません。

※下記あたりが情報源です<br>
・[https://github.com/jekyll/minima/issues/561][github-issue01]<br>
・[https://github.com/pages-themes/minimal/issues/88#issuecomment-784116273][github-issue02]

---

<a id="solution"></a>
### 3. __解決方法__

以下は、Jekyllのデフォルトのテーマ`minima`を使用して自動でサイトを構築していた場合の解決方法です。

方法は簡単です。
`_config.yml`にリモートリポジトリで開発が進んでいるテーマを指定するようにします。
（Google Analytics 4に対応できるように開発側の方々がJavascriptを修正してくれています。）

【参考】

・リモートリポジトリに存在する`minima`テーマのソース<br>
　[https://github.com/jekyll/minima][github-jekyll-minima]

・Google Analytics 4に対応したJavascriptの該当ソース<br>
　[https://github.com/jekyll/minima/blob/master/_includes/google-analytics.html][github-jekyll-minima-includes-google-analytics]

#### __手順__

※`_config.yml` の修正

まずプラグインを追加
```
plugins:
  - jekyll-remote-theme
```

次にテーマを指定します
```
remote_theme: jekyll/minima
```

元の設定はコメントアウトか削除します
```
#theme: minima
```

Google Analytics 4の計測IDを指定します
```
google_analytics: G-〇〇〇〇〇〇〇〇
```

最後にbuildすれば、リモートリポジトリの`minima`テーマをもとにサイトが構築されます。<br>
※注意点としては、デフォルトで設定していた`minima`テーマとはデザインや機能が少し異なる場合があります。

サイトページにアクセスして、Google Tag Assistantが正常であれば問題なく動いています。

ちなみに変更後のJavascriptは下記のようになっています。

![](/assets/screenshots/2021-09-28_231746.png)

---

今回の内容は以上となります。本記事が解決の手助けになれば幸いです。


[github-issue01]: https://github.com/jekyll/minima/issues/561
[github-issue02]: https://github.com/pages-themes/minimal/issues/88#issuecomment-784116273
[github-jekyll-minima]: https://github.com/jekyll/minima
[github-jekyll-minima-includes-google-analytics]: https://github.com/jekyll/minima/blob/master/_includes/google-analytics.html
