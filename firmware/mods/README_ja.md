# サンプルMOD

ｽﾀｯｸﾁｬﾝのユーザアプリケーション（MOD）のサンプル集です。
MODの書き込み方法は[プログラムのビルドと書き込み](../docs/flashing-firmware_ja.md)を参照ください。

一部のMODは動かすためにネットワーク接続や外部のサーバなどを準備を準備する必要があります。

## Look Around: きょろきょろｽﾀｯｸﾁｬﾝ  
![きょろきょろｽﾀｯｸﾁｬﾝ](../docs/images/stackchan.gif)  
- 環境設定時のAボタンに入っている動作と同じです。  
- modのインストール方法  
    - ホストのビルド時に、Wi-Fiの設定は不要です。  
        - $ npm run build --target=esp32/m5stack_cores3  
    - ホストのプログラムを書き込みます。  
        -  $ npm run deploy --target=esp32/m5stack_cores3  
    - modの書き込み  
        - $ npm run mod --target=esp32/m5stack_cores3 ./mods/look_around/manifest.json  
- ｽﾀｯｸﾁｬﾝの顔が出たらAボタンを押すと動作します。  
- [look_around](./look_around/)  

## Monologue: ぽしょぽしょ独り言ｽﾀｯｸﾁｬﾝ  
- TTS(合成音声)を使用して音声を再生します。TTSの使用については、[こちら](../docs/text-to-speech_ja.md)を参照ください。
- ここでの動作確認は、VoiceVoxを使った事前生成を使った方法のインストール方法を紹介します。
- TTSエンジンVoiceVoxをクローンします。
    - $ git clone https://github.com/VOICEVOX/voicevox_engine.git
- dockerを使って起動します。
    - $ docker pull voicevox/voicevox_engine:cpu-ubuntu20.04-latest
    - $ docker run --rm -p 50021:50021 voicevox/voicevox_engine:cpu-ubuntu20.04-latest
- TTSの環境を設定します。
    - stack_chan/firmware/stack_chan/manifest_local.jsonにあるttsのhostのアドレスをdockerを起動したPCのIPアドレスに修正します。
- JavaScpiptファイルに発話する文章を書き込みます。ランダムで発話します。
    - JavaScrptファイル : stack_chan/firmware/mods/monologue/speeches_monologue.js  
- ホストのビルド時に、Wi-Fiの設定が必要になります。Wi-Fiは2.4Gに接続してください。
    - $ npm run build --target=esp32/m5stack_cores3 ssid="SSIDの名前" password="SSIDのパスワード"
- ホストのプログラムを書き込みます。  
    -  $ npm run deploy --target=esp32/m5stack_cores3  
- modの書き込み  
    - $ npm run mod --target=esp32/m5stack_cores3 ./mods/monologue/manifest.json 
- ｽﾀｯｸﾁｬﾝの顔が出たらAボタンを押すと動作します。      
- [monologue](./monologue/)

## Cheerup: ｽﾀｯｸﾁｬﾝ応援団

![顔の同期](../docs/images/face-sync.gif)
![ｽﾀｯｸﾁｬﾝ応援団](../docs/images/cheerup.gif)

- [cheerup_ble_lite](./cheerup_ble_lite/): BLE版
- [cheerup_ws](./cheerup_ws/): WebSocket版

## Mimic: まねっこｽﾀｯｸﾁｬﾝ
- ｽﾀｯｸﾁｬﾝが2台必要になります。

![まねっこｽﾀｯｸﾁｬﾝ](../docs/images/mimic.gif)

- [mimic_main](./mimic_main/): ユーザが動かすほう
- [mimic_follow](./mimic_follow/): まねして動くほう

## Face Tracker: 顔を追いかけるｽﾀｯｸﾁｬﾝ
- ｽﾀｯｸﾁｬﾝと[M5Stack UnitV2](https://docs.m5stack.com/en/unit/unitv2)が必要になります。
- UnitV2の設定
    - UnitV2のドライバをインストールします。Ubuntuはインストール不要です。
        - https://docs.m5stack.com/ja/guide/ai_camera/unitv2/base_functions
    - Wi-Fiの設定をします。
        - USBでPCとUnitV2を接続し、ターミナルからUnitV2にログインします。
            - $ ssh m5stack@10.254.239.1
            - パスワードは 12345678 です。
        - 使用するSSIDのパスワードをパスフレーズに変換して/etc/wpa_supplicant.confに書き込みます。
        - SSIDのパスワードをパスフレーズに変換するコマンド
            - sudo wpa_passphrase Wi-FiのSSID　Wi-Fiのパスワード
        - ```
        wpa_supplicant.contをルート権限で開き、先ほど生成したパスフレーズとSSIDを書き込みます。
network={
	ssid=”Wi-FiのSSID”
	psk=生成された暗号文
}
```
    - Chromeを開き、URLにhttp://10.254.239.1を入力します
    - Face Detectorを選択して顔のデータが出力します。
- modのインストール
    - ホストのビルド時に、Wi-Fiの設定が必要になります。Wi-Fiは2.4Gに接続してください。
    - $ npm run build --target=esp32/m5stack_cores3 ssid="SSIDの名前" password="SSIDのパスワード"
- ホストのプログラムを書き込みます。  
    -  $ npm run deploy --target=esp32/m5stack_cores3  
- modの書き込み  
    - $ npm run mod --target=esp32/m5stack_cores3 ./mods/face_tracker/manifest.json 
![顔を追いかけるｽﾀｯｸﾁｬﾝ](../docs/images/face-tracker.gif)

- [face_tracker](./face_tracker/)

## Face: ｽﾀｯｸﾁｬﾝの表情と顔色の変化
- 顔の表情と顔色を順番に変化します。
- modのインストール方法  
    - ホストのビルド時に、Wi-Fiの設定は不要です。  
        - $ npm run build --target=esp32/m5stack_cores3  
    - ホストのプログラムを書き込みます。  
        -  $ npm run deploy --target=esp32/m5stack_cores3  
    - modの書き込み  
        - $ npm run mod --target=esp32/m5stack_cores3 ./mods/face/manifest.json  
