---
layout: content
title:  "tcpreplay"
categories: tcpreplay wiki
description: "tcpreplay - replay a PCAP packet capture file"
---

- [概要／Overview](#overview)
- [基本的な使い方／Basic usage](#basic-usage)
- [実行例／Examples](#examples)
- [高度な使い方／Advanced Usage](#advanced-usage)
	- [ルータやスイッチのテスト／Testing Routers or Switches](#testing-routers-or-switches)
- [マニュアル／Man pages](tcpreplay-man.html)

<h2><a name="overview"></a>概要／Overview</h2>
*tcpreplay* は長い年月をかけて開発されてきました。
1.x の頃は、単にパケットを読み込みネットワークに再送信するだけでした。
2.x では、非常に多くの書き換え機能が実装されましたが、
複雑さやパフォーマンスやコードの肥大化を犠牲にしていました。
3.x では、パケットを送信するマシンになるという根本に立ち戻り、
パケットの書き換え機能は [tcprewrite][] と、
送信と書き換えの両方の機能を持った強力な [tcpreplay-edit][]
の 2つに移行されました。

4.x では [IP Flow][flow] と [netmap][nm] 機能が追加されました。
若干のパケット編集機能も追加されましたが、パフォーマンスを犠牲にはしていません。
本質的に、tcpreplay は高速であるべきで、
全てのオプションはワイヤーレートで動作するように設計されています。
パケットを編集しながら送信するようなパフォーマンスに影響を与えかねないオプションは、
[tcpreplay-edit][] に移行されてます。

<h2><a name="basic-usage"></a>基本的な使い方／Basic usage</h2>

与えられた pcap ファイルがキャプチャされたのと同様に再送信するには、
pcap ファイルとトラフィックを送信するインターフェイスを指定するだけです。
sample.pcap ファイルを `eth0` から送信するには:

```
# tcpreplay -i eth0 sample.pcap
```

<h2><a name="examples"></a>実行例／Examples</h2>
下記の例では、i7 プロセッサとマルチポートの Intel 82599 の 10GbE NIC を
取り付けたマシンで、[サンプルキャプチャファイル／sample captures][captures]
のファイルの 1つを使用しています。

デフォルトでは、pcap ファイルはキャプチャされた時と同じ速度で再送信されます。
特定の速度で再送信したい場合は、`--mbps` オプションを追加します。

```
# tcpreplay -i eth0 --mbps=510.5 smallFlows.pcap 
Actual: 14261 packets (9216531 bytes) sent in 0.144495 seconds.
Rated: 63784428.5 Bps, 510.27 Mbps, 98695.45 pps
Flows: 1209 flows, 8367.07 fps, 14243 flow packets, 18 non-flow
Statistics for network device: eth0
	Attempted packets:         14261
	Successful packets:        14261
	Failed packets:            0
	Truncated packets:         0
	Retried packets (ENOBUFS): 0
	Retried packets (EAGAIN):  0
```

パケットの送信間隔を 0 で再送信するには:

```
# tcpreplay -i eth0 --topspeed smallFlows.pcap 
Actual: 14261 packets (9216531 bytes) sent in 0.012960 seconds.
Rated: 711152083.3 Bps, 5689.21 Mbps, 1100385.80 pps
Flows: 1209 flows, 93287.03 fps, 14243 flow packets, 18 non-flow
Statistics for network device: eth0
	Attempted packets:         14261
	Successful packets:        14261
	Failed packets:            0
	Truncated packets:         0
	Retried packets (ENOBUFS): 0
	Retried packets (EAGAIN):  0
```

`-t` オプションは `--topspeed` と同じ意味です。`--mbps=0` でも同様です。
pcap ファイルを連続して送信するには `--loop` オプションを使い、
この時はパフォーマンスが多少向上します。

```
# tcpreplay -i eth0 -t --loop=1000 smallFlows.pcap 
Actual: 14261000 packets (9216531000 bytes) sent in 10.06 seconds.
Rated: 864516674.2 Bps, 6916.13 Mbps, 1337691.18 pps
Flows: 1209 flows, 113.40 fps, 14243000 flow packets, 18000 non-flow
Statistics for network device: eth0
	Attempted packets:         14261000
	Successful packets:        14261000
	Failed packets:            0
	Truncated packets:         0
	Retried packets (ENOBUFS): 0
	Retried packets (EAGAIN):  0
```

ここで、-K オプションによりパフォーマンスが向上したことが分かります。
-K オプションは disk でなく memory からキャプチャファイルを読み込みます。
pcap ファイルに対して十分なメモリがある時には、
このオプションの利用を検討したくなるでしょう。

```
# tcpreplay -i eth0 -t -K --loop=1000 smallFlows.pcap 
File Cache is enabled
Actual: 14261000 packets (9216531000 bytes) sent in 8.02 seconds.
Rated: 1114523106.8 Bps, 8916.18 Mbps, 1724533.23 pps
Flows: 1209 flows, 146.20 fps, 14243000 flow packets, 18000 non-flow
Statistics for network device: eth0
	Attempted packets:         14261000
	Successful packets:        14261000
	Failed packets:            0
	Truncated packets:         0
	Retried packets (ENOBUFS): 0
	Retried packets (EAGAIN):  0
```

<h2><a name="advanced-usage"></a>高度な使い方／Advanced Usage</h2>

ワイヤーレートに近い速度を得るためには、[netmap][nm]
ネットワークドライバをコンパイルしてインストールする必要があります。
netmap はテストのための duration のネットワークドライバをバイパスし、
*tcpreplay* がネットワークカードに直接書き込むことを許可します。
テストの間は、選択されたネットワークカードでネットワークスタックが
動作しないことに注意してください。

```
# tcpreplay -i eth0 -tK -l1000 --netmap smallFlows.pcap 
Switching network driver for eth0 to netmap bypass mode... done!
File Cache is enabled
Actual: 14261000 packets (9216531000 bytes) sent in 7.07 seconds.
Rated: 1193506409.4 Bps, 9548.05 Mbps, 1846746.34 pps
Flows: 1209 flows, 156.56 fps, 14243000 flow packets, 18000 non-flow
Statistics for network device: eth0
	Attempted packets:         14261000
	Successful packets:        14261000
	Failed packets:            0
	Truncated packets:         0
	Retried packets (ENOBUFS): 0
	Retried packets (EAGAIN):  0
Switching network driver for eth0 to normal mode... done!
```

*flows per second (fps)* を増やすには、
`--unique-ip` オプションを指定して
ループごとの IPアドレスを固定する必要があります。

```
# tcpreplay -i eth0 -tK -l1000 --netmap --unique-ip smallFlows.pcap  
Switching network driver for eth0 to netmap bypass mode... done!
File Cache is enabled
Actual: 14261000 packets (9216531000 bytes) sent in 7.07 seconds.
Rated: 1193507027.6 Bps, 9548.05 Mbps, 1846747.29 pps
Flows: 1209000 flows, 156561.07 fps, 14243000 flow packets, 18000 non-flow
Statistics for network device: eth0
	Attempted packets:         14261000
	Successful packets:        14261000
	Failed packets:            0
	Truncated packets:         0
	Retried packets (ENOBUFS): 0
	Retried packets (EAGAIN):  0
Switching network driver for eth0 to normal mode... done!
```

flows per second を制御するには
`--mbps` (or `-M`) オプションの値を変更して試します:

```
# tcpreplay -i eth0 -K -l1000 -M9000 --netmap --unique-ip smallFlows.pcap 
Switching network driver for eth0 to netmap bypass mode... done!
File Cache is enabled
Actual: 14261000 packets (9216531000 bytes) sent in 8.01 seconds.
Rated: 1124999450.7 Bps, 8999.99 Mbps, 1740743.57 pps
Flows: 1209000 flows, 147574.43 fps, 14243000 flow packets, 18000 non-flow
Statistics for network device: eth0
	Attempted packets:         14261000
	Successful packets:        14261000
	Failed packets:            0
	Truncated packets:         0
	Retried packets (ENOBUFS): 0
	Retried packets (EAGAIN):  0
Switching network driver for eth0 to normal mode... done!
```

<h3><a name="testing-routers-or-switches"></a>ルータやスイッチのテスト／Testing Routers or Switches</h3>
*tcpreplay* を動作させるデバイスでは、
device under test (DUT) の入力と出力のそれぞれに接続する 2つのネットワークカードが必要です。
トラフィックをどう分類するかは、[tcpprep][] のヘルプを見てください。
例えば、pcap ファイルの中のパケットをクライアントまたはサーバに分類することができます。
あるいは，プライベートとパブリックに分類できます

*tcpprep* の出力はキャッシュファイルです。このキャッシュファイルにより、
*tcpreplay* がデバイスを経由して 2つの方向にトラフィックを送信できます。
また、(pcap ファイルの中のパケットの)ステートを保持できるようになります。

```
# tcpreplay --cachefile=sample.prep --intf1=eth0 --intf2=eth1 sample.pcap
```

上記とは反対に、すでにトラフィックが 2つのファイルに分離されているなら、
つまりネットワークタップを使ってトラフィックをキャプチャしてえるような場合、
tcpreplay は同時に 2つのファイルをそれぞれのインターフェイスに対して読み込めます:

```
# tcpreplay --dualfile --intf1=eth0 --intf2=eth1 side-a.pcap side-b.pcap
```


[tcprewrite]:          tcprewrite.html
[tcpreplay-edit]:      tcpreplay-edit.html
[tcpprep]:             tcpprep.html
[flow]:                http://en.wikipedia.org/wiki/Traffic_flow_%28computer_networking%29
[nm]:                  http://info.iet.unipi.it/~luigi/netmap/
[captures]:            captures.html
