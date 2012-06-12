---
layout: logalimacs_default
title: logalimacsについて
---

# logalimacsについて

## logalimacsはlogaling-command用のフロントエンド
次のような特徴になります:

### 英語のドキュメントを見ている時に...
英語のドキュメントを見ている時に、カーソル位置より手前の単語を調べられます。
ツールチップで表示したり、リージョンの単語を検索するか、ミニバッファから任意の単語を入力しlogalimacs用のバッファに表示できます。

### 一つのキーバインドで複数の機能
logalimacsは、[logaling-command](/about.html)のフロントエンドなので、logaling-commandが利用できるコマンドはEmacs経由で実行できます。
ターミナルでlogaling-commandを利用する場合、`% loga [行いたいタスク] [任意の引数]..`となりますがlogalimacsでは、`M-x loga-interactive-command`とタイプするか、これを割り当てたキーバインドを入力すると対話的に、logalingコマンドを呼び出す事が出来ます。これを行うとミニバッファ上に利用できるコマンドが出るので、その頭文字をタイプする事で、対応するコマンドを利用できます。

### ターミナルからの入力に比べlogalimacsだと少ない入力で済みます。

良く使うであろう検索と単語の追加、更新については、  ターミナル上では、英語で複数の単語をスペース区切りで登録するのに、クオートする必要がありますがlogalimacsの場合は、元の単語、翻訳された単語、注釈(任意)の順にミニバッファにリクエストが出るので、この点を気にする必要はありません。

※ 詳しい使い方は、[チュートリアル](/logalimacs_tutorial.html) や [コマンド](/logalimacs_commands.html) を参照して下さい。

## githubで開発しています

ハックしてくださる方は、随時募集しています。
またEmacsで翻訳する方は、po-modeを使われると思いますがバッファを3画面以上開くと、編集モードから出た時に画面が元の.poのファイルの位置がずれます。
この不具合?を修正したものが[github](https://github.com/logaling/logalimacs)にありますのでよければ利用して下さい。
またMarmalade用にrakefileを作ったので、パッケージをアップロードしたい方は、よかったら利用して下さい。

## 問題点、要望

logaling-command共通の問題点、要望などはlogalingと同じ[コミュニティ](/contribution.html)からお願いします。
logalimacsに対する問題点、要望は[issues](https://github.com/logaling/logalimacs/issues)にお願いします。

## バージョン情報

Version:1.0.1(2012/6/12) : EmacsリポジトリサイトをMermaladeからMELPAに変更
Version:1.0.0(2012/6/7) : logaling-commandの--dictionaryオプションに対応
Version:0.9.0(2012/2/13) : 初版

## ライセンス
このプラグラムは、フリーソフトウェアです。あなたはこれを、フリーソフトウェア財団によって発行された[GNU GENERAL PUBLIC LICENSE Version 3](www.gnu.org/licenses/gpl-3.0.txt)の定める条件の下で再頒布または、改変する事が出来ます。
このプログラムは有用であることを願って頒布されますが、_全くの無保証_です。
商業可能性の保証や特定の目的への適合性は、言外に示されたものも含め存在しません。
詳しくは、[GNU GENERAL PUBLIC LICENSE Version 3](www.gnu.org/licenses/gpl-3.0.txt)をご覧下さい。
