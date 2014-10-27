---
layout: content
title:  "tcpreplay-edit man page"
categories: tcpreplay wiki
description: "tcpreplay-edit のマニュアル／tcpreplay-edit manual"
---

<!-- Creator     : groff version 1.20.1 -->
<!-- CreationDate: Fri Jan 10 00:00:01 2014 -->
<!-- <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"
"http://www.w3.org/TR/html4/loose.dtd">
 -->
 <html>
<head>
<meta name="generator" content="groff -Thtml, see www.gnu.org">
<meta http-equiv="Content-Type" content="text/html; charset=US-ASCII">
<meta name="Content-Style" content="text/css">
<style type="text/css">
       p       { margin-top: 0; margin-bottom: 0; vertical-align: top }
       pre     { margin-top: 0; margin-bottom: 0; vertical-align: top }
       table   { margin-top: 0; margin-bottom: 0; vertical-align: top }
       h1      { text-align: center }
</style>
<title>tcpreplay-edit</title>

</head>
<body>

<h1 align="center">tcpreplay-edit</h1>

<a href="#NAME">NAME</a><br>
<a href="#SYNOPSIS">SYNOPSIS</a><br>
<a href="#DESCRIPTION">DESCRIPTION</a><br>
<a href="#OPTIONS">OPTIONS</a><br>
<a href="#OPTION PRESETS">OPTION PRESETS</a><br>
<a href="#FILES">FILES</a><br>
<a href="#EXIT STATUS">EXIT STATUS</a><br>
<a href="#AUTHORS">AUTHORS</a><br>
<a href="#COPYRIGHT">COPYRIGHT</a><br>
<a href="#BUGS">BUGS</a><br>
<a href="#NOTES">NOTES</a><br>

<hr>


<h2>名前／NAME
<a name="NAME"></a>
</h2>


<p style="margin-left:11%; margin-top: 1em">tcpreplay-edit
&minus; pcap ファイルに保存されたネットワークトラフィックを再送信する</p>

<h2>書式／SYNOPSIS
<a name="SYNOPSIS"></a>
</h2>



<p style="margin-left:11%; margin-top: 1em"><b>tcpreplay-edit</b>
[<b>&minus;</b><i>flag</i> [<i>value</i>]]...
[<b>&minus;&minus;</b><i>opt&minus;name</i> [[=|
]<i>value</i>]]...<b>&lt;pcap_file(s)&gt;</b></p>

<p style="margin-left:11%; margin-top: 1em">
tcpreplay は tcpdump や他の pcap(3) ファイルに保存された
ネットワークトラフィックを再送信するツールです。</p>

<h2>概要／DESCRIPTION
<a name="DESCRIPTION"></a>
</h2>


<p style="margin-left:11%; margin-top: 1em">
tcpreplay の基本的な使い方は、指定されたファイル群を
キャプチャされた時と同じ速度で再送信したり、
あるいは、ハードウェアが許す限りの速度の範囲内であれば、
指定したデータ速度で再送信します。
オプションとして、2つの NIC にトラフィックを分割したり、
ファイルに保存したり、様々な方法でフィルタしたり編集したり、
FireWall や NIDS や他のネットワークデバイスのテスト環境を提供します。
Tcpreplay Manual http://tcpreplay.appneta.com のページに、
もっと詳細な情報が記載されています。</p>

<h2>オプション／OPTIONS
<a name="OPTIONS"></a>
</h2>



<p style="margin-left:11%; margin-top: 1em"><b>&minus;r</b>
<i>string</i>,
<b>&minus;&minus;portmap</b>=<i>string</i></p>

<p style="margin-left:22%;">
TCP/UDP ポート番号を編集する。
このオプションは -1回まで指定できます。</p>

<p style="margin-left:22%; margin-top: 1em">
コロン(:) 区切りのポートのマッピングを、
カンマ(,) 区切りのリストを指定します。
コロン(:) で区切られたポートのペアが、書き換えのペアになります。
例えば:<br>

&minus;-portmap=80:8000 &minus;-portmap=8080:80
# 80 は 8000 に、8080 は 80 になります<br>
&minus;-portmap=8000,8080,88888:80
# 3 つのポート(8000 と 8080 と 88888)が 80 になります<br>
&minus;-portmap=8000-8999:80
# 8000 から 8999 までのポートが 80 になります</p>

<p style="margin-left:11%;"><b>&minus;s</b> <i>number</i>,
<b>&minus;&minus;seed</b>=<i>number</i></p>

<p style="margin-left:22%;">
与えられた seed を使って、
送信元／送信先の IPv4/v6 アドレスをランダムに書き換えます。
このオプションは 1回だけ指定できます。
オプションには整数を指定します。</p>

<p style="margin-left:22%; margin-top: 1em">
送信元／送信先の IPv4/v6 アドレスは擬似的にランダムに書き換わりますが、
クライアントのサーバの関係性(通信のペア)は保たれたままになります。
seed をベースにランダムに書き換わるので，
同じ seed を指定すると、
前回と同様にランダマイズされた通信を再現することができます。</p>

<p style="margin-left:11%;"><b>&minus;N</b> <i>string</i>,
<b>&minus;&minus;pnat</b>=<i>string</i></p>

<p style="margin-left:22%;">
擬似 NAT 機能を使って IPv4/v6 アドレスを書き換えます。
このオプションは 2回まで指定できます。
このオプションは下記のオプションと一緒には使用できません: srcipmap</p>

<p style="margin-left:22%; margin-top: 1em">
コロン(:) 区切りの CIDR 表記のネットワークブロックのペアを、
カンマ(,) で区切って複数指定できます。
それぞれのネットワークブロックのペアは、
指定された IPアドレスの順番で処理されます。
もし、パケットが最初のネットワークブロックにマッチした場合、
2番目のネットワークブロックに書き換えられます。
さらに上位のビットは参照されません。IPv4 の実行例:<br>

&minus;-pnat=192.168.0.0/16:10.77.0.0/16,172.16.0.0/12:10.1.0.0/24
<br>
IPv6 の実行例:<br>

&minus;-pnat=[2001:db8::/32]:[dead::/16],[2001:db8::/32]:[::ffff:0:0/96]</p>

<p style="margin-left:11%;"><b>&minus;S</b> <i>string</i>,
<b>&minus;&minus;srcipmap</b>=<i>string</i></p>

<p style="margin-left:22%;">
擬似 NAT 機能を使って送信元 IPv4/v6 アドレスを書き換えます。
このオプションは 1回だけ指定できます。
このオプションは下記のオプションと一緒には使用できません: pnat</p>

<p style="margin-left:22%; margin-top: 1em">
&minus;-pnat オプションと同様に動きますが、
IPv4/v6 ヘッダの送信元の IPアドレスだけに機能します。</p>

<p style="margin-left:11%;"><b>&minus;D</b> <i>string</i>,
<b>&minus;&minus;dstipmap</b>=<i>string</i></p>

<p style="margin-left:22%;">
擬似 NAT 機能を使って送信先 IPv4/v6 アドレスを書き換えます。
このオプションは 1回だけ指定できます。
このオプションは下記のオプションと一緒には使用できません: pnat</p>

<p style="margin-left:22%; margin-top: 1em">
&minus;-pnat オプションと同様に動きますが、
IPv4/v6 ヘッダの送信先の IPアドレスだけに機能します。</p>

<p style="margin-left:11%;"><b>&minus;e</b> <i>string</i>,
<b>&minus;&minus;endpoints</b>=<i>string</i></p>

<p style="margin-left:22%;">
2つのエンドポイントの IPアドレスを書き換えます。
このオプションは 1回だけ指定できます。
このオプションは、下記のオプションと一緒に使用する必要があります: cachefile</p>

<p style="margin-left:22%; margin-top: 1em">
全てのトラフィックを、
コロン(:) 区切りの IPv4/v6 アドレスのペアに書き換えます。
IPv4 の実行例:<br>
&minus;-endpoints=172.16.0.1:172.16.0.2 <br>
IPv6 の実行例:<br>

&minus;-endpoints=[2001:db8::dead:beef]:[::ffff:0:0:ac:f:0:2]</p>

<p style="margin-left:11%;"><b>&minus;b</b>,
<b>-&minus;skipbroadcast</b></p>

<p style="margin-left:22%;">
broadcast/multicast の IPv4/v6 アドレスの書き換えをスキップします。</p>

<p style="margin-left:22%; margin-top: 1em">
デフォルトでは、
&minus;-seed, &minus;-pnat and &minus;-endpoints を指定すると、
ブロードキャストとマルチキャストの
IPv4/v6 アドレスと MAC アドレスを書き換えます。
このフラグを指定すると、ブロードキャストとマルチキャストの
IPv4/v6 アドレスと MAC アドレスを書き換えません。</p>

<p style="margin-left:11%;"><b>&minus;C</b>,
<b>-&minus;fixcsum</b></p>

<p style="margin-left:22%;">
IPv4 の TCP/UDP ヘッダのチェックサムを強制的に再計算します。</p>

<p style="margin-left:22%; margin-top: 1em">
IPv4/v6 のヘッダのチェックサムを再計算します。
下記のオプションを指定した場合は、自動的に再計算されます。
<b>--seed</b>, <b>--pnat</b>, <b>--endpoints</b> <b>--fixlen</b></p>

<p style="margin-left:11%;"><b>&minus;m</b> <i>number</i>,
<b>&minus;&minus;mtu</b>=<i>number</i></p>

<p style="margin-left:22%;">
デフォルトの MTU サイズ(1500バイト)を上書きします。
このオプションは 1回だけ指定できます。
オプションには整数を指定します。</p>
<i>number</i> の値は下記が想定されています:</p>

<p style="margin-left:28%;">
1 から MAXPACKET までの値を指定します。</p>

<p style="margin-left:22%; margin-top: 1em">
padding できる最大サイズ (--fixlen=pad)、あるいは、
truncate する(切り詰める)サイズ(--mtu-trunc) を決定するために、
デフォルトの 1500バイトの MTU サイズを上書きします。</p>

<p style="margin-left:11%;"><b>&minus;&minus;mtu&minus;trunc</b></p>

<p style="margin-left:22%;">
指定された MTU より大きいパケットを truncate します(切り詰めます)。
このオプションは 1回だけ指定できます。</p>

<p style="margin-left:22%; margin-top: 1em">
&minus;-fixlen と同様に、
Layer 3 以上のパケットを MTU よりも大きくならないように
truncate します(切り詰めます)。</p>

<p style="margin-left:11%;"><b>&minus;E</b>,
<b>-&minus;efcs</b></p>

<p style="margin-left:22%;">Ethrenet フレームの最後に付与される FCS
(Frame Check Sequence)を取り除きます。</p>

<p style="margin-left:22%; margin-top: 1em">
このオプションはかなり危険です!
(tcpreplay-edit は) FCS が実際に付与されているかどうかは確認せず、
ただ単にフレームの最後の 4バイトを削除するだけです。
従って、使用している OS が raw パケットを処理する時に、
FCS を付与している時だけこのオプションを指定すべきです。</p>

<p style="margin-left:11%;"><b>&minus;&minus;ttl</b>=<i>string</i></p>

<p style="margin-left:22%;">
IPv4/v6 の TTL/Hop Limit を書き換えます。</p>

<p style="margin-left:22%; margin-top: 1em">
全ての IPv4/v6 パケットの TTL/Hop Limit を書き換えます。
値を直接指定するか、(1～255 の間で) 増減させる値を指定します。
実行例:
<br>
&minus;-ttl=10 <br>
&minus;-ttl=+7 <br>
&minus;-ttl=-64</p>


<p style="margin-left:11%;"><b>&minus;&minus;tos</b>=<i>number</i></p>

<p style="margin-left:22%;">
IPv4 の TOS/DiffServ/ECN の値を設定します。
このオプションは 1回だけ指定できます。
オプションには整数を指定します。</p>
<i>number</i> の値は下記が想定されています:</p>

<p style="margin-left:28%;">0 から 255 までの値</p>

<p style="margin-left:22%; margin-top: 1em">
IPv4 の (DiffServ/ECN としても知られている) TOS の値を上書きします。</p>

<p style="margin-left:11%;"><b>&minus;&minus;tclass</b>=<i>number</i></p>

<p style="margin-left:22%;">
IPv6 Traffic Class の値を設定します。
このオプションは 1回だけ指定できます。
オプションには整数を指定します。</p>
<i>number</i> の値は下記が想定されています:</p>

<p style="margin-left:28%;">0 から 255 までの値</p>

<p style="margin-left:22%; margin-top: 1em">
IPv6 の Traffic Class の値を上書きします。</p>


<p style="margin-left:11%;"><b>&minus;&minus;flowlabel</b>=<i>number</i></p>

<p style="margin-left:22%;">
IPv6 の Flow Label を設定します。
このオプションは 1回だけ指定できます。
オプションには整数を指定します。</p>
<i>number</i> の値は下記が想定されています:</p>

<p style="margin-left:28%;">0 から 1048575 までの値</p>

<p style="margin-left:22%; margin-top: 1em">
IPv6 の Flow Label の 20bit の値を上書きします。
IPv4 のパケットは何も処理しません。</p>

<p style="margin-left:11%;"><b>&minus;F</b> <i>string</i>,
<b>&minus;&minus;fixlen</b>=<i>string</i></p>

<p style="margin-left:22%;">
ヘッダ長に合致するように、
パケットに padding (追加)したり truncate (切り詰め)したりします。
このオプションは 1回だけ指定できます。</p>

<p style="margin-left:22%; margin-top: 1em">
snaplen がパケットのサイズよりも小さかった場合、
パケットは truncate されます(切り詰められます)。


Packets may be
truncated during capture if the snaplen is smaller then the
packet. This option allows you to modify the packet to pad
the packet back out to the size stored in the IPv4/v6 header
or rewrite the IP header total length to reflect the stored
packet length.</p>

<p style="margin-left:22%; margin-top: 1em"><b>pad</b>
Truncated packets will be padded out so that the packet
length matches the IPv4 total length</p>

<p style="margin-left:22%; margin-top: 1em"><b>trunc</b>
Truncated packets will have their IPv4 total length field
rewritten to match the actual packet length</p>

<p style="margin-left:22%; margin-top: 1em"><b>del</b>
Delete the packet</p>


<p style="margin-left:11%;"><b>&minus;&minus;skipl2broadcast</b></p>

<p style="margin-left:22%;">Skip rewriting
broadcast/multicast Layer 2 addresses.</p>

<p style="margin-left:22%; margin-top: 1em">By default,
editing Layer 2 addresses will rewrite broadcast and
multicast MAC addresses. Setting this flag will keep
broadcast/multicast MAC addresses from being rewritten.</p>


<p style="margin-left:11%;"><b>&minus;&minus;dlt</b>=<i>string</i></p>

<p style="margin-left:22%;">Override output DLT
encapsulation. This option may appear up to 1 times.</p>

<p style="margin-left:22%; margin-top: 1em">By default, no
DLT (data link type) conversion will be made. To change the
DLT type of the output pcap, select one of the following
values:</p>

<p style="margin-left:22%; margin-top: 1em"><b>enet</b>
Ethernet aka DLT_EN10MB</p>

<p style="margin-left:22%; margin-top: 1em"><b>hdlc</b>
Cisco HDLC aka DLT_C_HDLC</p>


<p style="margin-left:22%; margin-top: 1em"><b>jnpr_ether</b>
Juniper Ethernet DLT_C_JNPR_ETHER</p>


<p style="margin-left:22%; margin-top: 1em"><b>pppserial</b>
PPP Serial aka DLT_PPP_SERIAL</p>

<p style="margin-left:22%; margin-top: 1em"><b>user</b>
User specified Layer 2 header and DLT type</p>


<p style="margin-left:11%;"><b>&minus;&minus;enet&minus;dmac</b>=<i>string</i></p>

<p style="margin-left:22%;">Override destination ethernet
MAC addresses. This option may appear up to 1 times.</p>

<p style="margin-left:22%; margin-top: 1em">Takes a pair of
comma deliminated ethernet MAC addresses which will replace
the destination MAC address of outbound packets. The first
MAC address will be used for the server to client traffic
and the optional second MAC address will be used for the
client to server traffic. Example: <br>
&minus;-enet-dmac=00:12:13:14:15:16,00:22:33:44:55:66</p>


<p style="margin-left:11%;"><b>&minus;&minus;enet&minus;smac</b>=<i>string</i></p>

<p style="margin-left:22%;">Override source ethernet MAC
addresses. This option may appear up to 1 times.</p>

<p style="margin-left:22%; margin-top: 1em">Takes a pair of
comma deliminated ethernet MAC addresses which will replace
the source MAC address of outbound packets. The first MAC
address will be used for the server to client traffic and
the optional second MAC address will be used for the client
to server traffic. Example: <br>
&minus;-enet-smac=00:12:13:14:15:16,00:22:33:44:55:66</p>


<p style="margin-left:11%;"><b>&minus;&minus;enet&minus;vlan</b>=<i>string</i></p>

<p style="margin-left:22%;">Specify ethernet 802.1q VLAN
tag mode. This option may appear up to 1 times.</p>

<p style="margin-left:22%; margin-top: 1em">Allows you to
rewrite ethernet frames to add a 802.1q header to standard
802.3 ethernet headers or remove the 802.1q VLAN tag
information.</p>

<p style="margin-left:22%; margin-top: 1em"><b>add</b>
Rewrites the existing 802.3 ethernet header as an 802.1q
VLAN header</p>

<p style="margin-left:22%; margin-top: 1em"><b>del</b>
Rewrites the existing 802.1q VLAN header as an 802.3
ethernet header</p>


<p style="margin-left:11%;"><b>&minus;&minus;enet&minus;vlan&minus;tag</b>=<i>number</i></p>

<p style="margin-left:22%;">Specify the new ethernet 802.1q
VLAN tag value. This option may appear up to 1 times. This
option must appear in combination with the following
options: enet-vlan. This option takes an integer number as
its argument. The value of <i>number</i> is constrained to
being:</p>

<p style="margin-left:28%;">in the range 0 through 4095</p>


<p style="margin-left:11%;"><b>&minus;&minus;enet&minus;vlan&minus;cfi</b>=<i>number</i></p>

<p style="margin-left:22%;">Specify the ethernet 802.1q
VLAN CFI value. This option may appear up to 1 times. This
option must appear in combination with the following
options: enet-vlan. This option takes an integer number as
its argument. The value of <i>number</i> is constrained to
being:</p>

<p style="margin-left:28%;">in the range 0 through 1</p>


<p style="margin-left:11%;"><b>&minus;&minus;enet&minus;vlan&minus;pri</b>=<i>number</i></p>

<p style="margin-left:22%;">Specify the ethernet 802.1q
VLAN priority. This option may appear up to 1 times. This
option must appear in combination with the following
options: enet-vlan. This option takes an integer number as
its argument. The value of <i>number</i> is constrained to
being:</p>

<p style="margin-left:28%;">in the range 0 through 7</p>


<p style="margin-left:11%;"><b>&minus;&minus;hdlc&minus;control</b>=<i>number</i></p>

<p style="margin-left:22%;">Specify HDLC control value.
This option may appear up to 1 times. This option takes an
integer number as its argument.</p>

<p style="margin-left:22%; margin-top: 1em">The Cisco HDLC
header has a 1 byte &quot;control&quot; field. Apparently
this should always be 0, but if you can use any 1 byte
value.</p>


<p style="margin-left:11%;"><b>&minus;&minus;hdlc&minus;address</b>=<i>number</i></p>

<p style="margin-left:22%;">Specify HDLC address. This
option may appear up to 1 times. This option takes an
integer number as its argument.</p>

<p style="margin-left:22%; margin-top: 1em">The Cisco HDLC
header has a 1 byte &quot;address&quot; field which has two
valid values:</p>

<p style="margin-left:22%; margin-top: 1em"><b>0x0F</b>
Unicast</p>

<p style="margin-left:22%; margin-top: 1em"><b>0xBF</b>
Broadcast <br>
You can however specify any single byte value.</p>


<p style="margin-left:11%;"><b>&minus;&minus;user&minus;dlt</b>=<i>number</i></p>

<p style="margin-left:22%;">Set output file DLT type. This
option may appear up to 1 times. This option takes an
integer number as its argument.</p>

<p style="margin-left:22%; margin-top: 1em">Set the DLT
value of the output pcap file.</p>


<p style="margin-left:11%;"><b>&minus;&minus;user&minus;dlink</b>=<i>string</i></p>

<p style="margin-left:22%;">Rewrite Data-Link layer with
user specified data. This option may appear up to 2
times.</p>

<p style="margin-left:22%; margin-top: 1em">Provide a
series of comma deliminated hex values which will be used to
rewrite or create the Layer 2 header of the packets. The
first instance of this argument will rewrite both server and
client traffic, but if this argument is specified a second
time, it will be used for the client traffic. Example: <br>

&minus;-user-dlink=01,02,03,04,05,06,00,1A,2B,3C,4D,5E,6F,08,00</p>

<p style="margin-left:11%;"><b>&minus;d</b> <i>number</i>,
<b>&minus;&minus;dbug</b>=<i>number</i></p>

<p style="margin-left:22%;">Enable debugging output. This
option may appear up to 1 times. This option takes an
integer number as its argument. The value of <i>number</i>
is constrained to being:</p>

<p style="margin-left:28%;">in the range 0 through 5</p>

<p style="margin-left:22%;">The default <i>number</i> for
this option is: <br>
0</p>

<p style="margin-left:22%; margin-top: 1em">If configured
with &minus;-enable-debug, then you can specify a verbosity
level for debugging output. Higher numbers increase
verbosity.</p>

<p style="margin-left:11%;"><b>&minus;q</b>,
<b>-&minus;quiet</b></p>

<p style="margin-left:22%;">Quiet mode.</p>

<p style="margin-left:22%; margin-top: 1em">Print nothing
except the statistics at the end of the run</p>

<p style="margin-left:11%;"><b>&minus;T</b> <i>string</i>,
<b>&minus;&minus;timer</b>=<i>string</i></p>

<p style="margin-left:22%;">Select packet timing mode:
select, ioport, gtod, nano. This option may appear up to 1
times. The default <i>string</i> for this option is: <br>
gtod</p>

<p style="margin-left:22%; margin-top: 1em">Allows you to
select the packet timing method to use:</p>

<p style="margin-left:22%; margin-top: 1em"><i>nano</i> -
Use nanosleep() API</p>

<p style="margin-left:22%; margin-top: 1em"><i>select</i> -
Use select() API</p>

<p style="margin-left:22%; margin-top: 1em"><i>ioport</i> -
Write to the i386 IO Port 0x80</p>

<p style="margin-left:22%; margin-top: 1em"><i>gtod
[default]</i> - Use a gettimeofday() loop</p>


<p style="margin-left:11%;"><b>&minus;&minus;maxsleep</b>=<i>number</i></p>

<p style="margin-left:22%;">Sleep for no more then X
milliseconds between packets. This option takes an integer
number as its argument. The default <i>number</i> for this
option is: <br>
0</p>

<p style="margin-left:22%; margin-top: 1em">Set a limit for
the maximum number of milliseconds that tcpreplay will sleep
between packets. Effectively prevents long delays between
packets without effecting the majority of packets. Default
is disabled.</p>

<p style="margin-left:11%;"><b>&minus;v</b>,
<b>-&minus;verbose</b></p>

<p style="margin-left:22%;">Print decoded packets via
tcpdump to STDOUT. This option may appear up to 1 times.</p>

<p style="margin-left:11%;"><b>&minus;A</b> <i>string</i>,
<b>&minus;&minus;decode</b>=<i>string</i></p>

<p style="margin-left:22%;">Arguments passed to tcpdump
decoder. This option may appear up to 1 times. This option
must appear in combination with the following options:
verbose.</p>

<p style="margin-left:22%; margin-top: 1em">When enabling
verbose mode (<b>-v</b>) you may also specify one or more
additional arguments to pass to <b>tcpdump</b> to modify the
way packets are decoded. By default, &minus;n and &minus;l
are used. Be sure to quote the arguments like: &minus;A
&quot;-axxx&quot; so that they are not interpreted by
tcpreplay. Please see the tcpdump(1) man page for a complete
list of options.</p>

<p style="margin-left:11%;"><b>&minus;K</b>,
<b>-&minus;preload&minus;pcap</b></p>

<p style="margin-left:22%;">Preloads packets into RAM
before sending.</p>

<p style="margin-left:22%; margin-top: 1em">This option
loads the specified pcap(s) into RAM before starting to send
in order to improve replay performance while introducing a
startup performance hit. Preloading can be used with or
without <b>--loop</b>. This option also suppresses flow
statistics collection for every iteration, which can
significantly reduce memory usage. Flow statistics are
predicted based on options supplied and statistics collected
from the first loop iteration.</p>

<p style="margin-left:11%;"><b>&minus;c</b> <i>string</i>,
<b>&minus;&minus;cachefile</b>=<i>string</i></p>

<p style="margin-left:22%;">Split traffic via a tcpprep
cache file. This option may appear up to 1 times. This
option must appear in combination with the following
options: intf2. This option must not appear in combination
with any of the following options: dualfile.</p>

<p style="margin-left:22%; margin-top: 1em">If you have a
pcap file you would like to use to send bi-directional
traffic through a device (firewall, router, IDS, etc) then
using tcpprep you can create a cachefile which tcpreplay
will use to split the traffic across two network
interfaces.</p>

<p style="margin-left:11%;"><b>&minus;2</b>,
<b>-&minus;dualfile</b></p>

<p style="margin-left:22%;">Replay two files at a time from
a network tap. This option may appear up to 1 times. This
option must appear in combination with the following
options: intf2. This option must not appear in combination
with any of the following options: cachefile.</p>

<p style="margin-left:22%; margin-top: 1em">If you captured
network traffic using a network tap, then you can end up
with two pcap files- one for each direction. This option
will replay these two files at the same time, one on each
interface and inter-mix them using the timestamps in
each.</p>

<p style="margin-left:11%;"><b>&minus;i</b> <i>string</i>,
<b>&minus;&minus;intf1</b>=<i>string</i></p>

<p style="margin-left:22%;">Client to server/RX/primary
traffic output interface. This option may appear up to 1
times.</p>

<p style="margin-left:22%; margin-top: 1em">Required
network interface used to send either all traffic or traffic
which is marked as &rsquo;primary&rsquo; via tcpprep.
Primary traffic is usually client-to-server or inbound (RX)
on khial virtual interfaces.</p>

<p style="margin-left:11%;"><b>&minus;I</b> <i>string</i>,
<b>&minus;&minus;intf2</b>=<i>string</i></p>

<p style="margin-left:22%;">Server to client/TX/secondary
traffic output interface. This option may appear up to 1
times.</p>

<p style="margin-left:22%; margin-top: 1em">Optional
network interface used to send traffic which is marked as
&rsquo;secondary&rsquo; via tcpprep. Secondary traffic is
usually server-to-client or outbound (TX) on khial virtual
interfaces. Generally, it only makes sense to use this
option with &minus;-cachefile.</p>


<p style="margin-left:11%;"><b>&minus;&minus;listnics</b></p>

<p style="margin-left:22%;">List available network
interfaces and exit.</p>

<p style="margin-left:11%;"><b>&minus;l</b> <i>number</i>,
<b>&minus;&minus;loop</b>=<i>number</i></p>

<p style="margin-left:22%;">Loop through the capture file X
times. This option may appear up to 1 times. This option
takes an integer number as its argument. The value of
<i>number</i> is constrained to being:</p>

<p style="margin-left:28%;">greater than or equal to 0</p>

<p style="margin-left:22%;">The default <i>number</i> for
this option is: <br>
1</p>


<p style="margin-left:11%;"><b>&minus;&minus;pktlen</b></p>

<p style="margin-left:22%;">Override the snaplen and use
the actual packet len. This option may appear up to 1
times.</p>

<p style="margin-left:22%; margin-top: 1em">By default,
tcpreplay will send packets based on the size of the
&quot;snaplen&quot; stored in the pcap file which is usually
the correct thing to do. However, occasionally, tools will
store more bytes then told to. By specifying this option,
tcpreplay will ignore the snaplen field and instead try to
send packets based on the original packet length. Bad things
may happen if you specify this option.</p>

<p style="margin-left:11%;"><b>&minus;L</b> <i>number</i>,
<b>&minus;&minus;limit</b>=<i>number</i></p>

<p style="margin-left:22%;">Limit the number of packets to
send. This option may appear up to 1 times. This option
takes an integer number as its argument. The value of
<i>number</i> is constrained to being:</p>

<p style="margin-left:28%;">greater than or equal to 1</p>

<p style="margin-left:22%;">The default <i>number</i> for
this option is: <br>
-1</p>

<p style="margin-left:22%; margin-top: 1em">By default,
tcpreplay will send all the packets. Alternatively, you can
specify a maximum number of packets to send.</p>

<p style="margin-left:11%;"><b>&minus;x</b> <i>string</i>,
<b>&minus;&minus;multiplier</b>=<i>string</i></p>

<p style="margin-left:22%;">Modify replay speed to a given
multiple. This option may appear up to 1 times. This option
must not appear in combination with any of the following
options: pps, mbps, oneatatime, topspeed.</p>

<p style="margin-left:22%; margin-top: 1em">Specify a value
to modify the packet replay speed. Examples: <br>
2.0 will replay traffic at twice the speed captured <br>
0.7 will replay traffic at 70% the speed captured</p>

<p style="margin-left:11%;"><b>&minus;p</b> <i>number</i>,
<b>&minus;&minus;pps</b>=<i>number</i></p>

<p style="margin-left:22%;">Replay packets at a given
packets/sec. This option may appear up to 1 times. This
option must not appear in combination with any of the
following options: multiplier, mbps, oneatatime, topspeed.
This option takes an integer number as its argument.</p>

<p style="margin-left:11%;"><b>&minus;M</b> <i>string</i>,
<b>&minus;&minus;mbps</b>=<i>string</i></p>

<p style="margin-left:22%;">Replay packets at a given Mbps.
This option may appear up to 1 times. This option must not
appear in combination with any of the following options:
multiplier, pps, oneatatime, topspeed.</p>

<p style="margin-left:22%; margin-top: 1em">Specify a
floating point value for the Mbps rate that tcpreplay should
send packets at.</p>

<p style="margin-left:11%;"><b>&minus;t</b>,
<b>-&minus;topspeed</b></p>

<p style="margin-left:22%;">Replay packets as fast as
possible. This option must not appear in combination with
any of the following options: mbps, multiplier, pps,
oneatatime.</p>

<p style="margin-left:11%;"><b>&minus;o</b>,
<b>-&minus;oneatatime</b></p>

<p style="margin-left:22%;">Replay one packet at a time for
each user input. This option must not appear in combination
with any of the following options: mbps, pps, multiplier,
topspeed.</p>

<p style="margin-left:22%; margin-top: 1em">Allows you to
step through one or more packets at a time.</p>


<p style="margin-left:11%;"><b>&minus;&minus;pps&minus;multi</b>=<i>number</i></p>

<p style="margin-left:22%;">Number of packets to send for
each time interval. This option must appear in combination
with the following options: pps. This option takes an
integer number as its argument. The value of <i>number</i>
is constrained to being:</p>

<p style="margin-left:28%;">greater than or equal to 1</p>

<p style="margin-left:22%;">The default <i>number</i> for
this option is: <br>
1</p>

<p style="margin-left:22%; margin-top: 1em">When trying to
send packets at very high rates, the time between each
packet can be so short that it is impossible to accurately
sleep for the required period of time. This option allows
you to send multiple packets at a time, thus allowing for
longer sleep times which can be more accurately
implemented.</p>


<p style="margin-left:11%;"><b>&minus;&minus;unique&minus;ip</b></p>

<p style="margin-left:22%;">Modify IP addresses each loop
iteration to generate unique flows. This option must appear
in combination with the following options: loop. This option
must not appear in combination with any of the following
options: seed.</p>

<p style="margin-left:22%; margin-top: 1em">Ensure IPv4 and
IPv6 packets will be unique for each <b>--loop</b>
iteration. This is done in a way that will not alter packet
CRC, and therefore will genrally not affect performance.
This option will significantly increase the flows/sec over
generated over multiple loop iterations.</p>


<p style="margin-left:11%;"><b>&minus;&minus;netmap</b></p>

<p style="margin-left:22%;">Write packets directly to
netmap enabled network adapter.</p>

<p style="margin-left:22%; margin-top: 1em">This feature
will detect netmap capable network drivers on Linux and BSD
systems. If detected, the network driver is bypassed for the
execution duration, and network buffers will be written to
directly. This will allow you to achieve full line rates on
commodity network adapters, similar to rates achieved by
commercial network traffic generators. Note that bypassing
the network driver will disrupt other applications connected
through the test interface. See INSTALL for more
information.</p>


<p style="margin-left:11%;"><b>&minus;&minus;no&minus;flow&minus;stats</b></p>

<p style="margin-left:22%;">Suppress printing and tracking
flow count, rates and expirations.</p>

<p style="margin-left:22%; margin-top: 1em">Suppress the
collection and printing of flow statistics. This option may
improve performance when not using <b>--preload-pcap</b>
option, otherwise its only function is to suppress printing.
The flow feature will track and print statistics of the
flows being sent. A flow is loosely defined as a unique
combination of a 5-tuple, i.e. source IP, destination IP,
source port, destination port and protocol. If <b>--loop</b>
is specified, the flows from one iteration to the next will
not be unique, unless the packets are altered. Use
<b>--unique-ip</b> or <b>tcpreplay-edit</b> to alter packets
between iterations.</p>


<p style="margin-left:11%;"><b>&minus;&minus;flow&minus;expiry</b>=<i>number</i></p>

<p style="margin-left:22%;">Number of inactive seconds
before a flow is considered expired. This option must not
appear in combination with any of the following options:
no-flow-stats. This option takes an integer number as its
argument. The value of <i>number</i> is constrained to
being:</p>

<p style="margin-left:28%;">greater than or equal to 0</p>

<p style="margin-left:22%;">The default <i>number</i> for
this option is: <br>
0</p>

<p style="margin-left:22%; margin-top: 1em">This option
will track and report flow expirations based on the flow
idle times. The timestamps within the pcap file are used to
determine the expiry, not the actual timestamp of the
packets are replayed. For example, a value of 30 suggests
that if no traffic is seen on a flow for 30 seconds, any
subsequent traffic would be considered a new flow, and
thereby will increment the flows and flows per second (fps)
statistics. This option can be used to optimize flow timeout
settings for flow products. Setting the timeout low may lead
to flows being dropped when in fact the flow is simply slow
to respond. Configuring your flow timeouts too high may
increase resources required by your flow product. Note that
using this option while replaying at higher than original
speeds can lead to inflated flows and fps counts. Default is
0 (no expiry) and a typical value is 30-120 seconds.</p>

<p style="margin-left:11%;"><b>&minus;P</b>,
<b>-&minus;pid</b></p>

<p style="margin-left:22%;">Print the PID of tcpreplay at
startup.</p>


<p style="margin-left:11%;"><b>&minus;&minus;stats</b>=<i>number</i></p>

<p style="margin-left:22%;">Print statistics every X
seconds. This option takes an integer number as its
argument. The value of <i>number</i> is constrained to
being:</p>

<p style="margin-left:28%;">greater than or equal to 1</p>

<p style="margin-left:22%; margin-top: 1em">Note that this
is very much a &quot;best effort&quot; and long delays
between sending packets may cause equally long delays
between printing statistics.</p>

<p style="margin-left:11%;"><b>&minus;V</b>,
<b>-&minus;version</b></p>

<p style="margin-left:22%;">Print version information.</p>

<p style="margin-left:11%;"><b>&minus;h</b>,
<b>-&minus;less&minus;help</b></p>

<p style="margin-left:22%;">Display less usage information
and exit.</p>

<p style="margin-left:11%;"><b>&minus;H</b>,
<b>&minus;&minus;help</b></p>

<p style="margin-left:22%;">Display usage information and
exit.</p>

<p style="margin-left:11%;"><b>&minus;!</b>,
<b>&minus;&minus;more-help</b></p>

<p style="margin-left:22%;">Pass the extended usage
information through a pager.</p>

<p style="margin-left:11%;"><b>&minus;</b> [<i>rcfile</i>],
<b>&minus;&minus;save-opts</b>[=<i>rcfile</i>]</p>

<p style="margin-left:22%;">Save the option state to
<i>rcfile</i>. The default is the <i>last</i> configuration
file listed in the <b>OPTION PRESETS</b> section, below.</p>

<p style="margin-left:11%;"><b>&minus;</b> <i>rcfile</i>,
<b>&minus;&minus;load-opts</b>=<i>rcfile</i>,
<b>&minus;&minus;no-load-opts</b></p>

<p style="margin-left:22%;">Load options from
<i>rcfile</i>. The <i>no-load-opts</i> form will disable the
loading of earlier RC/INI files.
<i>&minus;&minus;no-load-opts</i> is handled early, out of
order.</p>

<h2>OPTION PRESETS
<a name="OPTION PRESETS"></a>
</h2>


<p style="margin-left:11%; margin-top: 1em">Any option that
is not marked as <i>not presettable</i> may be preset by
loading values from configuration (&quot;RC&quot; or
&quot;.INI&quot;) file(s). The <i>homerc</i> file is
&quot;<i>$$/</i>&quot;, unless that is a directory. In that
case, the file &quot;<i>.tcpreplay-editrc</i>&quot; is
searched for within that directory.</p>

<h2>FILES
<a name="FILES"></a>
</h2>


<p style="margin-left:11%; margin-top: 1em">See <b>OPTION
PRESETS</b> for configuration files.</p>

<h2>EXIT STATUS
<a name="EXIT STATUS"></a>
</h2>


<p style="margin-left:11%; margin-top: 1em">One of the
following exit values will be returned: <b><br>
0</b> (EXIT_SUCCESS)</p>

<p style="margin-left:22%;">Successful program
execution.</p>

<p style="margin-left:11%;"><b>1</b> (EXIT_FAILURE)</p>

<p style="margin-left:22%;">The operation failed or the
command syntax was not valid.</p>

<p style="margin-left:11%;"><b>66</b> (EX_NOINPUT)</p>

<p style="margin-left:22%;">A specified configuration file
could not be loaded.</p>

<p style="margin-left:11%;"><b>70</b> (EX_SOFTWARE)</p>

<p style="margin-left:22%;">libopts had an internal
operational error. Please report it to
autogen-users@lists.sourceforge.net. Thank you.</p>

<h2>AUTHORS
<a name="AUTHORS"></a>
</h2>


<p style="margin-left:11%; margin-top: 1em">Copyright
2013-2014 Fred Klassen - AppNeta Inc. Copyright 2000-2012
Aaron Turner For support please use the
tcpreplay-users@lists.sourceforge.net mailing list. The
latest version of this software is always available from:
http://tcpreplay.appneta.com/</p>

<h2>COPYRIGHT
<a name="COPYRIGHT"></a>
</h2>


<p style="margin-left:11%; margin-top: 1em">Copyright (C)
2000-2014 Aaron Turner and Fred Klassen all rights reserved.
This program is released under the terms of the GNU General
Public License, version 3 or later.</p>

<h2>BUGS
<a name="BUGS"></a>
</h2>


<p style="margin-left:11%; margin-top: 1em">Please send bug
reports to: tcpreplay-users@lists.sourceforge.net</p>

<h2>NOTES
<a name="NOTES"></a>
</h2>


<p style="margin-left:11%; margin-top: 1em">This manual
page was <i>AutoGen</i>-erated from the
<b>tcpreplay-edit</b> option definitions.</p>
<hr>
</body>
</html>
