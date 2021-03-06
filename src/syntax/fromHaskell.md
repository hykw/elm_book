#コラム：Haskellから見たElm

ElmはF#やOCamlやHaskellを参考にしています。

ElmとHaskellはどのように違うか、ということを考えてみました。（あまりHaskell知らないので間違っているかもしれません。。）

###Elmは正格評価です。

遅延評価を行うには専用のモジュールを使います。

###Elmには型クラスがない。

実装される話もあるようですが、あまり乗り気ではないそうです。

型クラス等がほしいならpuresciptとか、scala-js、ghc-js、typescriptとかを検討したほうがいいかもしれないです。

##main（エントリポイント）の型の違い

Elmはmainの型がSvg型、Html型、Program型となっており、ブラウザアプリケーションを生成する言語です。GUI用のDSLのような言語です。
Elmではアプリケーションアーキテクチャも規定しています。（The Elm Architecture）。view関数、update関数、初期化関数、Msg型というのを用意してブラウザアプリケーションをつくります。


###表記の違い

・Elmでの型の明記はコロンで、リストの表現は `::` になります。Haskellとは逆になっています。

以下はElmです。

```elm
answer : Int
answer = 42

1 :: [2,3,4]
```

・where構文が無いです。let式はあります。

・関数を並べるタイプのパターンマッチ構文はありません。case式を使います。

・ListのheadとtailがMaybeを返します。

```elm
head : List a -> Maybe a
tail : List a -> Maybe (List a)
```

メモ:組み込みのモジュールには必要最低限の関数があります、それ以上の便利な関数が必要ならElm Packagesに公開されている「○○-extra」という名前の拡張ライブラリを使います。

・do構文がありません。

・replに関してです。Elmのreplでは改行で型が出て、letとか書かなくても変数が定義できます。

・ElmのCoreライブラリに実装されている`andThen`関数は型の順番が以下のようになっています。

例：Task型のandThen

```elm
andThen : (a -> Task x b) -> Task x a -> Task x b
```

Elmではやりたい処理をパイプ演算子(|>)を使って左から右に表現するのが一般的になっています。
その時に使いやすいような引数の順番になっています。

```elm
tasks =
    task
        |> aresite
        |> koresite
        |> Task.andThen (\n -> Task.succeed (n + 5))
```

###型表記の違い。

列挙型、代数的データ型の定義を`type`で行い、  
型の別名付け、レコード型の定義を`type alias`で行います

```elm
type Bool = True | False   -- Elmでは | がある型をUnion Typeと読んでいる。
type Data = Data Int Int   -- 直積型
type alias Name = String   -- type aliasで型に別名をつけることが出来る。
```

###レコード表記の違い。

Elmのレコード型はオブジェクトにすこし近いです。(構造的部分型？)

```elm
type alias Position = { x : Int , y : Int}  
type alias User = {name : String}             

position : Position
position = {x = 10, y = 10}  

position2 : Position
position2 = Position 5 5   ---レコード表記しないでも型を作ることが出来ます。
```


こういう型が作れます。

```elm
type alias Position a = { a | x : Int , y : Int}

```

レコードの中の`a|`と言うのは、ここに型が入ることが出来るということです。
この構文を使うとレコード型を部分的に定義できます。

```elm
type alias Position a = { a | x : Int , y : Int}

type alias Chara = Position { name :String }

chara : Chara
chara = {x=0,y=0,name = "piyo"}                --３つのプロパティ

getPosition : Position a -> Int                --
getPosition {x} = x                            --パターンマッチ

getX : {a| x:Int } -> Int                      --xプロパティのある型を指定
getX target = target.x

```

レコード型を更新する構文があります。

```elm
position = {x = 0, y = 0}
{ position | x = position.x + 1}　  -- 結果は {x = 1 , y = 0} になります。 x = x + 1 みたいなものです。
```

###余談：Elmの特別な型

Elmでは、number、 comparable、 appendableという多相な型を内部で使っていて、組み込みの関数はその型を使っています。

numberは`Int`と`Float`  
comparableは `String`, `Char`,`Int`, `Float`, `Time`,`taple`  
appendable は`List` `String`  

```elm
(+) : number -> number -> number
(<) : comparable -> comparable -> Bool
(++) : appendable -> appendable -> appendable
```

```elm
> [1,2,3] ++ [4,5,6]
[1,2,3,4,5,6] : List number
> "hello" ++ "world"
"helloworld" : String
```
