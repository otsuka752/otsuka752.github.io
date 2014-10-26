---
layout: content
title:  "Tcpreplay 概要／Tcpreplay Overview"
categories: tcpreplay wiki
description: "Tcpreplay is a suite of utilities for editing and replaying network traffic which was previously captured by tools like tcpdump and Wireshark"
---

- [概要／Overview](#overview)
- [実行例／Examples](#examples)
- [スクリーンショットのビデオ／Screencast Videos](#screencast-videos)
- [役に立つリンク／Useful Links](#useful-links)

<h2 id="Overview"><a name="overview"></a>概要／Overview</h2>
Tcpreplay は [GPLv3] ライセンスの UNIX 用(Win32 は [Cygwin] 用)のユーティリティ群で、
事前に [tcpdump] や [Wireshark] などでキャプチャされたネットワークトラフィックを
編集したり再送信したりできます。
また、クライアントやサーバで分類したり、Layer2/3/4 のパケットを書き換えたり、
最終的にはネットワーク上にあるいはスイッチやルータやファイヤーウォールや NIDS/IPS などの
別なデバイスにトラフィックを再送信することができます。 
Tcprelay は sniffing(パケットキャプチャ)もインラインデバイスもテストできるよう、
1枚の NIC でも 2枚以上の NIC でも動作します。

Tcprelay はたくさんの FireWall、IDS、IPS、NetFlow、その他ネットワークベンダーで、
また、エンタープライズ、大学、研究所、オープンソースプロジェクトで利用されています。
もしあなたの組織で Tcpreplay を使っていたら、有用な機能を追加し続けることができるよう、
あなたがどんな人でどのように Tcpreplay を使っているかを教えてください。

Tcpreplay は NIC と連動して動作するように設計されており、Layer2 より下の層は処理しません。
TCP の pcapファイルを直接サーバに対して再送信できるよう、
[Cisco] の協力の元で Yazan Siam が *tcpliveplay* を開発しています
ネットワークスタック全体とアプリケーションまでテストしたい場合には、
このユーティリティを使ってください。

version 4.0 では、[IP Flow][flow]/[NetFlow] の装置のテストや
チューニングの複雑さに対応するために機能拡張されました。機能拡張は下記を含んでます:

* 10GbE のワイヤースピード性能のために修正された [netmap] ネットワークドライバのサポート
* 再送信速度の精度向上
* 実行結果のレポーティング精度向上
* Flows Per Second (fps)を含んだ Flow の統計情報
* 分析やフローの有効期限切れタイムアウトのチューニングのためのフロー分析
* 毎秒数十万フロー(pcapファイルのフローサイズに依存)

Version 4.0 は Fred Klassen により、そして [AppNeta] の支援の元でリリースされた
最初のバージョンです。Tcpreplay を作った Aaron Turner に感謝します。
新しいメンテナは、Tcpreplay が、通常は商用のネットワークテスト装置でしか
得られないようなパフォーマンスレベルになるよう努力します。

<h2 id="Examples"><a name="examples"></a>実行例／Examples</h2>
下記は、IP Flow アプライアンスに様々なパターンのトラフィックを生成するための
*tcpreplay* の例です:

<div class="highlight"><pre><code class="ruby language-ruby" data-lang="ruby"><span class="c1">root@pw29:~#</span> <span class="no">tcpreplay -i eth7 -t -K --loop 5000 smallFlows.pcap</span> 
<span class="n">File Cache is enabled
Actual: 71305000 packets (46082655000 bytes) sent in 38.03 seconds.
Rated: 1201832266.1 Bps, <span class="ss">9614.65 Mbps</span>, 1859629.17 pps
Flows: 1209 flows, <span class="ss">31.53 fps</span>, 71215000 flow packets, 90000 non-flow
Statistics for network device: eth7
	Attempted packets:         71305000
	Successful packets:        71305000
	Failed packets:            0
	Truncated packets:         0
	Retried packets (ENOBUFS): 0
	Retried packets (EAGAIN):  0
</span></code></pre></div>

<br />
<div class="highlight"><pre><code class="ruby language-ruby" data-lang="ruby"><span class="c1">root@pw29:~#</span> <span class="no">tcpreplay -i eth7 --mbps=9500 -K --loop 5000 smallFlows.pcap</span> 
<span class="n">File Cache is enabled
Actual: 71305000 packets (46082655000 bytes) sent in 38.08 seconds.
Rated: 1187499244.6 Bps, <span class="ss">9499.99 Mbps</span>, 1837451.28 pps
Flows: 1209 flows, <span class="ss">31.15 fps</span>, 71215000 flow packets, 90000 non-flow
Statistics for network device: eth7
	Attempted packets:         71305000
	Successful packets:        71305000
	Failed packets:            0
	Truncated packets:         0
	Retried packets (ENOBUFS): 0
	Retried packets (EAGAIN):  0
</span></code></pre></div>

<br />
<div class="highlight"><pre><code class="ruby language-ruby" data-lang="ruby"><span class="c1">root@pw29:~#</span> <span class="no">tcpreplay -i eth7 -tK --loop 50000 --netmap --unique-ip smallFlows.pcap</span> 
<span class="n">Switching network driver for eth7 to netmap bypass mode... done!
File Cache is enabled
Actual: 713050000 packets (460826550000 bytes) sent in 385.07 seconds.
Rated: 1194660947.8 Bps, <span class="ss">9557.28 Mbps</span>, 1848532.79 pps
Flows: 60450000 flows, <span class="ss">156712.44 fps</span>, 712150000 flow packets, 900000 non-flow
Statistics for network device: eth7
    Attempted packets:         713050000
    Successful packets:        713050000
    Failed packets:            0
    Truncated packets:         0
    Retried packets (ENOBUFS): 0
    Retried packets (EAGAIN):  0
Switching network driver for eth7 to normal mode... done!</span></code></pre></div>

## <a name="screencast-videos"></a>スクリーンショットのビデオ／Screencast Videos
Tcpreplay のスクリーンショットの動画の一覧は [ここ][playlist] にあります。
また、下記で全ての動画を見ることもできます。

<iframe width="620" height="460" 
     src="//www.youtube.com/embed/videoseries?list=PLFjjcN5EvTP0Hsxq1AEh7FhvaFH__Vd83" 
     frameborder="0" 
     allowfullscreen>
</iframe>

## <a name="useful-links"></a>役に立つリンク／Useful Links

* [ダウンロードとインストールの説明／Download and installation instructions][installation]
* [サポート/Support][Support]
* [Tcpreplay ユーザのメーリングリスト／Tcpreplay Users Mailing List][maillist]
* Tcpreplay の [Twitter]／Tcpreplay on [Twitter]
* [ライブチャット][Live chat]
* [よくある質問と回答／FAQ][FAQ]
* [...するには／How To][How To]
* [サンプルキャプチャ／Sample Captures][Sample Captures]
* [Tcpreplay の歴史／The history of Tcpreplay][history]
* [スクリーンショットの動画／Screencast Videos][playlist]
* Legacy
    * [Version 3.x の wiki／Version 3.x wiki][legacywiki]
    * [Tcpreplay ブログ／Tcpreplay Blog][legacyblog]
    * [RSS Feed／RSS Feed][rss]
    * [ポッドキャスト／Podcasts][Podcasts]
    


[GPLv3]:    http://www.gnu.org/licenses/gpl-3.0.html
[netmap]:   http://info.iet.unipi.it/~luigi/netmap/
[flow]:     https://ietf.org/wg/ipfix/
[NetFlow]:  http://www.cisco.com/go/netflow
[Cygwin]:   http://www.cygwin.com/
[Wireshark]: http://www.wireshark.org
[tcpdump]:  http://www.tcpdump.org
[Cisco]:    http://www.cisco.com
[AppNeta]:  http://www.appneta.com
[maillist]: https://lists.sourceforge.net/lists/listinfo/tcpreplay-users
[legacyblog]:  http://synfin.net/sock_stream/tag/tcpreplay
[legacywiki]:  http://tcpreplay.synfin.net/
[rss]:        http://synfin.net/sock_stream/tag/tcpreplay/rss
[Twitter]:    twitter.html
[Podcasts]:   http://tcpreplay.synfin.net/tcprecast/
[playlist]:   http://www.youtube.com/playlist?list=PLFjjcN5EvTP0Hsxq1AEh7FhvaFH__Vd83
[history]:        history.html
[installation]:   installation.html
[Live chat]:      chat.html
[Support]:        support.html
[FAQ]:            faq.html
[How To]:         howto.html
[Sample Captures]: captures.html
