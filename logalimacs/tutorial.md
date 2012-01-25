---
layout: default
title: logalimacsで何ができるの?
---

# logalimacsで何ができるの?

---
## 英語のドキュメントを見ている時に...
英語のドキュメントを見ている時に、カーソル位置より手前の単語を調べられます。  
ツールチップで表示したり(loga-lookup-for-popup)、  
リージョンの単語を検索するか、ミニバッファから任意の単語を入力し  
logalimacs用のバッファに表示できます。(loga-lookup-in-hand-or-region)

---
## 一つのキーバインドで複数の機能
logalimacsは、[logaling-command](/about.html)のフロントエンドなので、  
logaling-commandが利用できるコマンドはEmacs経由で実行できます。  
ターミナルでlogaling-commandを利用する場合、  
"% loga [行いたいタスク] [任意の引数].."  
となりますがlogalimacsでは、  
_M-x loga-interactive-command_とタイプするか、  
これを割り当てたキーバインドを入力すると対話的に、  
logalingコマンドを呼び出す事が出来ます。  
これを行うとミニバッファ上に利用できるコマンドが出るので、  
その頭文字をタイプする事で、そのコマンドを利用できます。

---
## logalimacsだと少ない入力で済みます。
良く使うであろう検索と単語の追加、更新については、  
ターミナル上では、英語で複数の単語をスペース区切りで登録するのに、  
クオートする必要がありますがlogalimacsの場合は、  
元の単語、翻訳された単語、注釈(任意)の順にミニバッファにリクエストが出るので、  
この点を気にする必要はありません。(loga addの場合)
