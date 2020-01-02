---
title: TypeScriptを使い始めた
---

Ruby で書いてみた NES エミュレータを、Web ブラウザで動かせるよう別言語に移植するにあたって、これまでほぼ使う機会の無かった TypeScript を使ってみることにしました。

## 公式ドキュメント

新しいソフトウェアを利用するときは、まず公式のドキュメントを読むのが基本だと思うので、まずは https://www.typescriptlang.org/ の内容を読みました。

導入としての簡単なサンプルと、既存の JavaScript のプロジェクトからの移行方法、React や Webpack との連携方法などのチュートリアル、それから過去のバージョンからの大まかな変更履歴や、細かい言語仕様、ディレクトリ構成例、コンパイラオプションについての説明などが書かれています。まあ結構長いんですが、後から振り返ると、これらを一通りすべて読んでおいたことによって、実際にコードを書くときに全く苦労しなかったなという感想があります。

## Prettier と TSLint を導入

公式のドキュメントを読み終え、いざコードを書き始める段階になったところで、コードフォーマッターと Linter を導入しておくことにしました。この手のツールは、その言語での多数派寄りの意見を元にしたデフォルト設定が提供されていることが多いので、新しい言語を学習する際に、その言語圏での慣習や文化を知るためにも重宝します。

TypeScript を利用する場合、幾つかのツールの選択肢がありますが、今回は以下のツール群を利用してみることにしました。

- prettier
- tslint
- tslint-config-prettier
- tslint-plugin-prettier

開発中は prettier を利用して整形しながらコードを書いていって、CI では tslint でルールに違反しているコードがないかチェックしていくという感じです。tslint-config-prettier は、prettier を利用するプロジェクトにおける tslint 用の適切なデフォルト設定を提供してくれるパッケージで、tslint-plugin-prettier は、tslint を実行したときに prettier で整形し忘れている箇所が無いかも検査してくれるようにするプラグインです。

## CircleCI を設定

CI 用のサービスは色々ありますが、今回は CircleCI を利用することにしました。CI では少なくとも、以下の3点を確認したいと考えました。

- TSLint の違反がないか
- Prettier での整形し忘れがないか
- TypeScript のコンパイルが成功するか

1つ目と2つ目は前述したように tslint で、3つ目は tsc (TypeScript 付属のコンパイラツール) でそれぞれ確認できるので、CI 上では tslint → tsc の順に実行することにしました。この手のものは基本的に実行時間が短い（結果が出るまでが早い）順で実行していった方が良いだろうという考えでこの順にしましたが、あまり深く考えていないので、違う順序でも良いかもしれません。

余談ですが、Circle CI 2.0 がまだβ版だった頃は Docker イメージを利用すると（キャッシュの復帰なども含めた）準備時間が結構長く掛かっていたため、実行環境のミドルウェアのバージョンの差異などをあまり気にしないプロジェクトでは Docker ではなく VM マシン上で動かしていたこともありましたが、今回試しに Docker で動かしてみたところ、問題無いぐらい素早くビルドが実行されるようになっていて嬉しくなりました。

## 実際のコード例

実際には以下のリポジトリで作業を進めています。

https://github.com/r7kamura/nes8

今から TypeScript やるぞという人には、tslint.json と package.json と .circleci/config.yml あたりが参考になるかもしれません。tsconfig.json は tsc --init で生成したものに少し手を加えただけなので、あまり情報量は無いと思います。

各 CPU 命令やアドレッシングモードの種類を Union Type で表現したり、それらを利用するときの switch 文の網羅性を never 型で検査したり、type alias でコードの意図の伝わりやすさを改善したり、const enum をレジスタのビットフィールドへのアクセスに利用したりと、楽しく NES エミュレータのコードを書いていますが、この辺についてはまた TypeScript 版の実装が一通り動くようになったときにでも書こうと思います。

## 余談

今回初めて TypeScript を書き始めるにあたって、設定ファイルを生成するために tsc --init というコマンドを利用したんですが、このとき生成される tsconfig.json の末尾に改行が付いていないことに少し不満を感じたので、TypeScript に Pull Request を出してみました。

https://github.com/Microsoft/TypeScript/pull/28772