---
layout: default
title: チュートリアル
---

チュートリアル
==============


<div id="indexDivision">
<h2><a id="index">目次</a></h2>
<h3>logaling-command を使うための準備をする</h3>
<ol>
<li><a href="#install">logaling-command をインストールする</a></li>
<li><a href="#preparation">logaling-command を使う準備をする</a>
  <ol>
  <li><a href="#new">プロジェクトに logaling-command を導入する場合</a></li>
  <li><a href="#register">既に logaling-command を使用しているプロジェクトに参加する場合</a></li>
  </ol>
</li>
</ol>



<h3>logaling-command を使う</h3>

<ol>
<li><a href="#lookup">用語を検索する</a>
  <ol>
  <li><a href="#lookup-csv">プロジェクトに既にある対訳用語集を logaling-command で検索できるようにする</a></li>
  <li><a href="#import">有名プロジェクトの用語集を logaling-command で検索できるようにする</a></li>
  </ol>
</li>
<li><a href="#add">用語を登録する</a>
  <ol>
  <li><a href="#add-term">プロジェクトの対訳用語集に用語を登録する</a></li>
  <li><a href="#config">一つのプロジェクトで複数言語への翻訳が同時に進行している場合</a></li>
  </ol>
</li>
<li><a href="#update">登録されている用語を編集する</a></li>
<li><a href="#delete">用語を削除する</a></li>
</ol>

</div>

logaling-command を使うための準備をする
------------------------------------------


### <a id="install">1. logaling-command をインストールする</a> ###

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
	logaling-command version 0.0.8

上記のようなバージョン情報が表示されたら、インストールは成功です。

#### <a id="requirements">Requirements</a> ####
※logaling-command は Ruby 1.9 環境で動作します。
また、内部では groonga、rroonga を利用しています。もしインストールされていなければ、 logaling-command をインストールすると同時に groonga と rroonga もインストールされます。

<ul class="listMark">
<li> <a href="http://www.ruby-lang.org/ja/">Ruby 1.9.x</a></li>
<li> <a href="http://groonga.org/ja/">groonga</a></li>
<li> <a href="http://groonga.rubyforge.org/index.html.ja">rroonga</a></li>
</ul>

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


------
<a id="kome1">※1言語の名称の略号2文字（ja や en など）<http://ja.wikipedia.org/wiki/ISO_639></a>

<p class="toTop"><a href="#index">目次へ戻る</a></p>



#### <a id="register">2-2. 既に logaling-command を使用しているプロジェクトに参加する</a> ####

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








logaling-command を使う
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




#### <a id="lookup-csv">1-1. プロジェクトに既にある対訳用語集を logaling-command で検索できるようにする</a> ###

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


----------
<a id="kome2">※2ファイルの文字コードはUTF-8のみに対応しています。</a>

<p class="toTop"><a href="#index">目次へ戻る</a></p>


#### <a id="import">1-2. 有名プロジェクトの用語集を logaling-command で検索できるようにする</a> ###

自分が参加しているプロジェクト以外で、同じ用語がどのように訳されているのかを知りたい場合があるかもしれません。 *loga import* で、いくつかの有名プロジェクトで利用されている用語集をインポートすることができます。

まずは、インポートできるプロジェクトの種類がどれくらいあるのかを知るために、以下のコマンドを実行して下さい。

	% loga import --list
	debian_project : Debian JP Project (http://www.debian.or.jp/community/translate/)
	gnome_project : GNOME Translation Project Ja (http://live.gnome.org/TranslationProjectJa)
	postgresql_manual : PostgreSQL7.1 Manual (http://osb.sraoss.co.jp/PostgreSQL/Manual/)

コマンドの結果から、インポート出来る用語集が3種類あることがわかりました。それでは、このうちの postgresql_manual をインポートしてみましょう。実際に用語集をインポートするためには *loga import* に用語集名をパラメータとして渡してあげます。この用語集名は上記のリストのコロンの前にある項目です。

	% loga import postgresql_manual

これでインポートができたので、試しに検索してみましょう。

	% loga lookup db
	DBA        DBA         (postgresql_manual)

先ほどインポートした postgresql_manual から検索できました。

<p class="toTop"><a href="#index">目次へ戻る</a></p>




### <a name="add">2. 用語を登録する</a> ###
#### <a id="add-term">2-1. プロジェクトの対訳用語集に用語を登録する</a> ###
プロジェクトの対訳用語集に新たに用語を登録するには次のようにします。

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




#### <a id="config">2-2. 一つのプロジェクトで複数言語への翻訳が同時に進行している場合</a> ###
一つのプロジェクトで複数言語への翻訳が同時に進行している場合には、プロジェクトのリポジトリに含める設定としての「翻訳言語（target-language）」を決めたくない場合があるかもしれません。
例えば、元のドキュメントは英語で、それを日本語とフランス語に翻訳したい。さらに、後から別の言語にも翻訳していく可能性があるような場合です。

そういう場合には、プロジェクトのトップレベルにある *.logaling/config* は以下のように *--target-language* が省略されているでしょう。

	% cat .logaling/config
	--glossary groonga
	--source-language en

この状態で *loga add* しようとすると、

	% loga add email Eメール
	input target-language code '-T <target-language code>'

このように翻訳言語の言語コードを指定するようにメッセージが表示されて追加できません。
もちろん、1つの用語だけを用語集に追加したいだけなら、このメッセージ通りにオプションとして翻訳言語を指定することで問題なく追加することができます。
ですが、次々に用語を追加していきたい場合には毎回オプションで翻訳言語の言語コードを指定するのは面倒です。

そのような場合には *loga config* を利用します。

	% loga config target-language ja --global
	Successfully set config.

`loga config [設定キー] [設定の値] --global` とすると、プロジェクトの .logaling/config ではなく、ユーザーホームの *.logaling.d/config* という設定ファイルが書き換えられます(ファイルが無かった場合には作成され設定が記述されます)。

	% ls -l ~/.logaling.d
	total 8
	drwxr-xr-x   5 suzuki  suzuki  170 12 14 15:11 cache
	drwxr-xr-x  18 suzuki  suzuki  612 12 15 17:33 db
	-rw-r--r--   1 suzuki  suzuki   61 12 16 15:38 config
	drwxr-xr-x   4 suzuki  suzuki  136 12 15 17:33 projects

この設定はグローバルな設定となるので、どこの階層にいても参照されることになります。設定ファイルの中身は .logaling/config と同じですが logaling-command は実行時に、 **コマンドラインオプション ＞ プロジェクトごとの設定 ＞ ユーザホームのグローバルな設定** という順序で設定を参照します。
.logaling.d/config に自分専用のグローバルな設定を持つことができるので、一つのプロジェクトで複数の言語への翻訳が同時進行していても普段と同じように用語集の編集ができるようになります。

*loga config* の使い方の詳細は[コマンドリファレンス](reference.html#config)を参照して下さい。

<p class="toTop"><a href="#index">目次へ戻る</a></p>




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
