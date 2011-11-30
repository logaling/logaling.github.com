k---
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




これで、loga