# -*- mode: ruby -*-
# vi: set ft=ruby :

MODE = "NODEV"  # "DEV"とするとデバッグ用の設定になる

# 設定バージョン
Vagrant.configure("2") do |config|

  # 起動ボックスの指定
  config.vm.box = "bento/ubuntu-16.04"
  # デバッグ用に起動タイムアウトを短くしておく設定
  config.vm.boot_timeout = MODE != "DEV" ? 300 : 60

  # ホストへのローカルアクセスをゲストOSに転送
  config.vm.network "forwarded_port", guest: 80, host: 8080
  config.vm.network "forwarded_port", guest: 3000, host: 3000  # Rails用

  # ゲストOSにアクセスするためのIPを設定
  config.vm.network "private_network", ip: "192.168.33.10"

  # シンクロ設定
  config.vm.synced_folder "./sync", "/home/vagrant/sync",
      created: true, owner: "vagrant", group: "vagrant"

  # プロバイダー固有の設定
  config.vm.provider "virtualbox" do |vb|
    vb.gui = true
    vb.memory = "1024"
    # ケーブルの設定
    vb.customize [
      'modifyvm', :id, '--cableconnected1', 'on', # ケーブルの設定
      "--vram", "256", # ビデオメモリ確保（フルスクリーンモードにするため）
      "--clipboard", "bidirectional", # クリップボードの共有
      "--draganddrop", "bidirectional", # ドラッグアンドドロップ可能に
      "--cpus", "2", # CPUは2つ
      "--ioapic", "on" # I/O APICを有効化
    ]
  end

  # 起動時のスクリプト
  config.vm.provision "shell", inline: <<-SHELL
   apt-get update
  SHELL
end
