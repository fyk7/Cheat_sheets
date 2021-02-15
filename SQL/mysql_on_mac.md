## Install MySQL to Mac

```sh
$ brew install mysql
$ brew info mysql
$ mysql —version
$ mysql.server start
$ mysql.server start --skip-grant-tables #　パスワード無しでログイン
$ mysql.server stop # 停止
$ mysl -uroot -p # enter password
```

## Check DB Info

```sh
$ mysql.server start
$ mysl -uroot -p # enter password
mysql> help
mysql> \h
mysql> show variables like 'version'; #バージョン確認
mysql> show status; #セッション統計情報の表示
mysql> show status like "Threads_connected"; #現在の同時接続数を表示
mysql> select user(); #ログイン中のユーザ表示
mysql> show grants; #現在ログイン中ユーザの権限確認
mysql> show databases; #データベース一覧を表示
mysql> use [db_name] #使用するデータベースを選択（セミコロン不要）

mysql> create database [db_name] #データベースの作成
mysql> drop database [db_name] #データベースの削除
mysql> show tables; #テーブル一覧を表示
mysql> show tables status; #テーブル一覧のステータス情報を表示
mysql> desc [table_name]; #カラム一覧を表示
mysql> show full columns from [table_name]; #カラム一覧を表示、Collation 付き。
mysql> show index form [table_name]; #特定テーブルのインデックスを確認
```

## Create DB User

```sh

# ユーザ名：hoge
# パスワード：fuga

mysql> create user ‘hoge’@localhost identified by ‘fuga’;
mysql> select user, host from mysql.user;
mysql> drop user ‘hoge’@localhost;

$ mysql -u hoge-p
mysql> exit
mysql> quit
mysql> \q

mysql> create database ginexample;
mysql> grant create on ginexample.\* to ginexample@localhost; # ユーザーに DB作成権限を付与
mysql> grant all on ginexample.\* to ginexample@localhost; # ユーザーに DB操作権限を付与
'''
```
