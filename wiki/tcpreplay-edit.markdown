---
layout: content
title:  "tcpreplay-edit"
categories: tcpreplay wiki
description: "tcpreplay-edit - replay a PCAP packet capture file while actively modifying packets"
---

- [概要／Overview][Overview](#overview)
- [man ページ／Man pages][Man pages](tcpreplay-edit-man.html)

<h2><a name="overview"></a>概要／Overview</h2>
[tcpreplay] は長い年月をかけて開発されてきました。
1.x の頃は、単にパケットを読み込みネットワークに再送信するだけでした。
2.x では、*tcpreplay* に非常に多くの書き換え機能が実装されましたが、
複雑さやパフォーマンスやコードの肥大化を犠牲にしていました。
3.x では、*tcpreplay* はパケットを送信するマシンになるという根本に立ち戻り、
パケットの書き換え機能は *tcprewrite* と、
送信と書き換えの両方の機能を持った強力な *tcpreplay-edit* の 2つに移行されました。
4.0 では、いくつかの書き換えの機能は *tcpreplay* に戻されましたが、
(`--unique-ip` のように) パフォーマンスに影響を与えないものだけに限られています。

*tcpreplay-edit* は、*tcpreplay* と *tcprewrite* の両方の機能を持っているので、
*tcpreplay-edit* の使い方は、これらのページを参照してください。

最後になりますが、パケットの書き換え機能のための (tcpreplay-edit の) コードは、
たとえパケットを書き換えなかったとしてもオーバーヘッドになることを忘れないでください。
従って、パフォーマンスが必要な場合は *tcprewrite* と *tcpreplay* を分けて使う
(事前に tcprewrite した pcap ファイルを tcpreplay で送信する) ことを勧めます。

[tcprewrite]:          tcprewrite.html
[tcpreplay]:           tcpreplay.html
[tcpprep]:             tcpprep.html
