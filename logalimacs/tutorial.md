---
layout: logalimacs_default
title: チュートリアル
---

# チュートリアル


インストールから英単語を検索する方法を紹介します。

## logalimacsをインストールします。

Emacs23と24でインストール方法が違うので、
詳しくは[インストール方法](/logalimacs/install.html)を参照してください。

## 英和,和英辞書のインポート

ターミナルから以下のコマンドを入力し辞書をインポートします。

### gene95の辞書をインポートする


    % loga import gene95

### edictの辞書をインポートする


    % loga import edict


## カーソル上の英単語を検索するには

例えば、英語のドキュメントがありrubyという単語があり意味を調べたいとします。
調べたい単語の上で`M-x loga-lookup-in-popup`と入力するか、インストールの項であったglobal-set-keyで自分でloga-lookup-in-popupを登録したキーバインドを入力すると、カーソル上の単語が辞書から検索されpopupで表示されます。
(画像のRubyの部分は私が単語を登録したので、表示されているだけです。
gene95とedictをインポートしただけでは通常表示されません。)


![カーソルでpopup](/logalimacs/images/popupCursor.png)


## 自分で検索する英単語を入力するには

### 方法1

例えば、テレビで見た英単語の意味を調べる等、コピペではなくどうしても、自分で検索する用語を入力したい時は、`M-x loga-lookup-at-manually`と入力するか、インストールの項であったglobal-set-keyで自分で登録したキーバインドを入力するとミニバッファに調べたい単語を聞かれるので、あなたが調べたい単語を入力して、入力が終わったらRETを押すとlogalingで検索され結果が\*logalimacs\*バッファに表示されます。  


![自分で入力して検索](/logalimacs/images/outputBuffer.png)

### 方法2

もう一つの検索方法として、loga-lookup-in-popupでも自分で入力して検索できます。
このコマンドは通常は、カーソル位置で検索しますがC-uのuniversal-argumentコマンドを利用する事で、挙動を変更し、自分で入力して検索する様にできます。

## リージョンで選択した単語or文章を検索するには

例えば、以下の英語ドキュメントで辞書から


    I make up documentation, however if you not read it, go up in smoke.
    Of course, you are looked this will have nothing to do with.
    Thank you for reading :)


"go up in smoke"のイディオムを検索したい場合、go up in smokeの部分をリージョン選択します。`M-x set-mark-command`を入力するか`C-SPC`または、`C-@`(Emacsのデフォルトのキーバインドを変更していなければ)で、リージョンの開始をマークできます。
カーソルを動かして、リージョンの範囲指定をしたら、そこで(この場合はgo up in smoke)、`M-x loga-lookup-at-manually`または`M-x loga-lookup-in-popup`または、上のコマンドを登録したキーバインドを入力します。
お気づきかもしれませんが、これらのコマンドはリージョン選択時はリージョンを検索、非選択時は、カーソル位置や入力を求めるという動作になっています。


![リージョンでpopup](/logalimacs/images/popupRegion.png)
