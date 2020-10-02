## Laravelチュートリアル
### 環境構築
とりあえずはWindows用の環境構築手順です
Macユーザは適宜読み替えてください
#### インストールするもの
 - Virtualbox
 https://www.oracle.com/technetwork/server-storage/virtualbox/downloads/index.html?ssSourceSiteId=otnjp
 - vagrant
 https://www.vagrantup.com/
 - Git
 https://git-scm.com/
 - PHP
 https://www.php.net/downloads.php
 - Composer
 https://getcomposer.org/
```
#各種インストーラに従ってインストール、ZIP形式の場合は任意の場所に解凍
#コマンドプロンプトでPATH環境確認

C:\WINDOWS\system32> git --version
git version 2.21.0.windows.1

C:\WINDOWS\system32> php -v
PHP 7.3.5 (cli) (built: May  1 2019 13:17:17) ( ZTS MSVC15 (Visual C++ 2017) x64 )
Copyright (c) 1997-2018 The PHP Group
Zend Engine v3.3.5, Copyright (c) 1998-2018 Zend Technologies

C:\WINDOWS\system32> composer -V
Composer version 1.8.5 2019-04-09 17:46:47

C:\WINDOWS\system32> vagrant -v
Vagrant 2.2.4
```

### PHP.iniの設定
```
# ComposerやLaravelの依存関係である拡張の有効化を行う
# PHPインストール先に「php.ini」がない場合
　「php.ini-development」をコピーし「php.ini」を作成する。
# php.iniに対し、以下の編集を行う。
# 「php.ini-development」から作成した場合には、該当行のコメントアウト「;」を外すだけでOK
extension_dir = "ext"
extension=mbstring
extension=openssl
※「extension_dir」はWindowsとその他で異なるようなので注意
```

#### 実行環境作成
```
# Homestead Vagrant Boxのインストール
> vagrant box add laravel/homestead
> ※進行途中で「Enter your choice:」と入力を求められるため【3(VirtualBox)】を入力し、Enter

# 適当にディレクトリを作る
# 作ったディレクトリに移動
# Gitをクローンする
> git clone https://gitlab.com/koh-hashimoto/laravel-traning.git
> ※ GitLabのID,PASSの入力を求められる

# laravel-traningに移動
> cd laravel-traning

# ライブラリ更新
$ composer update

# Homestead.yamlの用意
# コマンドでファイルを作成し、下部に記載している内容を転記
> type nul > Homestead.yaml

# 仮想マシンの起動
> vagrant up

# SSH鍵の作成
> mkdir ~/.ssh
> cd ~/.ssh
> ssh-keygen -t rsa
> ※入力を求められるがすべてそのままEnterでOK

# ワークスペースに戻る
> cd laravel-traning

# SSHで仮想環境に接続
> vagrant ssh

# ここから仮想環境内の作業
# .envファイルの作成
$ cd /vagrant
$ cp .env.example .env

# アプリケーションキーの設定
$ php artisan key:generate

# メモ帳で作業
# C:\Windows\System32\drivers\etc\hosts のファイルを管理者権限で開き、以下を追記
192.168.10.10  laravel-traning.com
```

- Homestead.yaml
```
ip: 192.168.10.10
memory: 2048
cpus: 1
provider: virtualbox
authorize: ~/.ssh/id_rsa.pub
keys:
    - ~/.ssh/id_rsa
folders:
    - map: '<laravel-traningディレクトリの絶対パス>'
      to: /home/vagrant/code
sites:
    - map: laravel-traning
      to: /home/vagrant/code/public
databases:
    - homestead
name: laravel-traning
hostname: laravel-traning
```

### 動作確認
ブラウザで http://laravel-traning.com にアクセスしてLaravelの文字が表示されれば完了！