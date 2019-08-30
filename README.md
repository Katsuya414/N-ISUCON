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



# わかったこと気づき
- スコア下がること結構ある
- ベンチマーク回らないことある
- 再起動を忘れるな(設定変更して再起動しないで放置してスコア伸びないことある)→いろんなものを再起動するシェル書いた。
- git pull、MySQL、Redis、memcached、app、nginxの再起動
- Goの編集
