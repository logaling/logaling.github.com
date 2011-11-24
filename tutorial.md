---
layout: default
title: チュートリアル
---

チュートリアル
==============

目次
----
1. [logaling-command をインストールする](#install)
2. [logaling-command を使う準備する](#preparation)
3. [既に対訳用語集があり、そこに登録されている用語を検索したい](#lookup)
4. [用語集を作成して用語を登録する](#add)
5. [登録されている用語を編集する](#update)
6. [用語を削除する](#delete)


<a name="install" />
1. logaling-command をインストールする
--------------------------------------

まずは logaling-command をインストールします。
logaling-command は RubyGems でインストールできます。

	% gem install logaling-command





<a name="preparation" />
2. logaling-command を使う準備する
-----------

インストールが無事終わったら、プロジェクトで logaling-command を使うための準備をします。

例えば、あなたが groonga というプロジェクトに関わっていて、そのリポジトリが /Users/suzuki/groonga に既に存在するとします。そして、そのプロジェクトにおいて、英語→日本語の用語集を作成したいとしましょう。その際には、そのリポジトリのトップディレクトリで以下のコマンドを実行して下さい。(
新たに用語集は作らなくてもいい、既存の用語集をとりあえず使いたい、などという場合も、このコマンドを実行する必要があります。)


	% cd /Users/suzuki/groonga
	% loga new groonga en ja
	Successfully created .logaling

`loga new` はプロジェクトで用語集を使うための設定ファイルやディレクトリなどを作成します。パラメータは「用語集名」と「原語の言語コード[※1](#kome1)」「変換後の言語コード(省略可能）」をこの順番で指定します。用語集名は必ずしもプロジェクト名と同じでなくても良いですが、後々プロジェクトを横断して検索を掛けたときにわかりやすくするためにも、プロジェクト名が分かるようにしておいたほうが良いでしょう。

さて、上記のコマンドを実行すると、 .logaling というディレクトリが作成されました。.logaling の中は次のようになっています。

	.logaling
	├── config
	└── glossary

config というのは、このプロジェクト内で logaling-command を使うときの設定ファイルです。中身は次のようになっていて、用語集名、原語の言語コード、変換後の言語コードが設定されています。

	% cat .logaling/config
	--glossary groonga
	--source-language en
	--target-language ja

glossary というのは、このプロジェクトの用語集を置くためのディレクトリで、現時点では何も用語集は置かれていません。


次に、以下のコマンドを実行します。

	% loga link
	Your project is now linked to /Users/suzuki/.logaling.d/projects/groonga.

`loga link` を実行すると、先に作られた .logaling ディレクトリへ、ユーザホームの .logaling.d/projects というディレクトリの下からシンボリックリンクが貼られます。後々、別のプロジェクトでも logaling-command を利用したくなった場合にはこの projects に各プロジェクトの用語集が増えていくことになり、そうすると検索でプロジェクトの横断検索が可能になります。

念のため、ユーザホームの .logaling.d を確認してみます。

	% ls -al ~/.logaling.d/projects
	total 8
	drwxr-xr-x  3 suzuki  suzuki  102 11 24 14:06 .
	drwxr-xr-x  5 suzuki  suzuki  170 11 22 11:46 ..
	lrwxr-xr-x  1 suzuki  suzuki  34 11 24 14:01 groonga -> /Users/suzuki/groonga/.logaling

これで、loglaing-commandを使うための準備が整いました。




<a name="lookup" />
3. 既に対訳用語集があり、そこに登録されている用語を検索したい
-------------------------------------------------------------

既に「用語-対訳」のペアを用語集として何らかの形式で持っている場合があります。
logaling-command では、そのような場合のために、1行1用語ペアの CSV/TSV 形式を検索対象の用語集としてサポートしています。[※2](#kome2)

例えば、下記のような対訳用語集がある場合、

	% cat groonga.csv
	patricia-trie,パトリシアトライ
	hash,ハッシュテーブル

その用語集のファイル名に原語の言語コードと変換後言語コードを明示して、.logaling/glossary へ置くことで検索対象の用語集にすることができるようになります。

	% mv groonga.csv .logaling/glossary/groonga.en.ja.csv

これで、CSVファイルの内容が検索用DBにインデックスされました。
さっそく patricia というキーワードで検索してみましょう。
検索するには `loga lookup <検索したいキーワード>` とします。

	% loga lookup patricia
	
	lookup word : patricia
	
	  patricia-trie
	  パトリシアトライ
	    note:
	    glossary:groonga

検索結果の見方は1行目の「patricia-trie」が検索にマッチした用語、2行目の「パトリシアトライ」が対訳、3行目の「note:」は後述しますが logaling-command で作成された用語集で備考が設定されていた場合にその内容が表示されます。最後の行の「glossary:groonga」はマッチした用語集名です。マッチした用語が複数ある場合は、これらのセットが複数表示されます。




<a name="add" />
4. 用語集を作成して用語を登録する
---------------------------------

新たに用語を登録したくなった場合は次のようにします。

	% loga add "storage engine" "ストレージエンジン" "groongaをベースとしたMySQLのストレージエンジン"

`loga add <用語> <対訳> <備考(省略可能)>` とすると、そのプロジェクトの対訳用語集（なければ作成して）に用語を登録していくことができます。
このコマンドを実行すると、.logaling/glossary 以下に groonga.en.ja.yml というファイルができたと思います。これが、logaling-command の用語集ファイルです。

	% ls -l .logaling/glossary
	total 16
	-rw-r--r--  1 suzuki  suzuki  61 11 24 14:41 groonga.en.ja.csv
	-rw-r--r--  1 suzuki  suzuki  99 11 24 15:14 groonga.en.ja.yml

拡張子でわかるように、YAML形式のファイルになっています。

	% cat .logaling/glossary/groonga.en.ja.yml
	- source_term: storage engine
	  target_term: ストレージエンジン
	  note: groongaをベースとしたMySQLのストレージエンジン

登録した内容を検索してみましょう。

	% loga lookup storage
	
	lookup word : storage
	
	  storage engine
	  ストレージエンジン
	    note:groongaをベースとしたMySQLのストレージエンジン
	    glossary:groonga

この用語登録は、ひとつの用語で複数の対訳を登録することが可能です。その場合も同じように`loga add`してください。

	% loga add "storage engine" "ストレージ・エンジン"

結果を検索すると、次のようになります。

	% loga lookup storage
	
	lookup word : storage
	
	  storage engine
	  ストレージエンジン
	    note:groongaをベースとしたMySQLのストレージエンジン
	    glossary:groonga
	
	  storage engine
	  ストレージ・エンジン
	    note:
	    glossary:groonga






<a name="update" />
5. 登録されている用語を編集する
-------------------------------

用語を登録する際に typo してしまった、または別の対訳にして登録しなおしたいときは、用語集を編集することができます。
用語集を編集するには `loga update <用語> <登録されている対訳> <新しい対訳> <新しい備(省略可)>` とします。

まずは編集したい用語を検索してみましょう。ここでは、先ほど登録した「storage engine」とします。

	% loga lookup storage
	
	lookup word : storage
	
	  storage engine
	  ストレージエンジン
	    note:groongaをベースとしたMySQLのストレージエンジン
	    glossary:groonga
	
	  storage engine
	  ストレージ・エンジン
	    note:
	    glossary:groonga

この、2つ目の対訳を「ストレージ　エンジン」とすることにします。

	% loga update "storage engine" "ストレージ・エンジン" "ストレージ　エンジン" "ストレージとエンジンの間にスペース1つ"


結果を確認してみましょう。
	% loga lookup storage
	
	lookup word : storage
	
	  storage engine
	  ストレージ　エンジン
	    note:ストレージとエンジンの間にスペース1つ
	    glossary:groonga
	
	  storage engine
	  ストレージエンジン
	    note:groongaをベースとしたMySQLのストレージエンジン
	    glossary:groonga

指定した通りに更新されました。




<a name="delete" />
6. 用語を削除する
-----------------

用語集に登録してある用語が必要なくなった場合には`loga delete <用語> <対訳>` とすると、削除することができます。
[storage engine] - [ストレージ　エンジン]という用語と対訳のペアを削除してみます。

	% loga delete "storage engine" "ストレージ　エンジン"

結果を確認してみましょう。
	% loga lookup storage
	
	lookup word : storage
	
	  storage engine
	  ストレージエンジン
	    note:groongaをベースとしたMySQLのストレージエンジン
	    glossary:groonga

指定した通りに削除されました。



----------
<a name="kome1" />※1言語の名称の略号2文字（ja や en など）<http://ja.wikipedia.org/wiki/ISO_639>

<a name="kome2" />※2ファイルの文字コードはUTF-8のみに対応しています。

