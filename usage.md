---
layout: default
title: 詳しい使い方
---

詳しい使い方
============


<div id="indexDivision">
<h2><a id="index">目次</a></h2>
<h3>logaling-command を使うための準備をする</h3>
<ol>
<li><a href="#install">logaling-command をインストールする</a></li>
<li>logaling-command を使う準備をする
  <ol>
  <li><a href="#new">プロジェクトに logaling-command を導入する場合</a></li>
  <li><a href="#register">既に logaling-command を使用しているプロジェクトに参加する場合</a></li>
  <li><a href="#new-personal">個人用途で logaling-command を使う場合</a></li>
  </ol>
</li>
</ol>



<h3>logaling-command を使う</h3>

<ol>
<li>用語を検索する
  <ol>
  <li><a href="#lookup">用語を検索する</a></li>
  <li><a href="#dictionary">logaling-command を辞書検索として使う</a></li>
  <li><a href="#output">検索結果を特定の形式で出力する</a></li>
  </ol>
</li>
<li>用語を登録する
  <ol>
  <li><a href="#add-term">プロジェクトの対訳用語集に用語を登録する</a></li>
  <li><a href="#config">一つのプロジェクトで複数言語への翻訳が同時に進行している場合</a></li>
  </ol>
</li>
<li><a href="#update">登録されている用語を編集する</a></li>
<li><a href="#delete">用語を削除する</a></li>
<li>用語集をインポートする
  <ol>
  <li><a href="#import-csv">CSV / TSV 形式の用語集が既にあるのでそれをインポートしたい</a></li>
  <li><a href="#import">有名プロジェクトの用語集を logaling-command にインポートしたい</a></li>
  <li><a href="#import-tmx">TMX(Translation Memory eXchange)形式の用語集を logaling-command にインポートしたい</a></li>
  </ol>
</li>
<li><a href="#copy">用語集をコピーする</a></li>
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



### 2. logaling-command を使う準備をする ###

#### <a id="new">2-1.プロジェクトに logaling-command 導入する場合</a> ####

プロジェクトに logaling-command を導入するための準備をします。

例えば、あなたが groonga というプロジェクトに関わっていて、そのリポジトリが /Users/suzuki/groonga に既に存在するとします。そして、そのプロジェクトで英語→日本語の用語集を作成したいとしましょう。その場合は、そのリポジトリのトップディレクトリで以下のコマンドを実行して下さい。(
新たに用語集は作らなくてもいい、既存の用語集をとりあえず使いたい、などという場合も、このコマンドを実行する必要があります。ただし、他プロジェクトの用語集から検索したいだけの場合はこの操作は必要ありません。)


	% cd /Users/suzuki/groonga
	% loga new groonga en ja
	Your project is now registered to logaling.
	Successfully created .logaling

`loga new` はプロジェクトで用語集を使うための設定ファイルやディレクトリなどを作成します。パラメータは「用語集名」と「原文の言語コード[※1](#kome1)」「対訳の言語コード(省略可能）」をこの順番で指定します。用語集名は必ずしもプロジェクト名と同じでなくても構いません。ですが、後々プロジェクトを横断して検索を掛けたときにわかりやすくするためにも、プロジェクト名が分かるようにしておいたほうが良いでしょう。

さて、上記のコマンドを実行すると、 .logaling というディレクトリが作成されました。.logaling の中は次のようになっています。

	.logaling
	├── config
	└── glossary

config はこのプロジェクト内で logaling-command を使うときの設定ファイルです。中身は次のようになっています。用語集名、原文の言語コード、対訳の言語コードが設定されています。

	% cat .logaling/config
	--glossary groonga
	--source-language en
	--target-language ja

glossary は、このプロジェクトの用語集を置くためのディレクトリです。現時点では何も用語集は置かれていません。

`loga new` は上記を行うと共に、ユーザーホームに .logaling.d/projects というディレクトリを作成し、そのディレクトリ配下にプロジェクトを登録します。

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

`loga register` を実行すると、データベースがなければ作成され、その中にプロジェクトが登録されます。

プロジェクトが登録されたかどうかは以下のコマンドで確認することができます。(ここでは、 groonga プロジェクトを例とします。)

	% loga list
	groonga

これで logaling-command を使うための準備が整いました。

<p class="toTop"><a href="#index">目次へ戻る</a></p>



#### <a id="new-personal">2-3.個人用途で logaling-command を使う場合</a> ####

ここまではオープンソースまたはそうでないプロジェクトで logaling-command を導入するやり方を紹介してきましたが、ここでは個人用の用語集を作りたい場合のやり方を紹介します。

用語集を作りたいと思ったら、任意のディレクトリで以下のコマンドを実行して下さい。

これは 2-1 で説明したコマンドと同じコマンドですが、オプションとして `--personal` がつくところがポイントです。

	% loga new myglossary en ja --personal
	Successfully created /Users/suzuki/.logaling.d/personal/myglossary.en.ja.yml

パラメータは通常の`loga new`同様「用語集名」と「原文の言語コード」「対訳の言語コード」をこの順番で指定し、最後にオプションの`--personal`を付けます。「対訳の言語コード」はこの場合は必須となります。
個人的な用途の用語集なので用語集名は何でも構いませんが、既に作成済みの用語集名とは同じものは使えません。

上記コマンドを実行すると、ユーザーホームに .logaling.d/personal というディレクトリを作成され、そのディレクトリ配下に用語集が作成されます。

<p class="toTop"><a href="#index">目次へ戻る</a></p>







logaling-command を使う
----------------------

### 1. 用語を検索する ###
#### <a id="lookup">1-1. 用語を検索する</a> ###
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




#### <a id="dictionary">1-2. logaling-command を辞書検索として使う</a> ###

logaling-command の lookup サブコマンドは、対訳用語集に登録されている用語の検索を目的としているため、用語が検索対象となっています。
けれど、edict や gene95 といった辞書をインポートして使用している場合など、
訳語からも検索できたり、設定している言語コードとは異なる対訳用語集からも検索をしたいといったこともあります。
そのような場合には --dictionary オプションを使用します。
--dictionary オプションを使用すると、原文だけではなく、訳文からも検索を行い、完全マッチしたものから順に結果出力します。
検索の優先度は、原文の完全一致 > 原文の部分一致 > 訳文の完全一致 > 訳文の部分一致 となっています。

	% loga lookup インタフェース --dictionary
	  インタフェース           (n) (comp) interface/  edict
	    (中略)
	  ユーザインタフェース     (n) (comp) user interface/  edict
	  ユーザーインタフェース   (n) (comp) user interface/ edict
	  INTERFACES            インタフェース(.Sh)[jpman,section4]   freebsd_jpman
	  interface             インタフェース        postgresql_manual
	  interface             インタフェース(インターフェース,インターフェイスでなく)[POSIX,kana]   freebsd_jpman
	  terminal interface    端末インタフェース[POSIX]     freebsd_jpman

----------
※--dictionary は --dict でも同様に動作します。

<p class="toTop"><a href="#index">目次へ戻る</a></p>


#### <a id="output">1-3. 検索結果を特定の形式で出力する</a> ###

logaling-command は CUI ツールですが、テキストエディタから利用したい場合は多々あると思います。
そのような場合には、[関連プロジェクト](related-projects.html) にある
Emacs や Vim 、zsh などから logaling-command を利用できるツールを利用するか、あるいは自作することもあるかもしれません。
そういったツールで検索結果を扱う場合には検索結果が構造化されていてほしいものです。
そのような場合には --output オプションを使います。
lookup サブコマンド実行時に --output=csv (または、--output=json) をオプションとして指定することで、結果を指定した形式で出力することができます。

	% loga lookup patricia --output=csv
	  patricia-trie,パトリシアトライ,,en,ja

<p class="toTop"><a href="#index">目次へ戻る</a></p>




### <a name="add">2. 用語を登録する</a> ###
#### <a id="add-term">2-1. プロジェクトの対訳用語集に用語を登録する</a> ###
プロジェクトの対訳用語集に新たに用語を登録するには次のようにします。

	% loga add "storage engine" "ストレージエンジン" "groongaをベースとしたMySQLのストレージエンジン"

用語を登録する際には `loga add <用語> <対訳> <備考(省略可能)>` とすると、そのプロジェクトの対訳用語集（なければ作成して）に用語を登録していくことができます。

用語集に登録されている用語のリストは `loga show` で確認することができます。

	% loga show
	now index groonga...
	  storage engine    ストレージエンジン    #groongaをベースとしたMySQLのストレージエンジン

実際には用語集は YAML 形式のファイルで、`loga add` が最初に実行されると .logaling/glossary 以下に groonga.en.ja.yml というファイルが作成されます。これが logaling-command の用語集ファイルです。

	% ls -l .logaling/glossary
	total 16
	-rw-r--r--  1 suzuki  suzuki  61 11 24 14:41 groonga.en.ja.csv
	-rw-r--r--  1 suzuki  suzuki  99 11 24 15:14 groonga.en.ja.yml

試しに登録した内容を検索してみましょう。

	% loga lookup storage
	  storage engine    ストレージエンジン   # groongaをベースとしたMySQLのストレージエンジン

また、この用語登録はひとつの用語で複数の対訳を登録することが可能です。その場合も同じように `loga add` してください。

	% loga add "storage engine" "ストレージ・エンジン"

結果を検索すると、次のようになります。

	% loga lookup storage
	now index groonga...
	  storage engine    ストレージエンジン    # groongaをベースとしたMySQLのストレージエンジン
	  storage engine    ストレージ・エンジン


用語の登録時点で、訳語が定まっていないが用語は登録したいという場合はノートに @wip と書いて登録して下さい。

	% loga add "database" "database" "@wip 訳語未定"

そうしておくことで`loga show`コマンド使用時に *--annotation* オプションを指定するとノートに @wip と書かれた用語だけを表示させることができます。

	% loga show --annotation
	  database      database        # @wip 訳語未定


<p class="toTop"><a href="#index">目次へ戻る</a></p>




#### <a id="config">2-2. 一つのプロジェクトで複数言語への翻訳が同時に進行している場合</a> ###
一つのプロジェクトで複数言語への翻訳が同時に進行している場合には、プロジェクトのリポジトリに含める設定としての「翻訳言語（target-language）」を決めたくない場合があるかもしれません。
例えば、元のドキュメントは英語で、それを日本語とフランス語に翻訳したい。さらに、後から別の言語にも翻訳していく可能性があるような場合です。

そういう場合には、プロジェクトのトップレベルにある *.logaling/config* は以下のように *--target-language* が省略されているでしょう。

	% cat .logaling/config
	--glossary groonga
	--source-language en

この状態で `loga add` しようとすると、

	% loga add email Eメール
	input target-language code '-T <target-language code>'

このように翻訳言語の言語コードを指定するようにメッセージが表示されて追加できません。
もちろん、1つの用語だけを用語集に追加したいだけなら、このメッセージ通りにオプションとして翻訳言語を指定することで問題なく追加することができます。
ですが、次々に用語を追加していきたい場合には毎回オプションで翻訳言語の言語コードを指定するのは面倒です。

そのような場合には `loga config` を利用します。

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

`loga config` の使い方の詳細は[コマンドリファレンス](reference.html#config)を参照して下さい。

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



### 6. 用語集をインポートする ###
#### <a id="import-csv">6-1. CSV / TSV 形式の用語集が既にあるのでそれをインポートしたい</a> ###

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


#### <a id="import">6-2. 有名プロジェクトの用語集を logaling-command にインポートしたい</a> ###

自分が参加しているプロジェクト以外で、同じ用語がどのように訳されているのかを知りたい場合があるかもしれません。 `loga import` で、いくつかの有名プロジェクトで利用されている用語集をインポートすることができます。

まずは、インポートできるプロジェクトの種類がどれくらいあるのかを知るために、以下のコマンドを実行して下さい。

	% loga import --list
	debian_project : Debian JP Project (http://www.debian.or.jp/community/translate/)
	gnome_project : GNOME Translation Project Ja (http://live.gnome.org/TranslationProjectJa)
	postgresql_manual : PostgreSQL7.1 Manual (http://osb.sraoss.co.jp/PostgreSQL/Manual/)

コマンドの結果から、インポート出来る用語集が3種類あることがわかりました。それでは、このうちの postgresql_manual をインポートしてみましょう。実際に用語集をインポートするためには `loga import` に用語集名をパラメータとして渡してあげます。この用語集名は上記のリストのコロンの前にある項目です。

	% loga import postgresql_manual

これでインポートができたので、試しに検索してみましょう。

	% loga lookup db
	DBA        DBA         (postgresql_manual)

先ほどインポートした postgresql_manual から検索できました。

<p class="toTop"><a href="#index">目次へ戻る</a></p>


#### <a id="import-tmx">6-3. TMX(Translation Memory eXchange) 形式の用語集を logaling-command にインポートしたい</a> ###

翻訳ツールなどを利用して翻訳を行なっている場合には、CSV や TSV だけでなく、TMX 形式で用語集を扱っているかもしれません。
そのような場合にも  `loga import tmx <用語集名> <url/path 原文の言語コード> <訳文の言語コード>` で用語集をインポートすることができます。

例えば、TMX 形式の用語集ファイル groonga.tmx がカレントディレクトリにあるとします。その場合は以下のコマンドを実行することで用語集をインポートすることができます。

	% loga import tmx groonga ./groonga.tmx en ja

ここで、最後の「原文の言語コード」と「訳文の言語コード」ですが、英語( en )→日本語( ja )をデフォルトとして扱うため、同様の言語コードの場合は省略できます。
ですので、上記コマンドは以下のように実行しても、同様の動作をします。

	% loga import tmx groonga ./groonga.tmx

TMX 形式のファイルを指定するには、ローカルファイルのパスだけでなく、URL でも可能です。そのような場合でもコマンドパス指定と同様に以下のようになります。

	% loga import tmx groonga http://example.com/groonga.tmx


<p class="toTop"><a href="#index">目次へ戻る</a></p>




#### <a id="copy">6-4. 用語集をコピーする</a> ###

個人的な用途の用語集を作る際に、既存の用語集を流用したいことがあるかもしれません。
そのような場合には  `loga copy <元用語集名> <原文の言語コード> <訳文の言語コード> <新しい用語集名> <原文の言語コード> <訳文の言語コード>` で用語集をコピーすることができます。

例えば、既に存在している groonga プロジェクトの英→日の用語集を元に自分の用語集を作成したいという場合は、以下のコマンドを実行します。（自分の用語集は myglossary という名前にします）

	% loga copy groonga en ja myglossary en ja

あるいは、自分で作成した英→日の用語集を元にして英→仏の用語集を作り、後から訳語だけ編集したい、という場合は以下のようにします。

	% loga copy myglossary en ja myglossary en fr

コピー元とコピー先の言語コードは必ずしも一致しなくても問題はありません。あとで自分で内容を言語コードに合うように編集しましょう。


コピー元のプロジェクトの用語集が logaling-command で作成した用語集だけでなく、CSV や TSV の用語集を含んでいる場合はそれらをマージしたものを元用語集としてコピーします。

コピー元となる事ができる用語集はプロジェクトの用語集、または`loga new`時に`--personal`オプション付きで作成した個人の用語集です。インポートしてきた用語集は含まれません。


<p class="toTop"><a href="#index">目次へ戻る</a></p>

