---
layout: content
title:  "Sample Captures"
categories: tcpreplay wiki
description: "Tcpreplay ツール群で使うサンプルキャプチャファイル／List of sample network capture files for use with Tcpreplay suite"
---

- [概要／Overview][Overview](#overview)
    - [小さな flow／smallFlows.pcap][smallFlows.pcap](#smallflows-pcap)
    - [大きな flow／bigFlows.pcap][bigFlows.pcap](#bigflows-pcap)
    - [テスト／test.pcap][test.pcap](#test-pcap)
- [Contributions][Contributions](#contributions "Project Contributions")


<h2 id="Sample Captures"><a name="overview"></a>概要／Overview</h2>
Tcpreplay ツール群はたいていのキャプチャファイルを読み込めます。
簡単に試してみるために、サンプルのキャプチャファイルを用意しました。
[IP Flow][flow]/[NetFlow][NetFlow] をテストするために準備されたものですが、
スイッチやネットワーク機器のパフォーマンスを試験するのにも使えます。

pcap ファイルは [こちら／here][pcaps] にあります。

### <a name="smallflows-pcap"></a>[小さな flow／smallFlows.pcap][small]
このキャプチャファイルは、
いくつかの異なるアプリケーションのキャプチャファイルから作りました。
比較的ネットワークトラフィックが少ない環境において、
様々なプロトコルでたくさんの flow を作るように考えられています。
小さなサイズのファイルでたくさんの flow を使いたい場合や、
flow の組み合わせの再現性に配慮しなくて良い場合などは、
このキャプチャファイルを使ってみてください。

Size:			9.4 MB   
Packets:		14261   
Flows:			1209   
Average packet size:	646 bytes   
Duration:		5 minutes   
Number Applications:	28


### <a name="bigflows-pcap"></a>[大きな flow／bigFlows.pcap][big]
このキャプチャファイルは、
混雑したプライベートネットワークのインターネットへの出口における、
実際のネットワークトラフィックをキャプチャしたものです。
前のキャプチャファイル([小さな flow／smallFlows.pcap][small])と比べると、
ファイルサイズはかなり大きく、パケットの平均サイズは小さいです。
異なるアプリケーションのたくさんの flow が含まれています。
大きなサイズのファイルを取り扱えるのであれば、
このファイルでテストしてください。

Size:			368 MB   
Packets:		791615    
Flows:			40686  
Average packet size:	449 bytes   
Duration:		5 minutes   
Number Applications:	132

### <a name="test-pcap"></a>[テスト／test.pcap][test]
ソースコードの tar.gz に同梱されている小さなキャプチャファイルです。
`sudo make test` の時に *tcpreplay* の精度をテストするのにも使われます。
*tcpprep* の性能をデモするのにも使えます。

Size:           0.07 MB   
Packets:        141    
Flows:          37  
Average packet size:    445 bytes   
Duration:       3 seconds   
Number Applications:    1


## <a name="contributions"></a>Contributions／Contributions
Tcpreplay で動かすのに良いサンプルの pcap ファイルがあるなら、
ぜひメンテナに連絡してください。

[flow]:     https://ietf.org/wg/ipfix/
[NetFlow]:  http://www.cisco.com/go/netflow
[pcaps]:    https://www.dropbox.com/sh/j8i8v1nujvbumyb/ujd3biK6Fk/captures
[small]:    https://dl.dropboxusercontent.com/u/98306176/captures/smallFlows.pcap
[big]:      https://dl.dropboxusercontent.com/u/98306176/captures/bigFlows.pcap
[test]:     https://dl.dropboxusercontent.com/u/98306176/captures/test.pcap
