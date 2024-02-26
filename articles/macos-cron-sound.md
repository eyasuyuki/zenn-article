---
title: "macOSで平日の定時にチャイムを鳴らす方法"
emoji: "🏫"
type: "tech"
topics:
- "macOS"
- "cron"
- "afplay"
- "GarageBand"
- "Music"
published: true
published_at: "2024-02-26 13:00"
---

# macOSの"時計"アプリだとアラームが鳴り止まない

みなさん定時に何か音を鳴らしたい場合にどうしてますか?

私はmacOSの"時計"アプリ(Clock.app)を使っていたのですが、止めるまで音が鳴り止まないので家族にたいへん不評でした。

![](/images/macos-cron-sound/Clock_app.png)

月〜金の定時に1回だけ音を再生するアプリを自作しようかとも考えたのですが、macOSはUnix互換OSなので```cron```が使えるじゃん、と気づいたので以下の方法をやってみました。

# チャイム音を自作する

フリー素材を使っても良いのですが気に入った音がないので自分で作ることにしました。

![](/images/macos-cron-sound/GarageBand.png)

完成したら```[共有]-[曲を"ミュージック"に...]```で"ミュージック"アプリ(Music.app)に共有します。

![](/images/macos-cron-sound/Share2Music.png)

アーチスト名やアルバム名も入力しておきます。これは共有後の楽曲ファイルのパス名として使用されます。

![](/images/macos-cron-sound/Music.png)

Music.app側で再生できることを確認したら、曲名の右端の```[…]```をクリックします。

![](/images/macos-cron-sound/Music_about.png)

ポップアップメニューから```[情報を見る]```を選択します。曲の情報ダイアログの```[ファイル]```タブで楽曲ファイルのフルパスを確認しておきます。

![](/images/macos-cron-sound/Music_path.png)


この曲のフルパスは以下であることが確認できました。

```shell
/Users/yasuyuki/Music/iTunes/iTunes\ Music/Music/Yasuyuki\ ENDO/School\ Sounds/Chime.m4a
```

# コマンドラインで音を鳴らす

macOSには```afplay```というプログラムが最初から存在するのでこれで音を鳴らしてみましょう。

```shell
/usr/bin/afplay /Users/yasuyuki/Music/iTunes/iTunes\ Music/Music/Yasuyuki\ ENDO/School\ Sounds/Chime.m4a
```

これを実行すると1回だけ再生して音が止まることが確認できました。これを```cron```で実行すれば良さそうです。

# ```crontab```の確認と設定

まずログインユーザーの現在の```crontab```を確認しておきます。

```shell
crontab -l
```

何も表示されなければ現在の```crontab```は空です。何か表示されたらファイルにリダイレクトして保存しておきましょう。ここではホームディレクトリに```etc```というディレクトリを作って```~/etc/crontab```というファイルに保存することにします。

```shell
crontab -l > ~/etc/crontab
```

crontabのフォーマットは以下の仕様になっています。

```shell
分 時 日 月 曜日 コマンド
```

月曜から金曜の8:45と13:00と17:30にチャイムを鳴らしたい場合の```crontab```は以下の通りです。

```shell
45 8 * * 1,2,3,4,5 /usr/bin/afplay /Users/yasuyuki/Music/iTunes/iTunes\ Music/Music/Yasuyuki\ ENDO/School\ Sounds/Chime.m4a
0 13 * * 1,2,3,4,5 /usr/bin/afplay /Users/yasuyuki/Music/iTunes/iTunes\ Music/Music/Yasuyuki\ ENDO/School\ Sounds/Chime.m4a
30 17 * * 1,2,3,4,5 /usr/bin/afplay /Users/yasuyuki/Music/iTunes/iTunes\ Music/Music/Yasuyuki\ ENDO/School\ Sounds/Chime.m4a
```

ログインユーザーの```crontab```にすでに何か設定がある場合は```~/etc/crontab```ファイルに上記を追加し、何も設定がない場合には上記内容を```~/etc/crontab```ファイルとして保存します。

では```crontab```コマンドでこの設定を有効化しましょう。

```shell
crontab < ~/etc/crontab
```

設定が反映されたか```crontab -l```で確認しておきましょう。これで平日の定時に音を鳴らす設定は完了です。あとはその時間になったらチャイム音が鳴ることを確認してください。



