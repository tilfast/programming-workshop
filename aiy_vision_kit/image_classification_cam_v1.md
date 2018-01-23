Raspberry Pi Zero W + VisionBonnet accessory board + Camera Module Version 1によるカメラ画像分類
------------

ディープラーニングを使ったカメラ画像認識をRaspberry Pi上に実装する。
Raspberry Pi Zero Wに[Vision Kit](https://aiyprojects.withgoogle.com/vision/)のVisionBonnet accessory boardをにアタッチすることで、ほぼリアルタイムな分類処理を実現する。
また、Vision Kit組み立てにはカメラモジュールのv2が必要となるが、今回はv1カメラを用いて実装を行う。

1. セットアップ  
本家Raspberry Piサイトの[インストールガイド](https://www.raspberrypi.org/documentation/installation/installing-images/README.md)を参照しながら進めます。

2. ヘッドレスセットアップのための準備  
OSイメージが書き込まれたSDカードを、パソコンのSDカードリーダーで開き、`/boot`ディレクトリに中身が空の`ssh`ファイルと、下記の内容の`wpa_supplicant.conf`ファイルを置きます。使用する無線LAN環境に合わせてSSID名とパスワードの箇所を変える。
```
country=GB
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1

network={
        ssid="<SSID名>"
        psk="<パスワード>"
        key_mgmt=WPA-PSK
        proto=RSN
        pairwise=CCMP
        auth_alg=OPEN
}
```

3. Raspberry Piの起動  
パソコンからSDカードを取り出し、RPi本体に入れて起動する。

4. IPアドレスの確認  
同じネットワークセグメントにつながっているパソコンから下記コマンドでRasperry PiのIPアドレスを探す(下記はWindows上のコマンドプロンプトを使う場合)。
Raspberry PiのMACアドレスは`B8-27-EB`から始まるので、それらに紐づいているIPアドレスを探す。
```
for /l %i in (0,1,255) do ping -w 1 -n 1 192.168.1.%i && arp -a 192.168.1.%i  