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
- PCとｽﾀｯｸﾁｬﾝを一対一でBLEで接続します。
- PC側の環境設定
	- windowsの場合nodeを18.12以上にする必要があります。
		- volta install node@18
	- safariは対応していないためgoogle-chromeをインストールする必要があります。
	- https://github.com/meganetaaan/suburi-mediapipeをクローンします。
		- git clone https://github.com/meganetaaan/suburi-mediapipe
		- cd suburi-mediapipe/frontend
	- 必要なツールをインストールします。
		- sudo apt install -y curl
		- curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
		- echo "deb https://dl.yarnpkg.com/debian stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
		- sudo apt update && sudo apt install yarn(macOSはbrew install yarn,Windowsはvolta install yarnとyarn install)
		- npm install react react-dom
		- npm install react-scripts
	- google-chromeをインストール(windowsのみ)
		-  sudo apt update
		- sudo sh -c 'echo "deb http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google-chrome.list'
		- sudo wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | sudo apt-key add -
		- sudo apt install google-chrome-stable
	- opensslのバージョンとnodeのバージョンの互換性のため以下のコマンドが必要になります。
		- ターミナルを開くたびに以下のコマンドが必要になるため、毎回打ち込むかbashrcに追加する必要があります。
			- export NODE_OPTIONS=--openssl-legacy-provider
		- node_moduleのcacheに書き込み権限をついかします。
			- sudo chmod 777 /home/ubuntu/suburi-mediapipe/frontend/node_modules/.cache

	- ツールを機動
		- yarn start
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
        - wpa_supplicant.contをルート権限で開き、先ほど生成したパスフレーズとSSIDを書き込みます。
network={
	ssid=”Wi-FiのSSID”
	psk=生成された暗号文
}

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

## おしゃべりスタックチャン
- CharGPTを使ってスタックちゃんと会話をします。
- macOSは対象外になります。
- 使用するsimple-stt-serverが要求しているnodeのversionが16から20になっています
- Windowsは以下のインストールが必要になります。
	- Windows側
		- Pulse Audio On Windowsをダウンロードしインストールします。
			- https://www.freedesktop.org/wiki/Software/PulseAudio/Ports/Windows/Support/からzipfile containing preview binariesをクリックしてダウンロードします。
			- C:\Users\ユーザー名\pulseaudio-1.1に展開します。
			- C:\Users\ユーザー名\pulseaudio-1.1\etc\pulse\default.paを編集します。
				-  #load-module module-native-protocol-tcpとload-module module-esound-protocol-tcpのコメントを外してipアドレスを追加します
					- load-module module-esound-protocol-tcp auth-ip-acl=127.0.0.1;172.16.0.0/12
					- load-module module-native-protocol-tcp auth-ip-acl=127.0.0.1;172.16.0.0/12 
			- C:\User\ユーザー名\pulseaudio-1.1\etc\pulse\daemon.confを編集します。
				- セミコロンを外して数字を20から-1に変更します。
					- exit-idle-time = -1
		- Pulse Audioを起動します
			- C:\User\ユーザー名\puseaudio-1.1\bin\pulseaudio.exeをダブルクリックで起動します。一度目はすぐに終了してしまいます。もう一度ダブルクリックして起動します。エラーがありますが、終了しないのであればせそのままにします。
			- Windowsを起動するたびにpulseaduio.exeを起動します。
	- Ubuntu側
		- 必要なライブラリをインストールします。
			- sudo apt install alsa-utils
			- sudo apt install libpulse0
			- sudo apt install -y pulseaudio
		- WebSocketサーバの環境整えます
			- git clone https://github.com/meganetaaan/simple-stt-server.git
			- cd simple-stt-server
			- nodeが新しいとインストールできないためnodeのバージョンを16にします。
				volta install node@16
			- npm install
			- npm audit 
			- npm audit fix
		- VOSKのモデルをダウンロードしsimple-stt-serverのしたにmodelのフォルダーを作りダウンロードした出たデータを展開した状態でコピーします。
			- ダウンロードするデータ https://alphacephei.com/vosk/models/vosk-model-ja-0.22.zip
			- windowsからsimple-stt-serverのフォルダーにアクセスするにはエクスプローラーの左にあるLinuxのフォルダーがアクセスすることができます。Linux/Ubuntu-22.04/home/ubuntu/simple-sst-server
		- PULSE_SERVERの環境変数を設定します。~/.bashrcの最後に次の二行を追加します
			- export HOST_IP="$(ip route |awk '/^default/{print $3}')"
			- export PULSE_SERVER="tcp:$HOST_IP"
	        - 音声認識を起動します。
			- simple-stt-serverのところでnpm start -- --device default

- Ubuntu
	- 必要なliblaryをインストールします
		- sudo apt install alsa-utils
		- sudo apt install alsa-tools
	- WebSocketサーバの環境整えます
		- git clone https://github.com/meganetaaan/simple-stt-server.git
		- cd simple-stt-server
		npm install
	- VOSKのモデルをダウンロードしsimple-stt-serverのしたにmodelのフォルダーを作りダウンロードした出たデータを展開した状態でコピーします。
		- ダウンロードするデータ https://alphacephei.com/vosk/models/vosk-model-ja-0.22.zip
        - 音声認識を起動します。
		- simple-stt-serverのところでnpm start -- --device default
