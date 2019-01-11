# 自己紹介
- 村上大和
    - twitter
        - @pipopotamasu3
    - github
        - pipopotamasu
- フロントエンドエンジニア
- メドピアという会社にいます
- 野球が趣味

---

# TypeScript入れたい！
## 会社の環境にTypeScript入れたい！
- 以下が目的
    - 型による品質アップ
    - サジェスト出して開発効率あげる
    - 可読性の向上
---
## しかし、ある程度の学習コストがいる
---
## サーバーサイドエンジニアが片手間にフロントエンドも実装するような会社に最初からしっかりしたTypeScriptを書くことを強いるのは難しい
---
## まずは導入してみて、TypeScriptの良さをわかってもらう・こんなもんなんだとわかってもらうのが第一
---
## 上記のためにはいかにJavaScriptからの移行コストがかからない設定をしたい
---
## あえてゆるゆるなTypeScriptの設定にする
---
## まずはTypeScriptを慣れてもらうことから
---
## 「ちゃんと書かないTypeScript」
---

# ちゃんと書かないTypeScript設定
- compiler optionsに着目して、JavaScriptからTypeScriptへの移行コストがかからない形を模索していく
---
## allowJs
- JSをコンパイル対象に含めるか
- 今回の「TypeScriptを使ってみて欲しい」という趣旨から外れるので、falseにしておく

`allowJS: false`
---
## checkJs
- JSファイルのエラーをチェックする
- 今回JSは使って欲しくないのでfalse

`checkJs: false`
---

## noImplicitAny
- 暗黙のany型を許容しない
- 型定義ファイルのないライブラリの使用などおそらく一番ハマると思うのでdisabledにする

`noImplicitAny: false`
---

## noImplicitReturns
- 関数内のすべての経路で、返り値の型があっているかをチェック
- このくらいは型システムを強制したい。そんなに難しくないのでtrue

`noImplicitAny: true`
---
## noImplicitThis
- thisに型を指定していない場合エラーを出す
- 迷う...
- 一旦きつめにして、ダメだったら緩めるか...


`noImplicitThis: true`
---
## noStrictGenericChecks
- function typesでのGenericsの厳格なチェックをしない
- おそらくGenerics使わないのでfalseにしておいても問題ない

`noStrictGenericChecks: false`
---
## strict
- 以下4つの集まり
    - strictBindCallApply
        - bind, call, applyの厳格なチェックをする
            - おそらくbindもcallもapplyも普段の開発で使わないのでtrueでも大丈夫
    - strictFunctionTypes
        - こんなやつらしい
            - https://qiita.com/vvakame/items/d2c7cf142fa0af39d2d5#%E9%96%A2%E6%95%B0%E3%81%AE%E5%BC%95%E6%95%B0%E3%81%AE%E5%9E%8B%E3%81%AB%E3%81%A4%E3%81%84%E3%81%A6%E3%81%AE%E3%83%81%E3%82%A7%E3%83%83%E3%82%AF%E3%82%92%E5%BC%B7%E5%8C%96
        - おそらくこんなことは普段の開発ではほぼやらないと思うのでtrueでもいいや
    - strictPropertyInitialization
        - 定義済みのクラスのプロパティをコンストラクターで必ず初期化する
        - これはtruenしておきたい
    - strictNullChecks
        - 厳格なNullチェック
        - こんなの
            - https://qiita.com/IganinTea/items/f88bea469bff56cfbda6#--strictnullchecks
        - trueにしたい
- 結論、trueでいく

`strict: true`
---
# 終わりに
- compiler options膨大すぎて全て網羅できてないです、すみません汗
- 会社でTypeScript入れたことある人いたらこの辺の知見教えてください
