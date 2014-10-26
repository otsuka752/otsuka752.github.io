---
layout: content
title:  "tcpprep"
categories: tcpreplay wiki
description: "tcpprep - create a cache file which is used to 'split' traffic into two sides"
---

- [概要／Overview][Overview](#overview)
- [詳細／Details][Details](#details)
- [一般的な使い方／][Basic Usage](#basic-usage)
	- [自動判別/Bridge][Auto/Bridge](#autobridge)
- [自動判別/Router][Auto/Router](#autorouter)
	- [自動判別/Client][Auto/Client](#autoclient)
	- [自動判別/Server][Auto/Server](#autoserver)
	- [CIDR 表記で判別][CIDR](#cidr)
	- [正規表現で判別／Regex][Regex](#regex)
	- [Port 番号で判別／][Port](#port)
	- [MAC アドレスで判別／][MAC](#mac)
	- [判別のスキップ処理／][Skip Packets](#skip-packets)
	- [・・・を含む／][Include](#include)
		- [送信元 IP アドレス／Source IP Match][Source IP Match](#source-ip-match)
		- [送信先 IP アドレス／Destination IP Match][Destination IP Match](#destination-ip-match)
		- [送信元と送信先の両方の IP アドレス／][Both IP Match](#both-ip-match)
		- [送信元または送信先どちらかの IP アドレス／Either IP Match][Either IP Match](#either-ip-match)
		- [パケットの番号指定／Packet Number Match][Packet Number Match](#packet-number-match)
		- [BPF フィルタ指定／BPF Filter Match][BPF Filter Match](#bpf-filter-match)
	- [・・・を含まない／Exclude][Exclude](#exclude)
		- [送信元 IP アドレスを含まない／Source IP Negative Match][Source IP Negative Match](#source-ip-negative-match)
		- [送信先 IP アドレスを含まない／Destination IP Negative Match][Destination IP Negative Match](#destination-ip-negative-match)
		- [送信元と送信先の両方の IP アドレスを含まない／Both IP Negative Match][Both IP Negative Match](#both-ip-negative-match)
		- [送信元または送信先どちらかの IP アドレスを含まない／Either IP Negative Match][Either IP Negative Match](#either-ip-negative-match)
		- [パケット番号を含まない指定／Packet Number Negative Match][Packet Number Negative Match](#packet-number-negative-match)
- [その他のオプション／Other Options][Other Options](#other-options)
	- [コメントの挿入／Comments][Comments](#comments)
	- [詳細情報の挿入／Detailed Info][Detailed Info](#detailed-info)
- [IP(Internet Protocol) 以外のトラフィック／Non-IP Traffic][Non-IP Traffic](#non-ip-traffic)
- [マニュアルページ／Man pages][Man pages](tcpprep-man.html)

<h2><a name="overview"></a>概要／Overview</h2>
*tcpprep* は [tcpreplay] と [tcprewrite] で使う pcap ファイルのプリプロセッサーです。
*tcpprep* の目的は、トラフィックを 2方向に分割するためのキャッシュファイルを作成することです。
ここでの 2方向とは、プライマリ／セカンダリだったり、クライアント／サーバと呼ばれたりします。
2つの NIC を持つホストで tcpreplay を実行しようとする場合、
どちらの NIC にパケットを送信するかを決定します。
事前にキャッシュファイルを生成してからパケットを送出する方法を取ることで、
tcpreplay はトラフィックを分離しながら送信する時よりも高速に送信できます。

<h2><a name="details"></a>詳細／Details</h2>
pcap ファイルの中身や、どこでパケットを再送出するかによって、
トラフィックを(NIC ごとに)分離したくなります。
ここで重要なのは、
測定対象の機器 (DUT/Device Under Test) が 2つの NIC の間に接続されていることを認識することです。
つまり、クライアントからサーバへのパケットは DUT のとある方向に流れ、
また、そのレスポンスのパケットは反対の方向に流れます。

<h2><a name="basic-usage"></a>一般的な使い方／Basic Usage</h2>
*tcpprep* は複数の「モード」をサポートしています。
それぞれの「モード」は異なるロジックでトラフィックを分離します。
例えば、ルータを経由させてトラフィックを送出する場合は「ルータモード」が適切だと思われます。
ブリッジや Layer 2 の機器を経由させる場合は「ブリッジモード」が適切だと思われます。
現時点では、下記のモードがサポートされています:

* Auto/Bridge
* Auto/Router
* Auto/Client
* Auto/Server
* IPv4/v6 matching CIDR
* IPv4/v6 matching Regex
* TCP/UDP Port
*MAC address

Note the Auto modes do not yet 
support IPv6. As with tcprewrite IPv6 addresses must be enclosed in hard brackets 
like this: [2001::dead:beef]

<h3><a name="autobridge"></a>Auto/Bridge</h3>
In Auto/Bridge mode, *tcpprep* parses the pcap file and keeps track each time a host 
either behaves like a client or like a server. Currently client behavior is defined by:   
* Sending a TCP Syn packet to another host
* Making a DNS request
* Recieving an ICMP port unreachable

Server behavior is defined by:   
* Sending a TCP Syn/Ack packet to another host
* Sending a DNS Reply
* Sending an ICMP port unreachable

After all the packets have been processed, the totals for client-like behavior vs. server-like 
behavior are totalled and the server to client ratio (`--ratio`) is applied to 
classify each IP. If there is any IP address which is unclassifiable, then *tcpprep*
will fail with an error. By default the client/server ratio is set to 2.0. 
The ratio is the modifier to the number of client connections. 
Hence, by default, client connections are valued twice as high as server connections.

```
$ tcpprep --auto=bridge --pcap=input.pcap --cachefile=input.cache
```

If you want to change the client/server ratio then use the --ratio option:

```
$ tcpprep --auto=bridge --pcap=input.pcap --cachefile=input.cache --ratio=3.5
```

Note: The --ratio option is valid for all automatic processing modes.

<h2><a name="autorouter"></a>Auto/Router</h2>
In Auto/Router mode hosts are first ranked as client or server using the same method as in Auto/Bridge mode. Then each host is placed in a small subnet which is expanded until either all the unknown hosts are included or the maximum newtork size is reached. This works best when clients and servers are on diffierent networks. The default starting network size is a /30 (2 valid IP addresses) and the maximum network is a /8 (Class A with over 16 million addresses). The following images hopefully better explain this process:

1. First each host is categorized as a client or server as best as possible,
but sometimes one or more hosts are unclassifyable (denoted by ?)   
![router1][router1]


2. Each known host is placed in a subnet which is grown until all the unknown 
hosts are inside one of these subnets    
![router2][router2]

3. Unknown hosts are classified in the same category (server or client) based on which 
subnet they were in.  
![router3][router3]

```
$ tcpprep --auto=router --pcap=input.pcap --cachefile=input.cache
```

*tcpprep* also allows you to override the starting and maximum nework size using the --minmask and --maxmask arguments.

```
$ tcpprep --minmask=24 --maxmask=16 --auto=router --pcap=input.pcap --cachefile=input.cache
```

Would start searching using a Class C network and max out with a Class B network.

<h3><a name="autoclient"></a>Auto/Client</h3>
Auto/Client mode works just like Auto/Bridge mode, but IP addresses which have not been classified as either client or server after the initial ranking are classified as clients.

```
$ tcpprep --auto=client --pcap=input.pcap --cachefile=input.cache
```

<h2><a name="autoserver"></a>Auto/Server</h2>
Auto/Server mode works just like Auto/Bridge mode, but IP addresses which have not been
classified as either client or server after the initial ranking are classified as servers.

```
$ tcpprep --auto=server --pcap=input.pcap --cachefile=input.cache
```

<h3><a name="cidr"></a>CIDR</h3>
CIDR mode unlike the Auto/* modes does not use any automatic ranking system to classify
IP addresses. Instead, it requires the user to specify on or more networks in CIDR notation
which contain servers.

```
$ tcpprep --cidr=10.0.0.0/8,172.16.0.0/12 --pcap=input.pcap --cachefile=input.cache
```

<h3><a name="regex"></a>Regex</h3>
Regex mode works like CIDR mode, except that instead of providing a list of CIDR's, 
the user provides a regex which matches the source IP of the servers. In this case any 
IP starting with 10. or 20.

```
$ tcpprep --regex="(10|20)\..*" --pcap=input.pcap --cachefile=input.cache
```

<h3><a name="port"></a>Port</h3>

Port mode is used when you want to use the source and destination port of TCP/UDP packets
to classify hosts as client or server. Normally, ports 0-1023 are
considered server ports and everything else a client port. You can create your
own custom mapping file in the same format as /etc/services (see the services(5) 
man page for details) by specifying --services <file>.

```
$ tcpprep --port --services=/etc/services --pcap=input.pcap --cachefile=input.cache
```

<h3><a name="mac"></a>MAC</h3>

MAC address mode is used when you want to split traffic based on the source MAC address 
of the Ethernet header. Frames with a source address matching one of the listed MAC 
addresses will be treated as a server. Multiple MAC addresses can be provided in a
comma delimited format.

```
$ tcpprep --mac=00:21:00:55:23:AF,00:45:90:E0:CF:A2 --pcap=input.pcap --cachefile=input.cache
```

<h3><a name="skip-packets"></a>Skip Packets</h3>

Instead of marking a packet as client/server, it can also mark the packet to be skipped
when processed by *tcpreplay* and *tcprewrite*. Skipped packets are not sent by *tcpreplay*
and not edited by tcprewrite. By default, all packets are processed and none of them 
are skipped. Include and Exclude filters support IPv6 addresses.

<h3><a name="include"></a>Include</h3>

Change the default to be skip and only process packets which match the --include flag.

<h4><a name="source-ip-match"></a>Source IP Match</h4>

Only process packets with a matching source IP in the networks: 10.0.0.0/8 and 192.168.0.0/16:

```
$ tcpprep --include=S:10.0.0.0/8,192.168.0.0/16 --pcap=input.pcap --cachefile=input.cache <other args>
```

<h4><a name="destination-ip-match"></a>Destination IP Match</h4>

Only process packets with a matching destination IP in the networks: 10.0.0.0/8 and 192.168.0.0/16:

```
$ tcpprep --include=D:10.0.0.0/8,192.168.0.0/16 --pcap=input.pcap --cachefile=input.cache <other args>
```

<h4><a name="both-ip-match"></a>Both IP Match</h4>

Only process packets which both source and destination IP matches the networks:
10.0.0.0/8 and 192.168.0.0/16:

```
$ tcpprep --include=B:10.0.0.0/8,192.168.0.0/16 --pcap=input.pcap --cachefile=input.cache <other args>
```

<h4><a name="either-ip-match"></a>Either IP Match</h4>

Only process packets which either the source or destination IP matches the networks:
10.0.0.0/8 and 192.168.0.0/16:

```
$ tcpprep --include=E:10.0.0.0/8,192.168.0.0/16 --pcap=input.pcap --cachefile=input.cache <other args>
```

<h4><a name="packet-number-match"></a>Packet Number Match</h4>

Only process packets numbered 1 thru 5, 9, 15 and 72 until the end of the file:

```
$ tcpprep --include=P:1-5,9,15,72- --pcap=input.pcap --cachefile=input.cache <other args>
```

<h4><a name="bpf-filter-match"></a>BPF Filter Match</h4>

Only process packets which match the BPF filter: tcp port 22

```
$ tcpprep --include=F:"tcp port 22" --pcap=input.pcap --cachefile=input.cache <other args>
```

<h3><a name="exclude"></a>Exclude</h3>

Skip any packets which match the filter specified by the --exclude flag.

<h4><a name="source-ip-negative-match"></a>Source IP Negative Match</h4>

Only process packets which don't match the source IP in the networks: 10.0.0.0/8 and 192.168.0.0/16:

```
$ tcpprep --exclude=S:10.0.0.0/8,192.168.0.0/16 --pcap=input.pcap --cachefile=input.cache <other args>
```

<h4><a name="destination-ip-negative-match"></a>Destination IP Negative Match</h4>

Only process packets which don't match the destination IP in the networks:
10.0.0.0/8 and 192.168.0.0/16:

```
$ tcpprep --exclude=D:10.0.0.0/8,192.168.0.0/16 --pcap=input.pcap --cachefile=input.cache <other args>
```

<h4><a name="both-ip-negative-match"></a>Both IP Negative Match</h4>

Only process packets which neither source and destination IP matches the networks:
10.0.0.0/8 and 192.168.0.0/16:

```
$ tcpprep --exclude=B:10.0.0.0/8,192.168.0.0/16 --pcap=input.pcap --cachefile=input.cache <other args>
```

<h4><a name="either-ip-negative-match"></a>Either IP Negative Match</h4>

Only process packets which neither the source or destination IP matches the networks:
10.0.0.0/8 and 192.168.0.0/16:

```
$ tcpprep --exclude=E:10.0.0.0/8,192.168.0.0/16 --pcap=input.pcap --cachefile=input.cache <other args>
```

<h4><a name="packet-number-negative-match"></a>Packet Number Negative Match</h4>

Do not process packets numbered 1 thru 5, 9, 15 and 72 until the end of the file:

```
$ tcpprep --exclude=P:1-5,9,15,72- --pcap=input.pcap --cachefile=input.cache <other args>
```

<h2><a name="other-options"></a>Other Options</h2>

<h3><a name="comments"></a>Comments</h3>

*tcpprep* allows you to embed comments in your cache file which you can then read at a
later time. tcpprep also saves what the commands line arguments were when the cache file
was generated. To save a comment, use the `--comment` flag. To read the comment, 
use the `--print-comment` flag:

```
$ tcpprep <args> --pcap=input.pcap --cachefile=input.cache --comment="This is our evil packet pcap"

$ tcpprep --print-comment=input.cache
```

<h3><a name="detailed-info"></a>Detailed Info</h3>

*tcpprep* also allows you to view statistical information as well as detailed per-packet data.

To view the overall statistical info, use the `--print-stats` flag:

```
$ tcpprep --print-stats=input.cache
```

To view per-packet data, use the --print-info flag:

```
$ tcpprep --print-info=input.cache
```

<h2><a name="non-ip-traffic"></a>Non-IP Traffic</h4>
Many of tcpprep's modes rely on IP address information in the IPv4 header to
determine whether the packet was sent by a "client" or "server". Of course, not all
network packets are IPv4 and hence tcpprep in those cases has no way to determine which
direction the packet is going. Hence, by default, *tcpprep* will send all non-IPv4 traffic
out the client/secondary interface. You can change this to be the server/primary interface
by using the `--nonip` flag. If of course you need to split non-IP traffic, then you should
use the MAC address mode (`--mac`).



[tcprewrite]:          tcprewrite.html
[tcpreplay]:           tcpreplay.html
[tcpprep]:             tcpprep.html
[nm]:                  http://info.iet.unipi.it/~luigi/netmap/
[captures]:            captures.html
[router1]:             {{ site.url }}/images/router-mode1.png
[router2]:             {{ site.url }}/images/router-mode2.png
[router3]:             {{ site.url }}/images/router-mode3.png
