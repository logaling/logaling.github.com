---
layout: default
title: チュートリアル
---

チュートリアル
==============

<a id="index">目次</a>
-----------------------

### logaling-command を使うための準備をする ###
1. [logaling-command をインストールする](#install)
2. [logaling-command を使う準備をする](#preparation)
    1. [プロジェクトに logaling-command 導入する場合](#new)
    2. [既に logaling-command を使用しているプロジェクトに参加する場合](#register)

### logaling-command を使う ###
1. [用語を検索する](#lookup)
    1. [プロジェクトに既にある対訳用語集を logaling-command で検索できるようにする](#lookup-csv)
    2. [有名プロジェクトの用語集を logaling-command で検索できるようにする](#import)
2. [用語を登録する](#add)
    1. [一つのプロジェクトで複数言語への翻訳が同時に進行している場合](#without-target-lang)
3. [登録されている用語を編集する](#update)
4. [用語を削除する](#delete)



logaling-command を使うための準備をする
------------------------------------------


### <a id="install">1. logaling-command をインストールする</a> ###

まずは logaling-command をインストールします。
logaling-command は RubyGems でインストールできます。

	% gem install logaling-command

インストールが成功したことを確認するには、コマンドラインで `loga -v` と打ってみてください。

	% loga -v
	logaling-command version 0.0.8

上記のようなバージョン情報が表示されたら、インストールは成功です。

#### Requirements ####
logaling-command は Ruby1.9 環境で動作します。
また、内部では groonga、rroonga を利用しています。もしインストールされていなければ、 logaling-command をインストールすると同時に groonga と rroonga もインストールされます。

* [Ruby1.9.x](http://www.ruby-lang.org/ja/)
* [groonga](http://groonga.org/ja/)
* [rroonga](http://groonga.rubyforge.org/index.html.ja)


<p class="toTop"><a href="#index">目次へ戻る</a></p>



### <a id="preparation">2. logaling-command を使う準備をする</a> ###

#### <a id="new">2-1.プロジェクトに logaling-command 導入する場合</a> ####

プロジェクトに logaling-command を導入するための準備をします。

例えば、あなたが groonga というプロジェクトに関わっていて、そのリポジトリが /Users/suzuki/groonga に既に存在するとします。そして、そのプロジェクトで英語→日本語の用語集を作成したいとしましょう。その場合は、そのリポジトリのトップディレクトリで以下のコマンドを実行して下さい。(
新たに用語集は作らなくてもいい、既存の用語集をとりあえず使いたい、などという場合も、このコマンドを実行する必要があります。ただし、他プロジェクトの用語集から検索したいだけの場合はこの操作は必要ありません。)


	% cd /Users/suzuki/groonga
	% loga new groonga en ja
	Your project is now registered to logaling.
	Successfully created .logaling

*loga new* はプロジェクトで用語集を使うための設定ファイルやディレクトリなどを作成します。パラメータは「用語集名」と「原文の言語コード[※1](#kome1)」「変換後の言語コード(省略可能）」をこの順番で指定します。用語集名は必ずしもプロジェクト名と同じでなくても構いません。ですが、後々プロジェクトを横断して検索を掛けたときにわかりやすくするためにも、プロジェクト名が分かるようにしておいたほうが良いでしょう。

さて、上記のコマンドを実行すると、 .logaling というディレクトリが作成されました。.logaling の中は次のようになっています。

	.logaling
	├── config
	└── glossary

config はこのプロジェクト内で logaling-command を使うときの設定ファイルです。中身は次のようになっています。用語集名、原文の言語コード、変換後の言語コードが設定されています。

	% cat .logaling/config
	--glossary groonga
	--source-language en
	--target-language ja

glossary は、このプロジェクトの用語集を置くためのディレクトリです。現時点では何も用語集は置かれていません。

*loga new* は上記を行うと共に、ユーザーホームに .logaling.d/projects というディレクトリを作成し、そのディレクトリ配下に前述の .logaling へのシンボリックリンクを作成します。
後々、別のプロジェクトでも logaling-command を利用したくなった場合には、この .logaling.d/projects にプロジェクト毎の設定や用語集がリンクされることになるため、検索を行うときにプロジェクトの横断検索が可能になります。

ユーザホームの .logaling.d にシンボリックリンクが正しく設定されていることを確認します。

	% ls -al ~/.logaling.d/projects
	total 8
	drwxr-xr-x  3 suzuki  suzuki  102 11 24 14:06 .
	drwxr-xr-x  5 suzuki  suzuki  170 11 22 11:46 ..
	lrwxr-xr-x  1 suzuki  suzuki  34 11 24 14:01 groonga -> /Users/suzuki/groonga/.logaling

これで logaling-command を使うための準備が整いました。

<p class="toTop"><a href="#index">目次へ戻る</a></p>



#### <a id="register">2. 既に logaling-command を使用しているプロジェクトに参加する</a> ####

あなたが既に logaling-command を使用しているプロジェクトへ参加する場合、プロジェクトのリポジトリには .logaling というディレクトリが存在していると思います。(そのディレクトリの中に用語集があります。)
その場合にあなたがすることは、あなたのホームディレクトリに logaling-command 用のディレクトリを作り、用語集を検索するための準備をすることだけです。

そのためには、以下のコマンドを実行します。

	% loga register
	Your project is now registered to logaling.

*loga register* を実行すると、ユーザーホームに .logaling.d/projects というディレクトリが作成され、そこに前述の .logaling へのシンボリックリンクが作成されます。
後々、別のプロジェクトでも logaling-command を利用したくなった場合にはこの .logaling.d/projects にプロジェクト毎の設定や用語集がリンクされることになるため、検索を行うときにプロジェクトの横断検索が可能になります。

念のため、ユーザホームの .logaling.d を確認してみます。

	% ls -al ~/.logaling.d/projects
	total 8
	drwxr-xr-x  3 suzuki  suzuki  102 11 24 14:06 .
	drwxr-xr-x  5 suzuki  suzuki  170 11 22 11:46 ..
	lrwxr-xr-x  1 suzuki  suzuki  34 11 24 14:01 groonga -> /Users/suzuki/groonga/.logaling


これで logaling-command を使うための準備が整いました。

<p class="toTop"><a href="#index">目次へ戻る</a></p>








logaling-comand を使う
----------------------

### <a id="lookup">1. 用語を検索する</a> ###
キーワードで用語集を検索してみましょう。
検索するには `loga lookup <検索したいキーワード>` とします。

	% loga lookup patricia
	patricia-trie    パトリシアトライ

既に用語集が存在していて、検索キーワードにマッチする用語が登録されていた場合は上記のような検索結果になります。
検索結果の見方は最初の「patricia-trie」が検索にマッチした用語、少し間をあけて次の「パトリシアトライ」が対訳です。
もしマッチした用語に備考が設定されていた場合には対訳の後ろにその内容が表示されます。
また、用語集が複数（プロジェクト分）ある場合には、最後にカッコ書きでマッチした用語集名が表示されます。
マッチした用語が複数ある場合は、これらのセットが複数表示されます。

<p class="toTop"><a href="#index">目次へ戻る</a></p>




### <a id="lookup-csv">1-1. プロジェクトに既にある対訳用語集を logaling-command で検索できるようにする</a> ###

プロジェクトで既に「用語-対訳」のペアを用語集として何らかの形式で持っている場合があります。
logaling-command では、そのような場合のために、1行1用語ペアの CSV/TSV 形式を検索対象の用語集としてサポートしています。[※2](#kome2)

例えば、下記のような対訳用語集がある場合、

	% cat groonga.csv
	patricia-trie,パトリシアトライ
	hash,ハッシュテーブル

その用語集のファイル名に原文の言語コードと変換後言語コードを明示して .logaling/glossary へ置くことで検索対象の用語集にすることができるようになります。

	% mv groonga.csv .logaling/glossary/groonga.en.ja.csv

実際に検索すると以下のような結果になります。

	% loga lookup patricia
	patricia-trie    パトリシアトライ


### <a id="import">1-2. 有名プロジェクトの用語集を logaling-command で検索できるようにする</a> ###

自分が参加しているプロジェクト以外で、同じ用語がどのように訳されているのかを知りたい場合があるかもしれません。 *loga import* で、いくつかの有名プロジェクトで利用されている用語集をインポートすることができます。

まずは、インポートできるプロジェクトの種類がどれくらいあるのかを知るために、以下のコマンドを実行して下さい。

	% loga import --list
	debian_project : Debian JP Project (http://www.debian.or.jp/community/translate/)
	gnome_project : GNOME Translation Project Ja (http://live.gnome.org/TranslationProjectJa)
	postgresql_manual : PostgreSQL7.1 Manual (http://osb.sraoss.co.jp/PostgreSQL/Manual/)

コマンドの結果から、インポート出来る用語集が3種類あることがわかりました。それでは、このうちの postgresql_manual をインポートしてみましょう。実際に用語集をインポートするためには *loga import* に用語集名をパラメータとして渡してあげます。この用語集名は上記のリストのコロンの前にある項目です。

	% loga import postgresql_manual
	loga import postgresql_manual  2.83s user 0.38s system 74% cpu 4.331 total

これでインポートができたので、試しに検索してみましょう。

	% /Users/suzuki/work/logaling/logaling-command/bin/loga lookup db 
	DBA        DBA         (postgresql_manual)

先ほどインポートした postgresql_manual から検索できました。





### <a name="add">2. 用語を登録する</a> ###

新たに用語を登録したい場合は次のようにします。

	% loga add "storage engine" "ストレージエンジン" "groongaをベースとしたMySQLのストレージエンジン"

用語を登録する際には `loga add <用語> <対訳> <備考(省略可能)>` とすると、そのプロジェクトの対訳用語集（なければ作成して）に用語を登録していくことができます。
このコマンドを実行すると .logaling/glossary 以下に groonga.en.ja.yml というファイルができたと思います。これが logaling-command の用語集ファイルです。

	% ls -l .logaling/glossary
	total 16
	-rw-r--r--  1 suzuki  suzuki  61 11 24 14:41 groonga.en.ja.csv
	-rw-r--r--  1 suzuki  suzuki  99 11 24 15:14 groonga.en.ja.yml

用語集ファイルは拡張子でわかるように、YAML形式のファイルになっています。

	% cat .logaling/glossary/groonga.en.ja.yml
	- source_term: storage engine
	  target_term: ストレージエンジン
	  note: groongaをベースとしたMySQLのストレージエンジン

登録した内容を検索してみましょう。

	% loga lookup storage
	  storage engine : ストレージエンジン # groongaをベースとしたMySQLのストレージエンジン

この用語登録は、ひとつの用語で複数の対訳を登録することが可能です。その場合も同じように *loga add* してください。

	% loga add "storage engine" "ストレージ・エンジン"

結果を検索すると、次のようになります。

	% loga lookup storage
	  storage engine    ストレージエンジン    # groongaをベースとしたMySQLのストレージエンジン
	  storage engine    ストレージ・エンジン

<p class="toTop"><a href="#index">目次へ戻る</a></p>









<!-------------- ここから ----------------->
### <a id="without-target-lang">1. 一つのプロジェクトで複数言語への翻訳が同時に進行している場合</a> ###
（！！※未実装なので、内容は変わる可能性があります！！）
一つのプロジェクトで複数言語への翻訳が同時に進行している場合には、プロジェクトのリポジトリに含める設定としての「翻訳言語（target-language）」を決めたくない場合があるかもしれません。
例えば、元のドキュメントは英語で、それを日本語とフランス語に翻訳したい。さらに、後から別の言語にも翻訳していく可能性があるような場合です。

そういう場合には、プロジェクトのトップレベルにある *.logaling/config* は以下のように *--target-language* が省略されているでしょう。

	% cat config
	--glossary notyet
	--source-language en

この状態で *loga add* しようとすると、

	% loga add email Eメール
	input target-language code '-T <target-language code>'

このように翻訳言語の言語コードを指定するようにメッセージが表示されて追加できません。
もちろん、1つの用語だけを用語集に追加したいだけなら、このメッセージ通りにオプションとして翻訳言語を指定することで問題なく追加することができます。
ですが、次々に用語を追加していきたい場合には毎回オプションで翻訳言語を指定するのは面倒です。

そのような場合には *loga myconfig* を利用します。

	% loga myconfig groonga en ja
	Create myconfig.

*loga myconfig [用語集名] [原文の言語コード] [翻訳言語の言語コード]* とすると、プロジェクトの .logaling ではなく、ユーザーホームの *.logaling.d* に myconfig という設定ファイルが作成されます。

	% ls -l ~/.logaling.d
	total 8
	drwxr-xr-x   5 suzuki  suzuki  170 12 14 15:11 cache
	drwxr-xr-x  18 suzuki  suzuki  612 12 15 17:33 db
	-rw-r--r--   1 suzuki  suzuki   61 12 16 15:38 myconfig
	drwxr-xr-x   4 suzuki  suzuki  136 12 15 17:33 projects

設定ファイルの中身は .logaling/config とまったく同じですが、 logaling-command は実行時に myconfig の設定の方を優先します。
myconfig で自分専用の設定を持つことができるので、一つのプロジェクトで複数の言語への翻訳が同時進行していても普段と同じように用語集の編集ができるようになります。

myconfig は便利ですが、注意したいのは myconfig をそのままにしておくと、他のプロジェクトでの翻訳作業時にもこの設定を利用してしまうことです。そうならないためにも翻訳作業が終わったら myconfig は必ず削除しておいて下さい。
myconfig を削除するには以下のコマンドを実行します。

	% loga remove-myconfig
	Remove myconfig.

	% ls -l ~/.logaling.d
	total 0
	drwxr-xr-x   5 suzuki  suzuki  170 12 14 15:11 cache
	drwxr-xr-x  18 suzuki  suzuki  612 12 15 17:33 db
	drwxr-xr-x   4 suzuki  suzuki  136 12 15 17:33 projects

<p class="toTop"><a href="#index">目次へ戻る</a></p>
<!-------------- ここまで ----------------->









### <a id="update">3. 登録されている用語を編集する</a> ###

用語を登録する際に typo してしまった、または別の対訳にして登録しなおしたいときは、用語集を編集することができます。
用語集を編集するには `loga update <用語> <登録されている対訳> <新しい対訳> <新しい備考(省略可)>` とします。

まずは編集したい用語を検索してみましょう。ここでは、先ほど登録した「storage engine」とします。

	% loga lookup storage
	  storage engine    ストレージエンジン    # groongaをベースとしたMySQLのストレージエンジン
	  storage engine    ストレージ・エンジン

この、2つ目の対訳を「ストレージ　エンジン」とすることにします。

	% loga update "storage engine" "ストレージ・エンジン" "ストレージ　エンジン" "ストレージとエンジンの間にスペース1つ"


結果を確認してみましょう。
	% loga lookup storage
	
	lookup word : storage
	  storage engine    ストレージエンジン    # groongaをベースとしたMySQLのストレージエンジン
	  storage engine    ストレージ エンジン   # ストレージとエンジンの間にスペース1つ

指定した通りに更新されました。

<p class="toTop"><a href="#index">目次へ戻る</a></p>





### <a id="delete">4. 用語を削除する</a> ###

用語集に登録してある用語が必要なくなった場合には`loga delete <用語> <対訳>` とすると、削除することができます。
[storage engine] - [ストレージ エンジン]という用語と対訳のペアを削除してみます。

	% loga delete "storage engine" "ストレージ エンジン"

結果を確認してみましょう。
	% loga lookup storage
	  storage engine : ストレージエンジン # groongaをベースとしたMySQLのストレージエンジン

指定した通りに削除されました。

<p class="toTop"><a href="#index">目次へ戻る</a></p>






----------
<a id="kome1">※1言語の名称の略号2文字（ja や en など）<http://ja.wikipedia.org/wiki/ISO_639></a>

<a id="kome2">※2ファイルの文字コードはUTF-8のみに対応しています。</a>

