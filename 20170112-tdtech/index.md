Active Record Issues
==========

2017/01/12<br/>
Treasure Data Tech Talk 2017新春<br/>
<address>
[@kamipo](https://twitter.com/kamipo)<br/>
Ryuta Kamizono<br/>
Treasure Data, Inc.
</address>

About me
----------

* [twitter/kamipo](https://twitter.com/kamipo)
* [github/kamipo](https://github.com/kamipo)
* [かみぽわーる](http://blog.kamipo.net/)
* [Oracle ACE (Expertize MySQL)](https://apex.oracle.com/pls/apex/f?p=19297:4:::NO:4:P4_ID:12800)
* [Rails Issues Team](https://github.com/orgs/rails/people) <- **NEW**

Rails Contributors in 2016
----------

<a href="https://github.com/rails/rails/graphs/contributors?from=2016-01-01&to=2016-12-31">
<img src="img/contributors_2016.png" width="80%" alt="Rails Contributors in 2016">
</a>

Rails Contributors - Edge
----------

<a href="http://contributors.rubyonrails.org/edge/contributors">
<img src="img/contributors_edge.png" width="90%" alt="Rails Contributors - Edge">
</a>

きっかけ
----------

<a href="https://twitter.com/kamipo/status/408089672963739648">
<img src="img/tw_kamipo_sikato.png" width="70%" alt="きっかけ">
</a>

きっかけ
----------

* Rails+MySQL構成ではSQL手書きしないとまともなスキーマ定義ができなくて、手書きしたまともなスキーマ定義を正確に復元できないので `schema_format = :sql` でmysqldumpしていた
* [Rails複数DB Casual Talks](https://connpass.com/event/9560/)で[@a_matsuda](https://twitter.com/a_matsuda)さんにschema dumper使いものにならないからデフォルトを `schema_format = :sql` に変えるっていう議論がcore teamであるらしいことを聞いて戦慄する
 * たしかに使いものにならないけど、全然余裕で直せるのに直す気のある人がいない

きっかけ
----------

* ほとんどなにも直せずにRails 4.2が出てしまった！
 * `strict_mode: false` で `sql_mode = ''` にする breaking change も覆せなかった [#17370](https://github.com/rails/rails/issues/17370)
* MySQL対応含めおかしいところすべて自分で5.0までに直し切るしかないと思った
 * 困ってる人がいる問題が直るのではなく直す気のある人がいる問題が直る

滅茶苦茶MySQL対応直した
----------

* テーブルオプション対応 [#17569](https://github.com/rails/rails/pull/17569)
* unsigned / collation 対応 [#17696](https://github.com/rails/rails/pull/17696) / [#17574](https://github.com/rails/rails/pull/17574)
* custom primary key 対応 [#17851](https://github.com/rails/rails/pull/17851), [#18220](https://github.com/rails/rails/pull/18220), [#18228](https://github.com/rails/rails/pull/18228)
* composite primary key 対応 [#21614](https://github.com/rails/rails/pull/21614)
* default CURRENT_TIMESTAMP 対応 [#20005](https://github.com/rails/rails/pull/20005)
* MySQL JSON data type support [#21110](https://github.com/rails/rails/pull/21110)

滅茶苦茶MySQL対応直した
----------

* datetime round problem 対応 [#18067](https://github.com/rails/rails/pull/18067)
* mysql2ストアドプロシージャ対応 [#21932](https://github.com/rails/rails/pull/21932)
* mysql2プリペアドステートメント対応 [#23461](https://github.com/rails/rails/pull/23461)
* ONLY_FULL_GROUP_BY 対応 [#22241](https://github.com/rails/rails/pull/22241)
* sql_mode = kamipo TRADITIONAL [#24167](https://github.com/rails/rails/pull/24167), [#24362](https://github.com/rails/rails/pull/24362)
* Revert utf8_unicode_ci (ハハパパ問題) [#20000](https://github.com/rails/rails/pull/20000), [#22061](https://github.com/rails/rails/pull/22061)

MySQLとUnicodeの問題
----------

* [utf8_unicode_ci に対する日本の開発者の見解](http://blog.kamipo.net/entry/2015/03/08/145045)
* [MySQL と Unicode Collation Algorithm (UCA)](http://blog.kamipo.net/entry/2015/03/17/103457)
* [MySQL と寿司ビール問題](http://blog.kamipo.net/entry/2015/03/23/093052)
* [#76553: Sushi-Beer issue of MySQL with utf8mb4](https://bugs.mysql.com/bug.php?id=76553)

ハハパパ🍣🍺問題
----------

* utf8mb4_general_ci
 * ハハパパ🙆　🍣🍺🙅
* utf8mb4_unicode_ci
 * ハハパパ🙅　🍣🍺🙅
* utf8mb4_unicode_520_ci
 * ハハパパ🙅　🍣🍺🙆
* utf8mb4_bin
 * ハハパパ🙆　🍣🍺🙆

🍣🍺問題
----------

* MySQLで文字列の比較に使われるcollating weightの仕様がおかしい
 * collating weightがある文字はそれを使う。ない場合は以下に従う。
 * BMP文字でgeneral collation (xxx_general_ci)の場合、コードポイントを使う。
 * BMP文字でunicode collation (xxx_unicode_ci)の場合、なんかいい感じの計算式で導出する。
 * SMP文字(絵文字とか)の場合、0xfffd REPLACEMENT CHARACTERと同じweightになる😳

ハハパパ問題
----------

* MySQLがUnicode Collation Algorithm (UCA)の一部しか実装していない
* UCAではMulti-Level Comparisonといって複数レベルに分けて文字列の比較を行い、各レベルの比較で文字列が一致した場合(tie-breaking level)、次のレベルで比較を行うを繰り返して順序を決定する。
 * L1は大文字小文字アクセント無視(Base Charaters)
 * L2はアクセントを無視しない(Accents)
 * L3は大文字小文字を無視しない(Case/Variants), etc...

ハハパパ問題
----------

* Primary weight range(L1比較のためのweight)しか実装していない
 * Base Charatersの区別はされるがアクセント(濁点半濁点)の区別はされない
 * 日本語の文字列比較には適さない

ハハパパ問題
----------

[mysql-5.7.10/strings/ctype-uca.c#L17-L31](https://github.com/mysql/mysql-server/blob/mysql-5.7.10/strings/ctype-uca.c#L17-L31)

<pre style="font-size: 60%"><code>
/* 
   UCA (Unicode Collation Algorithm) support. 
   Written by Alexander Barkov <bar@mysql.com>
   
   Currently supports only subset of the full UCA:
   - Only Primary level key comparison
   - Basic Latin letters contraction is implemented
   - Variable weighting is done for Non-ignorable option
   
   Features that are not implemented yet:
   - No Normalization From D is done
     + No decomposition is done
     + No Thai/Lao orderding is done
   - No combining marks processing is done
*/
</code></pre>

MySQL 8.0.1 Release Notes
----------

* The default collation for the utf8mb4 character set has changed from utf8mb4_general_ci to utf8mb4_0900_ai_ci. (utf8mb4_general_ci does not handle characters outside the Basic Multilingual Plane (BMP) correctly.)

<br/>
[Changes in MySQL 8.0.1](https://dev.mysql.com/doc/relnotes/mysql/8.0/en/news-8-0-1.html)<br/>
[What MySQL 8.0.1 means to you as a Developer](https://blog.gabriela.io/2017/01/10/what-mysql-8-0-1-means-to-you-as-a-developer/)

コアには入れづらいやつ
----------

* [retryable_find_or_create_by](https://rubygems.org/gems/retryable_find_or_create_by)
 * find_or_create_byのcreateがUNIQUE制約に引っかかったらfindし直す
* [activerecord-suppress_range_error](https://rubygems.org/gems/activerecord-suppress_range_error)
 * コアから取り除きたい機能ナンバーワンだけど数年は無理そう

最近仕事で踏んだやつ
----------

<a href="https://twitter.com/kamipo/status/794925282658422784">
<img src="img/tw_kamipo_order.png" width="70%" alt="最近仕事で踏んだやつ">
</a>

Future work on Active Record
----------

* 可能な限りすべてのクエリのプレースホルダ対応
 * [#26612 Make association queries to preparable](https://github.com/rails/rails/pull/26612)

Future work on Active Record
----------

* Rails 3 から Rails 4 で100倍遅くなった has_many を170倍速くする
 * [#25970 Calling has_many association ~100x slower in Rails 4 vs Rails 3 (with associations preloaded)](https://github.com/rails/rails/issues/25970)
 * [#25877 Delegate to `scope` rather than `merge!` for collection proxy](https://github.com/rails/rails/pull/25877)

まとめ
----------

* Active Recordの困ってるところをひたすら直してきた話をした
* 困ってる人がいる問題が直るのではなく直す気のある人がいる問題が直る

That's all🍻
----------

