# Rasbery Pi ネットワーク設定
- 有線ネットワーク設定  
・設定ファイル編集  
      > sudo vim /etc/network/interfaces   
      iface eth0 inet dhcp -> iface eth0 inet static
      address <- IPアドレス
      netmask <- サブネットマスク
      gateway <- デフォルトゲートウェイ
  ・ネットワーク設定反映
      > reboot
      または
      > sudo /etc/init.d/networking reload
  ・確認方法  
      > ping 設定したIPアドレス
      または
      > ssh username@設定したIPアドレス(Mac or Linux)
- 無線Wi-fi設定  
・SSID/パスワード入力
      > wpa passphrase wi-fiルーターのSSID wi-fiルーターのパスワード
    を入力します。  
    入力後、
      network={
        ssid="Your SSID"
        psk="Your Passphrase"
      }
    と出力されたらコピーしておきます。  
・設定ファイル編集
      > sudo vim /etc/wpa_supplicant/wpa_supplicant.conf
      ctrl_interface=DIR=var/run/wpa_supplicant GROUP=netdev
      update_flag=1
      先ほどコピーで取り置きしていたテキストを貼り付け & 追記
      network={
        ssid="Your SSID"
        psk="Your Passphrase"
        # 以下の項目を追記
        key_mgmt=WPA_PSK
        proto=WPA2
        pairwise=CCMP
        group=CCMP
        priority=2
      }
　編集後、有線ネットワーク設定と同様、IPアドレスを設定します。  
      > sudo vim /etc/network/interfaces
      auto wlan0
      allow-hotplug wlan0
      iface wlan0 inet manual -> iface wlan0 inet static
      address <- IPアドレス
      netmask <- サブネットマスク
      gateway <- デフォルトゲートウェイ
      wpa-roam /etc/wpa_supplicant/wpa_supplicant.conf -> wpa-conf /etc/wpa_supplicant/wpa_supplicant.conf
      iface default inet dhcp
・ネットワーク設定反映
      reboot
      または
      sudo /etc/init.d/networking reload
・確認方法
      > ping 設定したIPアドレス
      または
      > ssh username@設定したIPアドレス(Mac or Linux)
