---
layout: content
title:  "tcpprep man page"
categories: tcpreplay wiki
description: "tcpprep manual"
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
<title>tcpprep</title>

</head>
<body>

<h1 align="center">tcpprep</h1>

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


<h2>NAME
<a name="NAME"></a>
</h2>


<p style="margin-left:11%; margin-top: 1em">tcpprep &minus;
Create a tcpreplay cache cache file from a pcap file.</p>

<h2>SYNOPSIS
<a name="SYNOPSIS"></a>
</h2>


<p style="margin-left:11%; margin-top: 1em"><b>tcpprep</b>
[<b>&minus;</b><i>flag</i> [<i>value</i>]]...
[<b>&minus;&minus;</b><i>opt&minus;name</i> [[=|
]<i>value</i>]]...</p>

<p style="margin-left:11%; margin-top: 1em">All arguments
must be options.</p>

<p style="margin-left:11%; margin-top: 1em">tcpprep is a
@file{pcap(3)} file pre-processor which creates a cache file
which provides &quot;rules&quot; for @file{tcprewrite(1)}
and @file{tcpreplay(1)} on how to process and send
packets.</p>

<h2>DESCRIPTION
<a name="DESCRIPTION"></a>
</h2>


<p style="margin-left:11%; margin-top: 1em">The basic
operation of tcpreplay is to resend all packets from the
input file(s) out a single file. Tcpprep processes a pcap
file and applies a set of user-specified rules to create a
cache file which tells tcpreplay wether or not to send each
packet and which interface the packet should be sent out of.
For more details, please see the Tcpreplay Manual at:
http://tcpreplay.appneta.com</p>

<h2>OPTIONS
<a name="OPTIONS"></a>
</h2>



<p style="margin-left:11%; margin-top: 1em"><b>&minus;d</b>
<i>number</i>, <b>&minus;&minus;dbug</b>=<i>number</i></p>

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

<p style="margin-left:11%;"><b>&minus;a</b> <i>string</i>,
<b>&minus;&minus;auto</b>=<i>string</i></p>

<p style="margin-left:22%;">Auto-split mode. This option
may appear up to 1 times. This option must not appear in
combination with any of the following options: cidr, port,
regex, mac.</p>

<p style="margin-left:22%; margin-top: 1em">Tcpprep will
try to automatically determine the primary function of hosts
based on the traffic captured and classify each host as
client or server. In order to do so, you must provide a hint
to tcpprep as to how to search for clients and servers.
Valid hints are:</p>

<p style="margin-left:22%; margin-top: 1em"><b>bridge</b>
Bridge mode processes each packet to try to determine if the
sender is a client or server. Once all the packets are
processed, the results are weighed according to the
server/client ratio (<b>--ratio</b>) and systems are
assigned an interface. If tcpprep is unable to determine
what role a system plays, tcpprep will abort.</p>

<p style="margin-left:22%; margin-top: 1em"><b>router</b>
Router mode works just like bridge mode, except that after
weighing is done, systems which are undetermined are
considered a server if they fall inside a network known to
contain other servers. Router has a greater chance of
successfully splitting clients and servers but is not 100%
foolproof.</p>

<p style="margin-left:22%; margin-top: 1em"><b>client</b>
Client mode works just like bridge mode, except that
unclassified systems are treated as clients. Client mode
should always complete successfully.</p>

<p style="margin-left:22%; margin-top: 1em"><b>server</b>
Server mode works just like bridge mode, except that
unclassified systems are treated as servers. Server mode
should always complete successfully.</p>

<p style="margin-left:22%; margin-top: 1em"><b>first</b>
First mode works by looking at the first time each IP is
seen in the SRC and DST fields in the IP header. If the host
is first seen in the SRC field, it is a client and if
it&rsquo;s first seen in the DST field, it is marked as a
server. This effectively replicates the processing of the
tomahawk test tool. First mode should always complete
successfully.</p>

<p style="margin-left:11%;"><b>&minus;c</b> <i>string</i>,
<b>&minus;&minus;cidr</b>=<i>string</i></p>

<p style="margin-left:22%;">CIDR-split mode. This option
may appear up to 1 times. This option must not appear in
combination with any of the following options: auto, port,
regex, mac.</p>

<p style="margin-left:22%; margin-top: 1em">Specify a comma
delimited list of CIDR netblocks to match against the source
IP of each packet. Packets matching any of the CIDR&rsquo;s
are classified as servers. IPv4 Example: <br>
&minus;-cidr=192.168.0.0/16,172.16.0.0/12,10.0.0.0/8 <br>
IPv6 Example: <br>
&minus;-cidr=[::ffff:0:0/96],[fe80::/16]</p>

<p style="margin-left:11%;"><b>&minus;r</b> <i>string</i>,
<b>&minus;&minus;regex</b>=<i>string</i></p>

<p style="margin-left:22%;">Regex-split mode. This option
may appear up to 1 times. This option must not appear in
combination with any of the following options: auto, port,
cidr, mac.</p>

<p style="margin-left:22%; margin-top: 1em">Specify a
regular expression to match against the source IP of each
packet. Packets matching the regex are classified as
servers.</p>

<p style="margin-left:11%;"><b>&minus;p</b>,
<b>-&minus;port</b></p>

<p style="margin-left:22%;">Port-split mode. This option
may appear up to 1 times. This option must not appear in
combination with any of the following options: auto, regex,
cidr, mac.</p>

<p style="margin-left:22%; margin-top: 1em">Specifies that
TCP and UDP traffic over IPv4 and IPv6 should be classified
as client or server based upon the destination port of the
header.</p>

<p style="margin-left:11%;"><b>&minus;e</b> <i>string</i>,
<b>&minus;&minus;mac</b>=<i>string</i></p>

<p style="margin-left:22%;">Source MAC split mode. This
option may appear up to 1 times. This option must not appear
in combination with any of the following options: auto,
regex, cidr, port.</p>

<p style="margin-left:22%; margin-top: 1em">Specify a list
of MAC addresses to match against the source MAC of each
packet. Packets matching one of the values are classified as
servers.</p>


<p style="margin-left:11%;"><b>&minus;&minus;reverse</b></p>

<p style="margin-left:22%;">Matches to be client instead of
server. This option may appear up to 1 times.</p>

<p style="margin-left:22%; margin-top: 1em">Normally the
<b>--mac</b>, <b>--regex</b> and <b>--cidr</b> flags specify
are used to specify the servers and non-IP packets are
classified as clients. By using <b>--reverse</b>, these
features are reversed so that the flags specify clients and
non-IP packets are classified as servers.</p>

<p style="margin-left:11%;"><b>&minus;C</b> <i>string</i>,
<b>&minus;&minus;comment</b>=<i>string</i></p>

<p style="margin-left:22%;">Embeded cache file comment.
This option may appear up to 1 times.</p>

<p style="margin-left:22%; margin-top: 1em">Specify a
comment to be imbedded within the output cache file and
later viewed.</p>


<p style="margin-left:11%;"><b>&minus;&minus;no&minus;arg&minus;comment</b></p>

<p style="margin-left:22%;">Do not embed any cache file
comment. This option may appear up to 1 times.</p>

<p style="margin-left:22%; margin-top: 1em">By default,
tcpprep includes the arguments passed on the command line in
the cache file comment (in addition to any user specified
&minus;-comment). If for some reason you do not wish to
include this, specify this option.</p>

<p style="margin-left:11%;"><b>&minus;x</b> <i>string</i>,
<b>&minus;&minus;include</b>=<i>string</i></p>

<p style="margin-left:22%;">Include only packets matching
rule. This option may appear up to 1 times. This option must
not appear in combination with any of the following options:
exclude.</p>

<p style="margin-left:22%; margin-top: 1em">Override
default of processing all packets stored in the capture file
and only send/edit packets which match the provided rule.
Rules can be one of:</p>


<p style="margin-left:22%; margin-top: 1em"><i>S:&lt;CIDR1&gt;,...</i>
- Source IP must match specified IPv4/v6 CIDR(s)</p>


<p style="margin-left:22%; margin-top: 1em"><i>D:&lt;CIDR1&gt;,...</i>
- Destination IP must match specified IPv4/v6 CIDR(s)</p>


<p style="margin-left:22%; margin-top: 1em"><i>B:&lt;CIDR1&gt;,...</i>
- Both source and destination IP must match specified
IPv4/v6 CIDR(s)</p>


<p style="margin-left:22%; margin-top: 1em"><i>E:&lt;CIDR1&gt;,...</i>
- Either IP must match specified IPv4/v6 CIDR(s)</p>


<p style="margin-left:22%; margin-top: 1em"><i>P:&lt;LIST&gt;</i>
- Must be one of the listed packets where the list
corresponds to the packet number in the capture file. <br>
&minus;x P:1-5,9,15,72- <br>
would process packets 1 thru 5, the 9th and 15th packet, and
packets 72 until the end of the file</p>


<p style="margin-left:22%; margin-top: 1em"><i>F:&rsquo;&lt;bpf&gt;&rsquo;</i>
- BPF filter. See the <i>tcpdump(8)</i> man page for
syntax.</p>

<p style="margin-left:11%;"><b>&minus;X</b> <i>string</i>,
<b>&minus;&minus;exclude</b>=<i>string</i></p>

<p style="margin-left:22%;">Exclude any packet matching
this rule. This option may appear up to 1 times. This option
must not appear in combination with any of the following
options: include.</p>

<p style="margin-left:22%; margin-top: 1em">Override
default of processing all packets stored in the capture file
and only send/edit packets which do NOT match the provided
rule. Rules can be one of:</p>


<p style="margin-left:22%; margin-top: 1em"><i>S:&lt;CIDR1&gt;,...</i>
- Source IP must not match specified IPv4/v6 CIDR(s)</p>


<p style="margin-left:22%; margin-top: 1em"><i>D:&lt;CIDR1&gt;,...</i>
- Destination IP must not match specified IPv4/v6
CIDR(s)</p>


<p style="margin-left:22%; margin-top: 1em"><i>B:&lt;CIDR1&gt;,...</i>
- Both source and destination IP must not match specified
IPv4/v6 CIDR(s)</p>


<p style="margin-left:22%; margin-top: 1em"><i>E:&lt;CIDR1&gt;,...</i>
- Either IP must not match specified IPv4/v6 CIDR(s)</p>


<p style="margin-left:22%; margin-top: 1em"><i>P:&lt;LIST&gt;</i>
- Must not be one of the listed packets where the list
corresponds to the packet number in the capture file. <br>
&minus;x P:1-5,9,15,72- <br>
would skip packets 1 thru 5, the 9th and 15th packet, and
packets 72 until the end of the file</p>

<p style="margin-left:11%;"><b>&minus;o</b> <i>string</i>,
<b>&minus;&minus;cachefile</b>=<i>string</i></p>

<p style="margin-left:22%;">Output cache file. This option
may appear up to 1 times.</p>

<p style="margin-left:11%;"><b>&minus;i</b> <i>string</i>,
<b>&minus;&minus;pcap</b>=<i>string</i></p>

<p style="margin-left:22%;">Input pcap file to process.
This option may appear up to 1 times.</p>

<p style="margin-left:11%;"><b>&minus;P</b> <i>string</i>,
<b>&minus;&minus;print&minus;comment</b>=<i>string</i></p>

<p style="margin-left:22%;">Print embedded comment in the
specified cache file. This option may appear up to 1
times.</p>

<p style="margin-left:11%;"><b>&minus;I</b> <i>string</i>,
<b>&minus;&minus;print&minus;info</b>=<i>string</i></p>

<p style="margin-left:22%;">Print basic info from the
specified cache file. This option may appear up to 1
times.</p>

<p style="margin-left:11%;"><b>&minus;S</b> <i>string</i>,
<b>&minus;&minus;print&minus;stats</b>=<i>string</i></p>

<p style="margin-left:22%;">Print statistical information
about the specified cache file. This option may appear up to
1 times.</p>

<p style="margin-left:11%;"><b>&minus;s</b> <i>string</i>,
<b>&minus;&minus;services</b>=<i>string</i></p>

<p style="margin-left:22%;">Load services file for server
ports. This option may appear up to 1 times. This option
must appear in combination with the following options:
port.</p>

<p style="margin-left:22%; margin-top: 1em">Uses a list of
ports used by servers in the same format as of
/etc/services: &lt;service_name&gt;
&lt;port&gt;/&lt;protocol&gt; # comment Example: http
80/tcp</p>

<p style="margin-left:11%;"><b>&minus;N</b>,
<b>-&minus;nonip</b></p>

<p style="margin-left:22%;">Send non-IP traffic out server
interface. This option may appear up to 1 times.</p>

<p style="margin-left:22%; margin-top: 1em">By default,
non-IP traffic which can not be classified as client or
server is classified as &quot;client&quot;. Specifiying
<b>--nonip</b> will reclassify non-IP traffic as
&quot;server&quot;. Note that the meaning of this flag is
reversed if <b>--reverse</b> is used.</p>

<p style="margin-left:11%;"><b>&minus;R</b> <i>string</i>,
<b>&minus;&minus;ratio</b>=<i>string</i></p>

<p style="margin-left:22%;">Ratio of client to server
packets. This option may appear up to 1 times. This option
must appear in combination with the following options: auto.
The default <i>string</i> for this option is: <br>
2.0</p>

<p style="margin-left:22%; margin-top: 1em">Since a given
host may have both client and server traffic being sent
to/from it, tcpprep uses a ratio to weigh these packets. If
you would like to override the default of 2:1 server to
client packets required for a host to be classified as a
server, specify it as a floating point value.</p>

<p style="margin-left:11%;"><b>&minus;m</b> <i>number</i>,
<b>&minus;&minus;minmask</b>=<i>number</i></p>

<p style="margin-left:22%;">Minimum network mask length in
auto mode. This option may appear up to 1 times. This option
must appear in combination with the following options: auto.
This option takes an integer number as its argument. The
value of <i>number</i> is constrained to being:</p>

<p style="margin-left:28%;">in the range 0 through 32</p>

<p style="margin-left:22%;">The default <i>number</i> for
this option is: <br>
30</p>

<p style="margin-left:22%; margin-top: 1em">By default,
auto modes use a minimum network mask length of 30 bits to
build networks containing clients and servers. This allows
you to override this value. Larger values will increase
performance but may provide inaccurate results.</p>

<p style="margin-left:11%;"><b>&minus;M</b> <i>number</i>,
<b>&minus;&minus;maxmask</b>=<i>number</i></p>

<p style="margin-left:22%;">Maximum network mask length in
auto mode. This option may appear up to 1 times. This option
must appear in combination with the following options: auto.
This option takes an integer number as its argument. The
value of <i>number</i> is constrained to being:</p>

<p style="margin-left:28%;">in the range 0 through 32</p>

<p style="margin-left:22%;">The default <i>number</i> for
this option is: <br>
8</p>

<p style="margin-left:22%; margin-top: 1em">By default,
auto modes use a maximum network mask length of 8 bits to
build networks containing clients and servers. This allows
you to override this value. Larger values will decrease
performance and accuracy but will provide greater chance of
success.</p>

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
not interpreted by tcprewrite. The following arguments are
valid: <br>
[ &minus;aAeNqRStuvxX ] <br>
[ &minus;E spi@ipaddr algo:secret,... ] <br>
[ &minus;s snaplen ]</p>

<p style="margin-left:11%;"><b>&minus;V</b>,
<b>-&minus;version</b></p>

<p style="margin-left:22%;">Print version information.</p>

<p style="margin-left:11%;"><b>&minus;h</b>,
<b>-&minus;less&minus;help</b></p>

<p style="margin-left:22%;">Display less usage information
and exit.</p>

<p style="margin-left:22%; margin-top: 1em">This option has
not been fully documented.</p>

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
case, the file &quot;<i>.tcppreprc</i>&quot; is searched for
within that directory.</p>

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
page was <i>AutoGen</i>-erated from the <b>tcpprep</b>
option definitions.</p>
<hr>
</body>
</html>
