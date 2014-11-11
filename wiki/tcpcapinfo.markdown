---
layout: content
title:  "tcpcapinfo"
categories: tcpreplay wiki
description: "tcpcapinfo - display information about a PCAP capture file"
---

- [概要／Overview](#overview)
- [一般的な使い方／Basic Usage](#basic-usage)
- [実行例／Example](#example)
- [マニュアル／Man pages](tcpcapinfo-man.html)

<h2><a name="overview"></a>概要／Overview</h2>
*tcpcapinfo* は、
tcprewrite の問題で pcap ファイルを壊してしまう時の診断ツールとして登場しました。
正直に言って、pcap ファイルを読み書きするようなアプリケーションを作る人達だけに役立ちます。
しかし、Tcpreplay のコマンド群を完全なものにするために tcpcapinfo もリリースしました。
tcpcapinfo は，3.4.5 が最初のリリースでした。

<h2><a name="basic-usage"></a>一般的な使い方／Basic Usage</h2>

```
$ tcpcapinfo file.pcap
```

file.pcap ファイルを処理して、libpcap のパケット／ファイルのヘッダ情報を出力します。
パケットのチェックサムや、もし pcap ファイルの snaplen ヘッダも大きいパケットだったり、
タイムスタンプが過去に戻ってしまっている場合などは、
基本的なサニタイズチェックを実施します。
「パケットのチェックサム」は IP ヘッダのチェックサムでなく、
2つの Ethernet フレームが同じでないかどうかのチェックする、
ということなので注意してください。

<h2><a name="example"></a>実行例／Example</h2>

```
$ tcpcapinfo smallFlows.pcap | head -n 30
file size   = 9444731 bytes
magic       = 0xa1b2c3d4 (tcpdump) (little-endian)
version     = 2.4
thiszone    = 0x00000000
sigfigs     = 0x00000000
snaplen     = 65535
linktype    = 0x00000001
Packet	OrigLen		Caplen		Timestamp	Csum	Note
1	 997		 997		4d3f1be6.76439	a413bd	OK
2	 440		 440		4d3f1be6.7d8ca	504113	OK
3	  66		  66		4d3f1be6.acec4	8db5c	OK
4	  54		  54		4d3f1be6.ae468	9c35b	OK
5	  66		  66		4d3f1be6.b1812	8db5c	OK
6	  54		  54		4d3f1be6.b1841	8e75c	OK
7	 998		 998		4d3f1be6.b19a3	a036c1	OK
8	  54		  54		4d3f1be6.b677e	8e75c	OK
9	  60		  60		4d3f1be6.b6bc3	7e75d	OK
10	 541		 541		4d3f1be6.b9cf8	65fffd	OK
11	  54		  54		4d3f1be6.b9d50	7e75d	OK
12	  60		  60		4d3f1be6.bb339	7e75d	OK
13	  66		  66		4d3f1be6.e2980	9ae5b	OK
14	  66		  66		4d3f1be6.e7223	bae59	OK
15	  54		  54		4d3f1be6.e727c	aba5a	OK
16	 231		 231		4d3f1be6.e7577	200944	OK
17	  60		  60		4d3f1be6.ebe8b	aba5a	OK
18	1484		1484		4d3f1be6.ec419	e92376	OK
19	 345		 345		4d3f1be6.ec752	3b9728	OK
20	  54		  54		4d3f1be6.ec76f	9ba5b	OK
21	 329		 329		4d3f1be6.ed5a1	48a71b	OK
22	 280		 280		4d3f1be6.f27e8	3ad829	OK
```
