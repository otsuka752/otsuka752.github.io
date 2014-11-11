---
layout: content
title:  "tcpliveplay"
categories: tcpreplay wiki
description: "tcpliveplay - replay a TCP capture file in a fashion that a remote server will respond to"
---

- [概要／Overview](#overview)
- [設計概要／Design Overview](#design-overview)
	- [再送信の成功と失敗／Successful vs. Unsuccessful Replays?](#successful-vs-unsuccessful-replays)
- [使い方／Usage](#usage)
- [サポートされる OS／Supported OS's](#supported-os's)
- [新規インストールガイド／Fresh Install Guide](#fresh-install-guide)
- [成功時の例／Examples of Successful Run](#examples-of-successful-run)
- [失敗時の例／Example of Unsuccessful Run](#example-of-unsuccessful-run)
- [Credits & Thanks／Credits & Thanks](#credits-&-thanks)
- [マニュアル／Man pages](tcpliveplay-man.html)

*Author & Publisher: Yazan Siam*   
*Email: tcpliveplay at gmail.com*    
*Updated: Dec. 28, 2013*   

<h2><a name="overview"></a>概要／Overview</h2>
*tcpliveplay* は、現在の Tcpreplay ツール群を拡張するために、
Cisco Systems がスポンサーとなりノースカロライナ州立大学
(North Carolina State University)のプロジェクトとしてスタートしました。
(tcplivplay の正式名称は 'New Conn' です。)
事前にキャプチャされた TCP のパケットを使い、
様々なデバイスにぜい弱性テストをしていく中で、
ぜい弱なデバイスを保護する必要性を強く感じてました。
ある特定の TCP パケットによって、
ネットワークデバイスがクラッシュしてしまうことがあるからです。
そこで、これらのぜい弱性からデバイスを守ろうとしている人達のソリューションとして、
このツールを提供しています。
ネットワークデバイスに対して新規に TCP コネクションを生成し、TCP 通信を再現できます。

<h2><a name="design-overview"></a>設計概要／Design Overview</h2>

*tcpliveplay* は、TCP のパケットを再送信できます。
このツールは、1つの TCP コネクションだけを含んだキャプチャファイルを再送信できます。
再送信する時は、ユーザの入力(pcap ファイル)をベースにして、
Layer2 と Layer 3 の情報だけが書き換わります。
例えば、キャプチャファイルに HTTP 通信のパケットが記録されている場合、
そのまま何も変更せずにリモートホストに送信されます。
ローカルホストのキャプチャファイルにキャプチャされている挙動や、
パケットが再送信された後の最後のアクションが、
リモートホストの期待通りの挙動だった場合のみ成功します。

TCP パケットの再送出プロセスを始める前に、
pcap ファイル(TCP のフローが 1つだけ保存されている)を読み込み、
キャプチャファイル全体の SEQ 番号と ACK 番号を把握します。
SEQ 番号と ACK 番号をベースに、再送出プロセスのイベントをスケジューリングします。
最初のパケット(SYN パケット)を送出するために、ランダムな番号を生成します。
そして、そのパケットをリモートホストに送出することで再送信プロセスを開始します。
リモートホストから正常なレスポンス(正常な SYN+ACK パケット)を受信すると、
TCP 3-way handshake は完了することになります。
TCP 3-way handshake 完了後は、すぐにパケットの再送信プロセスに遷移します。
tcpliveplay ツールが想定した範囲外の SEQ 番号と ACK 番号が戻ってくる場合には、
再送出プロセスは停止してしまいます。
ローカルホストの挙動と与えられた pcap ファイルを元にして、
tcpliveplay ツールは SEQ 番号や ACK 番号を想定しています。

<h3><a name="successful-vs-unsuccessful-replays"></a>再送信の成功と失敗／Successful vs. Unsuccessful Replays?</h3>

パケットの再送出は、リモートホストの挙動に依って失敗することもあるので注意してください。
tcpliveplay ツールは、事前にリモートホストの挙動を予想することしかできません。
リモートホストの挙動が tcpliveplay ツールの予想と異なる場合、
何度かパケットを再送信します。何度か試しても失敗する場合には、
tcpliveplay ツールは再送出できない理由を表示して停止します。
このような場合には、最初からパケット全体を再送出してみると良いです。
最後まで処理できるようになるかもしれません。
長い間通信していると、リモートホストの挙動が変わってしまい失敗することがあります。

<h2><a name="usage"></a>使い方／Usage</h2>

下記の書式でキャプチャした TCP 通信を再送出してください:

```
# tcpliveplay <device> <file.pcap> <Destination IP > <Destination MAC> <Source Port>
```

*Device*: eth0 や eth1 などの送出するデバイス

*file.pcap*: 再送出したい pcap ファイル。TCP 以外の全てのパケットはフィルタで削除してください。
1つの TCP フローだけを含む pcap ファイル以外は再送出できません。

*Destination IP*: 通信先リモートホストの IPアドレス

*Destination MAC*: 再送信するホストに直接接続された通信先の MACアドレス

*Source Port*: TCP の Srcポート番号。
特定のポートを使わない場合 "random" を指定すると、49152 から 65535 の間から自動的に決定されます。

再送信の性質上、カーネルの RST パケット送信機能を止める必要があります。
再送信はホストの NIC の前に割り込んでパケットを生成するためです。
下記を実行してください:

```
# sudo iptables -A OUTPUT -p tcp --tcp-flags RST RST -s <your ip> -d <dst ip> --dport <dst port, example 80 or 23 etc.> -j DROP
```

実行例:

```
# sudo iptables -A OUTPUT -p tcp --tcp-flags RST RST -s 10.0.2.15 -d 192.168.1.10 --dport 80 -j DROP
```

tcplivereplay の実行例:

```
# tcpliveplay eth0 sample1.pcap 192.168.1.5 52:51:01:12:38:02 random
# tcpliveplay eth0 sample2.pcap 192.168.1.5 52:51:01:12:38:02 52178
```

pcap ファイルの種類

tcpliveplay ツールは、1つの TCP フローだけが保存された pcap ファイルしか再送信できません。
将来的には、複数の TCP フローを同時に再送出できるようになる予定です。


<h2><a name="supported-os's"></a>サポートされる OS／Supported OS's</h2>

現在は Linux 環境だけがサポートされています。
Tcpreplay ツール群がサポートされる(Linux 以外の)プラットフォームでも実行できるようになる予定です。

<h2><a name="fresh-install-guide"></a>新規インストールガイド／Fresh Install Guide</h2>

Tcpreplay ツール群のインストールで問題がある場合、下記を試してみてください。
インストールしたばかりの Linux システムでの注意事項をまとめておきます:

1. `sudo apt-get remove libpcap0.8 tcpdump`
2. `sudo apt-get install libtool libc6 bison flex`
3. libpcap-1.2.1 と tcpdump-4.2.1 を [http://www.tcpdump.org/][tcpdump] からダウンロード
4. /usr/local 以下に展開
    * `sudo ./configure --prefix=/usr && sudo make && sudo make install`
    * `tar -C /usr/local -zxvf libpcap-1.2.1.tar.gz`
    * `tar -C /usr/local -zxvf tcpdump-4.2.1.tar.gz`
5.それぞれのディレクトリに移動(cd)して
    * `sudo ./configure`
    * `sudo make && sudo make install`
6. 標準の autogen を削除し: `sudo apt-get remove autogen`
7. [autogen 5.16.2][autogen] をインストール
8. 展開したディレクトリに移動(cd)し
    * `./configure`
    * `sudo make && sudo make install`
9. 下記のパッケージをインストール
10 `sudo apt-get install guile-1.8-libs libc6 libopts25 libopts25-dev libxml2 guile-1.8 guile-1.8-dev`
11. Issue: `mv /usr/lib/libopts.so.25 /usr/lib/~libopts.so.25`
12. Issue: `ln -s /usr/local/lib/libopts.so.25 /usr/lib/libopts.so.25`
13 code: 'git clone git@github.com:appneta/tcpreplay.git' をチェックアウトし
14. ディレクトリを移動(cd)し:
    * `./autogen.sh`
    * `./configure`
    * make && sudo make install

<h2><a name="examples-of-successful-run"></a>成功時の例／Examples of Successful Run</h2>

```
yhsiam:~$ sudo tcpliveplay eth0 sample1.pcap 192.168.1.5 52:51:01:12:38:02 random
[sudo] password for yhsiam: 
new source port:: 61487
Random Local SEQ: 859480898
Packets Scheduled 11
Sending Local Packet...............	[1]
Receiving Packets from remote host...
Received Remote Packet...............	[2]
Remote Pakcet Expectation met.
Proceeding in replay....
Sending Local Packet...............	[3]
Sending Local Packet...............	[4]
Receiving Packets from remote host...
>Received a Remote Packet
>>Checking Expectations
Received Remote Packet...............	[5]
Remote Packet Expectation met.
Proceeding in replay....
Receiving Packets from remote host...
>Received a Remote Packet
>>Checking Expectations
Received Remote Packet...............	[6]
Remote Packet Expectation met.
Proceeding in replay....
Sending Local Packet...............	[7]
Sending Local Packet...............	[8]
Receiving Packets from remote host...
>Received a Remote Packet
>>Checking Expectations
Received Remote Packet...............	[9]
Remote Packet Expectation met.
Proceeding in replay....
Receiving Packets from remote host...
>Received a Remote Packet
>>Checking Expectations
Received Remote Packet...............	[10]
Remote Packet Expectation met.
Proceeding in replay....
Sending Local Packet...............	[11]
----------------------------------------------------------------------------
- CONGRATS!!! You have successfully Replayed your pcap file ' sample1.pcap'  
----------------------------------------------------------------------------

----------------TCP Live Play Summary----------------
- Packets Scheduled to be Sent & Received: 	    11   
- Actual Packets Sent & Received:                   11   
- Total Local Packet Re-Transmissions due to packet       
- loss and/or differing payload size than expected: 5   
- Thank you for Playing, Play again!                      
----------------------------------------------------------
```

<h2><a name="example-of-unsuccessful-run"></a>失敗時の例／Example of Unsuccessful Run</h2>

```
yhsiam:~$ sudo tcpliveplay eth0 sample2.pcap 192.168.1.5 52:51:01:12:38:02 52139
new source port:: 52139
Random Local SEQ: 2095660524
Packets Scheduled 15
Sending Local Packet...............	[1]
Receiving Packets from remote host...
Received Remote Packet...............	[2]
Remote Pakcet Expectation met.
Proceeding in replay....
Sending Local Packet...............	[3]
Sending Local Packet...............	[4]
Receiving Packets from remote host...
>Received a Remote Packet
>>Checking Expectations
Received Remote Packet...............	[5]
Remote Packet Expectation met.
Proceeding in replay....
Receiving Packets from remote host...
>Received a Remote Packet
>>Checking Expectations
Received Remote Packet...............	[6]
Payload size of received packet does not meet expectations

+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
+ WARNING: Remote host is not meeting packet size expectations.               +
+ for packet 6. Application layer data differs from capture being replayed.  +
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Requesting retransmission.
 Proceeding...
Sending Local Packet...............	[7]
Sending Local Packet...............	[8]
Receiving Packets from remote host...
>Received a Remote Packet
>>Checking Expectations
Sending Local Packet...............	[7]
Sending Local Packet...............	[8]
Receiving Packets from remote host...
>Received a Remote Packet
>>Checking Expectations
Sending Local Packet...............	[7]
Sending Local Packet...............	[8]
Receiving Packets from remote host...
>Received a Remote Packet
>>Checking Expectations
Receiving Packets from remote host...
>Received a Remote Packet
>>Checking Expectations
Receiving Packets from remote host...
>Received a Remote Packet
>>Checking Expectations
Receiving Packets from remote host...
>Received a Remote Packet
>>Checking Expectations
Received Remote Packet...............	[9]
Payload size of received packet does not meet expectations

+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
+ WARNING: Remote host is not meeting packet size expectations.               +
+ for packet 9. Application layer data differs from capture being replayed.  +
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Requesting retransmission.
 Proceeding...
Sending Local Packet...............	[7]

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
+ ERROR: Re-sent packet [7] 3 times, but remote host is not  +
+ responding as expected. 3 resend attempts are a maximum.     +
+ Closing replay...                                            +
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

-------------------------------------------------------------------------
- Unfortunately an error has occurred  halting the replay of  
- the pcap file 'sample2.pcap'. Please see error above for details...   
-------------------------------------------------------------------------

----------------TCP Live Play Summary----------------
- Packets Scheduled to be Sent & Received: 	    15   
- Actual Packets Sent & Received:                   6   
- Total Local Packet Re-Transmissions due to packet       
- loss and/or differing payload size than expected: 8   
- Thank you for Playing, Play again!                      
----------------------------------------------------------
```

<h2><a name="credits-&-thanks"></a>Credits & Thanks</h2>

Thanks to Aaron Turner for providing extensive support throughout d
eveloping this tool. Aaron, you have been very helpful and very supportive. 
Thanks to Cisco Systems for sponsoring the project and providing expertise in 
the subject area, Mr. Panos Kampanakis. Thanks to Dr. Yannis Viniotis of the 
Electrical & Computer Engineering department at North Carolina State University 
for advising on this project and sponsoring this project with Cisco Systems.

[tcpdump]: http://www.tcpdump.org/#latest-release
[autogen]: http://sourceforge.net/projects/autogen/files/AutoGen/AutoGen-5.16.2/
