---
layout: logalimacs_default
title: logalimacsで利用できるコマンド
---

# logalimacsで利用できるコマンド

## loga-interactive-command
対話的に、logalimacsを実行します。
一つのキーで、logaling-commandを利用したい時に便利です。


実行すると、以下の様にミニバッファに利用可能なタスクが表示されると思います。

    types prefix of feature that want you :
    a)dd,c)onfig,d)elete,h)elp,i)mport,l)ookup,n)ew,r)egister,U)nregister,u)pdate,v)ersion,f)ly-mode

上のメッセージで、a)ddから始まる文字が利用可能なタスクです。
もしadd(単語の追加)をしたい場合は、キーボードで_a_をタイプします。
すると、\*logalimacs\*バッファが開き、へルプメッセージが表示されます。(lookup以外)
そのメッセージを参考にミニバッファに内容を打ち込みます。
(logaling-commandをラップしているものは、全て処理前にヘルプメッセージが表示されます。)

この例で打ち込む内容は、loga-add-wordの項を参照してください。
他のタスクも頭文字を指定して同様に、選択可能です。

## loga-add
ターミナルからの"% loga add SOURCE TARGET NOTE"をラップするコマンドです。
ターミナル上では、英語で複数の単語をスペース区切りで登録するのに、クオートする必要がありますがlogalimacsでこのコマンドを利用する場合は、必要ありません。
コマンド実行後、source(元の単語)、target(翻訳後の単語)、note(任意の注釈)を個別に聞いてきます。

## loga-update
ターミナルからの "% loga update SOURCE TARGET(old) TARGET(new) NOTE(optional)"をラップするコマンドです。
ターミナル上では、英語で複数の単語をスペース区切りで登録するのに、クオートする必要がありますがlogalimacsでこのコマンドを利用する場合は、必要ありません。
コマンド実行後、SOURCE(元の単語)、TARGET(変更前の単語)、TARGET(変更後の単語)、NOTE(任意の注釈)を個別に聞いてきます。

## loga-lookup-at-manually
ターミナルからの "% loga lookup TARGET"をラップするコマンドです。
logalimacs独自の機能として、リージョンを選択していた場合、その単語を、検索します。
それ以外の場合、ミニバッファに検索する文字を入力し、RETする事で利用できます。
出力先はbufferになります。

## loga-lookup-in-popup
多階層ポップアップで検索内容を表示します。
C-fキーを押すことで注釈を表示できます。
C-uのuniversal-argumentを押してから実行すると挙動が変わります。
尚、表示幅を超える検索結果は表示しません。
全ての検索結果を表示したい場合loga-lookup-in-bufferを利用するか
ポップアップ表示後"d"を押してください

### C-u無し:
カーソル位置の文字※を検索対象にしてlogalingでlookup検索します。
入力後popupで検索結果を表示します。

### C-u有り:
manualで文字を入力するようminibufferに入力画面が出ます。
入力後popupで検索結果を表示します。
両方ともリージョン選択をしていた場合、リージョン内の文字列で検索します。

### popup表示後に受付けるコマンド:

コマンド | 動作
--------|------
d | bufferに表示を切り替えます。
q | popupの表示を消します。
p | カラムを上に移動します。
k | カラムを上に移動します。
n | カラムを下に移動します。
j | カラムを下に移動します。

※ カーソル位置の検索についての注意点:
ひらがなと漢字を含んだ文字の場合、漢字が名詞の場合が多いので、漢字だけで検索するようにしています。

## loga-lookup-in-buffer
loga-lookup-in-popupの
出力先がbufferに変わったもので違いとしては、
buffer表示後に受付けるコマンドが違います。

### buffer表示後に受付けるコマンド:

コマンド | 動作
--------|------
d | bufferを削除します。
q | bufferを削除します。
p | bufferを上に画面移動します。
k | bufferを上に画面移動します。
n | bufferを下に画面移動します。
j | bufferを下に画面移動します。

<!-- ## loga-fly-mode -->
<!-- logalimacs独自の機能で、カーソル位置にある単語を`loga lookup (検索)`します。 -->
<!-- (空白では、その位置より左側の単語になります。) -->
<!-- この機能は、通常はoffで`loga-interactive-command`から実行するか、`M-x loga-fly-mode`または、任意のキーバインドで実行する事で、onとoffをトグルできます。 -->

<!-- ## loga-get-flymake-error -->
<!-- 実行するとflymakeでのエラーをlogalimacsバッファに出力します。 -->
