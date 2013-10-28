mysql-build<br/>よもやま話
==========

2013/10/25<br/>
MySQL Casual Talks Vol.5<br/>
<address>
[@kamipo](http://twitter.com/kamipo)<br/>
[github/kamipo](https://github.com/kamipo)
</address>


今週ネタ仕込む余裕なかったのでmysql-buildに絡めていろいろ話そうと思います
----------

mysql-build --plugins
----------

mysql-build --plugins
----------

    $ mysql-build --plugins
    handlersocket-1.1.1  <--  NEW!!
    mroonga-3.07
    q4m-0.9.10

mysql-build --plugins
----------

    $ mysql-build --verbose --with-debug \
                    5.6.14 ~/opt/mysql/5.6.14+q4m+hs \
                    q4m-0.9.10,handlersocket-1.1.1
    Downloading http://cdn.mysql.com/Downloads/MySQL-5.6/mysql-5.6.14.tar.gz...
    (snip)

[Percona Server 5.6.14-62.0 is now available](http://www.mysqlperformanceblog.com/2013/10/24/percona-server-5-6-14-62-0-now-available/)
----------

　
----------

    $ mysql-build --definitions
    (snip)
    5.6.13
    5.6.14
    5.7.1-m11
    5.7.2-m12
    facebook-5.6.12
    (snip)
    percona-5.5.31-30.3
    percona-5.5.32-31.0
    percona-5.5.33-31.1
    percona-5.6.13-61.0
    percona-5.6.14-62.0  <-- NEW!!
    twitter-5.5.33.t12

[MySQL 5.6における大量データロード時の考慮点 - SH2の日記](http://d.hatena.ne.jp/sh2/20131007)
----------

diff
----------

[https://gist.github.com/kamipo/7152154](https://gist.github.com/kamipo/7152154)

[facebook/mysql-5.6](https://github.com/facebook/mysql-5.6)
----------

[Yoshinori Matsunobu's blog: Making full table scan 10x faster in InnoDB](http://yoshinorimatsunobu.blogspot.jp/2013/10/making-full-table-scan-10x-faster-in.html)
[INDEX FULL SCANを狙う - MySQL Casual Advent Calendar 2011 - SH2の日記](http://d.hatena.ne.jp/sh2/20111217)

diff
----------

[https://gist.github.com/kamipo/7152690](https://gist.github.com/kamipo/7152690)

mysql-buildでビルドできないときは言ってくれたらとりあえずなんとかできるかがんばってみるので気軽に@kamipoください
----------

おしまい
----------

