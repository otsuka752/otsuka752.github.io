---
layout: content
title:  "tcprewrite"
categories: tcpreplay wiki
description: "tcprewrite - modify a PCAP capture file"
---

- [概要／Overview](#overview)
- [一般的な使い方／Basic Usage](#basic-usage)
	- [パケットの向きと選択／Direction & Selection](#direction-&-selection)
- [Layer2 の書き換え／Rewriting Layer 2](#rewriting-layer-2)
	- [DLT(Data Link Type)プラグイン／DLT Plugins](#dlt-plugins)
	- [DLT_EN10MB (Ethernet)](#dlt_en10mb-ethernet)
		- [Source & Destination MAC アドレスの書き換え／Rewriting Source & Destination MAC addresses](#rewriting-source-&-destination-mac-addresses)
		- [802.1q VLAN's／802.1q VLAN's](#8021q-vlan's)
	- [Cisco HDLC／DLT_CHDLC (Cisco HDLC)](#dlt_chdlc-cisco-hdlc)
	- [ユーザ定義の DLT／DLT_USER0 (User Defined)](#dlt_user0-user-defined)
- [Layer3 の書き換え／Rewriting Layer 3](#rewriting-layer-3)
	- [2台のホスト間の通信にする／Forcing Traffic Between Two Hosts](#forcing-traffic-between-two-hosts)
	- [IPアドレスのランダム化／Randomizing IP Addresses](#randomizing-ip-addresses)
	- [擬似 NAT や Src/Dst IP Map 機能でのアドレス変更／Changing Networks via Pseudo-NAT, Source/Destination IP Map](#changing-networks-via-pseudo-nat-sourcedestination-ip-map)
	- [IPv4 の ToS や DiffServ や ECN の書き換え／Editing IPv4 TOS/DiffServ/ECN](#editing-ipv4-tosdiffservecn)
	- [IPv6 の Traffic Class の書き換え／Editing IPv6 Traffic Class](#editing-ipv6-traffic-class)
	- [IPv6 の Flow Label の書き換え／Editing IPv6 Flow Label](#editing-ipv6-flow-label)
- [Layer 4 の書き換え／Rewriting Layer 4](#rewriting-layer-4)
	- [ポートの再マッピング／Re-Mapping Ports](#re-mapping-ports)
	- [強制的にチェックサムを計算／Forcing Checksum Calculation](#forcing-checksum-calculation)
- [Layer 5-7 の書き換え／Rewriting Layers 5-7](#rewriting-layers-5-7)
- [MTU の問題の取り扱い／Dealing with MTU problems](#dealing-with-mtu-problems)
- [Fragroute](#fragroute)
	- [概要／Overview](#overview2)
	- [使い方／Usage](#usage)
- [マニュアル／Man pages](tcprewrite-man.html)

<h2><a name="overview"></a>概要／Overview</h2>
バージョン 3.0 で、[tcpreplay] のパケット編集機能は [tcprewrite] に移行しました。
3.4.1 で [tcpreplay-edit] コマンドが追加されたことにより、
このパケット編集機能は再び *tcpreplay* に取り込まれました。
従って、下記の全てのオプションは *tcprewrite* でも *tcpreplay-edit* でも両方で有効です。

<h2><a name="basic-usage"></a>一般的な使い方／Basic Usage</h2>
*tcprewrite* を実行するには、
入力する pcap ファイルと出力する pcap のファイル名(上書きされます)が必要です。

```
$ tcprewrite --infile=input.pcap --outfile=output.pcap
```

実際にパケットを編集するための追加の引数は下記に示します。

<h3><a name="direction-&-selection"></a>パケットの向きと選択／Direction & Selection</h3>
パケットを編集する前に、書き換えオプションのいくつかは、
パケットの向きによって異なる結果になることを理解することが重要です。
パケットの向きは、*tcpprep cache file* を参照することで決定されます。
*tcpprep cache file* は、様々な基準によりパケットの向きを決定できます。

*tcpprep cache files* を使うことで、
*tcprewrite* の処理をスキップさせるようにパケットに印をつけることもできます。
この機能を使うことで、どのパケットを編集しどのパケットを編集しないかを決められます。
キャッシュファイルは実際の pcap ファイルとは分離されているので、
*tcprewrite* の複数経路で異なる処理をするために、複数のキャッシュファイルを使えます。

処理の過程で tcpprep cache file を指定するには `--cachefile` オプションを使います。

<h2><a name="rewriting-layer-2"></a>Layer2 の書き換え／Rewriting Layer 2</h2>
*tcprewrite* は多くの Layer2 ヘッダを書き換えできます。
これにより、スイッチ、FireWall、ルータ、IPS など、
数多くのフォワーディングデバイスにトラフィックを再送信できます。


<h3><a name="dlt-plugins"></a>DLT(Data Link Type)プラグイン／DLT Plugins</h3>
3.0 の時点では、
tcprewrite は異なる DLT(Data LinkType)/Layer2 をサポートするためにプラグインを使用します。 
これはメンテナンスしやすいコードにはならないですが、
ユーザに対して何がサポートされ何がサポートされていないかを明確にします。
それぞれのプラグインはパケットの読み込み and/or 書き出しをサポートします。
デフォルト状態では、読み込みに使われたプラグインは，書き出しにも使われます。
しかし、`--dlt` オプションを使うことで書き出すプラグインを指定できます。
DLT プラグインを使うことで、ある DLT/Layer2 のタイプのパケットを別なタイプに変換できます。
例えば、Ethernet インターフェイスでキャプチャしたトラフィックを Cisco HDLC に再送信したり、
BSD ループバックインターフェイスでキャプチャしたトラフィックを Ethrenet で再送信できます。

出力をサポートするプラグイン／Plugins supported in output mode:

* Ethernet (enet)
* Cisco HDLC (hdlc)
* User defined Layer 2 (user)

入力をサポートするプラグイン／Plugins supported in input mode:

* Ethernet
* Cisco HDLC
* Linux SLL
* BSD Loopback
* BSD Null
* Raw IP
* 802.11
* Juniper Ethernet (version >= 4.0)

従って、`--dlt<output>` オプションを使うことで、
入力をサポートする DLT タイプでキャプチャしたファイルを、
出力をサポートする DLT タイプのいずれかに変換できます。
入力する DLT タイプによっては、追加で DLT プラグインのフラグが必要になります。

<h3><a name="dlt_en10mb-ethernet"></a>DLT_EN10MB (Ethernet)</h3>
Ethernet プラグインは、
source と destination の MAC アドレスを制御できます。
また、IEEE802.1q の Vlan tag ヘッダを追加・削除できます。

<h4><a name="rewriting-source-&-destination-mac-addresses"></a>Source & Destination MAC アドレスの書き換え／Rewriting Source & Destination MAC addresses</h4>
Layer2 ヘッダの書き換えでよく使われるのは
source(送信元)と destination(送信先)の MAC アドレスの書き換えです。
これにより、適切なデバイスで処理されるようになります。
`--enet-dmac` や `--enet-smac` オプションを使用することで、
それぞれに新しい MAC アドレスを指定することができます。

下記の例では、
destination MAC アドレスを 00:55:22:AF:C6:37 に、
source MAC アドレスを 00:44:66:FC:29:AF に変換しています。

```
$ tcprewrite --enet-dmac=00:55:22:AF:C6:37 --enet-smac=00:44:66:FC:29:AF --infile=input.pcap --outfile=output.pcap
```

ところで、全てのトラフィックが一方向の場合でない限り、上記の例は便利ではありません。
MACアドレスが 00:55:22:AF:C6:37 と 00:44:66:FC:29:AF のルータを経由して、
双方向のトラフィックが流れている場合はどうでしょう？
クライアントの MACアドレスが 00:22:55:AC:DE:AC で、
サーバの MACアドレスが 00:66:AA:D1:32:C2 の場合を考えてみます。
最初に、トラフィックを分離するための tcpprep cache file を作る必要があります。
cache ファイルを作ってしまえば、下記のように *tcprewrite* を実行できます。

```
$ tcprewrite --enet-dmac=00:44:66:FC:29:AF,00:55:22:AF:C6:37 --enet-smac=00:66:AA:D1:32:C2,00:22:55:AC:DE:AC --cachefile=input.cache --infile=input.pcap --outfile=output.pcap
```

ここで重要なことは、
dmac/smac フラグに列挙される最初の MACアドレスがサーバで、
2つ目の MACアドレスがクライアントのものになるということです。

`--skipbroadcast` は覚えておくと便利なフラグです。
これは、ブロードキャスト宛て(FF:FF:FF:FF:FF:FF) や
マルチキャスト宛(最初のオクテットが奇数)の MAC アドレスを
tcprewrite で書き換わらないようにできます。
ブロードキャストやマルチキャストの MACアドレスを書き換えてしまうと、
ARP や DHCP などが動作しなくなります。


<h4><a name="8021q-vlan's"></a>802.1q VLAN's／802.1q VLAN's</h4>

*tcprewrite* は、
Ethernet フレームの IEEE802.1q Vlan tag の情報を追加・削除もできます。
削除するには、単に `--vlan=del` を使います。

```
$ tcprewrite --enet-vlan=del --infile=input.pcap --outfile=output.pcap
```

Vlan tag を追加するには `--enet-vlan=add` を使い、
`--enet-vlan-tag` 、 `--enet-vlan-cfi` 、 `--enet-vlan-pri` などを指定します。

VLAN tag = 40 で、CFI = 1 で、VLAN priority = 4 の tag を付与するには、下記を実行します。

```
$ tcprewrite --enet-vlan=add --enet-vlan-tag=40 --enet-vlan-cfi=1 --enet-vlan-pri=4 --infile=input.pcap --outfile=output.pcap
```


<h3><a name="dlt_chdlc-cisco-hdlc"></a>Cisco HDLC／DLT_CHDLC (Cisco HDLC)</h3>

Cisco HDLC は Layer 2 に address と control のヘッダがあります。
このプラグインを使って両方ともセットできます:

* `--hdlc-address`
* `--hdlc-control`

<h3><a name="dlt_user0-user-defined"></a>ユーザ定義の DLT／DLT_USER0 (User Defined)</h3>

ユーザ定義の DLT を使うには、下記の 2つのオプションを使います:

* `--user-dlt` - Set pcap DLT type
* `--user-dlink` - Set packet layer 2 header

<h2><a name="rewriting-layer-3"></a>Layer3 の書き換え／Rewriting Layer 3</h2>
バージョン 3.4.2 で *tcprewrite* は IPv4 と IPv6 の両方に対応しました。
書き換えたい内容ごとに、たくさんの書き換え方法があります。
Layer 3 の書き換えルールを指定すると *tcprewrite* は自動的にチェックサムを再計算するようになります。
`--fixcsum` を指定する必要はありません。
IPv6 のアドレスを指定する場合は、
[2001::dead:beef] や [2001::/16] のように鍵括弧([ ])を使います。

Not only do the following options edit the IP header, but in the case of IPv4,
also modified ARP requests/replies to match as well.


<h3><a name="forcing-traffic-between-two-hosts"></a>2台のホスト間の通信にする／Forcing Traffic Between Two Hosts</h3>
たくさんのホストが通信している pcap ファイルを使う場合、
全ての通信を 2台のホスト間に(10.10.1.1 と 10.10.1.2 のように)、
あるいは "endpoints" に書き換えてしまいたいことがあります。
`--endpoints` オプションを使って 2つの IPアドレスを指定できます。

```
$ tcprewrite --endpoints=10.10.1.1:10.10.1.2 --cachefile=input.cache --infile=input.pcap --outfile=output.pcap --skipbroadcast
```

`--endpoints` を使うには( tcpprep で生成された) キャッシュファイルが必要です。
また、`--skipbroadcast` オプションの使用が強く推奨されます。

<h3><a name="randomizing-ip-addresses"></a>IPアドレスのランダム化／Randomizing IP Addresses</h3>
IPアドレスを隠して pcap ファイルを誰かに渡したい場合などは、
この機能を使ってください。
この機能は IPアドレスヘッダと ARP のメッセージだけを書き換えます。
アプリケーションが保持しているオリジナルの IPアドレスまでは書き換えないので、
注意してください。

IPアドレスがランダム化される時は、
指定された 'seed' の値をベースに決まったルールでランダム化されるため、
あるホスト間のセッションはそのまま保持されます。
'seed' の値を変更すると、IPアドレスも変わります。

```
$ tcprewrite --seed=423 --infile=input.pcap --outfile=output.pcap
```

<h3><a name="changing-networks-via-pseudo-nat-sourcedestination-ip-map"></a>擬似 NAT や Src/Dst IP Map 機能でのアドレス変更／Changing Networks via Pseudo-NAT, Source/Destination IP Map</h3>

擬似 NAT(Pseudo-NAT)は NAT(network address translation)と同じように機能します。
あるサブネットを別のサブネットにマッピングできます。
送信元と宛先のサブネットは CIDR 表記で記述でき、サブネット長は同じである必要はありません。
CIDR 表記の (match/rewrite の)ペアは複数回指定でき、
(tcpprep で生成される)キャッシュファイルを使えば `--pnat` フラグは 2回まで指定できます。
フォーマットは: `<match_cidr>:<rewrite_cidr>,...` です

下記は、10.0.0.0/8 または 192.168.0.0/16 のレンジを 172.16.0.0/12 に書き換えます。

```
$ tcprewrite --pnat=10.0.0.0/8:172.16.0.0/12,192.168.0.0/16:172.16.0.0/12 --infile=input.pcap --outfile=output.pcap --skipbroadcast
```

(tcpprep で生成されるキャッシュファイルを使えば)
パケットの方向ごとに IPアドレスを書き換えることも可能です。

下記は、(キャッシュファイルにより)クライアントまたはサーバを判別し、
10.0.0.0/8 のアドレスを異なるアドレス(192.168.0.0/24 または 192.168.1.0/24)に書き換えます。
もちろん、通信のセッションを保持できるよう、送信元と送信先のアドレスを適切に書き換えます。

```
$ tcprewrite --pnat=10.0.0.0/8:192.168.0.0/24 --pnat=10.0.0.0/8:192.168.1.0/24 --cachefile=input.cache --infile=input.pcap --outfile=output.pcap --skipbroadcast
```

送信元と送信先のアドレスに異なる変換ルールを適用するために、
`--pnat` の代わりに `--srcipmap` や `--dstipmap` を使えます。
`--srcipmap` と `--dstipmap` は、
`--pnat` と同様のフォーマットで
`<match_cidr>:<rewrite_cidr>,...` のように使います。

<h3><a name="editing-ipv4-tosdiffservecn"></a>IPv4 の ToS や DiffServ や ECN の書き換え／Editing IPv4 TOS/DiffServ/ECN</h3>
ToS(DiffServ/ECN) を変更するには `--tos` で値を指定します。

```
$ tcprewrite --tos=50 --infile=input.pcap --outfile=output.pcap
```

上記を実行すると、
全ての(IPv4 パケットの) IPv4 ヘッダの ToS が 50 に書き換わります。

<h3><a name="editing-ipv6-traffic-class"></a>IPv6 の Traffic Class の書き換え／Editing IPv6 Traffic Class</h3>

Traffic Class フィールドを変更するには `--tclass` で値を指定します。

```
$ tcprewrite --tclass=33 --infile=input.pcap --outfile=output.pcap
```

上記を実行すると、
全ての(IPv6 パケットの) IPv6 ヘッダの Traffic Class フィールドが 33 に書き換わります。

<h3><a name="editing-ipv6-flow-label"></a>IPv6 の Flow Label の書き換え／Editing IPv6 Flow Label</h3>

Flow Label フィールドを変更するには `--flowlabel` で値を指定します。

```
$ tcprewrite --flowlabel=67234 --infile=input.pcap --outfile=output.pcap
```

上記を実行すると、
全ての(IPv6 パケットの) IPv6 ヘッダの Flow Label が 67234 に書き換わります。

<h2><a name="rewriting-layer-4"></a>Layer 4 の書き換え／Rewriting Layer 4</h2>

*tcprewrite* は、TCP/UDP の書き換えもサポートします。
Layer 4 のパケットを書き換えた場合、
tcprewrite は自動的にチェックサムを再計算します。

<h3><a name="re-mapping-ports"></a>ポートの再マッピング／Re-Mapping Ports</h3>
tcprewrite で、TCP や UDP のセッションを別のポートの
(別の Src と Dst のポートの)組み合わせに再マッピングできます。
例えば、HTTP 通信を 80 番ポートでなく 8080番ポートに変更できます。
再マッピングするには `--portmap` を使います。

```
$ tcprewrite --portmap=80:8080,22:8022 --infile=input.pcap --outfile=output.pcap
```

上記を実行すると、
TCP/UDP を 80 から 8080 に、また、22 を 8022 に再マッピングされます。

<h3><a name="forcing-checksum-calculation"></a>強制的にチェックサムを計算／Forcing Checksum Calculation</h3>
多くの NIC には TCP/UDP のチェックサム計算オフロード機能があります。
ホスト自身で生成したトラフィックをキャプチャした場合、
チェックサムの値は正しくないように見えます。
トラフィックを再送出しようとした場合に問題となるのは明らかです。
`--fixsum` オプションを使うことで、*tcprewrite* はチェックサムを強制的に計算します。
*tcprewrite* は、パケットを書き換えた場合には自動的にチェックサムを再計算するので注意してください。

```
$ tcprewrite --fixcsum --infile=input.pcap --outfile=output.pcap
```

<h2><a name="rewriting-layers-5-7"></a>Layer 5-7 の書き換え／Rewriting Layers 5-7</h2>
パケットがトランケートされている(切り詰められている)ことはよくあるので、
パケットの中のアプリケーションのデータは失われています。
トラフィックを処理しているデバイスの種類によって、
アプリケーションのデータは重要かもしれないですし、
そうでないかもしれません。
例えば、ルータやファイヤーウォールは、
通常はパケットの中のアプリケーションデータ全体は処理しません。

*tcprewrite* には、失われたデータを "fix" する方法が 3つあります。
(パケット長に合わせて)パケットに 0x00 のデータをパディングする
(0x00 のデータを追加する)、または、
キャプチャされたパケット長に合わせてパケットのヘッダを書き換える、
という方法を選択できます。
どちらの方法も、パケットのデータは invalid(無効)ですが、
すくなくともパケットとしては valid(有効) です。
3番目の方法は、パケットを完全に取り除いてしまいます。
出力する pcap ファイルには、
元の(サイズが異なる)パケットは含まれないことに注意してください。
従って、再度 *tcpprep cache file* を適応すべきではありません。
pcap ファイルの中に、もうマッチするパケットは存在しないからです。

0x00 でパディングします:

```
$ tcprewrite --fixlen=pad --infile=input.pcap --outfile=output.pcap
```

キャプチャしたパケットサイズに合わせて(pcap ファイルの)パケットヘッダ長を書き換えます:

```
$ tcprewrite --fixlen=trunc --infile=input.pcap --outfile=output.pcap
```

pcap ファイルからパケットを取り除きます:

```
$ tcprewrite --fixlen=del --infile=input.pcap --outfile=output.pcap
```

パケットにパディングする時、その最大サイズは(通常は 1500バイトの)
MTU の値に制限されます。MTU の値は `--mtu` オプションで上書きできます。

<h2><a name="dealing-with-mtu-problems"></a>MTU の問題の取り扱い／Dealing with MTU problems</h2>
インターフェイスの最大送信サイズ(MTU)が、
送信しようとするパケットよりも小さいことがあります。
通常、*tcpreplay* はこれらのパケットをスキップしますが、
以下のようなオプションもあります:

1, MTU の大きさにパケットを切り詰める(標準では 1500 バイト):

```
$ tcprewrite --mtu-trunc --infile=input.pcap --outfile=output.pcap
```

2, 指定した MTU サイズに切り詰める(1000 バイトの場合):

```
$ tcprewrite --mtu=1000 --mtu-trunc --infile=input.pcap --outfile=output.pcap
```

3. 大きなパケットを小さく分割するには IPフラグメンテーションを使ってください。 バージョン 3.3.0 からは，MTU 以下のパケットに分割するために fragroute エンジンが使えます。 fragroute を使うには，下記のような内容の frag.cfg ファイルを作成します。`ip_frag 1400`

```
ip_frag 1400
```

そして、下記のように tcprewrite を実行します:

```
$ tcprewrite --fragroute=frag.cfg --infile=input.pcap --outfile=output.pcap
```
上記では、1400バイトのパケットに分割するように tcprewrite されます。
IPフラグメンテーションは IPレイヤーでなされるため、
(分割されたパケットに) Ethernet や IPv4 ヘッダを付与できるように、
実際の MTU (この例だと Ethernet の 1500バイトを想定しています) よりも小さい値を指定します。
もちろん、IPパケット以外では利用できません。
状況によっては、送信できないパケットもあります。

<h2><a name="fragroute"></a>Fragroute／Fragroute</h2>

<h3><a name="overview2"></a>概要／Overview</h3>
Tcpreplay 3.3.0 で、tcprewrite は Dug Song の fragroute エンジン が実装されました。
ライブラリの制限であなたの環境で fragroute は動作しないかもしれません。
`tcprewrite -V` を実行すると確認できます。
また、fragroute の実装上の制限により、
Vlan tag がある Ethernet フレームはサポートされません(DLT_EN10MB)。

*tcprewrite* は fragroute と同じ書式を利用できますが、
いくつかのコマンドはサポートされていません:

* delay
* echo
* print

Tcpreplay 3.4.2 以降のバージョンでは、
以下の書式で IPv6 の fragroute がサポートされてます:

* ip6_opt [route <segments> <ip6-addr> ...] | [raw <type> <byte stream>]
* ip6_qos <traffic-class> <flow-label>

<h3><a name="usage"></a>使い方／Usage</h3>

pcap ファイルに含まれる全てのパケットをフラグメントするには:

```
$ tcprewrite --fragroute=frag.conf --infile=input.pcap --outfile=output.pcap
```

(tcpprep で作成された) *tcpprep cache file* があって、
パケットの向きで(クライアントからサーバ、または，サーバからクライアント)
fragroute する場合は `--fragdir` オプションを使います:

```
$ tcprewrite --fragroute=frag.conf --fragdir=c2s --cachefile=input.cache --infile=input.pcap --outfile=output.pcap
```

fragroute エンジンは，一番最後に処理されます。tcprewrite のオプションが先に適応されます。

[tcpreplay-edit]:      tcpreplay-edit.html
[tcpreplay]:           tcpreplay.html
[tcpprep]:             tcpprep.html

