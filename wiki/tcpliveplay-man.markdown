---
layout: content
title:  "tcpliveplay man page"
categories: tcpreplay wiki
description: "tcpliveplay manual"
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
<title>tcpliveplay</title>

</head>
<body>

<h1 align="center">tcpliveplay</h1>

<a href="#NAME">NAME</a><br>
<a href="#SYNOPSIS">SYNOPSIS</a><br>
<a href="#DESCRIPTION">DESCRIPTION</a><br>
<a href="#OPTIONS">OPTIONS</a><br>
<a href="#OPTION PRESETS">OPTION PRESETS</a><br>
<a href="#FILES">FILES</a><br>
<a href="#EXIT STATUS">EXIT STATUS</a><br>
<a href="#AUTHORS">AUTHORS</a><br>
<a href="#COPYRIGHT">COPYRIGHT</a><br>
<a href="#NOTES">NOTES</a><br>

<hr>


<h2>NAME
<a name="NAME"></a>
</h2>


<p style="margin-left:11%; margin-top: 1em">tcpliveplay
&minus; Replays network traffic stored in a pcap file on
live networks using new TCP connections</p>

<h2>SYNOPSIS
<a name="SYNOPSIS"></a>
</h2>



<p style="margin-left:11%; margin-top: 1em"><b>tcpliveplay</b>
[<b>&minus;</b><i>flag</i> [<i>value</i>]]...
[<b>&minus;&minus;</b><i>opt&minus;name</i> [[=|
]<i>value</i>]]...<b>&lt;eth0/eth1&gt;</b>&lt;file.pcap&gt;<b>&lt;Destinatin</b>IP<b>[1.2.3.4]&gt;</b>&lt;Destination<b>mac</b>[0a:1b:2c:3d:4e:5f]&gt;<b>&lt;&rsquo;random&rsquo;</b>dst<b>port</b>OR<b>specify</b>dport<b>#&gt;</b></p>

<p style="margin-left:11%; margin-top: 1em">This program,
&rsquo;tcpliveplay&rsquo; replays a captured set of packets
using new TCP connections with the captured TCP payloads
against a remote host in order to do comprehensive
vulnerability testings.</p>

<h2>DESCRIPTION
<a name="DESCRIPTION"></a>
</h2>


<p style="margin-left:11%; margin-top: 1em">The basic
operation of tcpliveplay is it rewrites the given pcap file
in a scheduled event format and responds with the apporiate
packet if the remote host meets tcp protocal&rsquo;s SEQ/ACK
expectation. Once expectations are met, then the local
packets are sent with the same payload except with new tcp
SEQ &amp; ACK numbers meeting the response from the remote
hose. The inputted pcap file are rewritten to start at the
first encounter of the SYN packet for correct operation
making this packet be the first action in the event schedule
of local host doing the replay. For more details, please see
the Tcpreplay Manual at: http://tcpreplay.appneta.com</p>

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
case, the file &quot;<i>.tcpliveplayrc</i>&quot; is searched
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


<p style="margin-left:11%; margin-top: 1em">Copyright 2012
Yazan Siam For support please use the
tcpreplay-users@lists.sourceforge.net mailing list. The
latest version of this software is always available from:
http://tcpreplay.appneta.com</p>

<h2>COPYRIGHT
<a name="COPYRIGHT"></a>
</h2>


<p style="margin-left:11%; margin-top: 1em">Copyright (C)
2012 Yazan Siam all rights reserved. This program is
released under the terms of the Modified Berkeley Software
Distribution License.</p>

<h2>NOTES
<a name="NOTES"></a>
</h2>


<p style="margin-left:11%; margin-top: 1em">This manual
page was <i>AutoGen</i>-erated from the <b>tcpliveplay</b>
option definitions.</p>
<hr>
</body>
</html>
