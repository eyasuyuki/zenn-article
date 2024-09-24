---
title: "macOS Sequoia 15.0以降で平日の定時にチャイムを鳴らす方法"
emoji: "🕰"
type: "tech"
topics:
- "macOS"
- "launchctl"
- "plist"
- "afplay"
- "GarageBand"
- "Music"
published: true
published_at: "2024-09-22 00:00"
---

# これまでのあらすじ

[macOSで平日の定時にチャイムを鳴らす方法](https://zenn.dev/eyasuyuki/articles/macos-cron-sound)では以下の手順でチャイム音源を定時に鳴らす設定を行いました。

1. Garagebandでチャイム音を作成
2. ```afplay```でチャイム音を再生
3. ```crontab```で定期実行を登録

macOS Sequoia 15.0以降では```cron```がうまく動かないので手順3を```launchctl```で行います。

手順1と2については上記記事を参照してください。

# plistファイルを作成する

```launchctl```で自動実行するには、まずplistファイルを作成します。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple/DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
  <dict>
    <key>Label</key>
    <string>org.javaopen.chime</string>
    <key>ProgramArguments</key>
    <array>
      <string>/usr/bin/afplay</string>
      <string>/Users/yasuyuki/Music/iTunes/iTunes\ Music/Music/Yasuyuki\ ENDO/School\ Sounds/Chime.m4a</string>
    </array>
    <key>StartCalendarInterval</key>
     <array>
      <dict>
	<key>Weekday</key>
	<integer>12345</integer>
	<key>Hour</key>
	<integer>8</integer>
	<key>Minute</key>
	<integer>45</integer>
      </dict>
      <dict>
	<key>Weekday</key>
	<integer>12345</integer>
	<key>Hour</key>
	<integer>9</integer>
	<key>Minute</key>
	<integer>0</integer>
      </dict>
      <dict>
	<key>Weekday</key>
	<integer>12345</integer>
	<key>Hour</key>
	<integer>12</integer>
	<key>Minute</key>
	<integer>0</integer>
      </dict>
      <dict>
	<key>Weekday</key>
	<integer>12345</integer>
	<key>Hour</key>
	<integer>13</integer>
	<key>Minute</key>
	<integer>0</integer>
      </dict>
      <dict>
	<key>Weekday</key>
	<integer>12345</integer>
	<key>Hour</key>
	<integer>17</integer>
	<key>Minute</key>
	<integer>30</integer>
      </dict>
     </array>
  </dict>
</plist>
```

これで以下の```crontab```と等価になるはずです。

```shell
45 8 * * 1,2,3,4,5 /usr/bin/afplay /Users/yasuyuki/Music/iTunes/iTunes\ Music/Music/Yasuyuki\ ENDO/School\ Sounds/Chime.m4a
0 9,12,13 * * 1,2,3,4,5 /usr/bin/afplay /Users/yasuyuki/Music/iTunes/iTunes\ Music/Music/Yasuyuki\ ENDO/School\ Sounds/Chime.m4a
30 17 * * 1,2,3,4,5 /usr/bin/afplay /Users/yasuyuki/Music/iTunes/iTunes\ Music/Music/Yasuyuki\ ENDO/School\ Sounds/Chime.m4a
```

あと必要なのか不明ですが実行パーミッションを付けておきます。

```shell
chmod 0755 org.javaopen.chime.plist
```

# plistを設置する

plistファイルをユーザーのホームディレクトリの```~/Library/LaunchAgents```にコピーします。

```shell
cp org.javaopen.chime.plist ~/Library/LaunchAgents/.
```

ファイルがコピーされると以下の通知が表示されるはずです。

![](/images/macos-sequoia-launchctl-sound/notification.png)

```[システム設定]-[一般]-[ログイン項目と機能拡張]```で確認してみましょう。

![](/images/macos-sequoia-launchctl-sound/control-panel.png)

今回作成したplistがログイン項目と機能拡張に登録され有効になっています。

あとは定時に実行されるのを確認しましょう。
