---
layout: content
title:  "コマンド群／Products"
categories: tcpreplay wiki
description: "Tcpreplay に含まれるユーティリティ一覧／List of utilities included with Tcpreplay suite"
---

<h2 id="Products">概要／Overview</h2>
Tcpreplay には下記のツールが含まれています:

###ネットワークの再生コマンド／Network playback products:
* [tcpreplay]({{ site.url }}tcpreplay.html) - pcap ファイルを任意のスピードでネットワークに再送信します。オプションを指定することでランダムな IPアドレスで送信できます。
* [tcpreplay-edit]({{ site.url }}tcpreplay-edit.html) - pcap ファイルを任意のスピードでネットワークに再送信します。オプションを指定することで動的にパケットを書き換えることができます。
* [tcpliveplay]({{ site.url }}tcpliveplay.html) - pcap ファイルに保存された TCP トラフィックを、リモートサーバの応答に合わせてネットワークに再送信します。


###pcap ファイルのエディタとユーティリティ／Pcap file editors and utilities:
* [tcpprep]({{ site.url }}tcpprep.html) - マルチパスな(複数の NIC から送信される) pcap ファイルに対して、パケットがクライアントのものかサーバのものかを判断してファイルに出力し、tcpreplay や tcprewrite で使用できるように前処理します。
* [tcprewrite]({{ site.url }}tcprewrite.html) - TCP/IP や Layer2 のヘッダを書き換えて pcap ファイルを編集します。
* [tcpcapinfo]({{ site.url }}tcpcapinfo.html) - 生の pcap file をデコードしてデバッグします。
* [tcpbridge]({{ site.url }}tcpbridge.html) - tcprewrite でヘッダを書き換えながら 2つのネットワークをブリッジできます。
