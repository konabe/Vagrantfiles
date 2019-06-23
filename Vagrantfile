# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrant CloudにあるBOXの名前を指定する
VAGRANT_CLOUD_BOX_NAME = "bento/ubuntu-16.04"
# "DEV"とするとデバッグ用の設定になる
MODE = "NODEV"
# trueとするとGUIがONになる
# +- そうしただけでは動かないので注意
#   +- 参照：https://qiita.com/mhagita/items/8f339106c3b48eb098ca
GUI = false
# trueとするとdocker/docker-composeがインストールされる
DOCKER = true
# 空文字以外では、docker-compose.ymlの場所を指定する
DOCKERCOMPOSE_PATH = ""
# 仮想側の共有データのパス(ホスト側は自動的にdataフォルダに入る)
# +- これによって自動的にWindowsでのホスティングによるシンボリックリンクを許可できる
DATA_FOLDER_PATH = "/home/vagrant/shared"

# 設定バージョン
Vagrant.configure("2") do |config|
  # 起動ボックスの指定
  config.vm.box = VAGRANT_CLOUD_BOX_NAME
  # デバッグ用に起動タイムアウトを短くしておく設定
  config.vm.boot_timeout = MODE != "DEV" ? 300 : 60
  # ホストへのローカルアクセスをゲストOSに転送
  config.vm.network "forwarded_port", guest: 80, host: 8080
  config.vm.network "forwarded_port", guest: 3000, host: 3000  # Rails用
  # ゲストOSにアクセスするためのIPを設定
  config.vm.network "private_network", ip: "192.168.33.10"

  # シンクロ設定
  config.vm.synced_folder "data", DATA_FOLDER_PATH,
      create: true, owner: "vagrant", group: "vagrant"

  # プロバイダー固有の設定
  config.vm.provider "virtualbox" do |vb|
    enable_gui(vb) if GUI
    # シンボリックリンクを許可する設定
    vb.customize ["setextradata", :id, "VBoxInternal2/SharedFoldersEnableSymlinksCreate/#{DATA_FOLDER_PATH}","1"]
  end

  enable_docker(config, DOCKERCOMPOSE_PATH) if DOCKER

end

# GUIをONにする設定
# vb: provider object(virtual boxに限る)
def enable_gui(vb)
  vb.gui = true
  vb.memory = "1024"
  vb.customize [
    'modifyvm', :id, '--cableconnected1', 'on', # ケーブルの設定
    "--vram", "256", # ビデオメモリ確保（フルスクリーンモードにするため）
    "--clipboard", "bidirectional", # クリップボードの共有
    "--draganddrop", "bidirectional", # ドラッグアンドドロップ可能に
    "--cpus", "2", # CPUは2つ
    "--ioapic", "on", # I/O APICを有効化
  ]
end

def enable_docker(config, yml_path)
  # dockerの導入
  config.vm.provision :docker
  if yml_path.empty?
    config.vm.provision :docker_compose
  else
    config.vm.provision :docker_compose,
      yml: yml_path,
      run: "always"
  end
end
