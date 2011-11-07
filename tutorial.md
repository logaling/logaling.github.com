---
layout: default
title: チュートリアル
---

# チュートリアル

## インストールする

インストールするには gem コマンドを実行します。

    % gem install logaling


## 用語集を作成する

用語集を作成してみます。用語集を作成するには、任意のディレクトリで以下のコマンドを実行します。

    % loga --glossary <用語集名> --source_term <原文言語> --target_term <翻訳言語>

例えば、 **logaling-project** というプロジェクト名で、英語から日本語へ翻訳するための用語集だと以下のようになります。

    % loga create --glossary logaling-project --source_term en --target_term ja

このコマンドを実行すると、あなたのホームディレクトリに **.logaling.d** というディレクトリが作成され、そのなかに、 **用語集名.原文言語.翻訳言語.yml** という空のファイルが作成されたと思います。それが、用語集ファイルです。

もしかしたら、プロジェクトの git リポジトリ内で共有するなどして .logaling.d 以外のディレクトリに用語集ファイルを置いておきたいことがあるかもしれません。
その場合は、`--logaling_home </path/to/logalinghome>` として、ファイルの置き場所のディレクトリを指定することも可能です。


## 用語を登録する

用語集へ用語を登録するには、以下のコマンドを実行します。

    % loga --glossary <用語集名> --source_term <原文言語> --target_term <翻訳言語> --source_term <用語> --target_term <対訳> --note <備考（省略可）>

先ほど作成した用語集 **logaling-project** へ **「user」** という用語を **「ユーザ」** という対訳を付けて登録してみましょう。

    % loga add --glossary logaling-project --source_term en --target_term ja --source_term user --target_term 'ユーザ' --note 'ユーザーではない'

備考やメモも `--note <備考やメモ>` として同時に登録することができます。
このコマンドを実行すると .logaling.d 下の用語集ファイルに yaml 形式で用語が登録されます。
実際に登録されたファイルの中身はこんな感じです。

    - source_term: user
      target_term: ユーザ
      note: ユーザーではない



## 設定ファイルを作成する

ここまでで、コマンド実行時のオプションの指定が多いと感じるかもしれません。その場合には、設定ファイルに設定をまとめてしまいましょう。
loga コマンドを利用するディレクトリ内に **.logaling** というファイルを作成します。
例えば、あるプロジェクトのディレクトリ内で実行するような場合は、そのプロジェクトのディレクトリ直下に作ると良いでしょう。loga コマンド実行時に root まで .logaling ファイルを探して、見つかった場合にはその設定が利用されます。
.logaling ファイルには、コマンドラインオプションを1行につき1つ、そのまま書いていきます。
例えば、以下のようになります。

    --glossary logaling-project
    --source_term en
    --target_term ja
    --logaling_home /path/to/logalinghome

この設定ファイルがあると、前述の用語の登録は以下のようになります。

    % loga add --source_term user --target_term 'ユーザ' --note 'ユーザーではない'

また、 `--source_term` と `--target_term` と `--note` はそれぞれ、 `-s` と `-t` と `-n` に省略可能なので、これを使うと上記はさらに以下のようになります。

    % loga add -s user -t 'ユーザ' -n 'ユーザーではない'

だいぶ簡単になりました。


## 用語を検索する

登録された用語を検索してみましょう。
まずは、検索するための準備として、groonga DB に用語集を登録します。

    % loga index

このコマンドを実行すると、 .logaling.d の下に **.logadb** というディレクトリが作られます。そしてその中に groonga の DB が作成され、 .logaling.d 以下にある用語集が全てその DB に登録されます。

これで、検索の準備ができました。用語を検索してみましょう。
検索するには `loga lookup` として、 `--source_term（または -s ）` で用語を指定します。

    % loga lookup -s <用語>

この lookup では、.logaling 設定ファイルで指定された用語集だけでなく、.logaling.d 以下に置いてあった複数の用語集にまたがって検索を行います。その際、自分の用語集からの検索結果が一番上に表示されます。
先ほど登録した「user」を検索してみると、以下のような結果が表示されます。

    % loga lookup -s user
    
    lookup word : user
    
      user
      ユーザ
        note:ユーザーではない。
        glossary:logaling-project


## 用語を変更する

用語を変更するには、 `loga update` として、既に登録してある用語と対訳のペアと、`--new_target_term(または -nt )` で新しい対訳を指定します。

    % loga update -s <用語> -t <対訳> -n <備考> -nt <新しい対訳>

ここで、新しい対訳が省略されると、元あった用語と対訳のペアに対して、備考を更新します。また、備考を省略すると、元々の備考が適用されます。

先ほど登録した「user」の対訳を変更してみましょう。

    % loga update -s user -t ユーザ -n やっぱりユーザー --new_target_term ユーザー 

変更結果は `loga lookup` で確認できます。

    % loga index
    % loga lookup -s user
    
    lookup word : user
    
      user
      ユーザー
        note:やっぱりユーザー
        glossary:logaling-project


## 用語を削除する

用語を削除するには、 `loga delete` として、用語と対訳のペアを指定します。

    % loga delete -s <用語> -t <対訳>

実際に先ほどの「user」を削除するには以下のようにコマンドを実行します。

    % loga delete -s user -t ユーザー

検索して削除されていることを確認します。

    % loga index
    % loga lookup -s user
    source_term <user> not found


