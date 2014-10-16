---
layout: content
title:  "tcpcapinfo man page"
categories: tcpreplay wiki
description: "tcpcapinfo manual"
---

<!-- Creator     : groff version 1.20.1 -->
<!-- CreationDate: Tue Jan  7 15:29:42 2014 -->
<!-- <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" -->
<!-- "http://www.w3.org/TR/html4/loose.dtd"> -->
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
<title>tcpcapinfo</title>

</head>
<body>

<h1 align="center">tcpcapinfo</h1>

<a href="#NAME">NAME</a><br>
<a href="#SYNOPSIS">SYNOPSIS</a><br>
<a href="#DESCRIPTION">DESCRIPTION</a><br>
<a href="#OPTIONS">OPTIONS</a><br>
<a href="#EXIT STATUS">EXIT STATUS</a><br>
<a href="#AUTHORS">AUTHORS</a><br>
<a href="#COPYRIGHT">COPYRIGHT</a><br>
<a href="#BUGS">BUGS</a><br>
<a href="#NOTES">NOTES</a><br>

<hr>


<h2>NAME
<a name="NAME"></a>
</h2>


<p style="margin-left:11%; margin-top: 1em">tcpcapinfo
&minus; Pcap file dissector for debugging broken pcap
files</p>

<h2>SYNOPSIS
<a name="SYNOPSIS"></a>
</h2>



<p style="margin-left:11%; margin-top: 1em"><b>tcpcapinfo</b>
[<b>&minus;</b><i>flag</i> [<i>value</i>]]...
[<b>&minus;&minus;</b><i>opt&minus;name</i> [[=|
]<i>value</i>]]...<b>&lt;pcap_file(s)&gt;</b></p>

<p style="margin-left:11%; margin-top: 1em">tcpcapinfo is a
tool for decoding the structure of a pcap(3) file with a
focus on finding broken pcap files and determining how two
related pcap files might differ.</p>

<h2>DESCRIPTION
<a name="DESCRIPTION"></a>
</h2>


<p style="margin-left:11%; margin-top: 1em">tcpcapinfo will
first print out the pcap_file_header_t in human readable
form followed by a per-packet summary including the
pcap_pkthdr_t and simple checksum value of the packet.</p>

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

<p style="margin-left:11%;"><b>&minus;H</b>,
<b>&minus;&minus;help</b></p>

<p style="margin-left:22%;">Display usage information and
exit.</p>

<p style="margin-left:11%;"><b>&minus;!</b>,
<b>&minus;&minus;more-help</b></p>

<p style="margin-left:22%;">Pass the extended usage
information through a pager.</p>

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

<h2>AUTHORS
<a name="AUTHORS"></a>
</h2>


<p style="margin-left:11%; margin-top: 1em">Copyright
2000-2012 Aaron Turner Copyright 2013 Fred Klassen - AppNeta
Inc. For support please use the
tcpreplay-users@lists.sourceforge.net mailing list. The
latest version of this software is always available from:
http://tcpreplay.appneta.com/</p>

<h2>COPYRIGHT
<a name="COPYRIGHT"></a>
</h2>


<p style="margin-left:11%; margin-top: 1em">Copyright (C)
2000-2012 Aaron Turner and Fred Klassen all rights reserved.
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
page was <i>AutoGen</i>-erated from the <b>tcpcapinfo</b>
option definitions.</p>
<hr>
</body>
</html>

