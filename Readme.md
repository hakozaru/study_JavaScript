# JavaScript(以下JS)
- ES6 = ES2015 とする(呼び方多すぎ)

## 変数と定数の定義
- 基本的に `const` で定義するようにし、どうしても必要な時だけ `let` を使うと事故を減らせる
- `var` はES6で書けるのであればもう使わなくて良い
  - `let`
    - 変数を宣言する。宣言できるのは一度だけ
    - 宣言時に初期化を行わなくてもエラーにはならない(中身はundefinedとなる)
    - 値の再代入は可能
  - `const`
    - 定数を宣言する。宣言と同時に必ず初期化する必要がある
    - 値の再代入は不可能
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
var user = {}

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

```js
// 関数宣言
function func1(a, b) {
  console.log(a, b)
}

// 呼び出し
func1(1, 9) //=> 1 9

// 関数の変数への代入
var f1 = function() {
  console.log("あああ")
}

f1() //=> あああ

// パラメータ(引数)として使用
function func2(func) {
  func()
}

func2(f1) //=> あああ
```

- 通常のオブジェクトと同じ振る舞いが可能なので、メンバを動的に操作可能

```js
var f3 = function() {
  // 特になし
}

f3.tes //=> undefined

f3.tes = 12345

f3.tes //=> 12345
```

- 関数は () で処理を呼び出すことができ、 () をつけずに呼び出すと定義の内容を参照することができる

```js
var f4 = function() {
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

var aaa = new Test(1, 2)
aaa //=> Test {a: 1, b: 2}
aaa.a //=> 1
aaa.b //=> 2

var bbb = Test()
bbb //=> undefined
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

## 無名関数
- JSでは関数名が必要なければ省略が可能
- 複数回呼び出すことのない関数を定義する際に利用することが多い

```js
function 〇〇() { console.log(111) } //=> この〇〇がない関数
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

## 巻き上げ
- 関数内のどの位置でも var を使って変数宣言ができるが、これらの変数宣言は関数内のいかなる場所で宣言されたとしても「関数の先頭で宣言された」とみなされること

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

## 即時関数
- 関数を定義すると同時に実行可能な関数のことを即時関数と呼ぶ

```js
(function() {
  console.log(123)
}())
//=> 123
```

## this
- JSの this は使用状況によって参照先が変化する「危険物」
- 使用する際は誰が見ても誤解のないように、何を参照しているのかコメントで明記するのが望ましい
- また、 this は常に特定のものを指すよう「束縛」することが可能なので、可能であれば束縛を上手く利用したいところ

## thisの束縛(applyとcall)
- 関数には apply と call と言うメソッドが用意されている
- それぞれ関数内で参照する this を束縛するためのメソッド
- this は呼び出した時の状況によって参照先が変化するが、これらのメソッドを使用することにより、常に指定したものを this として動作させることができる

```js
var val = 1

function func11(a, b) {
  console.log(this.val + a + b)
}

func11(3, 6) //=> 10 (this.val はグローバル変数のvalを参照)

var val2 = { val: 5 }

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

var bind1 = func12.bind({val: 1})
var bind2 = func12.bind({val: 5}, 5)
var bind3 = func12.bind({val: 2}, 6, 9)

bind1(1, 2) //=> 4 (1 + 1 + 2)
bind2(3) //=> 13 (5 + 5 + 3)
bind3() //=> 17 (2 + 6 + 9)
```

- 上記の例からもわかる通り、束縛する引数の数は自由に設定が可能

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

var u1 = new User("ユーザー1", 24)
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

var usr1 = new User(123, 99) //=> User {name: 123, age: 99, getName: ƒ}
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

var usr = new User("ggg", 78) //=> User {name: "ggg", age: 78}
usr.getName() //=> "ggg"
```

- usr 自体は getName() をもっていないが、prototype を通して定義されたメソッドが参照できている

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
var a = 123
var b = "aiu"
var c = true

// このような使用は通常避けるべき
var a2 = new Number(123)
var b2 = new String("aiu")
var c2 = new Boolean(true)
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
var obj = { a: 1, b: 2, c: 3 }

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
var r
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

## グローバル変数
- JSでは var を使わずに変数を宣言するとグローバルな変数として宣言されてしまうが、極力グローバル変数などは作らないことが推奨されている
  - グローバルが汚染され将来的に変数名が衝突する可能性があるため
- そこで代表的な代替案として「名前空間」を使用する方法と、「クロージャ」を使用する方法がある

## 名前空間
- JSには他言語のように仕組みが存在するわけではなく、オブジェクトを利用してそれらと同等の仕組みを実現している

```js
var Hakozaru = {} //=> グローバル変数を定義

Hakozaru.tes = "test!"
Hakozaru.tes //=> "test!"

Hakozaru.getHako = function() {
  console.log("箱")
}

Hakozaru.getHako() //=> 箱
```

- 上記例でグローバル変数を一つだけ定義しているが、このグローバル変数の名前を会社名やプロジェクト名などにすれば、ほぼ確実に変数の衝突を回避することが可能

## クロージャ
- tmp

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

## Strictモード
- tmp

## typeof 演算子
- 対象のデータ型を調べる演算子
- JSは動的にデータ型が変化するため、型に応じた処理をしたい場合はこの演算子を使う
- 一部不思議な結果が返される
- 結論からいうと「文字列、数値、真偽値のプリミティブ値に対してのみ使用すべき」

```js
var a = "abc"
var b = 123
var c = true
var d = new String("abc")

typeof a //=> "string"
typeof b //=> "number"
typeof c //=> "boolean"
typeof d //=> "object" (NumberやBooleanでも同じ)
```

- null は object として判定されるので注意が必要

```js
var t = null
var z = undefined

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
var a = {}
var b = []
var c = function() {}

a instanceof Object //=> true
b instanceof Array //=> true
c instanceof Function //=> true
```

- 全てのオブジェクトは Object から派生しているため、 オブジェクト instanceof Object は必ず true となる
  - instanceof演算子はオブジェクトのプロトタイプチェーンを辿ってコンストラクタのチェックを行うため
- 自作のコンストラクタも判定可能

```js
var hoge = function() {}
var aaa = new hoge()

aaa instanceof hoge //=> true
```

## constructor
- コンストラクタから生成されたオブジェクトは constructor に生成に使用したコンストラクタ関数を保持している
  - ちなみに、この constructor はオブジェクトが参照しているコンストラクタ関数のプロトタイププロパティの値として存在する

```js
function Tes() {}

var tes = new Tes

tes.__proto__ //=> {constructor: ƒ}
Tes.prototype //=> {constructor: ƒ}

tes.__proto__ === Tes.prototype //=> true

tes.constructor //=> ƒ Tes(){}
Tes.prototype.constructor //=> ƒ Tes(){}

// 以上から、 tes.constructor は Tes.prototype.constructor を参照していることがわかる
```

- instanceof と異なり、コンストラクタの種類が判定できるため「出所不明」なオブジェクトの詳細を調べる際に使える

```js
var a = []
var b = {}
var c = function() {}

a.constructor //=> ƒ Array() { [native code] }
b.constructor //=> ƒ Object() { [native code] }
c.constructor //=> ƒ Function() { [native code] }

// 自作のコンストラクタ
var Myf1 = function() {}
var aaa = new Myf1
aaa.constructor //=> ƒ () {} (無名関数なので名前が表示されない)

// 名前が欲しければコンストラクタに名前をつけておく
var Myf1 = function myfunc() {}
var aaa = new Myf1
aaa.constructor //=> ƒ myfunc() {}
```

## map(連想配列)
- 以前は連想配列(ハッシュ)はオブジェクトで表現するのが当たり前だったが、ES2015より map がサポートされたことにより map を使用するのが一般的になった

```js
var m = new Map()

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
- ES2015でクラスが正式にサポートされた
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

var user = new User("たなかたろう")
user.name //=> "たなかたろう"
user.getName() //=> "たなかたろう"
User.getClassName() //=> "User"

// 継承は extends で行う
class User2 extends User {
  insMethod() {
    return "User2のインスタンスメソッド"
  }
}

var user2 = new User2("123")
user2.name //=> "123"
user2.getName() //=> "123"
User2.getClassName() //=> User
user2.insMethod() //=> "User2のインスタンスメソッド"
```
