# Install

## VirtualBoxのインストール
[VirtualBoxのダウンロードサイト](https://www.virtualbox.org/wiki/Download_Old_Builds_4_2)から落としてください。
インストーラに従いインストールしてください。

2013/12/20現在、VirtualBoxのバージョンを4.2.16以前にしないとVagrantが正常に動きません。

## Vagrantのインストール
[Vagrantのダウンロードサイト](http://www.vagrantup.com/downloads.html)から落としてください。
Vagrantはgemとして公開されていますが、サイトで公開されているもののほうがバージョンが新しいのでそちらを落とします。
ダウンロード後、インストーラに従いインストールしてください。

### 設定の変更
デフォルトの設定では、ゲストOSのメモリ容量が512MBしか割り当てられていません。
このままでは少しもの足らないので、1024MBに増量します。
Vagrantfileに以下の設定を追加してください。

```ruby
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  …
  config.vm.provider :virtualbox do |vb|
    # Use VBoxManage to customize the VM. For example to change memory:
    vb.customize ["modifyvm", :id, "--memory", "1024"]
  end
  …
end
```

### Vagrantプラグイン
Vagrantをさらに便利に使うためのプラグインを紹介します。

#### vagrant-omnibus
[Github::vagrant-omnibus](https://github.com/schisamo/vagrant-omnibus)

boxにシェフが入っていない場合に自動で入れてくれるプラグインです。
コマンドライン上で `vagrant plugin install vagrant-omnibus` と入力してインストールします。

Vagrantfileに以下の設定を追加してください。

```ruby
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  …
  config.omnibus.chef_version = :latest
  …
end
```

その後、`vagrant up` をするとChefの最新版がboxにインストールされます。

#### sahara
[Github::sahara](https://github.com/jedi4ever/sahara)

OSの途中の状態を記憶しておき、以前の状態に戻したいときはコマンドひとつでロールバックできるプラグイン。
コマンドライン上で `vagrant plugin install sahara` と入力してインストールします。
