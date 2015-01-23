---
layout: content
title:  "tcpprep"
categories: tcpreplay wiki
description: "tcpprep - トラフィックを 2方向に分割する時に参照するキャッシュファイルを生成します／create a cache file which is used to 'split' traffic into two sides"
---

- [概要／Overview](#overview)
- [詳細／Details](#details)
- [一般的な使い方／](#basic-usage)
	- [自動判別/Bridge](#autobridge)
- [自動判別/Router](#autorouter)
	- [自動判別/Client](#autoclient)
	- [自動判別/Server](#autoserver)
	- [CIDR 表記で判別／CIDR](#cidr)
	- [正規表現で判別／Regex](#regex)
	- [Port 番号で判別／Port](#port)
	- [MAC アドレスで判別／MAC](#mac)
	- [判別のスキップ処理／Skip Packets](#skip-packets)
	- [・・・を含む／Include](#include)
		- [送信元 IP アドレス／Source IP Match](#source-ip-match)
		- [送信先 IP アドレス／Destination IP Match](#destination-ip-match)
		- [送信元と送信先の両方の IP アドレス／Both IP Match](#both-ip-match)
		- [送信元または送信先どちらかの IP アドレス／Either IP Match](#either-ip-match)
		- [パケットの番号指定／Packet Number Match](#packet-number-match)
		- [BPF フィルタ指定／BPF Filter Match](#bpf-filter-match)
	- [・・・を含まない／Exclude](#exclude)
		- [送信元 IP アドレスを含まない／Source IP Negative Match](#source-ip-negative-match)
		- [送信先 IP アドレスを含まない／Destination IP Negative Match](#destination-ip-negative-match)
		- [送信元と送信先の両方の IP アドレスを含まない／Both IP Negative Match](#both-ip-negative-match)
		- [送信元または送信先どちらかの IP アドレスを含まない／Either IP Negative Match](#either-ip-negative-match)
		- [パケット番号を含まない指定／Packet Number Negative Match](#packet-number-negative-match)
- [その他のオプション／Other Options](#other-options)
	- [コメントの挿入／Comments](#comments)
	- [詳細情報の挿入／Detailed Info](#detailed-info)
- [IP(Internet Protocol) 以外のトラフィック／Non-IP Traffic](#non-ip-traffic)
- [マニュアル／Man pages](tcpprep-man.html)

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

* 自動判別／ブリッジ(Auto/Bridge)
* 自動判別／ルータ(Auto/Router)
* 自動判別／クライアント(Auto/Client)
* 自動判別／サーバ(Auto/Server)
* IPv4/IPv6 アドレスを CIDR 表記でマッチング(IPv4/v6 matching CIDR)
* IPv4/IPv6 アドレスを正規表現でマッチング(IPv4/v6 matching Regex)
* TCP/UDP ポート番号(TCP/UDP Port)
* MAC アドレス(MAC address)

自動(Auto)モードは、IPv6 をまだサポートしていないので注意してください。
tcprewrite と同様に、[2001::dead:beef] のようにカギ括弧で囲む必要があります。

<h3><a name="autobridge"></a>自動／ブリッジ(Auto/Bridge)</h3>
自動／ブリッジモードでは、
*tcpprep* は pcap ファイルをパースし、
ホストがクライアントもしくはサーバのいずれかとして振舞うごとに、
その通信を(キャッシュファイルに)定義します。
現在は、クライアントとしての動作は下記のように定義されてます:
* TCP の Syn パケットを他のホストに送信している
* DNS のリクエストを送信している
* ICMP port unreachable を受信している

サーバとしての動作は下記のように定義されてます:
* TCP の Syn/Ack パケットを他のホストに送信している
* DNS の応答を送信している
* ICMP port unreachable を送信している

全てのパケットが処理された後、それぞれの IPアドレスごとに
クライアントとしての動作として判断されたフローと
サーバとしての動作として判断されたフローが計算され、
クライアントの割合(`--ratio`)が適応されます。
(クライアントまたはサーバに)クラス分けできない IPアドレスがあると、
*tcpprep* はエラーで失敗します。
client/server の割合のデフォルト値は '2.0' です。
ratio(割合)はクライアントのコネクション数によって変わってきます。
だから、デフォルトでは、
クライアントのコネクション数をサーバの 2倍としています。

【追記】
IPアドレスごとに、クライアントとして動作した(と判断した)フロー数(=C)と、
サーバとして動作した(と判断した)フロー数(=S)を計算し、
client/server (= C/S) の値が ratio 以上の場合は、
その IPアドレスはクライアントと判断され 1つ目の NIC から送信する。
ratio 以下の場合はサーバとして判断され 2つ目の NIC から送信する。

```
$ tcpprep --auto=bridge --pcap=input.pcap --cachefile=input.cache
```

client/server の割合を(2.0 から別の値に)変更したい場合は
--ratio オプションを使用します:

```
$ tcpprep --auto=bridge --pcap=input.pcap --cachefile=input.cache --ratio=3.5
```

--ratio オプションは、全ての自動モードで機能します。

<h2><a name="autorouter"></a>自動／ルータ(Auto/Router)</h2>
自動／ルータ(Auto/Router)モードでは、
最初は自動／ブリッジ(Auto/Bridge)モードと同様にクライアントとサーバを計算します。
下記のようにクラス分けします)
まず、それぞれのホストは(IPアドレスは)小さなサブネットに属することにします。
その後、全てのホストがいずれかのサブネットに所属するようになるか、あるいは、
サブネットの最大サイズ(/8)(クラスA で 1600万(16000000)アドレス以上)になるまで、
サブネットを広げていきます。
以下の図で処理プロセスを理解できるかもしれません:

【追記】
クライアントあるいはサーバとしてクラス分けできなかったホストがあると、
自動／ブリッジ(Auto/Bridge)モードではエラーで終了してしまいます。
自動／ルータ(Auto/Router)モードでは、
クラス分けできなかったホストを以下のようにクラス分けします。
全てのクライアントまたはサーバを /30 のネットワークアドレスに所属させます。
クラス分けできないホストが、いずれかの /30 の中に収まっていれば終了。
収まっていなければ /29 /28 /27 ... と /8 まで順に広げていきます。
クラス分けされていないホストがいずれかのサブネットに収まったら、
同一サブネット内のクライアントまたはサーバのいずれかに
(ロンゲストマッチで)決定されます。

1. 最初に、クライアントまたはサーバのいずれかとしてクラス分けされますが、
何台かのホストはクラス分けできません。(「？」と表示されています)
![router1][router1]

2. クラス分けされたホストが含むサブネットを(/30 から順番に最大 /8 まで)
クラス分けされないホストがいずれかのサブネットに所属するようになるまで
広げて行きます。
![router2][router2]

3. クラス分けされていないホストは同一サブネット内のホストのカテゴリ
(サーバまたはクライアント)として分類されます。
![router3][router3]

```
$ tcpprep --auto=router --pcap=input.pcap --cachefile=input.cache
```

*tcpprep* は、最初のサブネットのサイズ(/30)も最大のサイズも、
--minmask と --maxmask オプションで指定できます。

```
$ tcpprep --minmask=24 --maxmask=16 --auto=router --pcap=input.pcap --cachefile=input.cache
```

上記の指定で、/24 (Class C)から開始して /8 (Class B)まで広げられます。

<h3><a name="autoclient"></a>自動／クライアント(Auto/Client)</h3>
自動／クライアント(Auto/Client)モードは、
自動／ブリッジ(Auto/Bridge)モードと同様にクラス分けします。
しかし、最初の(デフォルトだと /30 で --minmask で指定できる)
サブネットに所属していないホストをクライアントとします。

Auto/Client mode works just like Auto/Bridge mode, but IP addresses which have not been classified as either client or server after the initial ranking are classified as clients.

```
$ tcpprep --auto=client --pcap=input.pcap --cachefile=input.cache
```

<h2><a name="autoserver"></a>自動／サーバ(Auto/Server)</h2>
自動／クライアント(Auto/Client)モードは、
自動／ブリッジ(Auto/Bridge)モードと同様にクラス分けします。
しかし、最初の(デフォルトだと /30 で --minmask で指定できる)
サブネットに所属していないホストをサーバとします。

```
$ tcpprep --auto=server --pcap=input.pcap --cachefile=input.cache
```

<h3><a name="cidr"></a>CIDR 表記で判別/CIDR</h3>
CIDR モードは自動／* モードとは異なり、
IPアドレスごとの自動クラス分けの仕組みを使いません。
その代わりに、
サーバが所属するサブネット(複数も可)をCIDR 表記で指定します。

```
$ tcpprep --cidr=10.0.0.0/8,172.16.0.0/12 --pcap=input.pcap --cachefile=input.cache
```

<h3><a name="regex"></a>正規表現で判別／Regex</h3>
正規表現モードは CIDR モードと同様にクラス分けしますが、
サーバが所属するサブネットを CIDR でなく正規表現で指定できます。
下記のように指定すると、
10 または 20 で始まる IPアドレスのホストをサーバと指定できます。

```
$ tcpprep --regex="(10|20)\..*" --pcap=input.pcap --cachefile=input.cache
```

<h3><a name="port"></a>ポート番号で判別／Port</h3>
ポートモードは、クライアントやサーバを
TCP/UDP の送信元／送信先のポート番号でクラス分けできます。
一般的には、ポート番号が 0-1023 だとサーバとして、
それ以外だとクライアントとしてクラス分けされます。
/etc/services (詳細は services(5) の man ページを見てください)
と同じフォーマットでファイルを作成し、
--services <file> でファイルを指定します。

```
$ tcpprep --port --services=/etc/services --pcap=input.pcap --cachefile=input.cache
```

<h3><a name="mac"></a>MAC アドレスで判別/MAC</h3>

送信元の Ethernet MAC アドレスでトラフィックを分類したい場合に利用します。
送信元の MAC アドレスが指定したリストにマッチした場合、
サーバとしてクラス分けされます。
カンマ(,)区切りで複数の MAC アドレスを指定できます。

```
$ tcpprep --mac=00:21:00:55:23:AF,00:45:90:E0:CF:A2 --pcap=input.pcap --cachefile=input.cache
```

<h3><a name="skip-packets"></a>判別のスキップ処理／Skip Packets</h3>

クライアントあるいはサーバとしてマークしない代わりに、
*tcpreplay* や *tcprewrite* が処理する時に(送信や編集を)
スキップするようにマークします。
デフォルトの動作では全てのパケットが処理され、
スキップされることはありません。
include フィルタも exclude フィルタも IPv6 アドレスをサポートします。

<h3><a name="include"></a>・・・を含む／Include</h3>

デフォルトの挙動をスキップに変更し、
--include オプションにマッチしたパケットだけを処理するようにします。

<h4><a name="source-ip-match"></a>送信元 IP アドレス／Source IP Match</h4>

送信元 IP アドレスが 10.0.0.0/8 と 192.168.0.0/16 のパケットだけを処理するには:

```
$ tcpprep --include=S:10.0.0.0/8,192.168.0.0/16 --pcap=input.pcap --cachefile=input.cache <other args>
```

<h4><a name="destination-ip-match"></a>送信先 IP アドレス／Destination IP Match</h4>

送信先 IP アドレスが 10.0.0.0/8 と 192.168.0.0/16 のパケットだけを処理するには:

```
$ tcpprep --include=D:10.0.0.0/8,192.168.0.0/16 --pcap=input.pcap --cachefile=input.cache <other args>
```

<h4><a name="both-ip-match"></a>送信元と送信先の両方の IP アドレス／Both IP Match</h4>

送信元 IP アドレスも送信先 IP アドレスも
10.0.0.0/8 と 192.168.0.0/16 のパケットだけを処理するには:

```
$ tcpprep --include=B:10.0.0.0/8,192.168.0.0/16 --pcap=input.pcap --cachefile=input.cache <other args>
```

<h4><a name="either-ip-match"></a>送信元または送信先どちらかの IP アドレス／Either IP Match</h4>

送信元 IP アドレスまたは送信先 IP アドレスが
10.0.0.0/8 と 192.168.0.0/16 のパケットだけを処理するには:

```
$ tcpprep --include=E:10.0.0.0/8,192.168.0.0/16 --pcap=input.pcap --cachefile=input.cache <other args>
```

<h4><a name="packet-number-match"></a>パケットの番号指定／Packet Number Match</h4>

(pcap ファイルの)
パケットの番号が 1 から 5 と、9 と、72 以降のパケットだけを処理するには:

```
$ tcpprep --include=P:1-5,9,15,72- --pcap=input.pcap --cachefile=input.cache <other args>
```

<h4><a name="bpf-filter-match"></a>BPF フィルタ指定／BPF Filter Match</h4>

TCP のポート番号が 22 のパケットだけを処理するには:

```
$ tcpprep --include=F:"tcp port 22" --pcap=input.pcap --cachefile=input.cache <other args>
```

<h3><a name="exclude"></a>・・・を含まない／Exclude</h3>

--exclude オプションにマッチしたパケットは処理しないようにします。

<h4><a name="source-ip-negative-match"></a>送信元 IP アドレスを含まない／Source IP Negative Match</h4>

送信元 IP アドレスが
10.0.0.0/8 と 192.168.0.0/16 のパケット以外を処理するには:

```
$ tcpprep --exclude=S:10.0.0.0/8,192.168.0.0/16 --pcap=input.pcap --cachefile=input.cache <other args>
```

<h4><a name="destination-ip-negative-match"></a>送信先 IP アドレスを含まない／Destination IP Negative Match</h4>

送信先 IP アドレスが
10.0.0.0/8 と 192.168.0.0/16 のパケット以外を処理するには:

```
$ tcpprep --exclude=D:10.0.0.0/8,192.168.0.0/16 --pcap=input.pcap --cachefile=input.cache <other args>
```

<h4><a name="both-ip-negative-match"></a>送信元と送信先の両方の IP アドレスを含まない／Both IP Negative Match</h4>

送信元 IP アドレスも送信先 IP アドレスも
10.0.0.0/8 と 192.168.0.0/16 のパケット以外を処理するには:

```
$ tcpprep --exclude=B:10.0.0.0/8,192.168.0.0/16 --pcap=input.pcap --cachefile=input.cache <other args>
```

<h4><a name="either-ip-negative-match"></a>送信元または送信先どちらかの IP アドレスを含まない／Either IP Negative Match</h4>

送信元 IP アドレスまたは送信先 IP アドレスが
10.0.0.0/8 と 192.168.0.0/16 のパケット以外を処理するには:

```
$ tcpprep --exclude=E:10.0.0.0/8,192.168.0.0/16 --pcap=input.pcap --cachefile=input.cache <other args>
```

<h4><a name="packet-number-negative-match"></a>パケット番号を含まない指定／Packet Number Negative Match</h4>

(pcap ファイルの)
パケットの番号が 1 から 5 と、9 と、72 以降のパケット以外を処理するには:

```
$ tcpprep --exclude=P:1-5,9,15,72- --pcap=input.pcap --cachefile=input.cache <other args>
```

<h2><a name="other-options"></a>その他のオプション／Other Options</h2>

<h3><a name="comments"></a>コメントの挿入／Comments</h3>

*tcpprep* は cache ファイルにコメントを埋め込み、
後で読み出すことができます。
また、cache ファイルを生成する時に、
作成時のコマンドラインの引数も埋め込めます。
コメントを保存するには `--comment` を、
コメントを読み出すには `--print-comment` オプションを指定してください:

```
$ tcpprep <args> --pcap=input.pcap --cachefile=input.cache --comment="This is our evil packet pcap"

$ tcpprep --print-comment=input.cache
```

<h3><a name="detailed-info"></a>詳細情報の挿入／Detailed Info</h3>

*tcpprep* は、パケットごとの詳細情報と同様に統計情報も見れます。

統計情報を見るには `--print-stats` を指定します:

```
$ tcpprep --print-stats=input.cache
```

パケットごとの情報を見るには `--print-info` オプションを指定します:

```
$ tcpprep --print-info=input.cache
```

<h2><a name="non-ip-traffic"></a>IP(Internet Protocol) 以外のトラフィック／Non-IP Traffic</h4>
tcpprep の多くのモードにおいて、
パケットがクライアントから送信されるのかサーバから送信されるのかを、
IPv4 ヘッダの IP アドレス情報を参照して決定しています。
もちろん、全てのネットワークパケットが IPv4 のパケットではないので、
tcpprep は IPv4 以外のパケットをどちらに送信すればよいのか、
決定する方法がありません。
従って、*tcpprep* はデフォルトの動作では、
IPv4 以外のパケットをクライアント側/セカンダリインターフェースから送信します。
`--nonip` オプションを指定することで、
これらの(IPv4 以外の)パケットをサーバ側／プライマリインターフェースから送信できます。
もちろん、IPv4 以外のパケットを(どちらから送信するか)分割する必要があるので、
そのような場合には MAC アドレスモード(`--mac`) を使えば良いです。

[tcprewrite]:          tcprewrite.html
[tcpreplay]:           tcpreplay.html
[tcpprep]:             tcpprep.html
[nm]:                  http://info.iet.unipi.it/~luigi/netmap/
[captures]:            captures.html
[router1]:             {{ site.url }}/images/router-mode1.png
[router2]:             {{ site.url }}/images/router-mode2.png
[router3]:             {{ site.url }}/images/router-mode3.png
