---
layout: logalimacs_default
title: logalimacsのインストール方法
---

# logalimacsのインストール方法,設定

## 1. logaling-commandをインストールする
logalimacsではlogaling-commandをインストールする必要があります。
もしインストールしていない場合次のリンクを参照してインストールして下さい。


<a href="/"><img src="/logalimacs/images/commonLogalingLink.png" width="550" height="100" alt="logaling-commandのインストールはこちら"></a>


### rvmを利用している場合の設定:
[rvm.el](https://github.com/senny/rvm.el)が必要です。
端末で先に % rvm use 1.9.2 --defaultでデフォルトを1.9系にして、以下のコードを.emacs等に書きます。
(パスを通した所にrvm.elを置くかloadして下さい。わからなければ_6. 雑多な設定_へ)

	(require 'rvm)
	(rvm-use-default)

### logalingのconfig設定をする
登録したい翻訳元の言語と翻訳された後の言語を、
ホームディレクトリの~/.logaling/configに設定します。
以下はmy-dictionaryという用語集名で、翻訳元が英語(en)で翻訳後が日本語(ja)で
設定しています。

    --glossary my-dictionary
    --source-language en
    --target-language ja

詳しくは[こちら](/tutorial.html)をご覧ください

## 2. インストール方法
Emacs23ユーザーの方もpackage.elを利用すれば
M-x list-packagesコマンドからインストールできます。
(Emacs24ユーザーの方はpackage.elは標準添付なのでMELPAのリポジトリ追加を行ってください)

### package.elをインストール
Emacs24ユーザーの方は次のpackage.el用の設定に進んで";;; リポジトリにMELPAを追加"より下を
設定して下さい。
Emacs23ユーザーの方はpackage.elが必要なので先に準備する必要があります
ではpackage.elのダウンロード方法を説明します
以下はターミナルのコマンドでpackage.elを~/.emacs.d/elisp以下にダウンロードします。
注意:Emacs23用のpackage.elのリンクなのでEmacs24で利用した場合不具合がおこる可能性があります。

    $ mkdir -p ~/.emacs.d/package23
    $ cd ~/.emacs.d/package23
    $ wget http://repo.or.cz/w/emacs.git/blob_plain/1a0a666f941c99882093d7bd08ced15033bc3f0c:/lisp/emacs-lisp/package.el

上記を行うと~/.emacs.d/package23/ディレクトリ以下にpackage.elが作成されるはずです。
作成するディレクトリは適宜変更してください

### Emacsにpackage.el用の設定とlogalimacs用の設定を追加
~/.emacs.d/init.elなどに以下の設定を追加します

---

    ;; ~/.emacs.d/package23/以下にロードパスを通します
    ;; Emacs24未満の場合だけEmacs23用のpackage.elを読み込む
    (when (version< emacs-version "24.0.0")
      (add-to-list 'load-path "~/.emacs.d/package23")
      (require 'package))
    ;;; リポジトリにMELPAを追加
    (add-to-list 'package-archives ("melpa" . "http://melpa.milkbox.net/packages/"))
    お好み設定:
    ;;; package.elの設定
    ;; インストールするディレクトリを指定
    (setq package-user-dir "インストールされるディレクトリ")
    ;; インストールしたパッケージにロードパスを通してロードする
    (package-initialize)
    ;;; logalimacsの設定
    ;; keybinds
    (global-set-key (kbd "M-g M-i") 'loga-interactive-command)
    (global-set-key (kbd "M-g M-l") 'loga-lookup-at-manually)
    (global-set-key (kbd "M-g M-a") 'loga-add)
    (global-set-key (kbd "C-:") 'loga-lookup-in-popup)
    ;; 以下はお好みで設定してください
    ;; lookupに--dictionaryオプションを利用する場合
    (setq loga-use-dictionary-option t)
    ;; :autoがデフォルトでカーソル位置の近くにpopup
    ;; :maxにするとbuffer一杯までpopupをのばし行の最初から表示します
    ;; Version: 1.0.1から'maxの様なシンボル記法も対応しました
    (setq loga-popup-output-type 'max)

---

M-x load-fileコマンドを実行しミニバッファに~/.emacs.d/init.elなどの
上で入力した設定ファイルへのパスを入力して実行します
(Emacsを再起動する事で再読み込みしてもよいです)

注意1: もしエラーが出るのであれば、閉じ括弧後ろでC-x C-e(または、M-x eval-last-sexp)をタイプする事で、その行を評価でき行単位でのチェックができます。  
注意2: キーバインド(kbd "ここの部分")は、あなたが使いやすい所に設定して下さい。

### logalimacsのpackageをインストールします
これで準備が整いました。
EmacsからM-x list-packagesコマンドを入力します。
packageのリストが表示されるので
C-sなどでlogalimacsを検索します。
同じ行にカーソルを合わせて"i"でpackageのマーク,
"x"でインストールの実行になります。
実行するとpackage-user-dirで指定したディレクトリか,
デフォルトの~/.emacs.d/elpaにインストールされます。

## 3. git cloneからのインストール方法

gitが使用可能であれば、下記のコマンドでもダウンロード可能です。

    % cd YOUR-CLONE-DIRECTORY
    % git clone https://github.com/logaling/logalimacs.git

ダウンロードしたら、./logalimacs/の中へロードパスを通します。

### .emacsへの設定

以下のコードをあなたの設定用の.emacsへ書込みます。
(~/.emacs.d/init.elでもいいですし、他にload関数で読み込んだ所でもいいです)

    ;;; logalimacsディレクトリへのロードパスを通す(上のYOUR-CLONE-DIRECTORYの場所)
    ;; ~/.emacs.d/package/logalimacs/logalimacs.elが存在する場合
    (add-to-list 'load-path "~/.emacs.d/package/logalimacs")
    ;;; logalimacsの設定
    ;; keybinds
    (global-set-key (kbd "M-g M-i") 'loga-interactive-command)
    (global-set-key (kbd "M-g M-l") 'loga-lookup-at-manually)
    (global-set-key (kbd "M-g M-a") 'loga-add)
    (global-set-key (kbd "C-:") 'loga-lookup-in-popup)
    ;; 以下はお好みで設定してください
    ;; lookupに--dictionaryオプションを利用する場合
    (setq loga-use-dictionary-option t)
    ;; :autoがデフォルトでカーソル位置の近くにpopup
    ;; :maxにするとbuffer一杯までpopupをのばし行の最初から表示します
    ;; Version: 1.0.1から'maxの様なシンボル記法も対応しました
    (setq loga-popup-output-type 'max)

注意: logalimacsは[_popwin.el_](https://github.com/m2ym/popwin-el)と[_popup.el_](https://github.com/m2ym/popup-el)を利用しています。githubからcloneしたファイルを移動する場合、これらも必要となります。

## 4. 雑多な設定

### .emacsとは:

emacs用の設定ファイルで、通常は、~/.emacs.d/init.elまたは、~/.emacs(昔はこれでしたが今は.emacs.d/init.elに書くのがナウイようです)になります。
もし、設定ファイルを分割したいと思ったらinit.elに下の様に書きます。

    (load "ディレクトリを含めた設定したいファイルパス")
    ;; ~/.emacs.d/以下に設定ファイルを追加したいなら、
    (load "~/.emacs.d/logalimacs_config")

この二つ目の例は、~/.emacs.d/以下のlogalimacs.elcまたはlogalimacs_config.elを読むようにしています。
(init.elをバイトコンパイルしている場合、init.elcファイルが優先されますのでディレクトリ表示画面で対象のファイルに合わせて"B"を押すか、`M-x dired-do-byte-compile`を実行します。)


### ロードパスを追加するには:

add-to-list関数を使います。
以下をあなたの.emacsに設定します。

    ;; "~/.emacs.d/package/hoge/以下にパスを通す場合
    (add-to-list 'load-path "~/.emacs.d/package/hoge")
