---
layout: content
title:  "Authors and Contributors"
categories: tcpreplay wiki
description: "Information regarding authors, contributors and history of Tcpreplay. Also contains information on how to contribute to the project."
---

Tcpreplay は Aaron Turner(@synfinatic) によって作られました。
2013 年に、[AppNeta Inc.](http://appneta.com) の創業者の 1人で
VP Advanced Technology である Fred Klassen (@fklassen)
がパフォーマンスを向上させ、
その後 Tcpreplay のメンテナとして引き継ぎました。
[the history of Tcpreplay][history] に詳細情報が記載されています。

ソースコードのレポジトリは GitHub に移動しました。
**git** をインストールし下記を実行することで
開発中のレポジトリをコピーできます:

```
git clone git@github.com:appneta/tcpreplay.git
```

## コントリビュートする方法／How To Contribute

簡単です。単に...

* [Set up git][git]
* [Fork]
* 編集 (issue ごとに branch を切ります)
* [Send a PR][pr]

### 詳細／Details:

appneta/tcpreplay のレポジトリをクローンしても、
Tcpreplay プロジェクトに直接はコントリビュートできないことに気付くでしょう。
レポジトリにコントリビュートすることを考えている場合には、
GitHub は革新的なアプローチを提供してくれています。
@appneta/tcpreplay レポジトリを fork することで、
開発者にソースコードを commit して良いかなどを質問することなしに、
あなた自身のレポジトリを作りコミットすることができるのです。
レポジトリを fork することは、賛辞を送ったともみなされます:

* もしまだアカウントを持っていないなら、フリーの [GitHub](https://github.com) アカウントを作成し @appneta/tcpreplay にアクセスしてください
* **Fork** ボタンをクリックしレポジトリを手元にコピーしてください
* あなたの環境でレポジトリを clone してください:

```
git clone git@github.com:<your ID>/tcpreplay.git
```

* **master** branch はいつでもリリースできるように保っておきたいので、機能ごとあるいは修正する bug ごとに branch を作ることをお勧めします
* うまく修正できたらあなたの GitHub レポジトリに push してください
* あなたの GitHub レポジトリで新しい branch を選択し **master** に *Pull Request** してください
* PR の状態は [ここ／here](https://github.com/appneta/tcpreplay/network) で確認できます

開発者達は、GitHub のサービス上であなたのコードをレビューし議論します。
提案を受け入れる場合には、プロダクションブランチの **master** にすぐに取り込まれます。

## 追加情報／Additional Information
[ユーザ向けの wiki／wiki](http://tcpreplay.appneta.com) を見てください。

あるいは [開発者向けの wiki／developers wiki](https://github.com/appneta/tcpreplay/wiki)
にアクセスしてください。

[Fork]:   https://help.github.com/articles/fork-a-repo
[pr]:     https://help.github.com/articles/using-pull-requests
[git]:    https://help.github.com/articles/set-up-git
[history]: history.html
