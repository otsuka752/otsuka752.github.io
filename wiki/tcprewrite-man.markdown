---
layout: content
title:  "tcprewrite man page"
categories: tcpreplay wiki
description: "tcprewrite のマニュアル／tcprewrite manual"
---

<!-- Creator     : groff version 1.20.1 -->
<!-- CreationDate: Tue Jan  7 15:29:42 2014 -->
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
<title>tcprewrite</title>

</head>
<body>

<h1 align="center">tcprewrite</h1>

<a href="#NAME">名前／NAME</a><br>
<a href="#SYNOPSIS">書式／SYNOPSIS</a><br>
<a href="#DESCRIPTION">概要／DESCRIPTION</a><br>
<a href="#OPTIONS">オプション／OPTIONS</a><br>
<a href="#OPTION PRESETS">オプションの事前設定／OPTION PRESETS</a><br>
<a href="#FILES">ファイル／FILES</a><br>
<a href="#EXIT STATUS">exit コード／EXIT STATUS</a><br>
<a href="#AUTHORS">AUTHORS／AUTHORS</a><br>
<a href="#COPYRIGHT">COPYRIGHT／COPYRIGHT</a><br>
<a href="#BUGS">バグ／BUGS</a><br>
<a href="#NOTES">注意／NOTES</a><br>

<hr>


<h2>名前／NAME
<a name="NAME"></a>
</h2>


<p style="margin-left:11%; margin-top: 1em">tcprewrite
&minus; pcap ファイルに保存されたパケットの編集</p>

<h2>書式／SYNOPSIS
<a name="SYNOPSIS"></a>
</h2>



<p style="margin-left:11%; margin-top: 1em"><b>tcprewrite</b>
[<b>&minus;</b><i>flag</i> [<i>value</i>]]...
[<b>&minus;&minus;</b><i>opt&minus;name</i> [[=|
]<i>value</i>]]...</p>

<p style="margin-left:11%; margin-top: 1em">
全ての引数はオプションです。</p>

<h2>概要／DESCRIPTION
<a name="DESCRIPTION"></a>
</h2>


<p style="margin-left:11%; margin-top: 1em">Tcprewrite is a
tool to rewrite packets stored in @file{pcap(3)} file
format, such as crated by tools such as @file{tcpdump(1)}
and @file{ethereal(1)}. Once a pcap file has had it&rsquo;s
packets rewritten, they can be replayed back out on the
network using @file{tcpreplay(1)}. tcprewrite currently
supports reading the following DLT types: @item
@var{DLT_C_HDLC} aka Cisco HDLC @item @var{DLT_EN10MB} aka
Ethernet @item @var{DLT_LINUX_SLL} aka Linux Cooked Socket
@item @var{DLT_RAW} aka RAW IP @item @var{DLT_NULL} aka BSD
Loopback @item @var{DLT_LOOP} aka OpenBSD Loopback @item
@var{DLT_IEEE802_11} aka 802.11a/b/g @item
@var{DLT_IEEE802_11_RADIO} aka 802.11a/b/g with Radiotap
headers @item @var{DLT_JUNIPER_ETHER} aka Juniper
Encapsulated Ethernet @item @var{DLT_PPP_SERIAL} aka PPP
over Serial Please see the --dlt option for supported DLT
types for writing. The packet editing features of tcprewrite
which distinguish between &quot;client&quot; and
&quot;server&quot; traffic requires a tcpprep(1) cache file.
For more details, please see the Tcpreplay Manual at:
http://tcpreplay.appneta.com</p>

<h2>OPTIONS
<a name="OPTIONS"></a>
</h2>



<p style="margin-left:11%; margin-top: 1em"><b>&minus;r</b>
<i>string</i>,
<b>&minus;&minus;portmap</b>=<i>string</i></p>

<p style="margin-left:22%;">Rewrite TCP/UDP ports. This
option may appear up to -1 times.</p>

<p style="margin-left:22%; margin-top: 1em">Specify a list
of comma delimited port mappingings consisting of colon
delimited port number pairs. Each colon delimited port pair
consists of the port to match followed by the port number to
rewrite. Examples: <br>
&minus;-portmap=80:8000 &minus;-portmap=8080:80 #
80-&gt;8000 and 8080-&gt;80 <br>
&minus;-portmap=8000,8080,88888:80 # 3 different ports
become 80 <br>
&minus;-portmap=8000-8999:80 # ports 8000 to 8999 become
80</p>

<p style="margin-left:11%;"><b>&minus;s</b> <i>number</i>,
<b>&minus;&minus;seed</b>=<i>number</i></p>

<p style="margin-left:22%;">Randomize src/dst IPv4/v6
addresses w/ given seed. This option may appear up to 1
times. This option takes an integer number as its
argument.</p>

<p style="margin-left:22%; margin-top: 1em">Causes the
source and destination IPv4/v6 addresses to be pseudo
randomized but still maintain client/server relationships.
Since the randomization is deterministic based on the seed,
you can reuse the same seed value to recreate the
traffic.</p>

<p style="margin-left:11%;"><b>&minus;N</b> <i>string</i>,
<b>&minus;&minus;pnat</b>=<i>string</i></p>

<p style="margin-left:22%;">Rewrite IPv4/v6 addresses using
pseudo-NAT. This option may appear up to 2 times. This
option must not appear in combination with any of the
following options: srcipmap.</p>

<p style="margin-left:22%; margin-top: 1em">Takes a comma
delimited series of colon delimited CIDR netblock pairs.
Each netblock pair is evaluated in order against the IP
addresses. If the IP address in the packet matches the first
netblock, it is rewriten using the second netblock as a mask
against the high order bits. IPv4 Example: <br>

&minus;-pnat=192.168.0.0/16:10.77.0.0/16,172.16.0.0/12:10.1.0.0/24
<br>
IPv6 Example: <br>

&minus;-pnat=[2001:db8::/32]:[dead::/16],[2001:db8::/32]:[::ffff:0:0/96]</p>

<p style="margin-left:11%;"><b>&minus;S</b> <i>string</i>,
<b>&minus;&minus;srcipmap</b>=<i>string</i></p>

<p style="margin-left:22%;">Rewrite source IPv4/v6
addresses using pseudo-NAT. This option may appear up to 1
times. This option must not appear in combination with any
of the following options: pnat.</p>

<p style="margin-left:22%; margin-top: 1em">Works just like
the &minus;-pnat option, but only affects the source IP
addresses in the IPv4/v6 header.</p>

<p style="margin-left:11%;"><b>&minus;D</b> <i>string</i>,
<b>&minus;&minus;dstipmap</b>=<i>string</i></p>

<p style="margin-left:22%;">Rewrite destination IPv4/v6
addresses using pseudo-NAT. This option may appear up to 1
times. This option must not appear in combination with any
of the following options: pnat.</p>

<p style="margin-left:22%; margin-top: 1em">Works just like
the &minus;-pnat option, but only affects the destination IP
addresses in the IPv4/v6 header.</p>

<p style="margin-left:11%;"><b>&minus;e</b> <i>string</i>,
<b>&minus;&minus;endpoints</b>=<i>string</i></p>

<p style="margin-left:22%;">Rewrite IP addresses to be
between two endpoints. This option may appear up to 1 times.
This option must appear in combination with the following
options: cachefile.</p>

<p style="margin-left:22%; margin-top: 1em">Takes a pair of
colon delimited IPv4/v6 addresses which will be used to
rewrite all traffic to appear to be between the two IP
addresses. IPv4 Example: <br>
&minus;-endpoints=172.16.0.1:172.16.0.2 <br>
IPv6 Example: <br>

&minus;-endpoints=[2001:db8::dead:beef]:[::ffff:0:0:ac:f:0:2]</p>

<p style="margin-left:11%;"><b>&minus;b</b>,
<b>-&minus;skipbroadcast</b></p>

<p style="margin-left:22%;">Skip rewriting
broadcast/multicast IPv4/v6 addresses.</p>

<p style="margin-left:22%; margin-top: 1em">By default
&minus;-seed, &minus;-pnat and &minus;-endpoints will
rewrite broadcast and multicast IPv4/v6 and MAC addresses.
Setting this flag will keep broadcast/multicast IPv4/v6 and
MAC addresses from being rewritten.</p>

<p style="margin-left:11%;"><b>&minus;C</b>,
<b>-&minus;fixcsum</b></p>

<p style="margin-left:22%;">Force recalculation of
IPv4/TCP/UDP header checksums.</p>

<p style="margin-left:22%; margin-top: 1em">Causes each
IPv4/v6 packet to have their checksums recalcualted and
fixed. Automatically enabled for packets modified with
<b>--seed</b>, <b>--pnat</b>, <b>--endpoints</b> or
<b>--fixlen</b>.</p>

<p style="margin-left:11%;"><b>&minus;m</b> <i>number</i>,
<b>&minus;&minus;mtu</b>=<i>number</i></p>

<p style="margin-left:22%;">Override default MTU length
(1500 bytes). This option may appear up to 1 times. This
option takes an integer number as its argument. The value of
<i>number</i> is constrained to being:</p>

<p style="margin-left:28%;">in the range 1 through
MAXPACKET</p>

<p style="margin-left:22%; margin-top: 1em">Override the
default 1500 byte MTU size for determining the maximum
padding length (--fixlen=pad) or when truncating
(--mtu-trunc).</p>


<p style="margin-left:11%;"><b>&minus;&minus;mtu&minus;trunc</b></p>

<p style="margin-left:22%;">Truncate packets larger then
specified MTU. This option may appear up to 1 times.</p>

<p style="margin-left:22%; margin-top: 1em">Similar to
&minus;-fixlen, this option will truncate data in packets
from Layer 3 and above to be no larger then the MTU.</p>

<p style="margin-left:11%;"><b>&minus;E</b>,
<b>-&minus;efcs</b></p>

<p style="margin-left:22%;">Remove Ethernet checksums (FCS)
from end of frames.</p>

<p style="margin-left:22%; margin-top: 1em">Note, this
option is pretty dangerous! We do not actually check to see
if a FCS actually exists in the frame, we just blindly
delete the last 4 bytes. Hence, you should only use this if
you know know that your OS provides the FCS when reading raw
packets.</p>


<p style="margin-left:11%;"><b>&minus;&minus;ttl</b>=<i>string</i></p>

<p style="margin-left:22%;">Modify the IPv4/v6 TTL/Hop
Limit.</p>

<p style="margin-left:22%; margin-top: 1em">Allows you to
modify the TTL/Hop Limit of all the IPv4/v6 packets. Specify
a number to hard-code the value or +/-value to increase or
decrease by the value provided (limited to 1-255). Examples:
<br>
&minus;-ttl=10 <br>
&minus;-ttl=+7 <br>
&minus;-ttl=-64</p>


<p style="margin-left:11%;"><b>&minus;&minus;tos</b>=<i>number</i></p>

<p style="margin-left:22%;">Set the IPv4 TOS/DiffServ/ECN
byte. This option may appear up to 1 times. This option
takes an integer number as its argument. The value of
<i>number</i> is constrained to being:</p>

<p style="margin-left:28%;">in the range 0 through 255</p>

<p style="margin-left:22%; margin-top: 1em">Allows you to
override the TOS (also known as DiffServ/ECN) value in
IPv4.</p>


<p style="margin-left:11%;"><b>&minus;&minus;tclass</b>=<i>number</i></p>

<p style="margin-left:22%;">Set the IPv6 Traffic Class
byte. This option may appear up to 1 times. This option
takes an integer number as its argument. The value of
<i>number</i> is constrained to being:</p>

<p style="margin-left:28%;">in the range 0 through 255</p>

<p style="margin-left:22%; margin-top: 1em">Allows you to
override the IPv6 Traffic Class field.</p>


<p style="margin-left:11%;"><b>&minus;&minus;flowlabel</b>=<i>number</i></p>

<p style="margin-left:22%;">Set the IPv6 Flow Label. This
option may appear up to 1 times. This option takes an
integer number as its argument. The value of <i>number</i>
is constrained to being:</p>

<p style="margin-left:28%;">in the range 0 through
1048575</p>

<p style="margin-left:22%; margin-top: 1em">Allows you to
override the 20bit IPv6 Flow Label field. Has no effect on
IPv4 packets.</p>

<p style="margin-left:11%;"><b>&minus;F</b> <i>string</i>,
<b>&minus;&minus;fixlen</b>=<i>string</i></p>

<p style="margin-left:22%;">Pad or truncate packet data to
match header length. This option may appear up to 1
times.</p>

<p style="margin-left:22%; margin-top: 1em">Packets may be
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

<p style="margin-left:11%;"><b>&minus;i</b> <i>string</i>,
<b>&minus;&minus;infile</b>=<i>string</i></p>

<p style="margin-left:22%;">Input pcap file to be
processed. This option may appear up to 1 times.</p>

<p style="margin-left:11%;"><b>&minus;o</b> <i>string</i>,
<b>&minus;&minus;outfile</b>=<i>string</i></p>

<p style="margin-left:22%;">Output pcap file. This option
may appear up to 1 times.</p>

<p style="margin-left:11%;"><b>&minus;c</b> <i>string</i>,
<b>&minus;&minus;cachefile</b>=<i>string</i></p>

<p style="margin-left:22%;">Split traffic via tcpprep cache
file. This option may appear up to 1 times.</p>

<p style="margin-left:22%; margin-top: 1em">Use tcpprep
cache file to split traffic based upon client/server
relationships.</p>

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
are used. Be sure to quote the arguments so that they are
not interpreted by tcprewrite. Please see the tcpdump(1) man
page for a complete list of options.</p>


<p style="margin-left:11%;"><b>&minus;&minus;fragroute</b>=<i>string</i></p>

<p style="margin-left:22%;">Parse fragroute configuration
file. This option may appear up to 1 times.</p>

<p style="margin-left:22%; margin-top: 1em">Enable advanced
evasion techniques using the built-in fragroute(8) engine.
See the fragroute(8) man page for more details. Important:
tcprewrite does not support the delay, echo or print
commands.</p>


<p style="margin-left:11%;"><b>&minus;&minus;fragdir</b>=<i>string</i></p>

<p style="margin-left:22%;">Which flows to apply fragroute
to: c2s, s2c, both. This option may appear up to 1 times.
This option must appear in combination with the following
options: cachefile.</p>

<p style="margin-left:22%; margin-top: 1em">Apply the
fragroute engine to packets going c2s, s2c or both when
using a cache file.</p>


<p style="margin-left:11%;"><b>&minus;&minus;skip&minus;soft&minus;errors</b></p>

<p style="margin-left:22%;">Skip writing packets with soft
errors. This option may appear up to 1 times.</p>

<p style="margin-left:22%; margin-top: 1em">In some cases,
packets can not be decoded or the requested editing is not
possible. Normally these packets are written to the output
file unedited so that tcpprep cache files can still be used,
but if you wish, these packets can be suppressed. One
example of this is 802.11 management frames which contain no
data.</p>

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
case, the file &quot;<i>.tcprewriterc</i>&quot; is searched
for within that directory.</p>

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
page was <i>AutoGen</i>-erated from the <b>tcprewrite</b>
option definitions.</p>
<hr>
</body>
</html>
