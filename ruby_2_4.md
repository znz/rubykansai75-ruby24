# Ruby 2.4.0 の主な非互換

author
:   Kazuhiro NISHIYAMA

date
:   2016-11-05

allotted-time
:   30m

# 自己紹介

* 西山和広
* id:znz (github, twitter など)
* CRuby コミッター
* 最近はドキュメント周りに力を入れている

# Ruby リファレンスマニュアル (1/2)

* https://docs.ruby-lang.org/ja/
  生成されたドキュメント
* https://github.com/rurema
  * https://github.com/rurema/doctree
    ドキュメント原文
  * https://github.com/rurema/bitclust
    システム

# Ruby リファレンスマニュアル (2/2)

* 略称はる*り*ま
* 近況は
  Ruby Reference Manual 2016 Autumn
  http://rubykaigi.org/2016/presentations/okkez.html
  を参照

# Rubyist Magazine

* http://magazine.rubyist.net/
  『Rubyist Magazine』、略して『るびま』は、日本 Ruby の会の有志による Rubyist の Rubyist による、Rubyist とそうでない人のためのウェブ雑誌です。

# Ruby 2.4.0 とは?

* 毎年クリスマスに出ている最新安定版の次のリリース

* 2013-12-26 2.1.0
* 2014-12-25 2.2.0
* 2015-12-25 2.3.0

from https://gist.github.com/unak/3038095

# Integer 統合 (1/5)

* Fixnum クラスと Bignum クラスが Integer クラスに統合された
* 詳細は
  http://rubykaigi.org/2016/presentations/tanaka_akr.html
  Unifying Fixnum and Bignum into Integer

# Integer 統合 (2/5)

* 利点
  * ドキュメントが簡素化
  * 学習や説明しやすくなる
* 欠点
  * 非互換

# Integer 統合 (3/5)

* Fixnum と Bignum は内部実装の都合
  * 小さい数値は (`VALUE` に埋め込むというのが) Fixnum
  * 同様に (`VALUE` に浮動小数点数を埋め込む) Flonum というものが ruby 2.0.0 から入っているが Float クラスに統合されている

# Integer 統合 (4/5)

* 参照すると警告: `warning: constant ::Fixnum is deprecated`
* 対応が必要なら `0.class == Integer` で分岐

# Integer 統合 (5/5)

* Ruby 2.4.0 で導入予定の Integer Unification まとめ
  https://www.hsbt.org/diary/20160829.html#p01
  参照
* 非互換対策の例: json gem はバージョン指定せず default gem を使いましょう

# default gem と bundled gem

* 詳細は以下を参照
  * bundled gem と default gem の違い
    http://blog.n-z.jp/blog/2016-09-10-bundled-gem-and-default-gem.html
  * bundled gem と default gem の違いの具体例
    http://blog.n-z.jp/blog/2016-09-13-bundled-gem-and-default-gem-more.html

# bundled gem

* 普通の gem
* ruby のリリースに一緒に入っていて一緒にインストールされるというだけ
* 不要なら普通にアンインストールも可能

# default gem

* 特殊な gem
* ruby の標準添付ライブラリと同じところに入る
* アンインストールできない
* gem なので bundler の Gemfile で別バージョンを指定すれば、そちらが優先される

# gem 化が進んだ

* xmlrpc が bundled gem になった
* openssl が default gem になった
* Ruby/Tk が外れた
  * 必要なら別途 gem install tk でインストール

# upcase などの Unicode 対応 (1/4)

* String/Symbol#upcase/downcase/swapcase/capitalize(!) が Unicode 対応
* 詳細は
  Ups and Downs of Ruby Internationalization
  http://rubykaigi.org/2016/presentations/duerst.html

# upcase などの Unicode 対応 (2/4)

* US-ASCII の範囲外の文字にも対応
* 日本語圏に最も影響がありそうなのは全角英字

```ruby
"aａ".upcase #=> "AＡ"
"aａ".upcase(:ascii) #=> "Aａ"
```

# upcase などの Unicode 対応 (3/4)

* 従来の挙動は `upcase(:ascii)` のように `:ascii` 引数をつける
* 2.4 未満と 2.4 以降の両方で同じ挙動をする簡単な書き方はない
  * 必要なら `"ａ".upcase == "Ａ"` (全角英字の変換) などでチェックして分岐

# upcase などの Unicode 対応 (4/4)

* swapcase.swapcase で戻らなかったり長さが変わったり

```ruby
"\u{df}" #=> "ß"
"\u{df}".swapcase #=> "SS"
"\u{df}".swapcase.swapcase #=> "ss"
```

# その他メソッドの追加など

- NEWS
- サンプルコードでわかる！Ruby 2.4の新機能と変更点
  http://qiita.com/jnchito/items/9f9d45581816f121af07

などを参照
