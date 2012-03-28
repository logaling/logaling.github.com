---
layout: default
title: チュートリアル
---

チュートリアル
====================
このページでは logaling-command の基本的な使い方を紹介します。

### インストールする
まずは logaling-command をインストールします。
logaling-command は RubyGems でインストールできます。

※logaling-command は Ruby 1.9 環境で動作します。

<a href="#requirements"></a>

	% gem install logaling-command

もし、以下のようなエラーメッセージが出てしまってインストールできないようであれば、それはあなたの環境が Ruby 1.8 だということなので、Ruby1.9.2をインストールしてから再度インストールしてみてください。

	logaling-command requires Ruby version >= 1.9.2.
	ERROR:  Error installing logaling-command:


インストールが成功したことを確認するには、コマンドラインで `loga -v` と打ってみてください。

	% loga -v
	logaling-command version 0.1.3

上記のようなバージョン情報が表示されたら、インストールは成功です。

<ul class="listMark">
<li> <a href="http://www.ruby-lang.org/ja/">Ruby 1.9.x</a></li>
<li> <a href="http://groonga.org/ja/">groonga</a></li>
<li> <a href="http://groonga.rubyforge.org/index.html.ja">rroonga</a></li>
</ul>



### 用語集を作る
さっそく用語集を作ってみましょう。用語集を作るには、任意のディレクトリ（ここでは my_project とします）で `loga new <用語集名> <原文の言語コード> <対訳の言語コード(省略可)>` を実行します。

	% mkdir /Users/suzuki/my_project
	% cd /Users/suzuki/my_project
	% loga new my_project en ja
	Your project is now registered to logaling.
	Successfully created .logaling

`loga new` はプロジェクトで用語集を使うための設定ファイルやディレクトリなどを作成します。パラメータは「用語集名」と「原文の言語コード[※1](#kome1)」「対訳の言語コード(省略可能）」をこの順番で指定します。用語集名は必ずしもプロジェクト名と同じでなくても構いません。ですが、後々プロジェクトを横断して検索を掛けたときにわかりやすくするためにも、プロジェクト名が分かるようにしておいたほうが良いでしょう。

さて、上記のコマンドを実行すると、 .logaling というディレクトリが作成されました。.logaling の中は次のようになっています。

	.logaling
	├── config
	└── glossary

config はこのプロジェクト内で logaling-command を使うときの設定ファイルです。中身は次のようになっています。

	% cat .logaling/config
	--glossary my_project
	--source-language en
	--target-language ja

用語集名、原文の言語コード、対訳の言語コードが設定されています。
glossary は、このプロジェクトの用語集を置くためのディレクトリです。現時点では何も用語集は置かれていません。

`loga new` は上記を行うと共に、ユーザーホームに .logaling.d/projects というディレクトリを作成し、そのディレクトリ配下にプロジェクトを登録します。

------
<a id="kome1">※1言語の名称の略号2文字（ja や en など）<http://ja.wikipedia.org/wiki/ISO_639></a>


### 用語を登録する
できあがった用語集に用語を登録してみましょう。用語を登録するには `loga add <用語> <対訳> <備考(省略可能)>` とします。
ここでは「database」という用語に「データベース」という対訳を付けて登録してみます。

	% loga add database データベース

`loga add` のパラメータ3つのうち備考は省略可能で、上記では備考を省略しています。


### 用語を検索する
それでは、登録した用語を検索してみましょう。検索するには `loga lookup` を実行します。
パラメータは検索したい用語です。
先ほど登録した「database」を検索してみましょう。

	% loga lookup database
	now index my_project...
	  database   データベース

先ほど登録した用語が検索結果として表示されました。


### 他の用語集をインポートする
翻訳をしていると、自分のプロジェクト以外で用語がどのように訳されているかを知りたくなるものです。そのような場合のために、 logaling-command はいくつかの有名プロジェクトの用語集と辞書のインポートをサポートしています。

まずは、インポート可能な用語集 / 辞書のリストを確認しましょう。コマンドは `loga import --list` です。

	% loga import --list
	debian_project : Debian JP Project (http://www.debian.or.jp/community/translate/)
	edict : The EDICT Dictionary File (http://www.csse.monash.edu.au/~jwb/edict.html)
	freebsd_jpman : FreeBSD jpman(http://www.jp.freebsd.org/man-jp/)
	gene95 : GENE95 Dictionary (http://www.namazu.org/~tsuchiya/sdic/data/gene.html)
	gnome_project : GNOME Translation Project Ja (http://live.gnome.org/TranslationProjectJa)
	mozilla_japan : Mozilla Japan (http://www.mozilla-japan.org/jp/l10n/term/l10n.html)
	postgresql_manual : PostgreSQL7.1 Manual (http://osb.sraoss.co.jp/PostgreSQL/Manual/)

リストが表示されました。それでは、「postgresql_manual」をインポートしてみましょう。用語集 / 辞書のインポートは `loga import` に上記リストの最初に出てくる名前をパラメータとして渡します。

	% loga import postgresql_manual
	now index postgresql_manual...

用語集のサイズによっては少し時間がかかるかもしれません。
さて、ここで再度「database」で検索してみましょう。インポートしたのが PostgreSQL のマニュアルですので、似た用語が登録されているかもしれません。

	% loga lookup database
	  database                 データベース my_project
	  Database Administrator   データベース管理者   postgresql_manual
	  database                 データベース postgresql_manual
	  database server          データベースサーバ   postgresql_manual
	  the Supplier Database    納入業者データベース postgresql_manual

予想通り、インポートした用語集の中に似た用語があったため、検索結果にも反映されています。

### 辞書検索する
ここで、もうひとつの検索を実行してみましょう。もう一つの検索とは、辞書検索です。
いままでは、翻訳作業を前提とした用語の検索を行なっていました。翻訳作業をすることが前提であるので、自分が翻訳するのに関係のない他の言語は、実はこの検索時には検索対象にはなっていません。また、同様の理由で対訳側からの逆引きもしません。例えば、 `loga lookup データベース`
では結果は返ってきません。ですが、 logaling-command を辞書として使いたい場合には検索した語にマッチした用語または対訳は表示してほしいものです。
そういった場合は *--dictionary* オプションを付けて下さい。

	% loga lookp データベース --dictionary
	  Database Administrator           データベース管理者   postgresql_manual
	  database                         データベース postgresql_manual
	  database                         データベース my_project
	  database server                  データベースサーバ   postgresql_manual
	  the Supplier Database            納入業者データベース postgresql_manual

このように、対訳側からも検索が可能になります。

以上が logaling-command の基本的な使い方となります。

### 詳しい使い方は...
もっと詳しく logaling-command を知りたい方は[詳しい使い方](usage.html) を参照して下さい。
また、各コマンドの詳細については [コマンドリファレンス](reference.html) を参照して下さい。

