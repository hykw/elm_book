#Elmを使った開発環境構成

ここではElmを使った開発環境を構成していきます。

* フォーマッタ(elm-format)
* フロントエンドのビルドツール(webpack)

elm-formatでElmソースコードを整形します。
そして、webpackを使ってホットリロードによる開発と、ビルドを行います。

##elm-format

[avh4/elm-format](https://github.com/avh4/elm-format)

elm-formatはElmソースコードをElm公式スタイルガイドのスタイルで整形してくれるツールです。
今回は、エディタでelm-formatを使えるようにして、保存するたびに自動で整形するようにします。

まずelm-formatバイナリをダウンロードして、パスを通します。
MacとLinuxと、

elm-formatのドキュメントに、エディタの対応状況とプラグイン名が載っています。
例えばAtomでは、atom-elm-formatかatom-beautifyとなっています。自分のエディタにあったプラグインをインストールして、elm-formatをセッティングしてください。

##webpack

webpackとは、フロントエンド環境にあるソースコード全般を管理するツールです。

例えば、フロントエンド環境にはHTMLやCSSやJSやElmといったコードがあります。それらはHTMLならミニファイし、SCSSならコンパイル、JSならbabelでトランスパイラし、リンク（一つのファイルにする）して、適切な位置に配置する必要があります。そういった面倒な作業をwebpackが設定ファイルを書くことで、すべてやってくれるというわけです。

自分もGrantなど色々試しました。今のところwebpackを使うのが、一番安定、ドキュメントも多い、概ね簡単、と感じたのでおすすめします。