---
layout: content
title:  "tcpcapinfo man page"
categories: tcpreplay wiki
description: "tcpcapinfo �̃}�j���A���^tcpcapinfo manual"
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

<a href="#NAME">���O�^NAME</a><br>
<a href="#SYNOPSIS">�����^SYNOPSIS</a><br>
<a href="#DESCRIPTION">�T�v�^DESCRIPTION</a><br>
<a href="#OPTIONS">�I�v�V�����^OPTIONS</a><br>
<a href="#EXIT STATUS">exit �R�[�h�^EXIT STATUS</a><br>
<a href="#AUTHORS">AUTHORS�^AUTHORS</a><br>
<a href="#COPYRIGHT">COPYRIGHT�^COPYRIGHT</a><br>
<a href="#BUGS">�o�O�^BUGS</a><br>
<a href="#NOTES">���Ӂ^NOTES</a><br>

<hr>


<h2>���O�^NAME
<a name="NAME"></a>
</h2>


<p style="margin-left:11%; margin-top: 1em">tcpcapinfo
&minus; �j������ pcap �t�@�C���̒������邽�߂� pcap �t�@�C����̓R�}���h</p>

<h2>�����^SYNOPSIS
<a name="SYNOPSIS"></a>
</h2>



<p style="margin-left:11%; margin-top: 1em"><b>tcpcapinfo</b>
[<b>&minus;</b><i>flag</i> [<i>value</i>]]...
[<b>&minus;&minus;</b><i>opt&minus;name</i> [[=|
]<i>value</i>]]...<b>&lt;pcap_file(s)&gt;</b></p>

<p style="margin-left:11%; margin-top: 1em">
tcpcapinfo �́Apcap(3) �`���̃t�@�C�����f�R�[�h����c�[���ł��B
�j������ pcap �t�@�C������������A
����� 2�� pcap �t�@�C�����ǂ��قȂ�̂��A
�������邱�ƂɃt�H�[�J�X���Ă��܂��B</p>

<h2>�T�v�^DESCRIPTION
<a name="DESCRIPTION"></a>
</h2>


<p style="margin-left:11%; margin-top: 1em">
tcpcapinfo �́A
pcap_pkthdr_t ���܂߃p�P�b�g���Ƃ̃T�}���[�̌�ɁA
�l�Ԃ��ǂ݂₷���`���� pcap_file_header_t ���o�͂��܂��B
�܂��A�p�P�b�g�̊ȒP�ȃ`�F�b�N�T�����\�����܂��B</p>


<h2>�I�v�V�����^OPTIONS
<a name="OPTIONS"></a>
</h2>



<p style="margin-left:11%; margin-top: 1em"><b>&minus;d</b>
<i>number</i>, <b>&minus;&minus;dbug</b>=<i>number</i></p>

<p style="margin-left:22%;">
�f�o�b�O�o�͂�\�����܂��B
���̃I�v�V������ 1�񂾂��w��ł��܂��B
�I�v�V�����ɂ͐������w�肵�܂��B
<i>number</i> �͉��L�͈̔͂ɐ�������܂�:</p>

<p style="margin-left:28%;">0 ���� 5 �܂ł̒l</p>

<p style="margin-left:22%;"><i>number</i> �̃f�t�H���g�l��:<br>
0</p>

<p style="margin-left:22%; margin-top: 1em">
&minus;-enable-debug �I�v�V���������� configure ���ꂽ�ꍇ�́A
�f�o�b�O�o�͂̃��x�����w��ł��܂��B
���l���傫���Ȃ�Ƃ�������̏����o�͂��܂��B</p>

<p style="margin-left:11%;"><b>&minus;V</b>,
<b>-&minus;version</b></p>

<p style="margin-left:22%;">�o�[�W��������\�����܂��B</p>

<p style="margin-left:11%;"><b>&minus;H</b>,
<b>&minus;&minus;help</b></p>

<p style="margin-left:22%;">�g�p���@��\�����ďI�����܂��B</p>

<p style="margin-left:11%;"><b>&minus;!</b>,
<b>&minus;&minus;more-help</b></p>

<p style="margin-left:22%;">�y�[�W���[�Ŋg���g�p���@��\�����܂��B</p>

<h2>exit �R�[�h�^EXIT STATUS
<a name="EXIT STATUS"></a>
</h2>


<p style="margin-left:11%; margin-top: 1em">
���L�� 1���Ԃ�܂�:<b><br>
0</b> (EXIT_SUCCESS)</p>

<p style="margin-left:22%;">���s����</p>

<p style="margin-left:11%;"><b>1</b> (EXIT_FAILURE)</p>

<p style="margin-left:22%;">���s���s�܂��͕��@�G���[</p>

<h2>AUTHORS�^AUTHORS
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

<h2>�o�O�^BUGS
<a name="BUGS"></a>
</h2>


<p style="margin-left:11%; margin-top: 1em">
�o�O���� tcpreplay-users@lists.sourceforge.net �ɕ񍐂��Ă��������B</p>

<h2>NOTES
<a name="NOTES"></a>
</h2>


<p style="margin-left:11%; margin-top: 1em">This manual
page was <i>AutoGen</i>-erated from the <b>tcpcapinfo</b>
option definitions.</p>
<hr>
</body>
</html>

