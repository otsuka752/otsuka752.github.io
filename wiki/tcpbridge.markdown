---
layout: content
title:  "tcpbridge"
categories: tcpreplay wiki
description: "tcpbridge - emulate a learing bridge"
---

- [概要／Overview](#overview)
- [注意事項／Notes](#notes)
- [マニュアル／Man pages](tcpbridge-man.html)

<h2><a name="overview"></a>概要／Overview</h2>
*tcpbridge* を使うことで、2つのネットワークをブリッジ接続できます。
*tcpbridge* はブリッジ装置をエミュレートし、
[tcprewrite] と同じ方法で 様々なパケットを編集することができます。
(ブリッジ装置は、どの MACアドレスがどちらがわのネットワークに存在するかを学習します。)

<h2><a name="notes"></a>注意事項／Notes</h2>
1. *tcpbridge* は、2つの Ethernet セグメントをうまく接続します。
2. 無線のセグメントを接続する実績はあまりありませんが、動作するかどうかは OS／ハードウェア／ドライバに依存します。
3. 一般的に、あまりよいパフォーマンスは期待できません。
4. Linux は Windows や OS X などよりはパフォーマンスが良いです。
5. Windows ではあまり tcpbridge の試験ができていないので、バグレポートは歓迎します。
6. tcpbridge でパケットを編集・書き換えできますが、率直に言うと、tcpbridge は stateful でないのできちんと動作しないかもしれません。

[tcprewrite]:          tcprewrite.html
