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

# サウンドファイルを空白を含まないパスにコピーする(2024.9.25追記)

```launchd```によるサウンド再生ではパス名に空白を含むサウンドファイルでI/Oエラーが発生しました。plistファイルの```ProgramArguments```の値は```execvp```に渡されて実行されるため空白を含むパス名はそのまま書けば良いはずですが、私の環境ではうまく動作しませんでした。

なので空白を含まないパスにサウンドファイルをコピーします。

```shell
cp /Users/yasuyuki/Music/iTunes/iTunes\ Music/Music/Yasuyuki\ ENDO/School\ Sounds/Chime.m4a /Users/yasuyuki/Music/.
```

これで```ProgramArguments```には空白を含まないサウンドファイルのパスが記述できるようになりました。

```shell
$ ls -l /Users/yasuyuki/Music/Chime.m4a 
-rw-r--r--@ 1 yasuyuki  staff  605224  9 25 13:35 /Users/yasuyuki/Music/Chime.m4a
```

# plistファイルを作成する

```launchctl```で自動実行するにはplistファイルを作成します。

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
            <string>/Users/yasuyuki/Music/Chime.m4a</string></array>
        <key>SessionCreate</key>
        <true/>
        <key>StartCalendarInterval</key>
        <array>
            <!-- 月曜から金曜までの8:45 -->
            <dict>
                <key>Weekday
                </key>
                <integer>1</integer>
                <!-- 月曜日 -->
                <key>Hour</key>
                <integer>8</integer>
                <key>Minute</key>
                <integer>45</integer>
            </dict>
            <dict>
                <key>Weekday</key>
                <integer>2</integer>
                <!-- 火曜日 -->
                <key>Hour</key>
                <integer>8</integer>
                <key>Minute</key>
                <integer>45</integer>
            </dict>
            <dict>
                <key>Weekday</key>
                <integer>3</integer>
                <!-- 水曜日 -->
                <key>Hour</key>
                <integer>8</integer>
                <key>Minute</key>
                <integer>45</integer>
            </dict>
            <dict>
                <key>Weekday</key>
                <integer>4</integer>
                <!-- 木曜日 -->
                <key>Hour</key>
                <integer>8</integer>
                <key>Minute</key>
                <integer>45</integer>
            </dict>
            <dict>
                <key>Weekday</key>
                <integer>5</integer>
                <!-- 金曜日 -->
                <key>Hour</key>
                <integer>8</integer>
                <key>Minute</key>
                <integer>45</integer>
            </dict>
            <!-- 同じ要領で、9:00, 12:00, 13:00, 17:00 の設定 -->
            <dict>
                <key>Weekday</key>
                <integer>1</integer>
                <key>Hour</key>
                <integer>9</integer>
                <key>Minute</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Weekday</key>
                <integer>2</integer>
                <key>Hour</key>
                <integer>9</integer>
                <key>Minute</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Weekday</key>
                <integer>3</integer>
                <key>Hour</key>
                <integer>9</integer>
                <key>Minute</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Weekday</key>
                <integer>4</integer>
                <key>Hour</key>
                <integer>9</integer>
                <key>Minute</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Weekday</key>
                <integer>5</integer>
                <key>Hour</key>
                <integer>9</integer>
                <key>Minute</key>
                <integer>0</integer>
            </dict>
            <!-- 同様に 12:00, 13:00, 17:00 も設定します -->
            <dict>
                <key>Weekday</key>
                <integer>1</integer>
                <key>Hour</key>
                <integer>12</integer>
                <key>Minute</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Weekday</key>
                <integer>2</integer>
                <key>Hour</key>
                <integer>12</integer>
                <key>Minute</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Weekday</key>
                <integer>3</integer>
                <key>Hour</key>
                <integer>12</integer>
                <key>Minute</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Weekday</key>
                <integer>4</integer>
                <key>Hour</key>
                <integer>12</integer>
                <key>Minute</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Weekday</key>
                <integer>5</integer>
                <key>Hour</key>
                <integer>12</integer>
                <key>Minute</key>
                <integer>0</integer>
            </dict>
            <!-- 13:00 -->
            <dict>
                <key>Weekday</key>
                <integer>1</integer>
                <key>Hour</key>
                <integer>13</integer>
                <key>Minute</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Weekday</key>
                <integer>2</integer>
                <key>Hour</key>
                <integer>13</integer>
                <key>Minute</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Weekday</key>
                <integer>3</integer>
                <key>Hour</key>
                <integer>13</integer>
                <key>Minute</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Weekday</key>
                <integer>4</integer>
                <key>Hour</key>
                <integer>13</integer>
                <key>Minute</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Weekday</key>
                <integer>5</integer>
                <key>Hour</key>
                <integer>13</integer>
                <key>Minute</key>
                <integer>0</integer>
            </dict>
            <!-- 17:30 -->
            <dict>
                <key>Weekday</key>
                <integer>1</integer>
                <key>Hour</key>
                <integer>17</integer>
                <key>Minute</key>
                <integer>30</integer>
            </dict>
            <dict>
                <key>Weekday</key>
                <integer>2</integer>
                <key>Hour</key>
                <integer>17</integer>
                <key>Minute</key>
                <integer>30</integer>
            </dict>
            <dict>
                <key>Weekday</key>
                <integer>3</integer>
                <key>Hour</key>
                <integer>17</integer>
                <key>Minute</key>
                <integer>30</integer>
            </dict>
            <dict>
                <key>Weekday</key>
                <integer>4</integer>
                <key>Hour</key>
                <integer>17</integer>
                <key>Minute</key>
                <integer>30</integer>
            </dict>
            <dict>
                <key>Weekday</key>
                <integer>5</integer>
                <key>Hour</key>
                <integer>17</integer>
                <key>Minute</key>
                <integer>30</integer>
            </dict>
        </array>
        <key>StandardOutPath</key>
        <string>/Users/yasuyuki/log/chime.log</string>
        <key>StandardErrorPath</key>
        <string>/Users/yasuyuki/log/chime-error.log</string>
    </dict>
</plist>
```

これで以下の```crontab```と等価になるはずです。

```shell
45 8 * * 1,2,3,4,5 /usr/bin/afplay /Users/yasuyuki/Music/iTunes/iTunes\ Music/Music/Yasuyuki\ ENDO/School\ Sounds/Chime.m4a
0 9,12,13 * * 1,2,3,4,5 /usr/bin/afplay /Users/yasuyuki/Music/iTunes/iTunes\ Music/Music/Yasuyuki\ ENDO/School\ Sounds/Chime.m4a
30 17 * * 1,2,3,4,5 /usr/bin/afplay /Users/yasuyuki/Music/iTunes/iTunes\ Music/Music/Yasuyuki\ ENDO/School\ Sounds/Chime.m4a
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

# うまくいかない場合のデバッグ方法

うまくいかない場合のデバッグ方法としては以下を行いました。

1. plistでのログの設定
2. ```launchctl list```での確認
2. launchd.logの確認

## plistでのログの設定

plistファイルでは```StandardOutPath```と```StandardErrorPath```で標準出力と標準エラー出力をファイルに保存できます。

```xml
        <key>StandardOutPath</key>
        <string>/Users/yasuyuki/log/chime.log</string>
        <key>StandardErrorPath</key>
        <string>/Users/yasuyuki/log/chime-error.log</string>
```

例えば標準エラー出力に以下のエラーが出ていたら、ファイルI/Oに問題がありそうということが分かります。

```shell
Error: AudioFileOpen failed (-54)
```

## ```launchctl list```での確認

```launchctl list```で起動項目の一覧を表示するとプログラムの終了ステータスが確認できます。

```shell
launchctl list | grep javaopen
```

例えば以下の表示であれば、```exit(1)```でエラー終了していることが分かります。

```shell
-	1	org.javaopen.chime
```

## launchd.logでの確認

```[システム設定]-[一般]-[ログイン項目と機能拡張]```でバックグラウンド項目をオン/オフしながら、コンソール.appでlaunchd.logを表示するとログから実行状態が確認できます。

![](/images/macos-sequoia-launchctl-sound/launchd-log.png)

検索窓にplistファイルで設定した```Label```を入力してフィルタすると分かりやすくなります。
