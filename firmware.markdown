[< 記事一覧](Readme.markdown)

# ファームウェアの書き込み

マイコンの動作が怪しいとき、半田付け時のミスなのか、それとも初期不良だったのか切り分けができるよう、私は半田作業に入る前に先にファームウェアの書き込みを試しておくことをおすすめしています。

ファームウェアの書き込み・カスタマイズには原則、コマンド操作が必要になります。

キーボードの話からそれてしまうので、ここではコマンド操作に経験があることを前提として説明します。コマンド操作自体が初めてだよという方は、まずはお使いの OS に合わせた入門記事などを読んでみてください。

## 注意

最新の QMK Firmware では `qmk` コマンドという独自コマンドでセットアップするのが標準になっているようです。

が、手元でまだ検証できていないので、ここでは私が使っている少し古いセットアップ方法を紹介します。

チャレンジしてみたい方は最新のセットアップ方法が [ここ](https://docs.qmk.fm/#/ja/newbs_getting_started) にあるので、試してみてください。

## QMK Firmware をダウンロード

Git を使って GitHub から QMK Firmware をダウンロードします。

公式のリポジトリは [ここ](https://github.com/qmk/qmk_firmware/) ですが、私のキーボード用のファームウェアは [こちら](https://github.com/zk-phi/qmk_firmware) の Fork にあります。

```terminal
$ git clone https://github.com/zk-phi/qmk_firmware -o qmk_firmware_zk-phi
```

すでに公式のリポジトリを clone していて、わざわざ２つも clone したくない場合は、 git remote コマンドでリモートリポジトリだけ追加する手もあります (わかる人向け)。

```terminal
$ cd /path/to/qmk_firmware
$ git remote add zk-phi https://github.com/zk-phi/qmk_firmware
$ git fetch zk-phi
$ git checkout zk-phi/master
```

## QMK Firmware の初期設定

必要なツールを自動でインストールするスクリプトがあるのでそれを実行します。

Windows の場合は MSYS2 上で、 Mac の場合は Homebrew をインストールしてから実行します。

```terminal
$ sh util/qmk_install.sh
```

## ファームウェアのビルドと書き込み

`make` コマンドでファームウェアのビルドと書き込みができます。

ProMicro を USB ケーブルで接続し、下記コマンドを実行します。

```terminal
$ make <キーボード名>:<キーマップ名>:avrdude
```

末尾の `:avrdude` はビルドだけでなく書き込みまで行うことを示します。キーボード名やキーマップ名は、各キーボードの組立ガイドを参照してください。

ビルドに成功すると、 ProMicro を再起動しろというメッセージが表示されます。

```terminal
reset your controller now....
```

ProMicro の `GND` ピンと `RST` を、電気を通すピンセットなどでショートすることで再起動します。素早く２回行わないと再起動されない個体もあるようです。

このメッセージまでたどり着かない場合、 QMK Firmware 自体の初期設定に失敗している可能性が高いです。 [公式の FAQ](https://docs.qmk.fm/#/ja/faq_build) などを参照してみてください。

## トラブルシューティング

ProMicro がコンピュータに認識されない場合は、接続の問題の可能性が高いです。ケーブルが充電専用ケーブルでないかなど、接続を確認します。 USB ハブが原因になることもあるようです。

どうにもならない場合は、初期不良の可能性もなくはないので販売店さんに相談してみてください。
