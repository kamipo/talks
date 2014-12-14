MySQLとActiveRecord<br/>その後
==========

2014/12/12<br/>
MySQL Casual Talks vol.7<br/>
<address>
[@kamipo](https://twitter.com/kamipo)<br/>
[github/kamipo](https://github.com/kamipo)
</address>

About me
----------

* [twitter/kamipo](https://twitter.com/kamipo)
* [github/kamipo](https://github.com/kamipo)
* [qiita/kamipo](http://qiita.com/kamipo)
* [かみぽわーる](http://blog.kamipo.net/)

[Rails複数DB Casual Talks](http://connpass.com/event/9560/)
----------

* [Rails複数DB Casual TalksでMySQLとActiveRecordの話をしてきた](http://blog.kamipo.net/entry/2014/11/16/112225)

Rails複数DB Casualでの話
----------

* ActiveRecordのMySQL対応は放置される傾向にある
* ActiveRecordからMySQLを使うときのハマりどころ

ActiveRecordのMySQL対応は放置される傾向にある
----------

* [Rebuild: 56](http://rebuild.fm/56/)
* a_matsudaさんがゲストの回でActiveRecordのMySQL対応について言及されてる

[Rebuild: 56](http://rebuild.fm/56/)
----------

    miyagawa「MySQLのdatetimeでmicrosecondサポートするやつマージされるのに1年半ぐらいかかった」

* [MySQL 5.6 and later supports microsecond precision in datetime. #8240](https://github.com/rails/rails/pull/8240)

[Rebuild: 56](http://rebuild.fm/56/)
----------

    a_matsuda「ARメンテしてる人が全員ポスグレ派でMySQL使ってる人がたぶん誰もいない」

[Rebuild: 56](http://rebuild.fm/56/)
----------

    a_matsuda「MySQLまわりのパッチはめっちゃ放置される傾向にあります」

ActiveRecordのMySQL対応は放置される傾向にある
----------

* メンテナが全員ポスグレ派なので使ってる人が問題を上げていくしかない
* MySQL使ってる人たちが問題を上げていかないと解決されないままになる

MySQL対応パッチを投げてみた
----------

* [Pull Requests by kamipo · rails/rails](https://github.com/rails/rails/pulls/kamipo)

MySQL対応パッチを投げてみた
----------

* いっぱい投げてみたけどほとんど4.2.0には入れれなかった
 * for MySQLって書いたPRはひとつもマージされなかった
 * IntegerのRangeErrorは超絶リグレッションだから直せてよかった
* まだまだ直したいissueがいっぱいあるので5.0.0に向けてがんばりたい

[activerecord-mysql-awesome](https://rubygems.org/gems/activerecord-mysql-awesome)
----------

* ActiveRecord 4.x向けにバックポートすることにした


バックポートしたPR
----------

* [Add SchemaDumper support table_options for MySQL. #17569](https://github.com/rails/rails/pull/17569)
* [Add charset and collation options support for MySQL string and text columns. #17574](https://github.com/rails/rails/pull/17574)
* [Add bigint pk support for MySQL #17631](https://github.com/rails/rails/pull/17631)
* [If do not specify strict_mode explicitly, do not set sql_mode. #17654](https://github.com/rails/rails/pull/17654)
* [Add unsigned option support for MySQL numeric data types #17696](https://github.com/rails/rails/pull/17696)
* [Support for any type primary key #17851](https://github.com/rails/rails/pull/17851)

バックポートしたPR
----------

* [Refactor `SchemaCreation#visit_AddColumn` #17798](https://github.com/rails/rails/pull/17798)
* [Refactor `visit_ChangeColumnDefinition` #17822](https://github.com/rails/rails/pull/17822)
* [Add default value for `create_table_definition` #17894](https://github.com/rails/rails/pull/17894)

まだまだバックポートするのでよろしくお願いします！
----------

Enjoy!!🍻
----------

