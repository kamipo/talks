SQLがむずかしくて生きるのがつらい
==========

2013/04/17<br/>
MySQL Casual Talks Vol.4<br/>
<address>
[@kamipo](http://twitter.com/kamipo)<br/>
[github/kamipo](https://github.com/kamipo)
</address>


むずかしいと思うところ
----------

ウェブアプリケーションの場合

むずかしいと思うところ
----------

当初の見込みに反してサービスが大ヒットしたり
長いこと運用することになったサービスだと
データがめっちゃ増えてて検索の絞り込みが効かなくなっていく(遅くなっていく)
　
----------

 - そもそもインデックスから引いても絞り込めない
  - MySQL内部では絞り込みにLIMITはほぼ使われない
 - これまで設計でカバーしてきた
  - mailbox方式
  - recent db
  - 時系列sharding

生きるのがつらいところ
----------

- 絞り込む方法が物理的に存在しないわけではない
- SQLで絞り込めないのはSQLの制約

こういうふうにしていた
----------

MySQLのHandler APIを直に叩くUDFを書けば
SQLではうまく絞り込めなくて無駄な行に触ってしまう検索を
可能な限り必要な行だけ取ってくることでSQLとくらべて
数倍から数十倍効率よく検索できてうれしい。

こういうふうにしていた
----------

UDFでHandler APIを叩いて複数行取ってきても
普通のやり方ではUDFは1つの値しか返せない。
kazuhoさん方式ではUDF内でテンポラリテーブルにINSERTしていて
pixiv方式ではJSONにシリアライズして1つの値として返して
アプリケーション側でデシリアライズしていた。


複数行取ってきてるんだからそのまま複数行で返したい
----------

ストレージエンジンを書けば複数行返せる
----------

mruby_storage_engine
----------

ストレージエンジンの処理をmrubyで書けるやつを作った<br/>
[https://github.com/kamipo/mruby_storage_engine](https://github.com/kamipo/mruby_storage_engine)


ストレージエンジンの処理を書けてうれしいこと
----------

 - SQLにはできない絞り込みかたができたり
 - 隣接リストモデルのツリーを再帰的に辿れたり
 - JSONのkey/valueをカラムに展開しながら検索できたり

example (JSON parse)
----------
    CREATE TABLE t (
      user_id int,
      status_id int,
      data varchar(255)
    ) ENGINE=MRUBY
      COMMENT=' # mruby handler
      def json_get(json, key)
        json = JSON.parse(json)
        json[key]
      end
      ';

さいごに
----------

SQLを勉強しましょう
----------

おしまい
----------

