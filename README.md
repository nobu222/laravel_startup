## Laravel 5.1をhomesteadで動作するまでの手順

__事前に必要なもの__
* VirtualBox (VMWareでも可、今回はVirtualBoxを使用)
* Vagrant (version 1.7.推奨)
* gitコマンド

## Homesteadのvagrant boxをインストール

```
vagrant box add laravel/homestead
```

うまくいかない場合は
```
vagrant box add laravel/homestead https://atlas.hashicorp.com/laravel/boxes/homestead
```

## Homesteadのリポジトリをクローンする

```
git clone https://github.com/laravel/homestead.git Homestead
```
このソースはvagrantでLaravelを動作させるサーバを構築するためのソースが入っている
クローンによりできたHomesteadフォルダ内のinit.shを実行、ホームディレクトリに`~/.homestead`フォルダが生成されその中にHomestead.ymlファイルが作られる
```
bash init.sh
```

## Providerを設定する

```
vi ~/.homestead/Homestead.yml
```
5行目あたり`provider: virtualbox`

## SSH Keyを持っていない人は作る
```
ssh-keygen -t rsa
```

## Laravelのソースをシンクするフォルダを設定する
```
vi ~/.homestead/Homestead.yml
```
12行目辺り
```
# Homestead.yml内
folders:
    - map: ~/Code # ここに任意のフォルダを指定。あとでこのフォルダにLaravel本体のソースをダウンロードします。
      to: /home/vagrant/Code
```

## Hostsの書き換え
ローカルマシン上で`http://homestead.app`でアクセスするとLaravelを実行している仮想マシンにつながるようにするためHostsファイルを書き換える
```
vi /etc/hosts
```
以下を追記
```
192.168.10.10 homestead.app
```

## Laravelソースのインストール
先ほど設定したフォルダに移動しcomposerからLaravelプロジェクトを作成する
```
composer create-project laravel/laravel --prefer-dist
```

これでフォルダ内に`laravel`フォルダが出来る

## 仮想マシンを立ち上げる
Homesteadフォルダに移動し`vagrant up`する
一応`vagrant ssh`してホームディレクトリに`laravel`フォルダがあることを確認。

ブラウザで`http://homestead.app`が"Laravel5"と表示されたら完了！




