MySQLとActive Recordの話
==========

2019/05/17<br/>
Oracle Code Tokyo 2019<br/>
<address>
Ryuta Kamizono ([@kamipo](https://github.com/kamipo))
</address>

About me
----------

<img src="img/contributors_edge.png" width="100%">
http://contributors.rubyonrails.org/edge/contributors

Rails 6.0.0 rc1 has released!
----------

[Rails 6.0.0 rc1 released](https://weblog.rubyonrails.org/2019/4/24/Rails-6-0-rc1-released/)

Rails 6.0のMySQL関連の変更
----------

* utf8mb4 by default
* デフォルト式
* 関数インデックス
* Optimizer Hints

MySQLのヘンなところ…
----------

* [知って得しない Active Record Issues (MySQL編)](https://kamipo.github.io/talks/20170201-mysqlcasual10/#/title)

ヘンなところはどんどんなくなっている
----------

* GROUP BY ... DESCの削除
* デフォルト式
* 関数インデックス
* LATERAL句
* CHECK制約

Maintenance Releaseとはなんだったのか
----------

* GROUP BY ... DESCの削除 8.0.13
* デフォルト式 8.0.13
* 関数インデックス 8.0.13
* LATERAL句 8.0.14
* CHECK制約 8.0.16

以上、ここがヘンだよMySQLでした。
----------
