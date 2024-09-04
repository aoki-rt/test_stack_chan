# 環境構築（MacOS/Linux）マニュアル

[English](./getting-started.md)

ｽﾀｯｸﾁｬﾝはWindows11、MacOS、Linuxで開発ができます。Windows 11の場合はWSL2を使った環境構築手順を参照してください。ここでは、MacOS/Linuxでの開発環境の方法を示します。

* **[Windows 11 のｽﾀｯｸﾁｬﾝ環境構築マニュアル（WSL2）](./getting-started-wsl2_ja.md)**

## 開発に必要なもの

* ホストPC
    * Linux(Ubuntu22.04 or Ubuntu24.04)でテスト済み
    * MacOS(Sonoma 14 Appleシリコン)でテスト済み
* [ｽﾀｯｸﾁｬﾝ アールティver.](https://rt-net.jp/products/rt-stackchan/) または その互換品
* USB type-Cケーブル
* 事前にインストールしておくアプリ
  * [git](https://git-scm.com/)
  * [Node.js](https://nodejs.org/en/)
    * v22.7.0でテスト済み
  * Python3.10(macOSの場合は、brewでインストールするのではなく[https://www.python.org](https://www.python.org)からダウンロードしインストールしてください。)
  * xcode-select(macOS)  

## ｽﾀｯｸﾁｬﾝリポジトリのクローンとnodeのmoduleのインストール

```console
$ git clone https://github.com/rt-net/stack-chan.git
$ cd stack-chan/firmware
$ npm install
```

## ModdableSDKのセットアップ

ホストPCで[ModdableSDK](https://github.com/Moddable-OpenSource/moddable)と
[ESP-IDF](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/get-started/index.html)をインストールします。
次の2通りの方法があります。

- xs-dev（CLI）を使う（推奨）
- 手動でセットアップする

### xs-dev（CLI）を使う（推奨）

ｽﾀｯｸﾁｬﾝはセットアップ手順をnpmスクリプト化しています。
`stack-chan/firmware`ディレクトリで次のコマンドを実行します。


```console
$ npm run setup
$ npm run setup -- --device=esp32
```
macOSの場合は、npm run setup -- --device=esp32のインストールの時、xcode-selectのバージョンが古くて"Error: Command failed with exit code 1: python3 -m pip install pyserial"で止まることがあります。その場合は、xcode-selectを手動で削除(sudo rm -rf /Library/Developer/CommandLineTools)したあとインストール(xcord-select –install)してください。  
内部で[`xs-dev`](https://github.com/HipsterBrown/xs-dev)を使ってModdableSDKやESP-IDFのセットアップを自動化しています。  
macOSのの場合、自動で.zshrcにsource /User/username/.local/share/xs-dev-export.shが追加されません。手動で追加をする必要があります。

### 手動でセットアップする

[公式サイトの手順（英語）](https://github.com/Moddable-OpenSource/moddable/blob/public/documentation/Moddable%20SDK%20-%20Getting%20Started.md)に従ってModdableSDKとESP-IDFをインストールします。
xs-dev（CLI）でうまくセットアップできない場合はこちらを行ってください。

**ｽﾀｯｸﾁｬﾝ アールティver.では、Moddable SDK 4.9.5、ESP-IDF 5.3.0 での動作を想定しています。**
**intel macはModdable SDK 4.7.0 + ESP-IDF 5.1.0 python3.9.0で動作することは確認しています。intel macで使用するにはfirmware/package.jsonの"setup": "xs-dev setup --target-branch 4.9.5"を"setup": "xs-dev setup --target-branch 4.7.0"にすることでインストールできますがサポート対象外になります。**


## 環境のテスト

`npm run doctor`コマンドで環境のテストができます。
インストールに成功していれば次のようにModdable SDKのバージョンとして4.9.5が表示され、Supported target devicesにesp32が表示されます。

```console
$ npm run doctor

> stack-chan@0.2.1 doctor
> echo stack-chan environment info: && git rev-parse HEAD && git rev-parse --show-toplevel && xs-dev doctor

stack-chan environment info:
55d005ac9f0764a4ebc561b7d0a2a29a66ee5199
/home/kurasawa/Projects/stack-chan
xs-dev environment info:
  CLI Version                0.28.1
  OS                         Linux
  Arch                       x64
  Shell                      /bin/bash
  NodeJS Version             v20.11.0 (/home/ubuntu/.volta/tools/image/node/20.11.0/bin/node)
  Python Version             3.10.12 (/home/ubuntu/.rye/shims/python)
  Moddable SDK Version       4.9.5 (/home/ubuntu/.local/share/moddable)
  Supported target devices   lin, esp32
  ESP32 IDF Directory        /home/ubuntu/.local/share/esp32/esp-idf
```

## 次のステップ

- [プログラムのビルドと書き込み](./flashing-firmware_ja.md)
