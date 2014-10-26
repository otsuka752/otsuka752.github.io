---
layout: content
title:  "サポート／Support"
categories: tcpreplay wiki
description: "Tcpreplay support page"
---

<h2 id="Support">ヘルプを受けるには／How To Get Help</h2>
トラブルですか？ [tcpreplay-users メーリングリスト][maillist] に質問してみてください。
ただし、メーリングリストに投稿する前にドキュメント
(man ページ、FAQ、/doc 配下の文章、メーリングリストのアーカイブ)
を読むことを強くお勧めします。

もしバグがあると思われるなら
[このページ][issues] からバグレポートを出せます。
私達があなたを助けるためにも、十分な情報を出すことが重要です

もし tcpreplay のコンパイルに関する問題であれば，下記の情報を出してください:

* コンパイルしようとしている tcpreplay のバージョン
* プラットフォーム(Red Hat Linux 9 on x86, Solaris 7 on SPARC, OS X on PPC, など)
* config.status の内容
* **configure** と **make** の実行結果
* 他に有用だと思われる追加情報

もし tcpreplay や他のツールを実行する場合の問題であれば、下記の情報を出してください:

* バージョン情報('コマンド -V' (例えば 'tcpreplay -V') の結果)
* コマンドの実行方法(オプションと引数)
* プラットフォーム (Red Hat Linux 9 on Intel, Solaris 7 on SPARC, など)
* ネットワークカード(NIC)のベンダーと型名とドライバーとバージョン
* エラーメッセージと，可能であれば問題の概要
* 可能であれば使用している pcap ファイルを添付してください(gzip2 や gzip で圧縮するのが望ましいです)
* 可能であれば，コアダンプやバックとレースファイル
* 問題やあなたが実行しようとしていることの詳細

注意：tcpreplay のメンテナは主に OS X と Linux を利用しています。
従って、別のプラットフォームに関するレポートは、
メンテナが再現できるように詳細な情報を出すことが重要です。

最後に、メンテナに直接メールで質問しないでください。
メーリングリストに表示されないため、
他のメンバーがあなたをヘルプできなくなります。

[maillist]: maillist.html
[issues]:   https://github.com/appneta/tcpreplay/issues
