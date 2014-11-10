---
layout: content
title:  "How To..."
categories: tcpreplay wiki
description: "使い方に関する情報／How-to usage information"
---

- [概要／Overview](#overview)
- [IP Flow アプライアンスのパフォーマンス試験の方法](#how-to-do-a-performance-test-for-an-ip-flow-appliance)
	- [Flows Per Second を減らす](#reducing-flows-per-second)
	- [Netmap で Flows Per Second を増やす](#increasing-flows-per-second-with-netmap)
- [パケットをキャプチャするデバイスでのパフォーマンス試験の方法](#how-to-do-a-performance-test-for-a-packet-capture-device)

<h2 id="How To"><a name="overview"></a>概要／Overview</h2>
このセクションは、あなたがすぐに簡単に実行できるよう、
様々なシナリオを順を追って提供することにしています。

以下の使い方に関する例は、
[サンプルキャプチャ／sample captures][captures]
で提供されているファイルの 1つを使用しています。
i7 プロセッサで複数の Intel 82599 10GigE ポートを持つ 2台の物理マシンを使用します。
10GbE の NIC は、LAN ケーブルで直接接続されています。
SSH での LAN 接続は (10GbE でなく) 1GbE の NIC を使用します。
1GbE の NIC は eth0 ～ eth5 で、10GbE の NIC は eth6 ～ eth7 です。

2台のマシンとも、3.1.10 カーネルの Ubuntu 10.04 が動いてます。

このセクションに関連する動画のスクリーンショットがあります。
動画のリストは [ここ／here][playlist] にあり、また、
このページにも全てリストアップされています。
詳細な情報が必要であれば、動画のスクリーンショットはスキップし、
次のセクションに進んでください。

<iframe width="620" height="460" 
    src="//www.youtube.com/embed/videoseries?list=PLFjjcN5EvTP0Hsxq1AEh7FhvaFH__Vd83" 
    frameborder="0" 
    allowfullscreen>
</iframe>

## <a name="how-to-do-a-performance-test-for-an-ip-flow-appliance"></a>IP Flow アプライアンスのパフォーマンス試験の方法
[IP Flow][flow]/[NetFlow]
の統計情報をキャプチャしたりレポートしたりするデバイスにおいて
パフォーマンスの試験を実施するのであれば、このサンプルを使ってください。
この例では、[nProbe][nprobe] を使って DUT(Device Under Test)にパケットを送信しています。
*nprobe* のログイング機能を使って、
1秒あたりのパケットやバイトやフローを測定しています。

ワイヤーレートでのトラフィック送信能力や、
とてもたくさんの毎秒あたりのフローの生成は、
Tcpreplay v4.0.0 の新機能です。物理的なセットアップはとてもシンプルです。
我々の経験上では、Tcpreplay (の送信性能)は受信装置の性能よりも優れています。
従って、送信したトラフィックの合計の送信数や受信数を追跡することは重要で、
データの(パケットの)欠落数を決定します。
 
* 最新の [Tcpreplay][download] をダウンロードしてインストールします
* [bigFlows.pcap] をテストマシンにダウンロードします (詳細は wiki の [captures] に記載があります)
* DUT に [nProbe][nprobe] をインストールします
* DUT で nProbe を動作させます...

```
# nprobe -T "%IPV4_SRC_ADDR %IPV4_DST_ADDR %IPV4_NEXT_HOP %INPUT_SNMP \
%OUTPUT_SNMP %IN_PKTS %IN_BYTES %FIRST_SWITCHED %LAST_SWITCHED %L4_SRC_PORT \
%L4_DST_PORT %TCP_FLAGS %PROTOCOL %SRC_TOS %SRC_AS %DST_AS %IPV4_SRC_MASK \
%IPV4_DST_MASK" -i eth7
03/Jan/2014 20:50:24 [nprobe.c:2822] Welcome to nprobe v.6.7.3 for x86_64
03/Jan/2014 20:50:24 [plugin.c:143] No plugins found in ./plugins
03/Jan/2014 20:50:24 [plugin.c:143] No plugins found in /usr/local/lib/nprobe/plugins
03/Jan/2014 20:50:24 [nprobe.c:4004] Welcome to nprobe v.6.7.3 for x86_64
03/Jan/2014 20:50:24 [plugin.c:665] 0 plugin(s) enabled
03/Jan/2014 20:50:24 [nprobe.c:3145] Using packet capture length 128
03/Jan/2014 20:50:24 [nprobe.c:4222] Flows ASs will not be computed 
03/Jan/2014 20:50:24 [nprobe.c:4298] Capturing packets from interface eth7
03/Jan/2014 20:50:24 [util.c:3262] nProbe changed user to 'nobody'
```

* テストマシンでトラフィックを発生させます ...
    * `--unique-ip` オプションを使うことで
 loop ごとの IP アドレスがユニークになることが保障されるのでそれぞれ個別のフローになります。
    * `-K` オプションを使うことでテスト開始前に pcap ファイルがメモリに事前にロードされます。
十分なメモリが無いならこのオプションは省略してください。
    * `-t` オプションを使うとできる限り高速にパケットを送信します。
送信レートを下げたい場合は `-t` ではなく `--mbps` オプションで速度を指定してください。
    * この例では約 20秒間だけパケットを送信します。
数分間の間トラフィックを発生させるために `--loop` オプションの指定をお勧めします。

```
tcpreplay -i eth7 -tK --loop 50 --unique-ip bigFlows.pcap  
File Cache is enabled
Actual: 39580750 packets (17770889200 bytes) sent in 20.02 seconds.
Rated: 876866137.5 Bps, 7014.92 Mbps, 1953026.60 pps
Flows: 2034300 flows, 100378.13 fps, 39558950 flow packets, 21800 non-flow
Statistics for network device: eth7
	Attempted packets:         39580750
	Successful packets:        39580750
	Failed packets:            0
	Truncated packets:         0
	Retried packets (ENOBUFS): 0
	Retried packets (EAGAIN):  0

```

* DUT で届いたフローの統計情報を観測します ...
    * DUT で 100K flows/sec を受信していた場合でも、
どんな時でも処理できる訳ではないことに注意してください。
これは、多くの製品で言えることです。
    * フローのツールが要求する処理が増えた時に、データの欠落が増加します。
例えば、アプリケーションのタイプをクラス分けするなどのように
Deep Packet Inspection(DPI) を有効にした場合に、
データの欠落が増えると予想できます。
    * Mbps あたり 5 flows/sec の場合、10GbE では 50Kfps (毎秒 50,000 フロー)になります。

```
03/Jan/2014 20:52:30 [nprobe.c:1557] ---------------------------------
03/Jan/2014 20:52:30 [nprobe.c:1558] Average traffic: [1.870 M pps][6 Gb/sec]
03/Jan/2014 20:52:30 [nprobe.c:1563] Current traffic: [569.089 K pps][2 Gb/sec]
03/Jan/2014 20:52:30 [nprobe.c:1568] Current flow export rate: [0.0 flows/sec]
03/Jan/2014 20:52:30 [nprobe.c:1571] Flow drops: [export queue too long=0]
[too many flows=0]
03/Jan/2014 20:52:30 [nprobe.c:1575] Export Queue: 0/512000 [0.0 %]
03/Jan/2014 20:52:30 [nprobe.c:1582] Flow Buckets: [active=749448][allocated=749448]
[toBeExported=0][frags=0]
03/Jan/2014 20:52:30 [nprobe.c:1465] Processed packets: 13089188 (max bucket search: 162)
03/Jan/2014 20:52:30 [nprobe.c:1468] Flow export stats: [0 bytes][0 pkts][0 flows]
03/Jan/2014 20:52:30 [nprobe.c:1477] Flow drop stats:   [0 bytes][0 pkts][0 flows]
03/Jan/2014 20:52:30 [nprobe.c:1482] Total flow stats:  [0 bytes][0 pkts][0 flows]
03/Jan/2014 20:52:30 [nprobe.c:235] Packet stats (pcap): 13100072/234355 pkts 
rcvd/dropped [1.8%] [Last 13334427/234355 pkts rcvd/dropped]
03/Jan/2014 20:53:00 [nprobe.c:1557] ---------------------------------
03/Jan/2014 20:53:00 [nprobe.c:1558] Average traffic: [626.567 K pps][2 Gb/sec]
03/Jan/2014 20:53:00 [nprobe.c:1563] Current traffic: [336.465 K pps][1 Gb/sec]
03/Jan/2014 20:53:00 [nprobe.c:1568] Current flow export rate: [0.0 flows/sec]
03/Jan/2014 20:53:00 [nprobe.c:1571] Flow drops: [export queue too long=0]
[too many flows=0]
03/Jan/2014 20:53:00 [nprobe.c:1575] Export Queue: 0/512000 [0.0 %]
03/Jan/2014 20:53:00 [nprobe.c:1582] Flow Buckets: [active=1542197][allocated=1542197]
[toBeExported=0][frags=0]
03/Jan/2014 20:53:00 [nprobe.c:1465] Processed packets: 23183007 (max bucket search: 336)
03/Jan/2014 20:53:00 [nprobe.c:1468] Flow export stats: [0 bytes][0 pkts][0 flows]
03/Jan/2014 20:53:00 [nprobe.c:1477] Flow drop stats:   [0 bytes][0 pkts][0 flows]
03/Jan/2014 20:53:00 [nprobe.c:1482] Total flow stats:  [0 bytes][0 pkts][0 flows]
03/Jan/2014 20:53:00 [nprobe.c:235] Packet stats (pcap): 23183007/11039913 pkts 
rcvd/dropped [32.3%] [Last 20888493/10805558 pkts rcvd/dropped]
```

### <a name="reducing-flows-per-second"></a>Flows Per Second を減らす

フローのレートを 50Kfps に減らすには `--mbps` オプションで調整します ...

```
tcpreplay -i eth7 -K --mbps 3500 --loop 50 --unique-ip bigFlows.pcap 
File Cache is enabled
Actual: 39580750 packets (17770889200 bytes) sent in 40.06 seconds.
Rated: 437499777.1 Bps, 3499.99 Mbps, 974434.59 pps
Flows: 2034300 flows, 50082.23 fps, 39558950 flow packets, 21800 non-flow
Statistics for network device: eth7
	Attempted packets:         39580750
	Successful packets:        39580750
	Failed packets:            0
	Truncated packets:         0
	Retried packets (ENOBUFS): 0
	Retried packets (EAGAIN):  0
```

### <a name="increasing-flows-per-second-with-netmap"></a>Netmap で Flows Per Second を増やす
最大の送信速度が 7 Gbps 以上であることはすでに示しました。
キャプチャファイルの最大フローレートを増やすためには、
Tcpreplay が NIC の送信バッファーに直接パケットを書き込めるように
[netmap] をインストールする必要があります。


**注意:** *netmap* を使うことは(ネットワークスタックへの)侵入です。
実行する前に、カーネルモジュールのコンパイルのような作業が必要です。
また、テストの実行中は、
使用している NIC からネットワークスタックが切り離されることになります。
SSH を使っている場合、テストする NIC からログインしないでください。

* [netmap] の tarball をダウンロードして展開してください。
最終的には *netmap* をコンパイルすることになるとは思いますが、
*netmap* のコンパイルが必須という訳ではありません。
もし単に Tcpreplay をコンパイルしただけでも、
*netmap* との連携機能を持つことになります。
`/usr/src` や `/usr/local/src` に展開するのであれば、
Tcpreplay のソースコードのディレクトリに移動して ...

```
./configure

 ...

##########################################################################
             TCPREPLAY Suite Configuration Results (4.0.0)
##########################################################################
libpcap:                    /usr (>= 0.9.6)
libdnet:                    no ()
autogen:                    /usr/local/bin/autogen (5.16.2)
Use libopts tearoff:        yes
64bit counter support:      yes
tcpdump binary path:        /usr/sbin/tcpdump
fragroute support:          no
tcpbridge support:          yes
tcpliveplay support:        yes

Supported Packet Injection Methods (*):
Linux TX_RING:              no
Linux PF_PACKET:            yes
BSD BPF:                    no
libdnet:                    no
pcap_inject:                yes
pcap_sendpacket:            yes **
Linux/BSD netmap:           yes /usr/src/netmap-release
```

* どこか別のディレクトリに展開した場合は、*netmap* のソースの場所を指定する必要があります。
例えば ...

```
./configure --with-netmap=/home/fklassen/git/netmap/

 ...

##########################################################################
             TCPREPLAY Suite Configuration Results (4.0.0)
##########################################################################
libpcap:                    /usr (>= 0.9.6)
libdnet:                    no ()
autogen:                    /usr/local/bin/autogen (5.16.2)
Use libopts tearoff:        yes
64bit counter support:      yes
tcpdump binary path:        /usr/sbin/tcpdump
fragroute support:          no
tcpbridge support:          yes
tcpliveplay support:        yes

Supported Packet Injection Methods (*):
Linux TX_RING:              no
Linux PF_PACKET:            yes
BSD BPF:                    no
libdnet:                    no
pcap_inject:                yes
pcap_sendpacket:            yes **
Linux/BSD netmap:           yes /home/fklassen/git/netmap/
```

* *README* ファイルの手順に従って *netmap* をコンパイルする必要があります。
どのようにインストールするかの詳細は、このドキュメントのスコープ外です。
しかし、*netmap_lin* モジュールと、
netmap が有効な *ixgbe* 10GigE ドライバのインストールスクリプトを提供します ...

``` sh
#!/bin/sh
ifdown eth6
ifdown eth7
rmmod ixgbe
rmmod netmap_lin
insmod ./netmap_lin.ko
mknod /dev/netmap c 10 59
modprobe mdio
insmod ./ixgbe.ko
ifup eth6
ifup eth7
```
* *netmap* ドライバをインストールすれば、
*tcpreplay* は `--netmap` オプションを指定することで
netmap の機能を有効に使用できるようになります。
インストールしなければ、
ノーマルモードで動作し普通通りアプリケーションは動作します ...

    * リンクは飽和状態になっており、この pcap を 10 GbE のリンクに送信する場合は、
    135 Kfps が最大の送信レートということになります。

```
tcpreplay -i eth7 -tK --loop 50 --unique-ip --netmap bigFlows.pcap 
Switching network driver for eth7 to netmap bypass mode... done!
File Cache is enabled
Actual: 39580750 packets (17770889200 bytes) sent in 15.00 seconds.
Rated: 1182219090.4 Bps, 9457.75 Mbps, 2633133.19 pps
Flows: 2034300 flows, 135333.03 fps, 39558950 flow packets, 21800 non-flow
Statistics for network device: eth7
	Attempted packets:         39580750
	Successful packets:        39580750
	Failed packets:            0
	Truncated packets:         0
	Retried packets (ENOBUFS): 0
	Retried packets (EAGAIN):  0
Switching network driver for eth7 to normal mode... done!
```

* [smallFlows.pcap] をダウンロードすることで、
更に高い flows per second の状態を見ることができます。
このキャプチャファイルは小さなフローが保存されているので、
10GbE のリンクが飽和する前に 157 Kfps を送信できます。

   * *netmap* を使う場合、
キャプチャファイルに含まれているフローのサイズは、
最大の fps を決定するパラメータになります。
64 バイトのパケットだけが保存されたキャプチャファイルを使用することで、
18 Mfps のトラフィックを送信することができました。

```
tcpreplay -i eth7 -tK --loop 50 --unique-ip --netmap smallFlows.pcap 
Switching network driver for eth7 to netmap bypass mode... done!
File Cache is enabled
Actual: 713050 packets (460826550 bytes) sent in 0.385312 seconds.
Rated: 1195982865.8 Bps, 9567.86 Mbps, 1850578.23 pps
Flows: 60450 flows, 156885.84 fps, 712150 flow packets, 900 non-flow
Statistics for network device: eth7
	Attempted packets:         713050
	Successful packets:        713050
	Failed packets:            0
	Truncated packets:         0
	Retried packets (ENOBUFS): 0
	Retried packets (EAGAIN):  0
Switching network driver for eth7 to normal mode... done!
```

## <a name="how-to-do-a-performance-test-for-a-packet-capture-device"></a>パケットをキャプチャするデバイスでのパフォーマンス試験の方法
パケットをキャプチャするデバイスや、
*tcpdump* のようにパケットをキャプチャするアプリケーションをテストする場合、
セットアップは *IP Traffic Flow Appliance* に記載された手順に似ています。
*nprobe* を実行する代わりに、パケットキャプチャのアプリケーションを起動してください。

[bigFlows.pcap]:  https://dl.dropboxusercontent.com/u/98306176/captures/bigFlows.pcap
[smallFlows.pcap]:    https://dl.dropboxusercontent.com/u/98306176/captures/smallFlows.pcap
[netmap]:         http://info.iet.unipi.it/~luigi/netmap/
[captures]: captures.html
[download]: installation.html
[nprobe]:   http://www.ntop.org/products/nprobe/
[flow]:     https://ietf.org/wg/ipfix/
[NetFlow]:  http://www.cisco.com/go/netflow
[playlist]:   http://www.youtube.com/playlist?list=PLFjjcN5EvTP0Hsxq1AEh7FhvaFH__Vd83
