** 頑張らないTypeScript **

---

## 自己紹介
- 村上大和
    - twitter
        - [@pipopotamasu3](https://twitter.com/pipopotamasu3)
    - github
        - [pipopotamasu](https://github.com/pipopotamasu)
- フロントエンドエンジニア
- メドピアという会社にいます
- 野球が趣味

---

** 会社の環境にTypeScript入れたい！ **
- 最近フロントのロジックをゴリゴリ書くようなものが増えてきている
- JSだけだと厳しい:tired_face:
- TypeScript入れたい
---
** しかし、ある程度の学習コストがいる **
---
** サーバーサイドエンジニアが片手間にフロントエンドも実装するような会社に、最初からしっかりしたTypeScriptを書くことを強いるのは難しい **
---
** まずは導入してみて、TypeScriptの良さをわかってもらう・こんなもんなんだとわかってもらうのが第一　**
---
** このためにはいかにJavaScriptからの移行コストがかからない設定をしたい　**
---
** あえてゆるゆるなTypeScriptの設定にする **
---
** まずはTypeScriptを慣れてもらうことから **
---
** 「ちゃんと書かないTypeScript」**
---

** ちゃんと書かないTypeScript設定 **
- compiler optionsに着目して、JavaScriptからTypeScriptへの移行コストがかからない形を模索していく
- TypeScriptビギナーなので間違っているところがあったら指摘していただけるとありがたいです(-人-)

---

** @size[1.6em](allowJs) **
- JSをコンパイル対象に含めるか
- TypeScriptからの逃げ道を用意するためtrue
- 逃げることは悪じゃない

`allowJS: true`

---
** @size[1.6em](checkJs) **
- JSファイルのエラーをチェックする
- JS許すのでtrue

`checkJs: true`
---

** @size[1.6em](noImplicitAny) **
- 暗黙のany型を許容しない
- 例えば引数とかは何かしらの型宣言が必要になる

```
function(arg) { // Parameter 'arg' implicitly has an 'any' type.
  ~~~~~~~~~~~
}
```

+++
- 引数の宣言以外に何かあるっけ?
- 引数だけなら型宣言書いて欲しいのでtrue

`noImplicitAny: true`
---

** @size[1.6em](noImplicitReturns) **
- 関数内のすべての経路で、返り値の型があっているかをチェック

```
function(arg: string) { // Not all code paths return a value.
   if (arg === 'hoge') {
     return 'return value';
   }
   // implicitly returns `undefined`
}
```
+++
- このくらいは型は強制したい。そんなに難しくないのでtrue

`noImplicitReturns: true`
---
** @size[1.6em](noImplicitThis) **
- thisに型を指定していない場合エラーを出す

```
class Person {
  name = 'Tom';
}

function sayName() { // 明示的に引数にthis: Personと指定する必要あり
  console.log(this.name); // 'this' implicitly has type 'any' because it does not have a type annotation.
}

sayName.bind(new Person())();
```

+++

- bind / call / applyのようにthisのコンテキスト変えるようなことをしなければ大丈夫?
- 凝ったbind / call / apply使えるくらいならtrueでも大丈夫そう

`noImplicitThis: true`
---
** @size[1.6em](noStrictGenericChecks) **
- function typesでのGenericsの厳格なチェックをしない

```
type A = <T, U>(x: T, y: U) => [T, U];
type B = <S>(x: S, y: S) => [S, S];

function f(a: A, b: B) {
    a = b;  // Error
    b = a;  // Ok
}
```

+++
- やべー意味わからん
- 引き渡す型の数が関係するのか?
- 誰か知っている人がいたら教えてください([このオプションがマージされた時のPR](https://github.com/Microsoft/TypeScript/pull/16368))
- 理解できないでenabledするのは危険なのでtrueにしておく

`noStrictGenericChecks: true`
---
** @size[1.6em](strict) **
- 以下4つの集まり
    - strictBindCallApply
        - bind, call, applyの厳格なチェックをする
+++

```
function foo(a: number, b: string): string {
    return a + b;
}

let a = foo.apply(undefined, [10]);              // error: too few argumnts
let b = foo.apply(undefined, [10, 20]);          // error: 2nd argument is a number
let c = foo.apply(undefined, [10, "hello", 30]); // error: too many arguments
let d = foo.apply(undefined, [10, "hello"]);     // okay! returns a string
```

- おそらくbindやcallやapplyを使う人はJSに習熟している人で、TypeScriptもすぐ覚えられそうなのでtrue

+++

- strictFunctionTypes
    - [こんなやつ](https://qiita.com/vvakame/items/d2c7cf142fa0af39d2d5#%E9%96%A2%E6%95%B0%E3%81%AE%E5%BC%95%E6%95%B0%E3%81%AE%E5%9E%8B%E3%81%AB%E3%81%A4%E3%81%84%E3%81%A6%E3%81%AE%E3%83%81%E3%82%A7%E3%83%83%E3%82%AF%E3%82%92%E5%BC%B7%E5%8C%96)らしい
        - おそらくこんなことは普段の開発ではほぼやらないと思うのでtrueでもいいや
+++
- strictPropertyInitialization
    - 定義済みのクラスのプロパティをコンストラクターで必ず初期化する
    - これはtrueしておきたい
- strictNullChecks
    - 厳格なNullチェック

+++

```
let x: number;
let y: number | undefined;
let z: number | null | undefined;
x = 1;  // Ok
y = 1;  // Ok
z = 1;  // Ok
x = undefined;  // Error
y = undefined;  // Ok
z = undefined;  // Ok
x = null;  // Error
y = null;  // Error
z = null;  // Ok
x = y;  // Error
x = z;  // Error
y = x;  // Ok
y = z;  // Error
z = x;  // Ok
z = y;  // Ok

```
- trueにしたい

+++
** 結論、trueでいく **

`strict: true`
---
## 終わりに
- compiler options膨大すぎて全て網羅できてないです、すみません汗
- 結局きつめの型設定でも大丈夫そう？
- 最悪anyで逃げるか...
- 会社でTypeScript入れたことある人いたらこの辺の知見教えてください
