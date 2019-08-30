# N-ISUCON
## やったこと
- netdataでアプリケーションや自分のPCの状態確認できることを確認
- スクショに掲載
- dstatをインストール
- ある程度当たりをつけて負荷計測 -> 設定 or コード変更をしているときには、netdataではなく dstatを使う。
- Installing siege
- https://jasonmccreary.me/articles/installing-siege-mac-os-x-lion/
- {
  "job_id": "",
  "ip_addrs": "192.168.33.10",
  "pass": true,
  "score": 657,
  "message": "ok",
  }

- https://muttan1203.hatenablog.com/entry/isucon_practice_setup
- https://blog.yuuk.io/entry/web-operations-isucon
- Perlに切り替え
- 基本的なお作法
- {
  "job_id": "",
  "ip_addrs": "192.168.33.10",
  "pass": true,
  "score": 859,
  "message": "ok",
  }

- 初期データ流し込み
- {
  "job_id": "",
  "ip_addrs": "192.168.33.10",
  "pass": true,
  "score": 432,
  "message": "ok",
  }


  - {
    "job_id": "",
    "ip_addrs": "192.168.33.10",
    "pass": true,
    "score": 2301,
    "message": "ok",
    }
- なるほど
```
sudo apt update
sudo apt install -y git vim mysql-server nginx
sudo update-alternatives --set editor /usr/bin/vim.basic
```


isucon ユーザーを作成
```
sudo adduser isucon

isucon ALL=NOPASSWD: ALL
```

isucon ユーザーでログインできるようにする

```
sudo su isucon
cd
mkdir .ssh/
chmod 700 .ssh
cd .ssh
touch authorized_keys
chmod 600 authorized_keys  # authorized_keys に公開鍵を追加する
```

ssh を秘密鍵でのみログインできるようにする
```
sudo vi /etc/ssh/sshd_config

#PasswordAuthentication yes
PasswordAuthentication no

sudo service ssh restart
```
 isucon ユーザーを作り、そのホームディレクトリ配下のディレクトリに リポジトリをチェックアウト
```
sudo apt install git
git clone https://github.com/isucon/isucon7-qualify.git
```

nginx と MySQL は Ubuntu の標準のもの
```
sudo apt install mysql-server nginx
```

```
sudo apt install -y git curl libreadline-dev pkg-config autoconf automake build-essential libmysqlclient-dev \
	libssl-dev python3 python3-dev python3-venv openjdk-8-jdk-headless libxml2-dev libcurl4-openssl-dev \
        libxslt1-dev re2c bison libbz2-dev libreadline-dev libssl-dev gettext libgettextpo-dev libicu-dev \
	libmhash-dev libmcrypt-dev libgd-dev libtidy-dev
```

ベンチマーカーのために、Go
```
cd
git clone https://github.com/tagomoris/xbuild.git

mkdir local
xbuild/ruby-install   -f 2.4.2   /home/isucon/local/ruby
xbuild/perl-install   -f 5.26.1  /home/isucon/local/perl
xbuild/node-install   -f v6.11.4 /home/isucon/local/node
xbuild/go-install     -f 1.9     /home/isucon/local/go
xbuild/python-install -f 3.6.2   /home/isucon/local/python
xbuild/php-install    -f 7.1.9   /home/isucon/local/php -- --disable-phar --with-pcre-regex --with-zlib --enable-fpm --enable-pdo --with-mysqli=mysqlnd --with-pdo-mysql=mysqlnd --with-openssl --with-pcre-regex --with-pcre-dir --with-libxml-dir --enable-opcache --enable-bcmath --with-bz2 --enable-calendar --enable-cli --enable-shmop --enable-sysvsem --enable-sysvshm --enable-sysvmsg --enable-mbregex --enable-mbstring --with-mcrypt --enable-pcntl --enable-sockets --with-curl --enable-zip --with-pearAA

```

ベンチマーカーの準備
Goの環境変数設定
```
export PATH=$HOME/local/go/bin:$HOME/go/bin:$PATH
go get github.com/constabulary/gb/...   # 初回のみ
cd ~/isubata/bench
gb vendor restore
make
```

初期データ設定
```
./bin/gen-initial-dataset
```

データベース初期化
```
$ sudo ./db/init.sh
$ sudo mysql
mysql> CREATE USER isucon@'%' IDENTIFIED BY 'isucon';
mysql> GRANT ALL on *.* TO isucon@'%';
mysql> CREATE USER isucon@'localhost' IDENTIFIED BY 'isucon';
mysql> GRANT ALL on *.* TO isucon@'localhost';
zcat ~/isubata/bench/isucon7q-initial-dataset.sql.gz
```

nginx

```
$ sudo cp ~/shima/files/app/nginx.* /etc/nginx/sites-available
$ cd /etc/nginx/sites-enabled
$ sudo unlink default
$ sudo ln -s ../sites-available/nginx.conf
$ sudo systemctl restart nginx
```

## インフラ担当
- ポータルサイトにログインしてsshできることを確認
- 全メンバーのssh鍵を登録する
- 必要なパッケージなどインストール
- 何もせずにベンチマークを流す
- Go実装に切り替えてベンチマークを流す
- nginxで計測できるようにする（parse.rbを使う）
- ハードウェアの構成を調べる
- 動作しているプロセスを確認しておおよその構成を理解する
- データベースなど各アプリケーションの設定値を確認してgitにコミットする
- Mysqlのshow logを読む
- CPUの気持ちになる。



# わかったこと気づき(勝つための気づき)
- スコア下がること結構ある
- ベンチマーク回らないことある
- 再起動を忘れるな(設定変更して再起動しないで放置してスコア伸びないことある)→いろんなものを再起動するシェル書いた。
- git pull、MySQL、Redis、memcached、app、nginxの再起動
- Goの編集
- 時間がかかるだけの作業多いので予備のPCを2台ぐらい持って言った方がいいなと思った
- ホワイトボード欲しい
- モニターも3台用意するべきだと思った。
