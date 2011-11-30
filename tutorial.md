---
layout: default
title: チュートリアル
---

チュートリアル
==============

<a name="index" />
目次
----
### プロジェクトに logaling-command を導入する ###

1. [logaling-command をインストールする](#install)
2. [logaling-command を使う準備をする](#preparation)
    1. [既にある対訳用語集を logaling-command で検索できるようにする](#import)
3. [用語を検索する](#lookup)
4. [用語を登録する](#add)
5. [登録されている用語を編集する](#update)
6. [用語を削除する](#delete)

### 既に logaling-command を使用しているプロジェクトに参加する ###

1. [logaling-command をインストールする](#install-user)
2. [logaling-command を使う準備をする](#preparation-user)
3. [用語を検索する](#lookup-user)
4. [用語を登録する](#add-user)
5. [登録されている用語を編集する](#update-user)
6. [用語を削除する](#delete-user)



プロジェクトに logaling-command を導入する
------------------------------------------

<a name="install" />
1. logaling-command をインストールする
--------------------------------------

まずは logaling-command をインストールします。
logaling-command は RubyGems でインストールできます。

	% gem install logaling-command

[目次へ戻る](#index)





<a name="preparation" />
2. logaling-command を使う準備をする
-----------

インストールが無事終わったら、プロジェクトで logaling-command を使うための準備をします。

例えば、あなたが groonga というプロジェクトに関わっていて、そのリポジトリが /Users/suzuki/groonga に既に存在するとします。そして、そのプロジェクトにおいて、英語→日本語の用語集を作成したいとしましょう。その際には、そのリポジトリのトップディレクトリで以下のコマンドを実行して下さい。(
新たに用語集は作らなくてもいい、既存の用語集をとりあえず使いたい、などという場合も、このコマンドを実行する必要があります。)


	% cd /Users/suzuki/groonga
	% loga new groonga en ja
	Your project is now registered to /Users/adzuki34/.logaling.d/projects/groonga.
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

また、これと同時に ユーザーホームに .logaling.d/projects というディレクトリが作成され、そこに前述の .logaling へのシンボリックリンクが作成されます。
後々、別のプロジェクトでも logaling-command を利用したくなった場合にはこの .logaling.d/projects にプロジェクト毎の設定や用語集がリンクされることになるため、検索を行うときにプロジェクトの横断検索が可能になります。

念のため、ユーザホームの .logaling.d を確認してみます。

	% ls -al ~/.logaling.d/projects
	total 8
	drwxr-xr-x  3 suzuki  suzuki  102 11 24 14:06 .
	drwxr-xr-x  5 suzuki  suzuki  170 11 22 11:46 ..
	lrwxr-xr-x  1 suzuki  suzuki  34 11 24 14:01 groonga -> /Users/suzuki/groonga/.logaling


<a name="import" />
2-1. 既にある対訳用語集を logaling-command で検索できるようにする
-----------------------------------------------------------------

既に「用語-対訳」のペアを用語集として何らかの形式で持っている場合があります。
logaling-command では、そのような場合のために、1行1用語ペアの CSV/TSV 形式を検索対象の用語集としてサポートしています。[※2](#kome2)

例えば、下記のような対訳用語集がある場合、

	% cat groonga.csv
	patricia-trie,パトリシアトライ
	hash,ハッシュテーブル

その用語集のファイル名に原語の言語コードと変換後言語コードを明示して、.logaling/glossary へ置くことで検索対象の用語集にすることができるようになります。

	% mv groonga.csv .logaling/glossary/groonga.en.ja.csv




これで、logaling-commandを使うための準備が整いました。

[目次へ戻る](#index)



<a name="lookup" />
3. 用語を検索する
-----------------
キーワードで用語集を検索してみましょう。
検索するには `loga lookup <検索したいキーワード>` とします。

	% loga lookup patricia
	
	lookup word : patricia
	
	  patricia-trie
	  パトリシアトライ
	    note:
	    glossary:groonga

2-1 の場合のように、既に用語集が存在していて、それを logaling-command で利用できるフォーマットに置き変えていた場合は、上記のような検索結果になります。
検索結果の見方は1行目の「patricia-trie」が検索にマッチした用語、2行目の「パトリシアトライ」が対訳、3行目の「note:」は後述しますが logaling-command で作成された用語集で備考が設定されていた場合にその内容が表示されます。最後の行の「glossary:groonga」はマッチした用語集名です。マッチした用語が複数ある場合は、これらのセットが複数表示されます。

[目次へ戻る](#index)





<a name="add" />
4. 用語を登録する
-----------------

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

[目次へ戻る](#index)







<a name="update" />
5. 登録されている用語を編集する
-------------------------------

用語を登録する際に typo してしまった、または別の対訳にして登録しなおしたいときは、用語集を編集することができます。
用語集を編集するには `loga update <用語> <登録されている対訳> <新しい対訳> <新しい備考(省略可)>` とします。

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

[目次へ戻る](#index)






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

[目次へ戻る](#index)







既に logaling-command を使用しているプロジェクトに参加する
----------------------------------------------------------

<a name="install-user" />
1. logaling-command をインストールする
--------------------------------------

インストール方法は [プロジェクトに logaling-command を導入する](#install) 場合と同じです。

[目次へ戻る](#index)




<a name="preparation-user" />
2. logaling-command を使う準備をする
------------------------------------

あなたが既に logaling-command を使用しているプロジェクトへ参加する場合、プロジェクトのリポジトリには .logaling というディレクトリが存在していると思います。(そのディレクトリの中に用語集があります。)
その場合にあなたがすることは、あなたのホームディレクトリに logaling-command 用のディレクトリを作り、用語集を検索するための準備をすることだけです。

そのためには、以下のコマンドを実行します。

	% loga register
	Your project is now registered to /Users/suzuki/.logaling.d/projects/groonga.

`loga register` を実行すると、ユーザーホームに .logaling.d/projects というディレクトリが作成され、そこに前述の .logaling へのシンボリックリンクが作成されます。
後々、別のプロジェクトでも logaling-command を利用したくなった場合にはこの .logaling.d/projects にプロジェクト毎の設定や用語集がリンクされることになるため、検索を行うときにプロジェクトの横断検索が可能になります。

念のため、ユーザホームの .logaling.d を確認してみます。

	% ls -al ~/.logaling.d/projects
	total 8
	drwxr-xr-x  3 suzuki  suzuki  102 11 24 14:06 .
	drwxr-xr-x  5 suzuki  suzuki  170 11 22 11:46 ..
	lrwxr-xr-x  1 suzuki  suzuki  34 11 24 14:01 groonga -> /Users/suzuki/groonga/.logaling

[目次へ戻る](#index)





<a name="lookup-user" />
3. 用語を検索する
-----------------

用語の検索方法は [プロジェクトに logaling-command を導入する](#lookup) 場合と同じです。

[目次へ戻る](#index)

<a name="add-user" />
4. 用語を登録する
-----------------

用語の登録方法は [プロジェクトに logaling-command を導入する](#add) 場合と同じです。

[目次へ戻る](#index)

<a name="update-user" />
5. 登録されている用語を編集する
-------------------------------

登録されている用語を編集する方法は [プロジェクトに logaling-command を導入する](#update) 場合と同じです。

[目次へ戻る](#index)

<a name="delete-user" />
6. 用語を削除する
-----------------

用語を削除する方法は [プロジェクトに logaling-command を導入する](#delete) 場合と同じです。

[目次へ戻る](#index)






----------
<a name="kome1" />※1言語の名称の略号2文字（ja や en など）<http://ja.wikipedia.org/wiki/ISO_639>

<a name="kome2" />※2ファイルの文字コードはUTF-8のみに対応しています。

