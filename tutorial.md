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

例えば、あなたが logaling というプロジェクトに関わっていて、そのリポジトリが`/Users/suzuki/logaling`に既に存在するとします。そして、そのプロジェクトにおいて、英語→日本語の用語集を作成したいとしましょう。その際には、そのリポジトリのトップディレクトリで以下のコマンドを実行して下さい。(
新たに用語集は作らなくてもいい、既存の用語集をとりあえず使いたい、などという場合も、このコマンドを実行する必要があります。)


	% cd /Users/suzuki/logaling
	% loga new logaling en ja
	Successfully created .logaling

`loga new` はプロジェクトで用語集を使うための設定ファイルやディレクトリなどを作成します。パラメータは「用語集名」と「原語の言語情報（jaとかenとか）」「変換後の言語情報（jaとかenとか。省略可能）」をこの順番で指定します。用語集名は必ずしもプロジェクト名と同じでなくても良いですが、後々プロジェクトを横断して検索を掛けたときにわかりやすくするためにも、プロジェクト名が分かるようにしておいたほうが良いでしょう。

さて、上記のコマンドを実行すると、.logalingというディレクトリが作成されました。.logaling の中は次のようになっています。

	.logaling
	├── config
	└── glossary

config というのは、このプロジェクト内で logaling-command を使うときの設定ファイルです。中身は次のようになっていて、用語集名、原語の言語情報、変換後の言語情報が設定されています。

	% cat .logaling/config
	--glossary logaling
	--source-language en
	--target-language ja

glossary というのは、このプロジェクトの用語集を置くためのディレクトリで、現時点では何も用語集は置かれていません。


次に、以下のコマンドを実行します。

	% loga link
	Your project is now linked to /Users/suzuki/.logaling.d/projects/logaling.

`loga link` を実行すると、上で作ったこのプロジェクトの .logaling ディレクトリへ、ユーザホームの .logaling.d/projects というディレクトリの下からシンボリックリンクが貼られます。今後、別のプロジェクトでも logaling-command を利用したくなった場合にはこの projects に各プロジェクトの用語集が増えていくことになり、そうすると検索でプロジェクトの横断検索が可能になります。

	% ls -al ~/.logaling.d/projects
	total 8
	drwxr-xr-x  3 suzuki  suzuki  102 11 24 14:06 .
	drwxr-xr-x  5 suzuki  suzuki  170 11 22 11:46 ..
	lrwxr-xr-x  1 suzuki  suzuki  34 11 24 14:01 logaling -> /Users/suzuki/logaling/.logaling

これで、loglaing-commandを使うための準備が整いました。




<a name="lookup" />
3. 既に対訳用語集があり、そこに登録されている用語を検索したい
-------------------------------------------------------------

既に「用語-対訳」のペアを用語集として何らかの形式で持っている場合があります。
logaling-command では、そのような場合のために、1行1用語の CSV/TSV 形式を検索対象の用語集としてサポートしています。
例えば、下記のような対訳用語集がある場合、

	% cat logaling.csv
	logaling,ろがりん
	glossary,対訳用語集

その用語集のファイル名に原語の言語情報(jaとかenとか)と変換後言語情(jaとかenとか)を明示して、.logaling/glossary へ置くことで検索対象の用語集にすることができるようになります。

	% mv logaling.csv .logaling/glossary/logaling.en.ja.csv

さっそく glossary という用語で検索してみましょう。
検索するには `loga lookup <検索したい用語>` とします。

	% loga lookup glossary
	
	lookup word : glossary
	
	  glossary
	  対訳用語集
	    note:
	    glossary:logaling

検索結果の見方は1行目の「glossary」が検索にマッチした用語、2行目の「対訳用語集」が対訳、3行目の「note:」は後述しますが logaling-command で作成された用語集で備考が設定されていた場合にその内容が表示されます。最後の行の「glossary:logaling」はマッチした用語集名です。マッチした用語が複数ある場合は、これらのセットが複数表示されます。




<a name="add" />
4. 用語集を作成して用語を登録する
---------------------------------

新たに用語を登録したくなった場合は次のようにします。

	% loga add lookup 検索 ルックアップそのままでも良い

`loga add <用語> <対訳> <備考(省略可能)>` とすると、そのプロジェクトの対訳用語集（なければ作成して）に用語を登録していくことができます。
このコマンドを実行すると、.logaling/glossary 以下に logaling.en.ja.yml というファイルができたと思います。これが、logaling-command の用語集ファイルです。

	% ls -l .logaling/glossary
	total 16
	-rw-r--r--  1 suzuki  staff  61 11 24 14:41 logaling.en.ja.csv
	-rw-r--r--  1 suzuki  staff  99 11 24 15:14 logaling.en.ja.yml

拡張子でわかるように、YAML形式のファイルになっています。

	% cat .logaling/glossary/logaling.en.ja.yml
	---
	- source_term: lookup
	  target_term: 検索
	  note: ルックアップそのままでも良い

登録した内容を検索してみましょう。

	% loga lookup lookup
	
	lookup word : lookup
	
	  lookup
	  検索
	    note:ルックアップそのままでも良い
	    glossary:logaling

この用語登録は、ひとつの用語で複数の対訳を登録することが可能です。その場合も同じように`loga add`してください。

	% loga add lookup ルックアップ 検索でも可

結果を検索すると、次のようになります。

	% loga lookup lookup
	
	lookup word : lookup
	
	  lookup
	  ルックアップ
	    note:検索でも可
	    glossary:logaling
	
	  lookup
	  検索
	    note:ルックアップそのままでも良い
	    glossary:logaling






<a name="update" />
5. 登録されている用語を編集する
-------------------------------

用語を登録する際に typo してしまった、または別の対訳にして登録したいときは、用語集を編集することができます。
用語集を編集するには `loga update <用語> <登録されている対訳> <新しい対訳> <新しい備(省略可)>` とします。

まずは編集したい用語を検索してみましょう。ここでは、先ほど登録した「lookup」とします。

	% loga lookup lookup
	
	lookup word : lookup
	
	  lookup
	  ルックアップ
	    note:検索でも可
	    glossary:logaling
	
	  lookup
	  検索
	    note:ルックアップそのままでも良い
	    glossary:logaling

この、一つ目の対訳を「検索(またはルックアップ)」とすることにします。

	% loga update 'lookup' 'ルックアップ' '検索(またはルックアップ)' '文脈に応じてどちらでも可'


結果を確認してみましょう。
	% loga lookup lookup
	
	lookup word : lookup
	
	  lookup
	  検索
	    note:ルックアップそのままでも良い
	    glossary:logaling
	
	  lookup
	  検索(またはルックアップ)
	    note:文脈に応じてどちらでも可
	    glossary:logaling


指定したとおりに更新されました。




<a name="delete" />
6. 用語を削除する
-----------------

用語集に登録してある用語が必要なくなった場合には`loga delete <用語> <対訳>` とすると、削除することができます。
「lookup-検索」という用語と対訳のペアを削除してみます。

	% loga delete lookup 検索

結果を確認してみましょう。
	% loga lookup lookup
	
	lookup word : lookup
	
	  lookup
	  検索(またはルックアップ)
	    note:文脈に応じてどちらでも可
	    glossary:logaling

指定したとおりに削除されました。

