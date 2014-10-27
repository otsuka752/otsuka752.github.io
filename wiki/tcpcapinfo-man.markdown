---
layout: content
title:  "tcpcapinfo man page"
categories: tcpreplay wiki
description: "tcpcapinfo のマニュアル／tcpcapinfo manual"
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

<a href="#NAME">名前／NAME</a><br>
<a href="#SYNOPSIS">書式／SYNOPSIS</a><br>
<a href="#DESCRIPTION">概要／DESCRIPTION</a><br>
<a href="#OPTIONS">オプション／OPTIONS</a><br>
<a href="#EXIT STATUS">exit コード／EXIT STATUS</a><br>
<a href="#AUTHORS">AUTHORS／AUTHORS</a><br>
<a href="#COPYRIGHT">COPYRIGHT／COPYRIGHT</a><br>
<a href="#BUGS">バグ／BUGS</a><br>
<a href="#NOTES">注意／NOTES</a><br>

<hr>


<h2>名前／NAME
<a name="NAME"></a>
</h2>


<p style="margin-left:11%; margin-top: 1em">tcpcapinfo
&minus; 破損した pcap ファイルの調査するための pcap ファイル解析コマンド</p>

<h2>書式／SYNOPSIS
<a name="SYNOPSIS"></a>
</h2>



<p style="margin-left:11%; margin-top: 1em"><b>tcpcapinfo</b>
[<b>&minus;</b><i>flag</i> [<i>value</i>]]...
[<b>&minus;&minus;</b><i>opt&minus;name</i> [[=|
]<i>value</i>]]...<b>&lt;pcap_file(s)&gt;</b></p>

<p style="margin-left:11%; margin-top: 1em">
tcpcapinfo は、pcap(3) 形式のファイルをデコードするツールです。
破損した pcap ファイルを見つけたり、
特定の 2つの pcap ファイルがどう異なるのか、
を見つけることにフォーカスしています。</p>

<h2>概要／DESCRIPTION
<a name="DESCRIPTION"></a>
</h2>


<p style="margin-left:11%; margin-top: 1em">
tcpcapinfo は、
pcap_pkthdr_t も含めパケットごとのサマリーの後に、
人間が読みやすい形式で pcap_file_header_t を出力します。
また、パケットの簡単なチェックサムも表示します。</p>


<h2>オプション／OPTIONS
<a name="OPTIONS"></a>
</h2>



<p style="margin-left:11%; margin-top: 1em"><b>&minus;d</b>
<i>number</i>, <b>&minus;&minus;dbug</b>=<i>number</i></p>

<p style="margin-left:22%;">
デバッグ出力を表示します。
このオプションは 1回だけ指定できます。
オプションには整数を指定します。
<i>number</i> は下記の範囲に制限されます:</p>

<p style="margin-left:28%;">0 から 5 までの値</p>

<p style="margin-left:22%;"><i>number</i> のデフォルト値は:<br>
0</p>

<p style="margin-left:22%; margin-top: 1em">
&minus;-enable-debug オプションをつけて configure された場合は、
デバッグ出力のレベルを指定できます。
数値が大きくなるとたくさんの情報を出力します。</p>

<p style="margin-left:11%;"><b>&minus;V</b>,
<b>-&minus;version</b></p>

<p style="margin-left:22%;">バージョン情報を表示します。</p>

<p style="margin-left:11%;"><b>&minus;H</b>,
<b>&minus;&minus;help</b></p>

<p style="margin-left:22%;">使用方法を表示して終了します。</p>

<p style="margin-left:11%;"><b>&minus;!</b>,
<b>&minus;&minus;more-help</b></p>

<p style="margin-left:22%;">ページャーで拡張使用方法を表示します。</p>

<h2>exit コード／EXIT STATUS
<a name="EXIT STATUS"></a>
</h2>


<p style="margin-left:11%; margin-top: 1em">
下記の 1つが返ります:<b><br>
0</b> (EXIT_SUCCESS)</p>

<p style="margin-left:22%;">実行成功</p>

<p style="margin-left:11%;"><b>1</b> (EXIT_FAILURE)</p>

<p style="margin-left:22%;">実行失敗または文法エラー</p>

<h2>AUTHORS／AUTHORS
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

<h2>バグ／BUGS
<a name="BUGS"></a>
</h2>


<p style="margin-left:11%; margin-top: 1em">
バグ情報は tcpreplay-users@lists.sourceforge.net に報告してください。</p>

<h2>NOTES
<a name="NOTES"></a>
</h2>


<p style="margin-left:11%; margin-top: 1em">This manual
page was <i>AutoGen</i>-erated from the <b>tcpcapinfo</b>
option definitions.</p>
<hr>
</body>
</html>

