---
layout: default
title: コマンドリファレンス
---

コマンドリファレンス
====================

<a id="command">コマンド</a>
----------------------------
* [add](#add)
* [delete](#delete)
* [help](#help)
* [import](#import)
* [lookup](#lookup)
* [new](#new)
* [register](#register)
* [update](#update)
* [unregister](#unregister)
* [version](#version)



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
#### オプション
##### -g, [--glossary=用語集名]
用語集名を指定する
##### -S, [--source-language=原文の言語コード]
原文の言語コードを指定する
##### -T, [--target-language=訳文の言語コード]
訳文の言語コードを指定する
##### -h, [--logaling-home=logaling-command ホームディレクトリ]
logaling-command ホームディレクトリ



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
用語集名を指定する
##### -S, [--source-language=原文の言語コード]
原文の言語コードを指定する
##### -T, [--target-language=訳文の言語コード]
訳文の言語コードを指定する
##### -h, [--logaling-home=logaling-command ホームディレクトリ]
logaling-command ホームディレクトリ



### <a id="help">help</a> - ヘルプを表示する ###
#### 書式
loga help [タスク(省略可)]
#### 説明
ヘルプを表示します。タスク名が指定されない場合は全体のヘルプを、
指定された場合はそのタスクのヘルプを表示します。





### <a id="import">import</a> - 有名プロジェクトの用語集をインポートする ###
#### 書式
loga import [プロジェクト名]
#### 説明
logaling-command で利用できる形式の用語集が存在する
有名プロジェクトの用語集ファイルをインポートする。
#### オプション
##### --list
読み込むことが出来るプロジェクトを一覽表示する。





### <a id="lookup">lookup</a> - 用語を検索する###
#### 書式
loga lookup [用語]
#### 説明
指定された用語を検索する。複数のプロジェクトがインデックスされている場合は、
複数プロジェクトにまたがって横断的に検索する。
#### オプション
##### -g, [--glossary=用語集名]
用語集名を指定する
##### -S, [--source-language=原文の言語コード]
原文の言語コードを指定する
##### -T, [--target-language=訳文の言語コード]
訳文の言語コードを指定する
##### -h, [--logaling-home=logaling-command ホームディレクトリ]
logaling-command ホームディレクトリ




### <a id="new">new</a> - 用語集を新規作成する ###
#### 書式
loga new [用語集名] [原文の言語コード] [訳文の言語コード] [--no-register(省略可)]
#### 説明
プロジェクトの用語集を新規に作成します。
同時に logaling-command ホームディレクトリ（ユーザーホームディレクトリ/.logaling.d）に
プロジェクトのディレクトリへのシンボリックリンクを作成します。
#### オプション
##### -h, [--logaling-home=logaling-command ホームディレクトリ]
logaling-command ホームディレクトリ
##### --no-register
logaling-command ホームディレクトリにシンボリックリンクを作成したくない場合は指定します




### <a id="register">register</a> - logaling-command ホームディレクトリにシンボリックリンクを作成する###
#### 書式
loga register -g [用語集名]
#### 説明
ローカルディレクトリにある用語集を検索対象とするために
logaling-command ホームディレクトリに指定した用語集のシンボリックリンクを作成します。
#### オプション
##### -g, [--glossary=用語集名]
用語集名を指定する
##### -S, [--source-language=原文の言語コード]
原文の言語コードを指定する
##### -T, [--target-language=訳文の言語コード]
訳文の言語コードを指定する
##### -h, [--logaling-home=logaling-command ホームディレクトリ]
logaling-command ホームディレクトリ





### <a id="update">update</a> - 登録されている用語を編集する###
#### 書式
loga update [用語] [対訳] [新しい対訳] [新しい備考(省略可)]
#### 説明
指定された用語と対訳が用語集に存在していたら、対訳を新しい対訳で上書きする。
#### オプション
##### -g, [--glossary=用語集名]
用語集名を指定する
##### -S, [--source-language=原文の言語コード]
原文の言語コードを指定する
##### -T, [--target-language=訳文の言語コード]
訳文の言語コードを指定する
##### -h, [--logaling-home=logaling-command ホームディレクトリ]
logaling-command ホームディレクトリ





### <a id="unregister">unregister</a> - logaling-command ホームディレクトリに作成されているシンボリックリンクを削除する ###
#### 書式
loga unregister -g [用語集]
#### 説明
logaling-command ホームディレクトリに指定された用語集のシンボリックリンクが
存在する場合、そのシンボリックリンクを削除する。
#### オプション
##### -g, [--glossary=用語集名]
用語集名を指定する
##### -S, [--source-language=原文の言語コード]
原文の言語コードを指定する
##### -T, [--target-language=訳文の言語コード]
訳文の言語コードを指定する
##### -h, [--logaling-home=logaling-command ホームディレクトリ]
logaling-command ホームディレクトリ





### <a id="version">version</a> - logaling-command のバージョンを表示する ###
#### 書式
loga version
#### 説明
インストールされている logaling-command のバージョンを表示する。
