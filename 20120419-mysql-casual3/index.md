AKBとmysql-buildの話
==========

2012/04/19<br/>
MySQL Casual Talks Vol.3<br/>
<address>
[@kamipo](http://twitter.com/kamipo)<br/>
[github/kamipo](https://github.com/kamipo)
</address>

AKBの話(бвб)
----------

(бвб)
----------

[@kojiharunyan](http://twitter.com/kojiharunyan)

今日4/19はにゃんにゃん(бвб)の24歳の誕生日です！
----------

涙サプライズ！です！！

Haruna storage engine
----------

仕事中でもターミナルに集中しながらこじはるのツイートをチェックできる
陽菜ストレージエンジンというのを作りました！

(бвб)
----------

    INSTALL PLUGIN haruna SONAME 'ha_haruna.so';

( *｀ω´)
----------

[@mariko_dayo](http://twitter.com/mariko_dayo)

( *｀ω´)八(бвб)
----------

はるにゃんとラブラブの麻里子様のツイートもいっしょにチェックできる機能も搭載してます！

( *｀ω´)八(бвб)
----------

    mariko_dayo ♥ kojiharunyan

( *｀ω´)八(бвб)
----------

    CREATE TABLE mariko_dayo LIKE kojiharunyan;

( *｀ω´)AKBおしまい(бвб)
----------

mysql-buildの話
----------

最近いろいろMySQLが出てましたね
----------

- MySQL 5.6.5 (5.6.5-m8)

- MySQL 5.6.6 (5.6.6-labs-april-2012)

- MariaDB 5.5 (5.5.23)

- Twitter MySQL 5.5 (5.5.19-t2)

こういうことしたい
----------

サービスの本番に突っ込む用とかじゃなくて自分のローカルにMySQLを入れたいんだけど
ディストリビューションのパッケージ管理システムとかだと複数のバージョンのMySQL扱ってないとかで面倒だし
ちょっと試しでこの前でた新しいバージョンのMySQLを入れたいんだけど
まだ安定版じゃないやつだと手元にソースコードも置いておきたいですよね


それmysql-buildでできるよ
----------

というのを目指しています

mysql-build
----------

    git clone git://github.com/kamipo/mysql-build.git
    export PATH="$HOME/mysql-build/bin:$PATH"

mysql-build --definitions
----------

    3.23.58
    4.0.30
    4.1.25
    5.0.95
    5.1.59
    5.5.14-spider-2.28
    5.5.18
    5.6.5-m8
    5.6.6-labs-april-2012
    mariadb-5.5.23
    twitter-5.5.19.t2

mysql-build
----------

    mkdir -p ~/opt/mysql
    mysql-build -v 5.6.5-m8 ~/opt/mysql/5.6.5-m8

mysql-build
----------

    cd ~/opt/mysql/5.6.5-m8
    ./scripts/mysql_install_db
    ./bin/mysqld_safe &

簡単ですね
----------

いとも
----------

    mysql-build -v 5.6.6-labs-april-2012 ~/opt/mysql/5.6.6-labs-april-2012

たやすく
----------

    mysql-build -v mariadb-5.5.23 ~/opt/mysql/mariadb-5.5.23

インストールできます！
----------

    mysql-build -v twitter-5.5.19.t2 ~/opt/mysql/twitter-5.5.19.t2

カジュアルにいろいろなMySQLを試してみましょう！
----------

おしまい
----------

