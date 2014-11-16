MySQLとActiveRecord
==========

2014/11/10<br/>
Rails複数DB Casual Talks<br/>
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

About me
----------

* クソクエリの話題にクソリプ業
 * [ORDER BY 狙いのキーの話](http://togetter.com/li/564015)
 * [ORDER BY 狙いのキーの話2](http://togetter.com/li/579823)
 * [2014年なんだからCOUNT(*)とかSQL_CALC_FOUND_ROWSとかLIMIT OFFSET以下略](http://togetter.com/li/640847)
* MySQL Casual
 * [ふつうのWeb開発者のためのクエリチューニング](http://kamipo.github.io/talks/20140711-mysqlcasual6/)

MySQL Casual
----------

* 次回のMySQL Casualは12月12日(金)開催です
 * [MySQL Casual Talks vol.7](http://mysql-casual.connpass.com/event/9767/)
* 今年もAdvent Calendarあります
 * [MySQL Casual Advent Calendar 2014](http://qiita.com/advent-calendar/2014/mysql-casual)

今日話す話題
----------

* ActiveRecordのMySQL対応は放置される傾向にある
 * でもMySQL対応がんばりたい
 * (みんなもMySQL使ってますよね？)
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

このへん対応されてほしい
----------

* テーブルオプションをschema.rbにダンプする
 * [#17569](https://github.com/rails/rails/pull/17569)
* カラムにcharsetとcollationを指定できるようにする
 * [#17574](https://github.com/rails/rails/pull/17574)
* utf8mb4以外のunicode対応
 * [#17601](https://github.com/rails/rails/pull/17601)
* primary_keyに任意の型を指定できるようにする
 * [#17631](https://github.com/rails/rails/pull/17631)

ActiveRecordからMySQLを使うときのハマりどころ
----------

* strict_mode
* utf8mb4
* validates :uniquenessのBINARY
* utf8_unicode_ci
* unsigned int

strict_mode
----------

* [ActiveRecordでstrict_modeを無効にする](http://qiita.com/kamipo/items/2ce1d668afbb3d2afd86)
* 接続時に`sql_mode = STRICT_ALL_TABLES`をセットする
 * rails 4.0から導入されデフォルトで有効
* MySQL側での設定を上書きされたくないので無効にしたい
 * database.ymlで`strict: false`を設定すると無効にできた
* rails 4.2.0(beta)で`strict: false`の挙動が変更された
 * `sql_mode = ''`をセットするようになった
 * [#17370](https://github.com/rails/rails/issues/17370)

strict_mode
----------

* rails 4.2.0(beta)でもstrict_modeを無効にする方法

<pre><code># database.yml
production:
  adapter: mysql2
  database: foo_prod
  user: foo
  variables:
    sql_mode: :default
</code></pre>

utf8mb4
----------

* [ActiveRecordをutf8mb4で動かす](http://qiita.com/kamipo/items/101aaf8159cf1470d823)
 * stringのデフォルトを`limit: 191`にする
 * rails 4.1以降を使う
 * innodb_large_prefixを使う

utf8mb4
----------

* stringのデフォルトを`limit: 191`にする

utf8mb4
----------

    module ActiveRecord
      module ConnectionAdapters
        class AbstractMysqlAdapter
          NATIVE_DATABASE_TYPES[:string][:limit] = 191
        end
      end
    end

utf8mb4
----------

* rails 4.1以降を使う
 * [8744632](https://github.com/rails/rails/commit/8744632)でutf8mb4対応したつもりだった
 * `rake db:migrate`ができないissueは上がってたけどutf8mb4対応が不完全なのにたぶん気づいてなかった
 * [#12252](https://github.com/rails/rails/pull/12252)がめっちゃ放置される(つらい)
 * [#12962](https://github.com/rails/rails/pull/12962)でようやくマージされる

utf8mb4
----------

* innodb_large_prefixを使う
* [MySQL(InnoDB) で "Index column size too large. The maximum column size is 767 bytes." いわれるときの対策](http://blog.kamipo.net/entry/2012/11/13/102024)

validates :uniquenessのBINARY
----------

* [Rails でユニーク制約 その２ - @tmtms のメモ](http://tmtms.hatenablog.com/entry/20120608/rails_unique)
* 調べたら2009年からみんな困ってた
 * [validates_uniqness_of + mysql == SLOW](http://grosser.it/2009/12/11/validates_uniqness_of-mysql-slow/)
 * [Case-insensitive validates_uniqueness_of slowness](http://techblog.floorplanner.com/case-insensitive-validates-uniqueness-of-slowness/)
 * [#1399](https://github.com/rails/rails/issues/1399)ではもう直ってるからってcloseしているけどなにも問題は解決されてないままだった
* [#13040](https://github.com/rails/rails/pull/13040)がめっちゃ放置されたけど周りの人たちが働きかけてくれたおかげでマージされる

utf8_unicode_ci
----------

* [ActiveRecordでデフォルトの照合順序を変更する](http://qiita.com/kamipo/items/d7863f0df24916005657)
* 意図せずにutf8_unicode_ciを使うと日本語の検索でハマる
* なぜデフォルトのcollationをutf8_unicode_ciに変えるのか
 * MySQLでなにも意図せずにutf8の文字列型を定義したときcollationはutf8_general_ciになるがそれで困った人はいなかったはずである
 * 問題のコミットは[f6ca7e4](https://github.com/rails/rails/commit/f6ca7e4)だけどそうしなければならない理由はないように見える
 * そうしなければならない理由がない限りデフォルトのcollation(utf8_general_ci)を尊重してほしい

unsigned int
----------

* ActiveRecord本体では未だ対応されず
 * [waka/activerecord-mysql-unsigned](https://github.com/waka/activerecord-mysql-unsigned)を使うとよい
* Lighthouse時代の遺産([Unsigned integers for MySQL #574](https://github.com/rails/rails/issues/574))が残ってるので古来より人々が求めていた機能だったということがうかがえる

PostgreSQL対応は活発
----------

* jsonカラムやarrayオプションサポート
* primary_keyにuuidを指定できる
* primary_keyにbigserialを指定できる
* primary_keyに任意の型を指定できる

MySQL対応もがんばりたい
----------

* [#13040](https://github.com/rails/rails/pull/13040)はめっちゃ放置されたけど周りの人たちが働きかけてくれたおかげでマージされた
* MySQL使ってる人たちはまだまだたくさんいるはずなのでみんなで問題を上げていきましょう

以上
----------

