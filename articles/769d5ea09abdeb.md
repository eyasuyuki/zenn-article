---
title: "Blueskyで伏せ字投稿をしよう"
emoji: "🎯"
type: "idea"
topics:
  - "webアプリ"
  - "bluesky"
  - "fusetter"
  - "ネタバレ"
  - "伏せ字"
published: true
published_at: "2024-02-08 09:40"
---

ついに招待制ではなくなった[Bluesky](https://bsky.app)。ネタバレの投稿を伏せ字にしたいけどfusetterはないしなあ、と思ったそこのあなた! BlueSpoilerがありますよ。

https://eyasuyuki.github.io/bluespoiler/

![BlueSpoilerの入力画面](https://storage.googleapis.com/zenn-user-upload/4b084d400649-20240208.png)


BlueSpoilerは[と]で括られた部分を伏せ字にして投稿してくれるWebアプリです。ネタバレの原文は画像のALTに入ります。

どうやって開発したかについては別の記事を書きましたのでこちらをどうぞ。

https://zenn.dev/eyasuyuki/articles/825b28b0ec0a4c

# ホーム画面にアイコンを登録してアプリのように起動する

AndroidやiPhoneではホーム画面にアイコンとして登録するとアプリのように起動できます。

# 用意するもの

- ネタバレ文章
- 976.56KB以下の画像ファイル

操作ミスなどで投稿が失われる場合に備えて、ネタバレ文章はあらかじめテキストエディタやメモアプリで編集しておくと良いでしょう。長文の場合はなおさらです。

Blueskyの本文は300文字、画像のALTは1000文字まで入力できます。BlueSpoilerでは300文字を超える投稿は300文字にカットされますが全文は画像のALTに入ります。300文字を超える長文を投稿したい場合も使えます。

画像のALTテキストにネタバレを入力する仕様のため画像ファイルが必要です。画像ファイルが976.56KBを超えると投稿できないので大きなサイズの画像はあらかじめ圧縮しておいてください。

# [と]で伏せ字投稿

テキスト入力では[と]で括った範囲が○記号で伏せ字にされます。

![テキスト編集画面](https://storage.googleapis.com/zenn-user-upload/a12e79f5f4df-20240208.jpg)

「[を入力」「]を入力」ボタンを押すとキャレット位置にそれぞれの括弧記号が挿入されます。

入力されたテキストはその下にリアルタイムでプレビューされます。

## 言語も選べます

テキスト入力の下の右側に言語を選べるリストボックスがあります。実行される環境のデフォルト言語が選ばれています。他の言語にも変えることができます。

![言語選択リスト](https://storage.googleapis.com/zenn-user-upload/eab340c7ea36-20240208.png)

# 画像を選択します

「画像を選択」ボタンを押して画像を選択します。

![画像を選択ボタンを押す](https://storage.googleapis.com/zenn-user-upload/70935d1e347a-20240208.jpg)

![写真ライブラリ画面](https://storage.googleapis.com/zenn-user-upload/4f02f33200d0-20240208.png)

![写真を選択](https://storage.googleapis.com/zenn-user-upload/b27bc01fe50f-20240208.png)

選択した写真はプレビューされます。

![画像プレビュー](https://storage.googleapis.com/zenn-user-upload/231fa6cc00e3-20240208.png)

選択した写真が気に入らない場合は右横の「×」ボタンを押して選択解除することができます。

# ユーザーIDとパスワードを入力してテストログイン

BlueskyのユーザーIDとパスワードを入力して「テストログイン」ボタンを押します。

![テストログイン](https://storage.googleapis.com/zenn-user-upload/a592cbfec48c-20240208.jpg)

テストログインが成功すると「ログインに成功しました」と表示されます。

BlueSpoilerはブラウザ上だけで稼働します。ユーザーIDとパスワードはBlueskyのサーバーにしか送信しません。

# いよいよネタバレ投稿する

テストログインが成功したらいよいよ「Post」ボタンを押して投稿します。

![Postボタン](https://storage.googleapis.com/zenn-user-upload/abeed27083e1-20240208.jpg)

成功すると「Postが成功しました」というメッセージの下に記事のリンクが表示されます。

![投稿確認画面](https://storage.googleapis.com/zenn-user-upload/b524a96a6ed8-20240208.png)


リンクをクリックすると投稿した記事が確認できます。

![記事画面](https://storage.googleapis.com/zenn-user-upload/b7b91a80ee89-20240208.png)

# fusetterとの違い

自前のWebサーバーを持つfusetterとは異なり、BlueSpoilerはブラウザ内だけで完結しています。バックエンドのサーバーはBlueskyだけです。

WebページとしてGitHub Pagesでホストしていますが完全にクライアントサイドだけで稼働しています。Bluesky以外のサードパーティのサーバーにテキストや画像やパスワードなどが保存されるということはありません。

# 作者にコーヒーをおごる

アプリが気に入ったら「Buy me a Coffee☕️ or Beer🍻」というリンクをクリックしてください。

![GitHub Sponsors](https://storage.googleapis.com/zenn-user-upload/5d708642d810-20240208.jpg)


https://github.com/sponsors/eyasuyuki

GitHub Sponsorの作者のプロフィールが表示されます。金額は自由に設定できます。「Monthly」で毎月課金、「One-time」で1度だけスポンサーになれます。

