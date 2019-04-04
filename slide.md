## 表参道.rb #45
- - -

## Ruby にコミットしよう

---

#### 自己紹介
- - -

* なまえ  : おしょー
* Twitter : [@pink_bangbi](https://twitter.com/pink_bangbi)
* github  : [osyo-manga](https://github.com/osyo-manga)
* ブログ  : [Secret Garden(Instrumental)](http://secret-garden.hatenablog.com)
* Rails 歴 1年の初心者
* 4ヶ月ぐらいずーっと ActiveRecord を読んでる
* スコープ、アソシエーションなにもわからない
* 開発は Vim
  * わしの vimrc は 5000行以上あるぞ

---

#### 進捗
- - -

* Ruby 2.6.0
  * 取り込まれたパッチ : 3
  * 取り込まれなかったパッチ : 5
  * 報告・修正したバグ : 2
  * 埋め込んだバグ : 1
* Ruby 2.6.3
  * 報告・修正したバグ : 1
* Ruby 2.7.0
  * 取り込まれたパッチ : 1
  * 容認されたパッチ : 1

---

#### 投げたパッチ
- - -

* Refinements の緩和案
  * [`#public_send`](https://bugs.ruby-lang.org/issues/15326)
  * [`#respond_to?`](https://bugs.ruby-lang.org/issues/15327)
  * [`#method`, `#instance_method`](https://bugs.ruby-lang.org/issues/15373)
  * [ブロック引数で `&:hoge` 渡した場合](https://bugs.ruby-lang.org/issues/15114)
* `#===` の追加(議案中)
  * [`Array#===`](https://bugs.ruby-lang.org/issues/14916), [`Hash#===`](https://bugs.ruby-lang.org/issues/14869)
* `{ id, name, age }` 記法の提案(却下済み)
  * `{ id: id, name: name, age: age }` を `{ id, name, age }` で
  * [`#expand(:id, :name, age)`](https://bugs.ruby-lang.org/issues/15286), [`%h(id name age)`](https://bugs.ruby-lang.org/issues/14973)
* [`Time#floor`](https://bugs.ruby-lang.org/issues/15653)

---

### 今日話すこと
- - -

## Ruby にコミットしよう

---

#### Ruby に新しい機能を提案するまでの流れ
- - -


1. Ruby に必要か考える                               <!-- .element: class="fragment" -->
1. パッチを書く                               <!-- .element: class="fragment" -->
1. bugs.ruby に issues を立てる                               <!-- .element: class="fragment" -->
1. 開発者会議の議題で取り上げてもらう                               <!-- .element: class="fragment" -->
1. matz がいいよーって言えばマージされる                               <!-- .element: class="fragment" -->
    * 問題があれば修正する
    * だめって言われたら却下される


---

#### 実例（Ruby 2.5 時点での挙動）
- - -

```ruby
module Ex
  refine String do
    def twice
      self + self
    end
  end
end
using Ex

p "homu".send(:twice) # OK
p "homu".public_send(:twice) # NG
```

* `#send` では Refinements が有効になるが `#public_send` では有効ならない
* `#public_send` でも有効にしたい

---

#### 1. Ruby に必要か考える
- - -

* ユースケースを考える                        <!-- .element: class="fragment" -->
  * より具体的なケースを提示する
  * → `#send` よりも `#public_send` の方が安全なので使う機会は多い
* 言語デザインとして一貫性がある                        <!-- .element: class="fragment" -->か
  * 他の機能と比べて整合性が取れているか
  * → `#send` で動作して `#public_send` で動作しないのは一貫性がない
* その機能、本当に Ruby に必要？                        <!-- .element: class="fragment" -->
  * gem ではダメ?
  * → 今回は本体に手を入れないと難しい…

---

#### 2. パッチを書く
- - -

* Ruby 本体に対して実装する                        <!-- .element: class="fragment" -->
  * Ruby は C言語で書かれているので C言語の知識が必要
  * [Ruby Hack Challenge](https://github.com/ko1/rubyhackchallenge) の資料が参考になる
* パッチを書くのが難しければまずは提案からでも大丈夫               <!-- .element: class="fragment" -->
  * 新しい構文とかはだいたい議論が発生するので先に提案するのもあり
* [今回書いたパッチ](https://github.com/ruby/ruby/pull/2019/files)               <!-- .element: class="fragment" -->

---

#### 3. bugs.ruby に issues を立てる
- - -

* bugs.ruby に issues を立てる                        <!-- .element: class="fragment" -->
  * https://bugs.ruby-lang.org/projects/ruby-trunk/issues
  * 英語でも日本語でも OK
  * [報告先] を選ぶ必要があるので注意                    <!-- .element: class="fragment" -->
* パッチファイルを添付するか github に pull request を投げて、その URL を貼る                    <!-- .element: class="fragment" -->
  * github に pull request を投げると CI が走るので便利
* [今回立てた issues](https://bugs.ruby-lang.org/issues/15326)                    <!-- .element: class="fragment" -->
  * [日本語の場合](https://bugs.ruby-lang.org/issues/15653)                 <!-- .element: class="fragment" -->

---

#### 4. 開発者会議の議題で取り上げてもらう
- - -

* 毎月コミッタの人たちが集まって会議している                   <!-- .element: class="fragment" -->
  * 提案された内容などについての議論を行っている
  * この会議で議論してもらえるように [DevelopersMeeting の issues](https://bugs.ruby-lang.org/projects/ruby-trunk/issues?query_id=156) に追加する必要がある
  * [今月の開発者会議](https://bugs.ruby-lang.org/issues/15546)
* この会議で問題ないと matz が判断すればマージされる                   <!-- .element: class="fragment" -->
  * 問題があれば議論が続いたり却下されたり…


---

### おまけ1 : Ruby をハックしたいなら
- - -

* メーリングリストに登録する
  * bugs.ruby の情報が流れてくるので最近の動向がわかる
* [Ruby Hack Challenge](https://rhc.connpass.com/event/125655/) に参加する
  * Ruby の開発を行うもくもく会
  * 次は 5月11日に開催
  * RubyKaigi 反省会をする

---


### おまけ2：最近の新機能・議論
- - -

* [`#method` のシンタックスシュガー](https://bugs.ruby-lang.org/issues/13581)
  * `obj.method(:hoge)` が `obj.:hoge`
* [`@1` 記法(numbered parameters)](https://bugs.ruby-lang.org/issues/4475)
  * `(1..10).map { @1 * 2 }` \
  * `(1..10).inject { @1 + @2 }` 
  * すでに trunk には入っているが議論は続いている
  * https://bugs.ruby-lang.org/issues/15723
* 先端無限Range
  * `(-Float::INFINITY .. 1)` が `( .. 1)`

---


### おまけ3：`Time#ciel` のユースケース募集




- - -

---

## まとめ

* Ruby の開発プロセスについて簡単に説明しました
* メーリングリストを見てると面白い議論が流れてくる
* Ruby を Hack したいなら Ruby Hack Challenge に参加してみよう
* Time#floor みたいに「ほしい！」と思ったメソッドを提案してみると案外すんなり採用されるかも  


---

## ご清聴
## ありがとうございました



https://bugs.ruby-lang.org/issues/15723
https://bugs.ruby-lang.org/issues/14799
