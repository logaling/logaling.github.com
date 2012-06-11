---
layout: logalimacs_home
title: logalimacs
---

# What's New

<!-- ## version 1.0.x -->
<!-- ?? 単語が見つからなかった場合text-translatorによるfallbackをできるようにしました -->
<!-- MELPAで自動でダウンロードした方がよいと思うが現状text-translatorはパッケージ依存の -->
<!-- フォーマットに対応してないのでtext-translatorの作者にpull request, -->
<!-- MELPAの作者にpull request後アナウンスしようと思っています -->

## version 1.0.1
1. Emacsリポジトリサイトのダウンロード方法がMermaladeからMELPAになりました
   (現在Marmaladeがpackageのアップロードができないようなので)
2. loga-popup-output-typeに'maxの様なシンボル記法も対応しました
3. bufferを上下した時に出る"p"とか"n"のキーボード入力の表示を出ないようにしました

## version 1.0.0
1. logalingの--dictionaryオプションに対応しました
2. 多階層ポップアップを利用するようにしました
3. ポップアップ時の幅を選択できるようにしました
4. viスタイルの移動方法の"j","k"で候補を上下に移動できるようになりました

## version 0.9.0
1. カーソル位置の単語を辞書から検索し、popupまたはbufferに表示
2. リージョンで指定した単語を辞書から検索し、popupまたはbufferに表示
3. ミニバッファに入力した文字から検索してbufferに表示
