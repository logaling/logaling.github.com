---
layout: default
title: コマンドリファレンス
---

コマンドリファレンス
====================


<div id="indexDivision">
<h2><a id="commands">コマンド一覧</a></h2>
<ul class="listMark">
<li><a href="#add">add</a></li>
<li><a href="#config">config</a></li>
<li><a href="#copy">copy</a></li>
<li><a href="#delete">delete</a></li>
<li><a href="#help">help</a></li>
<li><a href="#import">import</a></li>
<li><a href="#index">import</a></li>
<li><a href="#list">list</a></li>
<li><a href="#lookup">lookup</a></li>
<li><a href="#new">new</a></li>
<li><a href="#register">register</a></li>
<li><a href="#show">show</a></li>
<li><a href="#update">update</a></li>
<li><a href="#unregister">unregister</a></li>
<li><a href="#version">version</a></li>
</ul>
</div>



リファレンス
------------

### <a id="add">add</a> - 用語集に用語を登録する ###
#### 書式
loga add [用語] [対訳] [備考(省略可)]
#### 説明
プロジェクトの用語集に用語を登録します。
用語と対訳と備考(省略可)を一つのセットとして登録します。
用語集は、そのプロジェクトのディレクトリ直下にある .logaling/config の
設定に従って登録されますが、オプションで指定することも可能です。
訳語が定まっていないが用語は登録したいという場合には、備考に @wip と記述して下さい。
#### オプション
##### -g, [--glossary=用語集名]
用語集名を指定します
##### -S, [--source-language=原文の言語コード]
原文の言語コードを指定します
##### -T, [--target-language=訳文の言語コード]
訳文の言語コードを指定します
##### -h, [--logaling-home=logaling-command ホームディレクトリ]
logaling-command ホームディレクトリ
##### -c, [--logaling-config=logaling-command プロジェクト設定ディレクトリ]
logaling-command プロジェクト設定ディレクトリ

<p class="toTop"><a href="#commands">コマンド一覧へ戻る</a></p>


### <a id="config">config</a> - 設定を書き換える ###
#### 書式
loga config [KEY] [VALUE]
#### 説明
プロジェクトまたはグローバルな設定を書き換えます。
設定ファイルのキー項目とそれに対応する値をセットで指定します。
利用できるキー項目は以下の4つです。

* source-language（原文の言語コード）
* target-language（訳文の言語コード）
* glossary（用語集名）

--global オプションを指定しない場合は、プロジェクトの .logaling/config に書かれている設定を指定された内容で書き換えます。
--global オプションを指定した場合は、ユーザホームにある .logaling.d/config に書かれている設定を指定された内容で書き換えます。
また、このグローバルな設定ファイルがない場合はファイルごと新しく作成します。
#### オプション
##### --global
グローバルな設定を書き換える場合は指定します

<p class="toTop"><a href="#commands">コマンド一覧へ戻る</a></p>


### <a id="copy">copy</a> - 用語集をコピーする ###
#### 書式
loga copy [元用語集名] [原文の言語コード] [訳文の言語コード] [新しい用語集名] [原文の言語コード] [訳文の言語コード]
#### 説明
既存の用語集をコピーして新しい用語集を作ります。

<p class="toTop"><a href="#commands">コマンド一覧へ戻る</a></p>


### <a id="delete">delete</a> - 用語集に登録されている用語を削除する ###
#### 書式
loga delete [用語] [対訳(省略可)]
#### 説明
プロジェクトの用語集から指定された用語を削除します。
用語集に登録されている用語がひとつの（対訳が複数個ではない）場合は対訳を省略できます。
対訳を省略して実行したときに、対訳が複数ある場合は、削除はできません。
ただし *--force* オプションを指定すると、対訳が複数ある用語でも一括で削除することができます。
#### オプション
##### --force
指定すると一つの用語で複数の対訳がある用語を一括で削除することができます。
##### -g, [--glossary=用語集名]
用語集名を指定します
##### -S, [--source-language=原文の言語コード]
原文の言語コードを指定します
##### -T, [--target-language=訳文の言語コード]
訳文の言語コードを指定します
##### -h, [--logaling-home=logaling-command ホームディレクトリ]
logaling-command ホームディレクトリ
##### -c, [--logaling-config=logaling-command プロジェクト設定ディレクトリ]
logaling-command プロジェクト設定ディレクトリ

<p class="toTop"><a href="#commands">コマンド一覧へ戻る</a></p>


### <a id="help">help</a> - ヘルプを表示する ###
#### 書式
loga help [タスク(省略可)]
#### 説明
ヘルプを表示します。タスク名が指定されない場合は全体のヘルプを、
指定された場合はそのタスクのヘルプを表示します。

<p class="toTop"><a href="#commands">コマンド一覧へ戻る</a></p>




### <a id="import">import</a> - 有名プロジェクトの用語集をインポートする ###
#### 書式
loga import [プロジェクト名]
#### 説明
logaling-command で利用できる形式の用語集が存在する
有名プロジェクトの用語集ファイルをインポートする。
#### オプション
##### --list
読み込むことが出来るプロジェクトを一覽表示する。

<p class="toTop"><a href="#commands">コマンド一覧へ戻る</a></p>





### <a id="index">index</a> - 検索用のインデックスを更新する ###
#### 書式
loga index
#### 説明
検索用のインデックスを更新する

<p class="toTop"><a href="#commands">コマンド一覧へ戻る</a></p>





### <a id="list">list</a> - 登録されている用語集名を一覽表示する ###
#### 書式
loga list
#### 説明
現在登録されている用語集名の一覽を表示します。
#### オプション
##### --no-pager
ページングを無効にします。

<p class="toTop"><a href="#commands">コマンド一覧へ戻る</a></p>




### <a id="lookup">lookup</a> - 用語を検索する###
#### 書式
loga lookup [用語]
#### 説明
指定された用語を検索します。複数のプロジェクトがインデックスされている場合は、
複数プロジェクトにまたがって横断的に検索します。
--output オプションを使用すると、出力形式を CSV 又は JSON に変えることができます。
通常 lookup では原文や訳文の言語コードを見て検索を行いますが、
--dictionary オプションを使用すると、言語コードを無視して検索できるようになります。
また、このオプションを使用すると、訳語の方でマッチした用語も結果として表示されます。

検索結果が多い場合は通常は自動でページングを行いますが、--no-pager オプションを使用すると
ページング無しで出力します。

#### オプション
##### -g, [--glossary=用語集名]
用語集名を指定します
##### -S, [--source-language=原文の言語コード]
原文の言語コードを指定します
##### -T, [--target-language=訳文の言語コード]
訳文の言語コードを指定します
##### -h, [--logaling-home=logaling-command ホームディレクトリ]
logaling-command ホームディレクトリ
##### -c, [--logaling-config=logaling-command プロジェクト設定ディレクトリ]
logaling-command プロジェクト設定ディレクトリ
##### --output, [--output=出力形式 ( csv 又は json )]
出力形式を指定します。CSVとJSONが利用できます。
##### --dictionary, [--dict]
原文/訳文の言語コードを無視して辞書的に検索を行います。訳語も検索対象とします。
##### --no-pager
ページングを無効にします。
##### --no-color
検索結果の色付けを無効にします。
##### --fixed
訳語が定まっている用語だけを検索します。


<p class="toTop"><a href="#commands">コマンド一覧へ戻る</a></p>



### <a id="new">new</a> - 用語集を新規作成する ###
#### 書式
loga new [用語集名] [原文の言語コード] [訳文の言語コード] [--no-register(省略可)]
#### 説明
プロジェクトの用語集を新規に作成します。
同時に logaling-command ホームディレクトリ（ユーザーホームディレクトリ/.logaling.d）に
プロジェクトのディレクトリへのシンボリックリンクを作成します。

ただし、*--personal* オプションを指定された場合は、プロジェクトの用語集ではなく個人用途の用語集を logaling-command ホームディレクトリ下の /personal ディレクトリ直下に作成します。

#### オプション
##### -h, [--logaling-home=logaling-command ホームディレクトリ]
logaling-command ホームディレクトリ
##### -c, [--logaling-config=logaling-command プロジェクト設定ディレクトリ]
logaling-command プロジェクト設定ディレクトリ
##### --no-register
logaling-command ホームディレクトリにシンボリックリンクを作成したくない場合は指定します
##### --personal
個人用途の用語集を作成します。

<p class="toTop"><a href="#commands">コマンド一覧へ戻る</a></p>



### <a id="register">register</a> - logaling-command ホームディレクトリにシンボリックリンクを作成する###
#### 書式
loga register
#### 説明
ローカルディレクトリにある用語集を検索対象とするために
logaling-command ホームディレクトリに指定した用語集のシンボリックリンクを作成します。
#### オプション
##### -S, [--source-language=原文の言語コード]
原文の言語コードを指定します
##### -T, [--target-language=訳文の言語コード]
訳文の言語コードを指定します
##### -h, [--logaling-home=logaling-command ホームディレクトリ]
logaling-command ホームディレクトリ
##### -c, [--logaling-config=logaling-command プロジェクト設定ディレクトリ]
logaling-command プロジェクト設定ディレクトリ

<p class="toTop"><a href="#commands">コマンド一覧へ戻る</a></p>



### <a id="show">show</a> - 用語集に登録されている用語を一覧表示する###
#### 書式
loga show -g [用語集名]
#### 説明
用語集に登録されている用語を一覽表示します。
logaling-command を利用しているプロジェクトのディレクトリ配下で
このコマンドを使用する場合は、用語集を明示的に指定しなくても
用語の一覽を表示することができます。
#### オプション
##### -g, [--glossary=用語集名]
用語集名を指定します
##### -S, [--source-language=原文の言語コード]
原文の言語コードを指定します
##### -T, [--target-language=訳文の言語コード]
訳文の言語コードを指定します
##### -h, [--logaling-home=logaling-command ホームディレクトリ]
logaling-command ホームディレクトリ
##### -c, [--logaling-config=logaling-command プロジェクト設定ディレクトリ]
logaling-command プロジェクト設定ディレクトリ
##### --no-pager
ページングを無効にします。
##### --annotation
ノートに @wip と書かれている用語だけを一覧表示する。
ノートに @wip と書かれている用語は、訳語が確定していないものとみなす。wip は work in progress の略。

<p class="toTop"><a href="#commands">コマンド一覧へ戻る</a></p>




### <a id="update">update</a> - 登録されている用語を編集する###
#### 書式
loga update [用語] [対訳] [新しい対訳] [新しい備考(省略可)]
#### 説明
指定された用語と対訳が用語集に存在していたら、対訳を新しい対訳で上書きする。
#### オプション
##### -g, [--glossary=用語集名]
用語集名を指定します
##### -S, [--source-language=原文の言語コード]
原文の言語コードを指定します
##### -T, [--target-language=訳文の言語コード]
訳文の言語コードを指定します
##### -h, [--logaling-home=logaling-command ホームディレクトリ]
logaling-command ホームディレクトリ
##### -c, [--logaling-config=logaling-command プロジェクト設定ディレクトリ]
logaling-command プロジェクト設定ディレクトリ

<p class="toTop"><a href="#commands">コマンド一覧へ戻る</a></p>




### <a id="unregister">unregister</a> - logaling-command ホームディレクトリに作成されているシンボリックリンクを削除する ###
#### 書式
loga unregister
#### 説明
logaling-command ホームディレクトリに指定された用語集のシンボリックリンクが
存在する場合、そのシンボリックリンクを削除する。
#### オプション
##### -S, [--source-language=原文の言語コード]
原文の言語コードを指定します
##### -T, [--target-language=訳文の言語コード]
訳文の言語コードを指定します
##### -h, [--logaling-home=logaling-command ホームディレクトリ]
logaling-command ホームディレクトリ
##### -c, [--logaling-config=logaling-command プロジェクト設定ディレクトリ]
logaling-command プロジェクト設定ディレクトリ

<p class="toTop"><a href="#commands">コマンド一覧へ戻る</a></p>




### <a id="version">version</a> - logaling-command のバージョンを表示する ###
#### 書式
loga version
#### 説明
インストールされている logaling-command のバージョンを表示する。

<p class="toTop"><a href="#commands">コマンド一覧へ戻る</a></p>
