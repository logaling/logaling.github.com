---
layout: default
title: チュートリアル
---
 
# チュートリアル

| [logalimacsについて](/logalimacs/about.html) | [コマンド](/logalimacs/commands.html) | [インストール方法](/logalimacs/install.html) | [新機能](/logalimacs/whatsnew.html) | [github](https://github.com/logaling/logalimacs) |

インストールから英単語を検索する方法を紹介します。

---

* logalimacsをインストールします。

Emacs23と24でインストール方法が違うので、
詳しくは[インストール方法](/logalimacs/install.html)を参照してください。

* 英和,和英辞書のインポート

ターミナルから以下のコマンドを入力し辞書をインポートします:


gene95の辞書をインポートする

---

    % loga import gene95

edictの辞書をインポートする

---

    % loga import edict

---

* カーソル上の英単語を検索するには

例えば、英語のドキュメントがありcatという単語があり意味を調べたいとします。
調べたい単語の上で"M-x loga-lookup-for-popup"と入力するか、
インストールの項であったglobal-set-keyで自分で
loga-lookup-for-popupを登録したキーバインドを
入力すると、カーソル上の単語が辞書から検索されpopupで表示されます。

---

![カーソルでpopup](/logalimacs/images/popupCursor.png)

---

* 自分で検索する英単語を入力するには

例えば、テレビで見た英単語の意味を調べる等、コピペではなく
どうしても、自分で検索する用語を入力したい時は、
"M-x loga-lookup-region-or-manually"と入力するか、
インストールの項であったglobal-set-keyで自分で、
loga-lookup-region-or-manuallyを登録したキーバインドを
入力するとミニバッファに調べたい単語を聞かれるので、
あなたが調べたい単語を入力して、入力が終わったらRETを押すと
logalingで検索され結果が\*logalimacs\*バッファに表示されます。

* リージョンで選択した単語or文章を検索するには

例えば、以下の英語ドキュメントで辞書から  
"go up in smoke"のイディオムを検索したい場合、
go up in smokeの部分をリージョン選択します。"M-x set-mark-command"を入力するか
"C-SPC"または、"C-@"(Emacsのデフォルトのキーバインドを変更していなければ)で、
リージョンの開始をマークできます。カーソルを動かして、
リージョンの範囲指定をしたら、そこで(この場合はgo up in smoke)、
"M-x loga-lookup-region-or-manually"または"M-x loga-lookup-fof-popup"または、 
上のコマンドを登録したキーバインドを入力します。
お気づきかもしれませんが、これらのコマンドはリージョン選択時はリージョンを検索、
非選択時は、カーソル位置や入力を求めるという動作になっています。

---

    I make up documentation, however if you not read it, go up in smoke.
    Of course, you are looked this will have nothing to do with.
    Thank you for reading :)

---


![リージョンでpopup](/logalimacs/images/popupRegion.png)

