Prepared statements on MySQL in Ruby
==========

2015/11/20<br/>
MySQL Casual Talks vol.8<br/>
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

近況🐬
----------

<a href="https://twitter.com/kamipo/status/666537628603297792">
<img src="img/3306.png" width="50%" alt="フォロワー3306人になりました！！">
</a>

RubyのMySQLバインディング
----------

* **mysql** [luislavena/mysql-gem](https://github.com/luislavena/mysql-gem)
 * MySQL C APIに忠実なラッパー
 * Maintainer needed
* **mysql2** [brianmario/mysql2](https://github.com/brianmario/mysql2)
 * 後発のモダンなやつ、今はみんなこれ使ってる
 * Prepared statementsサポートしてなかった

mysql2 0.4.0
----------

* Prepared statementsが使えるmysql2 0.4.0が出た！
* ISUCON5予選で使おうとしたら破滅的な挙動で死んだ😇
 * [stmt.execute cause Error: 2014 (CR_COMMANDS_OUT_OF_SYNC) #694](https://github.com/brianmario/mysql2/issues/694)
 * ISUCON5では信頼と実績の[tagomoris/mysql2-cs-bind](https://github.com/tagomoris/mysql2-cs-bind)を使った


mysql2 0.4.1
----------

* ISUCON5予選が終わって破滅的な挙動を直そうとしたらなんかいけそうなやつが既に実装されていた！
 * [Support prepared_statement.close #673](https://github.com/brianmario/mysql2/pull/673)

mysql2 0.4.1
----------

* 試してみたら即セグる破滅的な実装だった😨
 * [stmt.close cause segv #690](https://github.com/brianmario/mysql2/issues/690)
* とりあえず直した
 * [Support result.free #692](https://github.com/brianmario/mysql2/pull/692)

mysql2 0.4.1
----------

* result.freeを明示的に呼ぶのは好みじゃないらしく別実装がきた
 * [Avoid crashing when Statement#close is called before a Result is collected #697](https://github.com/brianmario/mysql2/pull/697)
 * 最初の問題はまだ原因特定できてないしとりいそぎこの実装でもいいかなと思ってる

Prepared statements support for ActiveRecord
----------

* [Add prepared statements support for `Mysql2Adapter` #22352](https://github.com/rails/rails/pull/22352)
 * PRed Today!!

That's all🍻
----------

