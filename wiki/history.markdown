---
layout: content
title:  "Tcpreplay の歴史／History of Tcpreplay"
categories: tcpreplay wiki
description: "History of Tcpreplay"
---


Tcpreplay は最近のオープンソース／フリーソフトウェアとはちょっと異なる歴史があります。
*tcpreplay* には複数の著者がいて、互いに電子メールでしかやりとりしていない人達もいます。

Tcpreplay は最初にリリースされたのが 1999年であるため、とてもたくさんの著者がいます。
BSD ライセンスや GPL ライセンスのアドバンテージの 1つには、
誰かが開発を続けられなくなったり続けたくなくなったとしても、
他の誰かが変わりに開発を続けることができます。

一番最初は、Matt Undy の Anzen Computing が tcpreplay を作りました。
Matt は 1999年に [nidsbench toolkit][nidsbench] の一部として
バージョン 1.0.1 をリリースしました。
その後 Anzen Computing 社は(少なくとも一部の部門は) NFR に買収され、
開発は終了してしまいました。

2001年にある 2人の人物によって、それぞれ個別に *tcpreplay* の開発が続けられました。
[NFR][NFR] の Matt Bing と、OneSecure の [Aaron Turner][aaron] です。
たくさんのパッチ(-adt ブランチ)を作った後、
Aaron は開発のメインラインにパッチを送るようになりました。

Aaron と Matt Bing の話し合いで、彼らは一緒に開発を進めることになりました。
それ以来、2回の大きな変更が発生し、30種類以上の新機能が追加され、
たくさんのツール群が含まれるようになりました。

2013年に [AppNeta][AppNeta] が資金援助し [Fred Klassen][fklassen] がメンテナになるまで、
Aaron は開発を続けていました。 Version 4.0.0 は、Fred の最初のメジャーリリースです。

[nidsbench]:      http://dl.packetstormsecurity.net/UNIX/IDS/nidsbench/nidsbench.html
[NFR]:            http://www.nfr.com
[aaron]:          http://synfin.net
[fklassen]:       https://github.com/fklassen
[AppNeta]:        http://www.appneta.com
