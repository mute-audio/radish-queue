# radish
![Static Badge](https://img.shields.io/badge/-RaspberryPi-A22846?logo=raspberrypi&logoColor=white)
 ![Static Badge](https://img.shields.io/badge/-Raspi_Audio-red) ![Static Badge](https://img.shields.io/badge/-MPD-brightgreen)
 ![Static Badge](https://img.shields.io/badge/GNU_Bash-4EAA25?logo=gnubash&logoColor=white) ![Static Badge](https://img.shields.io/badge/license-MIT-blue) 

[NHKラジオ らじる★らじる](https://www.nhk.or.jp/radio/) / [radiko](http://radiko.jp/) / [ListenRadio](http://listenradio.jp/) / [渋谷のラジオ](https://shiburadi.com/) で現在配信中の番組を保存するシェルスクリプト[radi.sh](https://github.com/uru2/radish)を改造した再生専用スクリプト[radish-play](https://github.com/jg1uaa/radish-play)を、さらに[MPD](https://github.com/MusicPlayerDaemon/MPD)のQueue登録専用に改造したシェルスクリプトです。


## 必要なもの
- curl
- libxml2 (xmllintのみ使用)
- jq
- MPD & mpc


## 使い方

＊RaspberryPiでの使用を前提にしています。

[muteなどで、RaspberryPiにMPDとmpcをインストール](https://github.com/mute-audio/mute)後、sshでアクセスしてください。

```
$ ssh [user]@[hostname].local
```
まず、xmllintが必要なので、libxml2-utilsをインストールします。

```
$ sudo apt install libxml2-utils
```
つぎに、[radish-queue.sh](https://github.com/mute-audio/radish-queue/blob/master/radish-queue.sh)をダウンロードし、
権限を変更して実行可能にします。

```
$ wget https://raw.githubusercontent.com/mute-audio/radish-queue/master/radish-queue.sh
$ sudo chmod 755 radish-queue.sh
```

実行形式は以下の通り；
```
$ ./radish-queue.sh [options]
```

| 引数 | 必須 |説明 |備考 |
|:-|:-:|:-|:-|
|-t _SITE TYPE_|○|対象サービス|nhk: NHK らじる★らじる<br>radiko: radiko<br>lisradi: ListenRadio<br>shiburadi: 渋谷のラジオ
|-s _STATION ID_|△|放送局ID|`-l` オプションで表示されるID<br>渋谷のラジオは指定不要|
|-i _MAIL_||ラジコプレミアム ログインメールアドレス|環境変数 `RADIKO_MAIL` でも指定可能|
|-p _PASSWORD_||ラジコプレミアム ログインパスワード|環境変数 `RADIKO_PASSWORD` でも指定可能|
|-l||放送局ID/名称表示|結果は300行以上になります、また取得は(割と)重いです|

### 実行例
```
NHK らじる★らじる
$ ./radi.sh -t nhk -s tokyo-fm
```

```
radikoエリア内の局
$ ./radi.sh -t radiko -s INT
```

```
radikoエリア外の局 (ラジコプレミアム)
$ ./radi.sh -t radiko -s FMT -i "foo@example.com" -p "password"
```

```
radikoエリア外の局 (ラジコプレミアム 環境変数からログイン情報設定)
$ export RADIKO_MAIL="foo@example.com"
$ export RADIKO_PASSWORD="password"
$ ./radi.sh -t radiko -s FMJ
```

```
ListenRadio
$ ./radi.sh -t lisradi -s 30058
```

```
渋谷のラジオ
$ ./radi.sh -t shiburadi
```
このスクリプトはキューを登録するだけで再生はしません。実行後にMPDクライアントやMPDリモートアプリからキューに登録されていることを確認し再生してください。
また、Radikoサービスの場合、radish-queue実行後一定時間経過するとURLが無効になります。

Enjoy!

©2024 kitamura_design
