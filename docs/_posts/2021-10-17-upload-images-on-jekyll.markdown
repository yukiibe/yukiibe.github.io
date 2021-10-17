---
layout: post
title:  "Jekyllのブログサイトに画像やスクリーンショットを貼る方法"
date:   2021-10-17 11:00:00 +0900
permalink: /articles/upload-images-and-screenshots-on-jekyll
---
こんにちは、若手エンジニアのYUKIです。

この記事では、<br>
「__Jekyllのブログサイトに画像やスクリーンショットを貼る方法__」を紹介します。

![](/assets/images/pexels-eduardo-dutra-2115217.png)

ブログ記事を書いていると、視覚的に記事のテーマや説明をわかりやすく伝えるために
画像やスクリーンショットを使いたい場面がありますよね。
Jekyllで作成する記事ではマークダウンが使えるので、簡単に画像を貼ることができます。

今回は、Jekyllにおける画像やスクリーンショットの管理の仕方も含めて説明します。

### __目次__
1. [__画像・スクリーンショットの保管場所について__](#storage-of-images-and-screenshots)

2. [__マークダウンで画像を読み込む__](#load-images-on-markdown)

<a id="storage-of-images-and-screenshots"></a>
### 1. __画像・スクリーンショットの保管場所について__

Jekyllプロジェクト直下に`assets`ディレクトリを作成します。画像はこの assets ディレクトリに保存します。

#### __ちなみに__

私の環境ではUbuntuにJekyllサイトを構築しており、記事の編集はVS Codeでしています。<br>
一応、その場合の画像のアップロード方法も紹介しておきます。

方法は簡単で、Windows上に保存された画像をVS Codeへドラッグ＆ドロップするだけです。

![](/assets/screenshots/2021-10-16_225544.png)

<a id="load-images-on-markdown"></a>
### 2.__マークダウンで画像を読み込む__

下記のように保存した画像のパスを記載すれば画像が表示されるようになります。<br>
パスはjekyllプロジェクトからの絶対パスになります。

```
![](/assets/〇〇〇.png)
```

---

今回の内容は以上となります。本記事が解決の手助けになれば幸いです。
