# JavaScript(以下JS)
- ES6 = ES2015 とする(呼び方多すぎ)

## 変数と定数の定義
- 基本的に `const` で定義するようにし、どうしても必要な時だけ `let` を使うと事故を減らせる
- `var` はES6で書けるのであればもう使わなくて良い
  - `let`
    - 変数を宣言する。宣言できるのは一度だけ
    - 宣言時に初期化を行わなくてもエラーにはならない(中身はundefinedとなる)
    - 値の再代入は可能だが、再宣言はエラーになる
  - `const`
    - 定数を宣言する。宣言と同時に必ず初期化する必要がある
    - 値の再代入も再宣言も不可能
    - 定数名は大文字とアンダースコアを組み合わせた名前を使うことが多い模様

## 識別子
- 変数名、定数名として使われる文字列に関して以下の規則がある
  - 一文字目はUnicode文字、$、アンダースコアでなければならない
  - 二文字目以降には↑に加えて数字も使える
- JSでは `testUser` などのようなキャメルケースで定義することが多い模様
- ちなみにES6からは日本語も変数として使えるようになっている

```js
const たかし = "やまだ"
たかし // => やまだ
```

## テンプレートリテラル
- ES6から追加された、文字列の中に変数を埋め込むことができるリテラル(これマジで欲しかった)

```js
const NAME = "メアリー"
console.log(`私の名前は${NAME}です`) // => 私の名前はメアリーです
```

## シンボル
- ES6から使えるようになった
- `Symbol()` コンストラクタから作成し、作成されたシンボルは必ずユニークとなる
- 必ずユニークとなるので、オブジェクトプロパティの名前として使うと、バッティングを回避できる
- 必要に応じて説明のための文字列を渡すことも可能

```js
const obj = {}
const HOGE = Symbol()

obj[HOGE] = 123
obj.HOGE = 999
obj // => {HOGE: 999, Symbol(): 123}
// ぱっと見HOGEは同じに見えるが、
// obj[HOGE] はシンボルをキーとした値を返し(ここでは123)
// obj.HOGE (または obj["HOGE"]) は文字列の "HOGE" をキーとした値を返す(ここでは999)
// 分かりやすい変数名にしろという話ですが...
```

## MapとSet、WeakMapとWeakSet
- ES6から使えるようになったデータ型
- Map
  - オブジェクトと同じくキーと値が対応しているもの
  - 状況によってはオブジェクトよりも便利な場合もある
- Set
  - 配列に似ているが重複が不可能
- WeakMap, WeakSet
  - ほぼ同じような働きをする
  - 一部の機能が使えない代わりに特定の状況では優れたパフォーマンスを発揮する

## オブジェクト
- プロバティ(メンバーとも呼ばれる)とメソッドで構成される
- オブジェクトの状態や特性を表すのがプロパティで、名前(キーとも呼ばれる)と値で構成される
- オブジェクトのプロパティとして格納されているのがメソッド

```js
const user = {}

user.name = "ユーザー" //=> 「name」を名前、「ユーザー」を値に持つプロパティ
user.age = 30 //=> 「age」を名前、「30」を値に持つプロパティ

user.getInfo = function() { //=> メソッド
  return user.name + "" + user.age
}
```

## new 演算子
- new 演算子は「受け取った引数で初期化したオブジェクトのインスタンスを返しなさい、とコンストラクタに命令する」演算子
- new 演算子を使ってインスタンス化した場合、関数内の this はそのインスタンス自身を指す
  - この仕様のおかげでクラスがなくてもオブジェクト指向っぽい記述が可能になっている

## 関数
- JSの関数は通常のオブジェクトと同じ振る舞いが可能(第1級オブジェクト、ファーストクラス関数などと呼ばれるらしい)
- 変数や配列に代入ができ、パラメータ(引数)として利用することも可能
- `function hoge() {` から `}` までが関数の「宣言」で、 `hoge()` で関数を「呼び出す」
- 関数から何か戻り値を返す場合は `return` で指定する。 指定しなかった場合は `undefined` が戻り値となる
- 関数名に `()` をつけて呼ぶと関数の呼び出しとなり、 `()` をつけないで呼び出すと関数を参照するだけで実行はされない

```js
// 関数宣言
function func1(a, b) {
  console.log(a, b)
}

// 呼び出し
func1(1, 9) //=> 1 9

// 関数の変数への代入
const f1 = function() {
  console.log("あああ")
}

f1() //=> あああ

// パラメータ(引数)として使用
function func2(func) {
  func()
}

func2(f1) //=> あああ
```

- 通常のオブジェクトと同じ振る舞いが可能なので、メンバを動的に操作可能(非推奨)

```js
const f3 = function() {
  // 特になし
}

f3.tes //=> undefined

f3.tes = 12345

f3.tes //=> 12345
```

- 関数は () で処理を呼び出すことができ、 () をつけずに呼び出すと定義の内容を参照することができる

```js
const f4 = function() {
  console.log("かんすう")
}

f4() //=> かんすう
f4
//=>
  // ƒ () {
  //   console.log("かんすう")
  // }
```

- JSの関数は return 文で明示的に指定されていない場合、自動的に呼び出し元には undefined を返却する
- しかし、 new 演算子を用いて呼び出された場合はオブジェクトのインスタンスを返却する
- new 演算子を使って呼び出さなかった場合は、ただの関数呼び出しになる

```js
function Test(a, b) {
  this.a = a
  this.b = b
}

const aaa = new Test(1, 2)
aaa //=> Test {a: 1, b: 2}
aaa.a //=> 1
aaa.b //=> 2

const bbb = Test()
bbb //=> undefined
```

- オブジェクトのプロパティとして指定される関数をメソッドと呼ぶ
- メソッドはES6から省略記法が追加された

```js
const obj = {
  name: "hoge",
  test() { return "Test func!" }
  // old: function() { return "Old ver!" } 以前はこう
}

obj.test() //=> "Test func!"
```

- さらにES6ではアロー関数という表記の仕方も導入された
  - `function`の省略が可能
  - 引数が一つならカッコを省略可能
  - 関数本体が一つの式からなる場合 `{}` と `return` が省略可能
  - などの利点がある
- アロー関数は無名関数で、名前のついた関数を作る場合は今まで通り `function` を使って作成する
- 引数に無名関数を渡したりするときによく使用される
- 通常の関数ともっとも異なる点は `this` の扱われ方で、通常、関数の中で定義された関数の中で `this` を参照すると、 `undefined` かグローバルオブジェクトを参照してしまうが、アロー関数では `this` を一つ上の関数で呼んだときと同じ `this` を参照してくれる(非常にわかりにくい説明なので、下記 `this` も合わせて参照)
- オブジェクトのコンストラクタとして使えなかったり、 `arguments` 変数が使えないなどの違いもある

```js
const obj3 = {
  name: "hoge",
  test() {
    const innerFunc = () => {
      return this.name
    }
    return `${innerFunc()}, huga!`
  }
}

obj3.test() //=> "hoge, huga!"

// もし innerFunc = () => {} が innerFunc = function() {} だった場合
// obj3.test() は ", huga!" となる (this.nameがundefinedなので)
```

## 引数
- JSは引数チェックを一切行わない
- 多く渡しても、少なく渡してもエラーは発生しない
- 足りない分は undefined となり、余った分は何事もないかのように振る舞う

```js
function func5(a, b) {
  console.log(a, b)
}

func5(1) //=> 1 undefined
func5(1, 2, 3) //=> 1 2
```

- 関数に渡された引数は全て arguments というオブジェクトを使い、参照が可能
- arguments は関数内のスコープのみで利用できる

```js
function func6(a, b) {
  console.log(arguments)
}

func6(1, 2, 3, 4, 5, 6) //=> (5) [1, 2, 3, 4, 5, callee: ƒ, Symbol(Symbol.iterator): ƒ]

function func7(a, b) {
  console.log(arguments[0])
}

func7(5, 8, 1) //=> 5
```

- arguments は配列っぽい動作をするが配列とは別物なので注意
- ES6からスプレッド演算子(展開演算子)が導入されたのでそちらを使った方が扱いに悩む必要がないのでおすすめ

```js
function hoge(...aaa) {
  console.log(aaa)
}

hoge(1,2,3,4,5) //=> (5) [1, 2, 3, 4, 5]

function hoge(...aaa) {
  console.log(...aaa)
}

hoge(1,2,3,4,5) //=> 1 2 3 4 5
```

## 無名関数
- JSでは関数名が必要なければ省略が可能
- 複数回呼び出すことのない関数を定義する際に利用することが多い

```js
const func = function() { console.log(111) }
```

## コールバック
- 引数として渡される関数のことをコールバック(コールバック関数)と言う
- 引数として渡すことで関数内の任意のタイミングで発動することが可能
- 代表的な使用例として非同期処理などがある

```js
function func8(callback) {
  console.log("適当な処理")
  callback()
}

func8(function() {
  console.log("引数として渡される関数の処理")
})
//=> 適当な処理
//   引数として渡される関数の処理
```

## 巻き上げ(var変数)
- 関数内のどの位置でも `var` を使って変数宣言ができるが、これらの変数宣言は関数内のいかなる場所で宣言されたとしても「関数の先頭で宣言された」とみなされること
- あくまで巻き上げられるのは宣言のみで、値の代入は巻き上げられない
- これに対して `let` は宣言をするまでは存在しない(宣言前にアクセスしようとするとエラーとなる)

```js
var aaa = "tes"

var f9 = function() {
  console.log(aaa)
}

f9() //=> tes


var f10 = function() {
  console.log(aaa)
  var aaa = "tes2"
  console.log(aaa)
}

f10() //=> undefined
//         tes2

// 関数 f10 の内部は下記の記述と同じ
var f10 = function() {
  var aaa
  console.log(aaa)
  aaa = "tes2"
  console.log(aaa)
}
```

## 巻き上げ(関数)
- 関数もスコープの先頭に巻き上げられる

```js
f() //=> "hoge"
function f() { return "hoge" }
```

- しかし、変数に代入された関数は巻き上げられない

```js
f2() //=> Uncaught ReferenceError: f2 is not defined
let f2 = function() { return "hoge" }
```

## 即時関数(IIFE)
- 関数を定義すると同時に実行可能な関数のことを即時関数と呼ぶ

```js
(function() {
  console.log(123)
})();
//=> 123
```

## this
- 関数本体ではthisという参照のみが可能(値の代入はできない)な特別な値を利用することができる
- JSの this は使用状況によって参照先が変化する「危険物」
- 使用する際は誰が見ても誤解のないように、何を参照しているのかコメントで明記するのが望ましい
- また、 this は常に特定のものを指すよう「束縛」することが可能なので、可能であれば束縛を上手く利用したいところ
- thisは「関数の呼び出され方」に依存している。「関数の宣言された場所」には依存しない

```js
const obj = {
  name: "たなか",
  speak() { return `私の名前は${this.name}です` }
}

obj.speak() //=> 私の名前はたなかです

const func1 = obj.speak

func1 === obj.speak //=> true (同じ関数を参照している)

func1()
"私の名前はです" //=> thisはundefined

// func1内のthisはグローバルオブジェクトを参照している(chromeの場合)
window.name = "グローバル"
func1() //=> 私の名前はグローバルです

// ちゃんとオブジェクトに束縛させればOK
const obj2 = {
    name: "さとう"
}

obj2.speak = obj.speak

obj2.speak() //=> "私の名前はさとうです"
```

## thisの束縛(applyとcall)
- 関数には `apply` と `call` と言うメソッドが用意されている
- それぞれ関数内で参照する `this` を束縛するためのメソッド
- this は呼び出した時の状況によって参照先が変化するが、これらのメソッドを使用することにより、常に指定したものを `this` として動作させることができる

```js
const val = 1

function func11(a, b) {
  console.log(this.val + a + b)
}

func11(3, 6) //=> 10 (this.val はグローバル変数のvalを参照)

const val2 = { val: 5 }

func11.apply(val2, [6, 1]) //=> 12 (this.val は val2 の val を参照している)
func11.call(val2, 6, 1) //=> 12
```

- apply と call の違いは、引数を配列で渡すか、カンマ区切りで渡すかだけで結果に違いはない

## thisの束縛(bind)
- bind は this を束縛した関数オブジェクトを生成する

```js
function func12(a, b) {
  console.log(this.val + a + b)
}

const bind1 = func12.bind({val: 1})
const bind2 = func12.bind({val: 5}, 5)
const bind3 = func12.bind({val: 2}, 6, 9)

bind1(1, 2) //=> 4 (1 + 1 + 2)
bind2(3) //=> 13 (5 + 5 + 3)
bind3() //=> 17 (2 + 6 + 9)
```

- 上記の例からもわかる通り、束縛する引数の数は自由に設定が可能
- ただし、bindは永続的な束縛を行うため、見つけるのが困難なバグの原因となりかねないので注意が必要

## コンストラクタ
- コンストラクタはオブジェクトのインスタンス化の際に呼び出されるメソッド
  - Ruby でいうところの initialize メソッドに近い
- JSでは function で擬似的なクラスを作ることができ、 new 演算子でインスタンスを作ることができる
- 要するに class 内では constructor の部分で、関数であれば関数 = コンストラクタとなる

```js
function User(name, age) { //=> コンストラクタ
  this.name = name
  this.age = age
}

const u1 = new User("ユーザー1", 24)
u1 //=> User {name: "ユーザー1", age: 24}
```

- 上記例では User というコンストラクタを定義し、その中でnameとageを初期化している
- また、コンストラクタ内ではメソッドも定義可能

```js
function User(name, age) { //=> コンストラクタ
  this.name = name
  this.age = age
  this.getName = function() {
    return this.name
  }
}

const usr1 = new User(123, 99) //=> User {name: 123, age: 99, getName: ƒ}
usr1.getName() //=> "123"
```

- しかし、これだと User のインスタンスを作成するごとに、インスタンスに .getName() がコピーされるため、メモリを大量に消費する可能性がある
- そこでJSでは prototype というものを使ってメソッドを追加することができる

```js
function User(name, age) { //=> コンストラクタ
  this.name = name
  this.age = age
}

User.prototype.getName = function() {
  return this.name
}

const usr = new User("ggg", 78) //=> User {name: "ggg", age: 78}
usr.getName() //=> "ggg"
```

- usr 自体は getName() をもっていないが、prototype を通して定義されたメソッドが参照できている

## constructor
- コンストラクタから生成されたオブジェクトは constructor に生成に使用したコンストラクタ関数を保持している
  - ちなみに、この constructor はオブジェクトが参照しているコンストラクタ関数のプロトタイププロパティの値として存在する

```js
function Tes() {}

const tes = new Tes

tes.__proto__ //=> {constructor: ƒ}
Tes.prototype //=> {constructor: ƒ}

tes.__proto__ === Tes.prototype //=> true

tes.constructor //=> ƒ Tes(){}
Tes.prototype.constructor //=> ƒ Tes(){}

// 以上から、 tes.constructor は Tes.prototype.constructor を参照していることがわかる
```

- instanceof と異なり、コンストラクタの種類が判定できるため「出所不明」なオブジェクトの詳細を調べる際に使える

```js
const a = []
const b = {}
const c = function() {}

a.constructor //=> ƒ Array() { [native code] }
b.constructor //=> ƒ Object() { [native code] }
c.constructor //=> ƒ Function() { [native code] }

// 自作のコンストラクタ
const Myf1 = function() {}
const aaa = new Myf1
aaa.constructor //=> ƒ () {} (無名関数なので名前が表示されない)

// 名前が欲しければコンストラクタに名前をつけておく
const Myf1 = function myfunc() {}
const aaa = new Myf1
aaa.constructor //=> ƒ myfunc() {}
```

## データ型
- JSのデータ型は大きく分けて「基本型(プリミティブ型)」と「参照型(オブジェクト型)」に分類される
- 基本型と参照型は「値そのものを扱うのか、値の格納されているアドレスを扱うのか」が異なる

## プリミティブ型
- JSには以下の5つのプリミティブ型が存在する
  - Boolean
  - Number
  - String
  - null
  - undefined
  - Symbol <- new! ES6から！
- プリミティブ型とは、 true, 999, "テスト" などの「値そのもの」を扱うためのデータ型
- それぞれコンストラクタから「オブジェクト」として生成することも可能だが、ほぼ無意味に等しいので使う必要はない

```js
// 通常はこう使う
const a = 123
const b = "aiu"
const c = true

// このような使用は通常避けるべき
const a2 = new Number(123)
const b2 = new String("aiu")
const c2 = new Boolean(true)
```

- なぜそれぞれオブジェクトが存在しているのか？
- 上記a, b, cは「ただの値」であるプリミティブ型の変数だが、実は「オブジェクトであるかのようにメソッドを呼び出すこと」が可能

```js
a //=> 123
a.toString() //=> "123"
```

- JSではプリミティブ型の変数が「オブジェクトのように扱われる」と、一時的にオブジェクトに変換が行われる
- 上記例では .toString() がオブジェクトのように扱う行為なので、裏で自動的にオブジェクト変換が行われている
- このように裏で自動的に生成されるオブジェクトは「ラッパーオブジェクト」と呼ばれる
- ラッパーオブジェクトは必要に応じて生成され、不要になったら破棄される
- なお、 null と undefined にはラッパーオブジェクトは存在しないため、常にプリミティブな存在

## 参照型(オブジェクト型)
- 主に以下の種類がある
  - 配列を扱うための array
  - オブジェクトを扱うための object
  - 関数を扱うための function

## リテラル
- データ型に格納できる値そのもの、または値の表現方法のことを「リテラル」と呼ぶ
- 数値リテラル
  - 整数リテラルと浮動小数点リテラルに分類される

```js
100 //=> 整数(10進数)リテラル。
060 //=> 整数(8進数)リテラル。 先頭に0をつける。
0xCC55BB //=> 整数(16進数)リテラル。先頭に0xをつける。
1.1 //=> 浮動小点数リテラル。
```

- 文字列リテラル
  - シングルクォートまたはダブルクォートで値を囲んだもの
  - "aiueo" など
- 配列リテラル
  - カンマで区切った値をブラケット([])で囲った形式
  - [1, 2, 3] など
- オブジェクトリテラル
  - 名前をキーにしてアクセスができるもの(ハッシュや連想配列とも呼ばれる -> 最近は map を使うので呼ばない)
  - ドットまたはブラケットでアクセスが可能

```js
const obj = { a: 1, b: 2, c: 3 }

obj.a //=> 1
obj["b"] //=> 2
```

- 関数リテラル
  - 関数もデータ型の一種

## null と undefined
- 「null」 は 「nullを設定した状態」
- 「undefined」は「未定義、何も設定していない状態」
  - 要するに「nullは設定しない限り存在しない」
  - 定義した変数に対して何もしなければ undefined で、nullを代入すると null となる
- 変数が null または undefined であるかの確認は、単純な同値比較( === )を使えばよい

```js
let r
if (r === undefined) { console.log(123) }
//=> 123

r = null
if (r === null) { console.log(123) }
//=> 123
```

## if文で false と判定される条件
- 以下の5つがif文でfalseと判定される
  - undefined
  - null
  - 0
  - 空文字("")
  - false
  - NaN

## グローバル変数
- JSでは var を使わずに変数を宣言するとグローバルな変数として宣言されてしまうが、極力グローバル変数などは作らないことが推奨されている
  - グローバルが汚染され将来的に変数名が衝突する可能性があるため
- そこで代表的な代替案として「名前空間」を使用する方法と、「クロージャ」を使用する方法がある

## 名前空間
- JSには他言語のように仕組みが存在するわけではなく、オブジェクトを利用してそれらと同等の仕組みを実現している

```js
const Hakozaru = {} //=> グローバル変数を定義

Hakozaru.tes = "test!"
Hakozaru.tes //=> "test!"

Hakozaru.getHako = function() {
  console.log("箱")
}

Hakozaru.getHako() //=> 箱
```

- 上記例でグローバル変数を一つだけ定義しているが、このグローバル変数の名前を会社名やプロジェクト名などにすれば、ほぼ確実に変数の衝突を回避することが可能

## グローバルオブジェクトは変更しない
- JSでは既存のグローバルオブジェクトに対して操作が行える
- Rubyのように既存の String などに対しても独自のメソッドが追加できてしまう
- Rubyでも同じだが、余計な混乱を招かないためにもグローバルオブジェクトは触らないようにすることが望ましい

## 例外処理(try〜catch)
- JSの例外処理は非常にオーバーヘッドが大きい処理なので、むやみやたらに使用するのは危険
- 使用の際は「本当に例外処理をしないといけない処理なのか」を考えて使用すること
- なお、意図的に例外を発生させる場合は throw を使用する

```js
try {
  // 例外が発生する可能性がある処理
} catch(例外を受け取る変数) {
  // 例外が発生した場合に実行する処理
} finally {
  // 最後に実行される処理
}

throw new Error("message") //=> Uncaught Error: message at <anonymous>:1:7
```

## typeof 演算子
- 対象のデータ型を調べる演算子
- JSは動的にデータ型が変化するため、型に応じた処理をしたい場合はこの演算子を使う
- 一部不思議な結果が返される
- 結論からいうと「文字列、数値、真偽値のプリミティブ値に対してのみ使用すべき」

```js
const a = "abc"
const b = 123
const c = true
const d = new String("abc")
const e = []

typeof a //=> "string"
typeof b //=> "number"
typeof c //=> "boolean"
typeof d //=> "object" (NumberやBooleanでも同じ)
typeof e //=> "object" (配列もobjectが返る)
```

- null は object として判定されるので注意が必要

```js
const t = null
const z = undefined

typeof t //=> "object"
typeof z //=> "undefined"
```

- オブジェクトに対して使用した場合は、関数オブジェクトのみ function が返される

```js
typeof [] //=> "object"
typeof {} //=> "object"
typeof function() {} //=> "object"
```

## instanceof 演算子
- オブジェクトが何のコンストラクタから生成されたかを判定する

```js
const a = {}
const b = []
const c = function() {}

a instanceof Object //=> true
b instanceof Array //=> true
c instanceof Function //=> true
```

- 全てのオブジェクトは Object から派生しているため、 オブジェクト instanceof Object は必ず true となる
  - instanceof演算子はオブジェクトのプロトタイプチェーンを辿ってコンストラクタのチェックを行うため
- 自作のコンストラクタも判定可能

```js
const hoge = function() {}
const aaa = new hoge()

aaa instanceof hoge //=> true
```

## 三項演算子
- if ~ else に相当する式

```js
const result = true ? "true!" : "false"
console.log(result) // => "true!"
```

## map(連想配列)
- 以前は連想配列(ハッシュ)はオブジェクトで表現するのが当たり前だったが、ES6より map がサポートされたことにより map を使用するのが一般的になった

```js
const m = new Map()

m.set("key", "value") //=> Map(1) {"key" => "value"}
m.get("key") //=> "value"

m.set(1, 2) //=> Map(2) {"key" => "value", 1 => 2}

m.size //=> 2

m.keys() //=> MapIterator {"key", 1}
m.values() //=> MapIterator {"value", 2}
m.entries() //=> MapIterator {"key" => "value", 1 => 3}
m.has("key") //=> true
```

## クラス
- ES6でクラスが正式にサポートされた
- コンストラクタ内で、 this を介して追加した変数はメンバ変数となる(hoge.nameなどで呼び出し可能)
- extends で継承が可能
- クラスメソッドは static で定義する

```js
class User {
  constructor(name) {
    this.name = name
  }

  // インスタンスメソッド
  getName() {
    return this.name
  }

  // クラスメソッド
  static getClassName() {
    return "User"
  }
}

const user = new User("たなかたろう")
user.name //=> "たなかたろう"
user.getName() //=> "たなかたろう"
User.getClassName() //=> "User"

// 継承は extends で行う
class User2 extends User {
  insMethod() {
    return "User2のインスタンスメソッド"
  }
}

const user2 = new User2("123")
user2.name //=> "123"
user2.getName() //=> "123"
User2.getClassName() //=> User
user2.insMethod() //=> "User2のインスタンスメソッド"
```

## 分割代入(デストラクチャリング)
- ES6から分割代入が行えるようになった

```js
const obj = { a: 123, b: 456, c: 789, z: 999 }
const { a, b, c, d } = obj

a //=> 123
b //=> 456
c //=> 789
d //=> undefined
z //=> ReferenceError: z is not defined
```

## スコープ
- 静的スコープ
  - グローバルスコープ
    - グローバルスコープで宣言したものは全てのスコープで利用可能になる
    - その影響範囲から、グローバルスコープに変数や関数などを定義し使用する場合は注意が必要
  - ブロックスコープ
    - `let` と `const` は識別子をブロックスコープで定義する
    - ブロックスコープで定義された識別子はそれを囲むブロックでしか有効にならない
    - 以下はスタンドアロンなブロックを定義した場合の例

    ```js
    {
      const x = 3
      console.log(x) //=> 3
    }
    console.log(x) //=> Uncaught ReferenceError: x is not defined
    ```

    ```js
      let u = 0

      if(true) { // ブロックの始まり
        let u = 555 //=> ifの外で定義している u とは別の新しい変数が定義される(変数のマスキング)
        console.log(u) //=> 555
      } // ブロック終わり

      u //=> 0
    ```

  - 関数スコープ
    - `var` によって宣言された変数は関数スコープを持つ(宣言された関数内であればどこでも有効)
    - この辺りは「巻き上げ」とも関係があるので、そちらも参照

## strictモード
- `var` を使い忘れて宣言された変数などの、暗黙的なグローバルが禁止される厳格なモード
- `"use strict;"` と書くことで厳格モードでコードが実行される
- グローバルスコープに書くとスクリプト全体に適用され、関数内に書けばその関数が `strict` モードとして実行される
- スクリプト全てが厳格モードになると、それはそれで困る場合があるので、自分の書いたコードのみに適用する場合は即時関数などでラップしてしまうのがよさそう

```js
(function() {
  "use strict";

  // 以降全てstrictモードで実行される
  // この関数の外にあるコードには影響しない
})();
```
