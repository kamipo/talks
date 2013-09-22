mysql-buildでQ4Mやmroongaもビルドしたい
==========

2013/09/21<br/>
YAPC::Asia Tokyo 2013 LT<br/>
<address>
[@kamipo](http://twitter.com/kamipo)<br/>
[github/kamipo](https://github.com/kamipo)
</address>

Q4M
----------

[MySQL Casual Advent Calendar 2011 (kazeburoさん)](http://blog.nomadscafe.jp/2011/12/q4m---mysql-casual-advent-calendar-2011.html)

    最近livedoorからオープンソース化された3億ファイルを管理してるオブジェクトストレージ「STF」でも使ってるMessage QueueのQ4Mのインストール方法を紹介するよ！
    知ってる人も多いと思うけどQ4Mはkazuhoさんによって開発されたMySQLのストレージエンジンとして実装されてるMessage Queue。livedoorではもちろん、mixiやDeNAをはじめソーシャルゲーム各社でも使われている。

Q4M
----------

[MySQL Casual Advent Calendar 2011 (xaicronさん)](http://blog.livedoor.jp/xaicron/archives/53380670.html)

    仕事で Q4M とか HandlerSocket とか使う機会があります。
    更に言うと、手元の Mac の環境でそれらを手軽に使いたいので、簡単にセットアップが出来ると嬉しいですね。

mroonga
----------

[全文検索エンジンgroongaを囲む夕べ 4](http://atnd.org/events/43461)

mysql-build
----------

[mysql-build (feature/plugin)](https://github.com/kamipo/mysql-build/tree/feature/plugin)

mysql-build
----------

    $ mysql-build --help
    mysql-build, Compile and Install MySQL
    usage: mysql-build [-v|--verbose] [--with-debug] definition prefix [plugin[,...]]
           mysql-build --definitions
           mysql-build --plugins

      -v/--verbose     Verbose mode: print compilation status to stdout
      --with-debug     Debug build
      --definitions    List all built-in definitions
      --plugins        List all built-in plugins

mysql-build
----------

    $ mysql-build --definitions
    3.23.58
    4.0.30
    4.1.25
    5.0.95
    5.1.68
    5.1.69
    (snip)
    5.5.33
    5.5.34
    5.6.10
    5.6.11
    5.6.12
    5.6.13
    5.6.14
    5.7.1-m11
    (snip)

mysql-build
----------

    $ mysql-build --plugins
    mroonga-3.07
    q4m-0.9.10

mysql-build
----------

    $ mysql-build --verbose --with-debug \
                    5.6.14 ~/opt/mysql/5.6.14-yapcasia \
                    q4m-0.9.10,mroonga-3.07
    Downloading http://cdn.mysql.com/Downloads/MySQL-5.6/mysql-5.6.14.tar.gz...
    (snip)

みんなMySQLのプラグインもつかってくれるかな？
----------

Thanks!!
----------

