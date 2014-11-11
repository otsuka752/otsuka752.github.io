---
layout: content
title:  "FAQ"
categories: tcpreplay wiki
description: "よくある質問とその回答／Frequently Asked Questions"
---


- 一般的な質問／General Questions
	- [どうして大文字の Tcpreplay なの？](#how-should-tcpreplay-be-capitalized)
	- [Tcpreplay の入手方法](#where-do-i-get-tcpreplay)
	- [Tcpreplay のインストール方法](#how-do-i-install-tcpreplay)
	- [Windows 版はありますか？](#is-there-a-microsoft-windows-port)
	- [Tcpreplay のライセンスは？](#how-is-tcpreplay-licensed)
- Tcpreplay の実行
	- [tcpreplay はサーバにトラフィックを送信できますか？](#does-tcpreplay-support-sending-traffic-to-a-server)
	- [なぜ Tcpreplay は指定した速度でトラフィックを送信できないのですか？](#why-doesnt-tcpreplay-send-traffic-as-fast-as-i-told-it-to)
	- [tcpreplay を実行しているマシンでパケットを送出できますか？](#can-i-send-packets-on-the-same-computer-running-tcpreplay)
	- [古いバージョンの tcpreplay はパケットを書き換えできたのに(今はできません)](#older-versions-of-tcpreplay-allowed-me-to-edit-packets-what-happened)
	- [tcpprep でキャッシュファイルを作る必要性は？ tcprewrite だけでは動かない？](#why-do-i-need-to-use-tcpprep-to-create-cache-files-can't-this-be-done-in-tcprewrite)
	- [tcpreplay が全てのパケットを送出しない理由は？](#why-is-tcpreplay-not-sending-all-the-packets)
	- [tcpreplay の送信タイミングがデタラメなのはなぜ？](#why-are-tcpreplay-timings-all-messed-up)
	- [tcpreplay は複数のネットワークカード(NIC)で使えますか？(Tomahawk (というアプリケーション)のように)](#does-tcpreplay-support-dual-nic's-like-tomahawk)
	- [tcpreplay は gzip/bzip2 圧縮されたファイルを読み込めますか？](#can-tcpreplay-read-gzipbzip2-compressed-files)
	- [tcpreplay はどのくらいの速度でパケットを送出できますか？](#how-fast-can-tcpreplay-send-packets)
	- [どうやったらもっと速くパケットを送出できますか？](#how-can-i-make-tcpreplay-run-even-faster)
	- [tcpreplay は Endace DAG カードで使えますか？](#does-tcpreplay-support-endace-dag-cards)
	- [pcap ファイル以外のキャプチャファイルを使えますか？](#can-i-use-non-pcap-capture-files)
	- [Tcpreplay は Pcap-Ng/NTAR ファイルを読み込めますか？](#does-tcpreplay-support-pcap-ngntar-files)
	- [tcpreplay は Wi-Fi からパケットを送出できますか？](#can-tcpreplay-send-packets-over-wifi)
	- [loopback インターフェイスから送出したパケットが見えないのはなぜ？](#why-doesnt-my-application-see-packets-replayed-over-loopback)
	- [iptables などでトラフィックコントロールできますか？](#can-i-use-iptablestraffic-control-with-tcpreplay)
- Tcpreplay のコンパイル
	- [XXX 用のバイナリファイルはありますか？](#are-there-binaries-available-for-xxx-operating-system)
	- [お願いしたらバイナリを作ってくれますか？](#what-if-i-ask-you-really-nicely-to-build-a-binary-for-me)
	- [パケットを送出するための方法が見つかりません。libpcap をバージョンアップするか libdnet を有効化してください](#unable-to-find-a-supported-method-to-send-packets--please-upgrade-your-libpcap-or-enable-libdnet)
	- [tcpedit_stub.def: Command not found](#tcpedit_stubdef-command-not-found)
	- [tcpreplay_opts.h:72:3: error: #error option template version mismatches autoopts/options.h header](#tcpreplay_optsh723-error-#error-option-template-version-mismatches-autooptsoptionsh-header)
	- [autogen や libopts に関する話題][Issues with autogen/libopts](#issues-with-autogenlibopts)
	- [Fedora Core/RedHat で発生するリンクできない問題][Problems with linking under recent Fedora Core/RedHat](#problems-with-linking-under-recent-fedora-coreredhat)
- 一般的なエラー
	- [][Unable to send packet: Error with pcap_inject(packet #10): send: Message too long](#unable-to-send-packet-error-with-pcap_injectpacket-#10-send-message-too-long)
	- [Can't open eth0: libnet\_select\_device(): Can't find interface eth0](#can't-open-eth0-libnet_select_device-can't-find-interface-eth0)
	- [Can't open eth0: UID != 0](#can't-open-eth0-uid-0)
	- [100000 write attempts failed from full buffers and were repeated](#100000-write-attempts-failed-from-full-buffers-and-were-repeated)
	- [test.cache を処理できない／cache ファイルのバージョンミスマッチ](#unable-to-process-testcache-cache-file-version-mismatch)
	- [SLL loopback パケットがスキップされる](#skipping-sll-loopback-packet)
	- [8892 バイトのパケットが MTU より大きいくてスキップされる](#packet-length-8892-is-greater-then-mtu-skipping-packet)
	- [tcpreplay がパケットの一部しか送信しない／tcprewrite がパケットをトランケートする](#tcpreplay-doesn't-send-entire-packettcprewrite-truncates-packets)
	- [tcpreplay が順番通りにパケットを送信しない](#tcpreplay-is-sending-packets-out-of-order)
- Tcpreplay のユースケース
	- [ユースケース(外部サイト)](#external-use-cases)
	- [他のドキュメント](#other-documents)


一般的な質問／General Questions
=================

<h2 id="FAQ"><a name="how-should-tcpreplay-be-capitalized">Q:</a>どうして大文字の Tcpreplay なの？／How should Tcpreplay be capitalized?</h2>
大文字の 'T' の場合は、
Tcpreplay のツール群(tcpreplay, tcpprep, tcprewrite, など)を示します。
個別のユーティリティの tcpreplay を示す場合は小文字 't' になります。
'TCPreplay' や 'TCP replay' などと記述することはありません。


<h2><a name="where-do-i-get-tcpreplay">Q:</a>Tcpreplay の入手方法／Where do I get Tcpreplay?</h2>
[(ダウンロードとインストール) Download and Installation][installation] にアクセスしてください。


<h2><a name="how-do-i-install-tcpreplay">Q:</a>Tcpreplay のインストール方法／How do I install Tcpreplay?</h2>
コンパイルする必要があります。
[(ダウンロードとインストール) Download and Installation][installation] にアクセスしてください。

<h2><a name="is-there-a-microsoft-windows-port">Q:</a>Windows バージョンはありますか？／Is there a Microsoft Windows port?</h2>  
Windows 2000 以降なら Cygwin を使って動作します。
近い将来、Windows 上で (Cygwin を使わず)直接実行できるようになるかもしれません。
(ボランティアを募集してます。)
詳細情報は(Tcpreplay に含まれる) Win32Readme.txt ファイルを見てください。

<h2><a name="how-is-tcpreplay-licensed">Q:</a>Tcpreplay のライセンスは？／How is Tcpreplay licensed?</h2>
Tcpreplay のライセンスは [GPLv3][gplv3]。
詳細情報は(Tcpreplay に含まれる) docs/LICENSE ファイルを見てください。


Tcpreplay の実行／Running Tcpreplay
=================

<h2><a name="does-tcpreplay-support-sending-traffic-to-a-server">Q:</a>tcpreplay はサーバにトラフィックを送信できますか？／Does tcpreplay support sending traffic to a server?</h2>  
(ウェブサーバやメールサーバなどのように)ポートを LISTEN する、
Unix の daemon(デーモン)や Windows の service(サービス)との通信であれば、
*tcpliveplay* を試してみてください。
Tcpreplay に含まれる *tcpliveplay* 以外の他のツール・コマンドは、
TCP プロトコルで使われるようなステートを理解しません。
つまり、正常な TCP セッションを作るための SYN/ACK の同期ができません。
*tcpliveplay* は pcap ファイルに含まれる 1つの TCP ストリームを読み込み、
サーバとコネクションを確立し、pcap ファイルに含まれる内容をサーバに送出します。

*tcpliveplay* は ICMP や UDP では動作しませんが、
*tcpliveplay* 以外の再送信ツールでは、
MAC アドレスや IP アドレスが正しくセットされていれば動作します。
*tcprewrite* や *tcpreplay-edit* を使うと pcap ファイルを書き換えることができます。
プロトコルによっては ICMP や UDP のペイロードに Layer 3/4 の情報を含める場合があります。
(SIP はその 1つの例になります。) 従って、IP アドレスを書き換えた場合、
正しい SIP のパケットにならないことに注意してください。
このような場合は *NetDude* を使ってパケットのペイロードを書き換えてください。

<h2><a name="why-doesnt-tcpreplay-send-traffic-as-fast-as-i-told-it-to">Q:</a>なぜ Tcpreplay は指定した速度でトラフィックを送信できないのですか？／Why doesn't Tcpreplay send traffic as fast as I told it to?</h2>
[netmap][nm] ドライバを使って `--netmap` オプションを指定してください。

(指定した速度でトラフィックを流せないような症状は)
小さなパケットを再送出しようとする時に発生することが多いです。
例えば、平均 646 バイトのパケットを 9Gbps で再送出しようとする場合...

<div class="highlight"><pre><code class="ruby language-ruby" data-lang="ruby"><span class="c1"># </span><span class="no">tcpreplay -i eth7 -K --loop 5000 <span class="ss">-M 9000</span> --unique-ip <span class="ss">smallFlows.pcap</span></span>
File Cache is enabled
Actual: 71305000 packets (46082655000 bytes) sent in 40.09 seconds.
Rated: 1124993518.4 Bps, <span class="ss">8999.94 Mbps, 1740734.40 pps</span>
Flows: 6045000 flows, <span class="ss">147573.65 fps</span>, 71215000 flow packets, 90000 non-flow
Statistics for network device: eth7
    Attempted packets:         71305000
    Successful packets:        71305000
    Failed packets:            0
    Truncated packets:         0
    Retried packets (ENOBUFS): 0
    Retried packets (EAGAIN):  0</span></code></pre></div>

... そして、平均 77バイトの(小さな)パケットを再送出しようとする場合と比較してください。

<div class="highlight"><pre><code class="ruby language-ruby" data-lang="ruby"><span class="c1"># </span><span class="no">tcpreplay -i eth7 -K --loop 50000 <span class="ss">-M 9000</span> --unique-ip tiny-packets.pcap</span> 
File Cache is enabled
Actual: 550000 packets (42600000 bytes) sent in 0.259064 seconds.
Rated: 164438131.1 Bps, <span class="ss">1315.50 Mbps, 2123027.51 pps</span>
Flows: 100000 flows, <span class="ss">386005.00 fps</span>, 300000 flow packets, 250000 non-flow
Statistics for network device: eth7
    Attempted packets:         550000
    Successful packets:        550000
    Failed packets:            0
    Truncated packets:         0
    Retried packets (ENOBUFS): 0
    Retried packets (EAGAIN):  0</span></code></pre></div>

*pcap ファイル* の中身が小さいパケットの(後者の 77バイト)の方が、
単位時間あたりのパケット数 (*pps*) も単位時間あたりのフロー数 (*fps*) も大きいですが、
単位時間あたりの送信 bit 数 (*bps*) には影響されません(速度は大きくなりません)。

tcpreplay でなるべく速くトラフィックを送信するための工夫・アイディアがあります:

* Tcpreplay の最新の安定版バージョンを使ってください
* [netmap][nm] を使って `--netmap` オプションを指定してください
* `--preload-pcap` オプションを使って pcap ファイルを事前にメモリに読み込んでください
* 異なる時間間隔指定オプションを使ってみてください。`--timer=gtod` や `--timer=nano` などを指定できます。OS X の環境では `--timer=abstime` を使用すべきです
* `--pps` オプションでなく `--mbps` オプションを使ってください
* もし `--pps` オプションを指定する場合は `--pps-multi=X*` も併用してください。*tcpreplay* がスリープ時間中にも複数のパケットを送出できるようにするためです。
* `--topspeed` や `--mbps=0` オプションを使ってください。常に最大速でパケットを送出しようとします。
* 十分に速いネットワークに接続してください。10Mbps のリンクで 50Mbps のトラフィックは送出できません。
* 上記のオプションを組み合わせて試してみてください。送出しようとする速度に届くかもしれません。

<h2><a name="can-i-send-packets-on-the-same-computer-running-tcpreplay">Q:</a>tcpreplay を実行しているマシンでパケットを送出できますか？／Can I send packets on the same computer running tcpreplay?</h2>
一般的にはできません。*tcpreplay* がパケットを送出する時は、
システムの TCP/IP スタックと (NIC の)ドライバの間に割り込みます。
その結果、*tcpreplay* を動作させてるシステムの TCP/IP スタックは、
パケットが見えなくなってしまいます。

この問題の解決策として、VMWare や Parallels や Xen など(の仮想化機能)があります。
*tcpreplay* を仮想マシン(GuestOS) 上で動作させれば、
ホストマシン(HostOS) では送出したパケットを見れます。


<h2><a name="older-versions-of-tcpreplay-allowed-me-to-edit-packets-what-happened">Q:</a>古いバージョンの tcpreplay はパケットを書き換えできたのに(今はできません)／Older versions of tcpreplay allowed me to edit packets. What happened?</h2>
*tcpreplay-edit* や *tcprewrite* を使ってください。


<h2><a name="why-do-i-need-to-use-tcpprep-to-create-cache-files-can't-this-be-done-in-tcprewrite">Q:</a>tcpprep でキャッシュファイルを作る必要性は？ tcprewrite だけでは動かない？／Why do I need to use tcpprep to create cache files? Can't this be done in tcprewrite?</h2>
*tcpprep* のほとんどのモードでは、pcap ファイル全体を確認します。
どのパケットがサーバのものでどのパケットがクライアントのものであるかを決める前に、
(pcap ファイル中の)全てのパケットを確認します。
pcap ファイル全体を見る必要がありますし、2回読み込まれることもあります。
(高速な動作を求められる) *tcpreplay* に、
このような *tcpprep* のロジックを組み込むことは事実上許されません。

2つ目の理由として、*tcpreplay* も *tcpreplay-edit* も *tcprewrite* も
*tcpprep* のキャッシュファイルを利用できるので、
(*tcpprep* を)個別のユーティリティとして分離することは有効です。
そして、複数回にわたって pcap ファイルを書き換えたり、
実際にパケットを送出する時にも機能させることが可能になります。


<h2><a name="why-is-tcpreplay-not-sending-all-the-packets">Q:</a>tcpreplay が全てのパケットを送出しない理由は？</h2>
時折、(pcap ファイルの中の)全てのパケットが送信されない bug があるのではないか、
というメールが tcpreplay-users のメーリングリストに投稿されます。
`-t` オプションを使った場合に、全てのパケットが送信されない症状が発生しがちです。
このような場合は、NIC は(Tcpreplay から)パケットを受け取ったのですが、
(ネットワーク上に)送信できないのです。
この問題は、ハードウェアに大きく依存しています。

ネットワークソケットが混雑しているかどうかを表示させるのは重要です。
tcpreplay は、(pcap ファイルの)次のパケットの処理に移行する前に、
(NIC の)バッファが書き込める状態になるまで待ちます。
もしパケットが欠落する現象が発生した場合、
NIC がパケットを受け取った後に問題が発生したことになります。

以前のバージョンでは以下のような症状がありました:

tcpreplay を何度も実行し、かつ、
パケットを何発送信したかを数えるために *tcpdump* や他のスニファーツールを使っている場合、
(tcpreplay と tcpdump で)パケットの送信数が異なる場合があります。
これは *tcprepaly* の問題ではありません。下記を調査してください:

1. *tcpdump* や他のスニファーツールでは高速通信時に問題があることがよく知られています。
さらにその上、OS は多くの場合、どの位のパケットが欠落したかを間違って報告します。
*Tcpdump* はこれ(OS)が言うままに間違って報告します。言い換えるなら、
*tcpdump* は全てのパケットを見ている訳ではないのです。
たいていの場合、これは NIC やドライバや OS のカーネルの問題で、
問題を解決できるかできないかは不明です。NIC や NIC のドライバを変更してみてください。

2. *tcpreplay* がパケットを送信するのは、カーネルの送信バッファにコピーすることになります。
この祖バッファが一杯になっている場合、カーネルは tcpreplay に対して、
バッファにコピーしないよう伝えます。
カーネルがこのエラーを表示しないようなバグを持っている場合、
tcpreplay はパケットを再送信することもなく次のパケットの処理に進んでします。
現時点で、このようなバグを持つカーネルは見つかっていませんが、現実にはありえます。
もしこのような問題を持つ OS を見つけた場合は、ぜひ私達に報告してください。

3. Tcpreplay は、インターフェイスの MTU よりも大きなパケットは送信できません。
一般的には、
ifconfig ツールなどを使って Ethernet の MTU をデフォルトの 1500バイトよりも大きくしますが、
商用利用の環境において(MTU を大きくすること)は勧められません。
MTU の不一致の問題は非常に分かりづらいです。
そして、ネットワークのスピードは六分の一程度になってしまいます。

4. NIC のハードウェアで checksum を計算するような場合に、
(1Mbps のような)低レートな環境下でも
*tcpdump* のようなツールがパケットロスを発生するような報告があります。
NIC でこのような(ハードウェアで checksum を計算する)設定を無効にすることで、
パケットロスは発生しなくなります。これは *tcpreplay* の問題ではありません。

<h2><a name="why-are-tcpreplay-timings-all-messed-up">Q:</a>tcpreplay の送信タイミングがデタラメなのはなぜ？</h2>
時折、パケットの送信タイミングがデタラメだと文句を言います。
たいていの場合は、正しくないタイムスタンプを含んだ pcap ファイルが原因です。
もっと具体的に言うと、キャプチャファイルの中のとあるパケットが、
それ以前のパケットのタイムスタンプよりも古い時刻を持つなどです。
NIC のハードウェアでタイムスタンプが付与された後に、
(ソフトウェア的に)複数のキューに分割されてしまう場合に発生します。
このような場合には、
パケットが到着した順序とは異なる順序で(pcap ファイルに)保存されてしまいます。

<h2><a name="does-tcpreplay-support-dual-nic's-like-tomahawk">Q:</a>tcpreplay は複数のネットワークカード(NIC)で使えますか？(Tomahawk (というアプリケーション)のように)</h2>
使えます!
*tcpreplay* はずっと前から 2つのインターフェイスを使って高速で再送信する機能を持っています。
([Tomahawk][tomahawk] が登場するずっと前からです)
トラフィックを 2つのインタフェイスでどう分離するかについては、
*tcpprep* の man ページに詳細が記述されています。

<h2><a name="can-tcpreplay-read-gzipbzip2-compressed-files">Q:</a>tcpreplay は gzip/bzip2 圧縮されたファイルを読み込めますか？</h2>
読み込めますが、直接的には読み込めません。
*tcpreplay* は STDIN(標準入力) からファイルを読み込めるので、
下記のような方法でオンザフライで展開できます:

```
gzcat myfile.pcap.gz | tcpreplay -i eth0 -
```

オンザフライでファイルを展開するには CPU 時間が消費されるので、
*tcpreplay* のパフォーマンスが下がってしまいます。

<h2><a name="how-fast-can-tcpreplay-send-packets">Q:</a>tcpreplay はどのくらいの速度でパケットを送出できますか？</h2>
まず初めに、パフォーマンスが重要なのであれば、4.x にアップグレードする価値があります。
3.x シリーズから最適化されています。
次に、パフォーマンスや、
あなたがどう測定するか(packet/sec や bytes/sec)について影響する変数はたくさんあります。

パフォーマンスは、指定したオプション、再送信する pcap ファイル、
ハードウェアなどに依存します。
[netmap][nm] がサポートされている NIC を使うとパフォーマンスが大きく向上しますが、
自分の首を絞めないように注意してください。
`--netmap` オプションを使う間は、
テストの期間中はネットワークドライバは使われなくなります(バイパスされます)。

*tcpreplay* を i7 processor/Intel 82599 10GbE NIC で動作させた例を出します。
`--netmap` オプションを使うことで、
ほぼワイヤーレートで 157K flows/sec を実現できます。


<div class="highlight"><pre><code class="ruby language-ruby" data-lang="ruby"><span class="c1"># </span><span class="no">tcpreplay --preload-pcap -i eth0 --loop 500 -t --unique-ip <span class="ss">--netmap</span> smallFlows.pcap</span> 
Switching network driver for eth0 to netmap bypass mode... done!
File Cache is enabled
Actual: 7130500 packets (4608265500 bytes) sent in 3.08 seconds.
Rated: 1197981408.4 Bps, <span class="ss">9583.85 Mbps</span>, 1853670.63 pps
Flows: 604500 flows, <span class="ss">157148.01 fps</span>, 7121500 flow packets, 9000 non-flow
Statistics for network device: eth0
    Attempted packets: 7130500
    Successful packets: 7130500
    Failed packets: 0
    Truncated packets: 0
    Retried packets (ENOBUFS): 0
    Retried packets (EAGAIN): 0
Switching network driver for eth0 to normal mode... done!</span></code></pre></div>

**注意** 上記の例では、最初に出した例と比較して、ほぼワイヤーレートを実現しています。
この pcap ファイルの平均的なパケットサイズは
646 (4608265500 ÷ 7130500) バイトです。
10GbE の Ethernet では 17 バイトの IFG(Inter Frame Gap) とプリアンブルと
SOF(Start Of Frame) と FCS(Frame Check Sequence) が付与されるため、
663 (646 + 17) バイトになります。
そして Ethernet 上では (663 ÷ 646) × 9582.85 = 9836 Mbps となります。
Finally,
the manufacturer of this adapter does not claim 100% wire rate because it is front-ended by 
a hardware timestamp feature. You may achieve 100%.

(例えば GigabitEthernet において 1048 Mbps などのように)
最大値を超える値が表示されるかもしれません。
You may achieve 100% with your adapter and maybe even more.

The next example is the same except limited to 9500Mbps with the `-M` option. As of version
4.0 there is little overhead in using this option.

<div class="highlight"><pre><code class="ruby language-ruby" data-lang="ruby"><span class="c1"># </span><span class="no">tcpreplay --preload-pcap -i eth0 -l 500 <span class="ss">-M 9500</span> --unique-ip --netmap smallFlows.pcap</span> 
Switching network driver for eth0 to netmap bypass mode... done!
File Cache is enabled
Actual: 7130500 packets (4608265500 bytes) sent in 3.08 seconds.
Rated: 1187498663.2 Bps, <span class="ss">9499.98 Mbps</span>, 1837450.38 pps
Flows: 604500 flows, 155772.91 fps, 7121500 flow packets, 9000 non-flow
Statistics for network device: eth0
	Attempted packets:         7130500
	Successful packets:        7130500
	Failed packets:            0
	Truncated packets:         0
	Retried packets (ENOBUFS): 0
	Retried packets (EAGAIN):  0
Switching network driver for eth0 to normal mode... done!</span></code></pre></div>

When using pcap files with tiny packets, full wire rate is not achieved. The limiting
factor is the flows per second (fps). Notice that in the following example we achieve 1.8 million
fps and our packets per second (pps) rate has jumped dramatically.

<div class="highlight"><pre><code class="ruby language-ruby" data-lang="ruby"><span class="c1"># </span><span class="no"># tcpreplay --preload-pcap -i eth0 -l50000 -t --unique-ip --netmap <span class="ss">tiny-packets.pcap</span></span> 
Switching network driver for eth0 to netmap bypass mode... done!
File Cache is enabled
Actual: 550000 packets (42600000 bytes) sent in 0.054122 seconds.
Rated: 787110601.9 Bps, 6296.88 Mbps, <span class="ss">10162226.08 pps</span>
Flows: 100000 flows, <span class="ss">1847677.46 fps</span>, 300000 flow packets, 250000 non-flow
Statistics for network device: eth0
	Attempted packets:         550000
	Successful packets:        550000
	Failed packets:            0
	Truncated packets:         0
	Retried packets (ENOBUFS): 0
	Retried packets (EAGAIN):  0
Switching network driver for eth0 to normal mode... done!</span></code></pre></div>

If anyone achieves better results or has 40GigE results, please share.


<h2><a name="how-can-i-make-tcpreplay-run-even-faster">Q:</a>どうやったらもっと速くパケットを送出できますか？</h2>
tcpreplay では、
パケットをネットワークに書き出す時間がもっとも影響を与えています。
tcpreplay はこれにほとんどの時間を取られているため、
使っている OS のカーネルが raw sockets に書き込む実装方法は、
最も重要事項になります。

Profiling tcpreplay has shown that a significant amount of time is spent writing packets to the network. Hence, your OS kernel 
implementation of writing to raw sockets is one of the most important aspects since that is where tcpreplay spends most of it's time.

下記は順不同で:

* ハードウェア／Hardware:   
 1. 十分に高速なハードウェアを使ってください。*tcpreplay* your network card and CPU.
 2. 高速なメモリを使ってください。高速な CPU を使うことと同じくらい重要なことです。
 3. tcpreplay はマルチスレッドには対応していないので、SMP や dual-core でもあまり高速になりません。
 しかし、他のプロセス(アプリケーション)が(tcpreplay が使っている CPU とは)別の CPU を使えます。
 4. ハイパースレッド(hyperthreading (HT))対応の CPU の場合は HT を無効にしてください
 5. 良い NIC を使うことは重要です。10GbE の場合、Intel 82599 アダプタは良い結果を出しました。
 6. マザーボードの設定は重要です。マザーボード上に 4-6個の NIC がある場合もありますが、
サウスブリッジ(Southbridge)に接続されているのは間違いありません。
サウスブリッジは USB やシリアルポートなどの低速なデバイス用のものなので、
これらのオンボード NIC を使うと残念な結果になります。
速度を高速にするためには、高速なノースブリッジ(Northbridge)に接続されている
PCI スロットに NIC を接続してください。
 7. NIC に十分な PCI バスのレーンがあることを確認してください。
PCI Express のレーンは、各方向に 200MB/s の帯域が確保されています。
10GbE の NIC は、x8 レーンの PCI Express スロットに接続してください。
 8. このセクションに記述された内容は [Mark Wagner(PDF)][mark_wagner]にも記載されています。
特に，10GbE の割り込み(interrupts)のために
CPU アフィニティ(affinity)を制御することについて参考になります。
この資料に推奨される方法で、パフォーマンスは劇的に改善されました。
 
* 設定／Configuration:
 1. たくさんのメモリが搭載されているなら、pcap ファイルをメモリに展開する `--preload-pcap` を指定してください。
 2. 十分なメモリが搭載されていない場合は、
 *tcpreplay* が OS のディスクキャッシュから読み出せる程度の pcap ファイルにしてください。
 2回目以降に実行した時のパフォーマンスが向上します。
 3. (X を実行していたり、メールをダウンロードするなどのように)
 他のプロセス群が実行されている場合、
 *tcpreplay* のパフォーマンスやパケットの時間測定にもに影響します。
 4. 小さな (pcap) ファイル群を読み出すとパフォーマンスは下がります。
 [mergecap][mergcap] ツールを使って、
 1つの大きな (pcap) ファイルにまとめることを検討してください。
 5. ネットワークの I/O と同様に、あるデータ量を送信する場合、
 たくさんの小さなパケットを送信するのに比べ、
 少ない大きなパケットを送信する方が高速になります。
 6. Linux では新しいカーネルを使ってください。一般的には 2.6.18 以上が良いでしょう。
I/O 性能が向上した [NAPI][napi] がサポートされています。

<h2><a name="does-tcpreplay-support-endace-dag-cards">Q:</a>tcpreplay は Endace DAG カードで使えますか？</h2>
デフォルトでは *tcpreplay* は Endace DAG カードをサポートしません。
しかし、[Endace 社][endace] は、
Endance 社の DAG カードをサポートする特別な tcpreplay をリリースしました。
Tcpreplay の開発者達は、この特別な tcpreplay はサポートできませんので注意してください。
何か質問がある場合は、Endace 社に問い合わせてください。


<h2><a name="can-i-use-non-pcap-capture-files">Q:</a>pcap ファイル以外のキャプチャファイルを使えますか？</h2>
pcap ファイルのフォーマット以外にも、非常にたくさんのフォーマットがあります。
(Solaris snoop などのように) 別なツールで作られたキャプチャファイルを使う場合は、
[Wireshark's][wireshark] の tshark ツールなどを使って pcap フォーマットに変換してください。

```
tshark -r blah.snoop -w blah.pcap
```

<h2><a name="does-tcpreplay-support-pcap-ngntar-files">Q:</a>Tcpreplay は Pcap-Ng/NTAR ファイルを読み込めますか？</h2>
はい、読み込めます。Tcpreplay ツール群は、pcap ファイルの読み書きに
[libpcap][tcpdump] を使っています。libpcap 1.1.0 以上を使っていれば、
*tcpreplay* や *tcprewrite* などは pcap-ng ファイルを読み込めます。
古い libpcap を使っている場合は、最新の libpcap にバージョンアップすべきです。
初期の libpcap には pcap-ng ファイルに関する問題があります。


<h2><a name="can-tcpreplay-send-packets-over-wifi">Q:</a>tcpreplay は Wi-Fi からパケットを送出できますか？</h2>
送信できるかどうかは、OS やハードウェア依存の話題になります。
しかし、たいていの場合は送信できます。
送信するためには、通常は下記の手順になります:

* Wi-Fi カードをマネージモード(managed mode)にします
* pcap ファイルの DLT は DLT_EN10MB (Ethernet) で 803.2 として有効なヘッダを含めます
* 送信元の MAC アドレスは Wi-Fi カードの MAC アドレスにします


<h2><a name="why-doesnt-my-application-see-packets-replayed-over-loopback">Q:</a>loopback インターフェイスから送出したパケットが見えないのはなぜ？</h2>
loopback インターフェイスを経由して UDP パケットを再送信しようとする時、
loopback インターフェイスで待ち受けているデーモンにおいて、
(tcpreplay が送信した)パケットを受信できないということに、
たくさんのユーザが驚くようです。
これは、多くの OS での loopback インターフェイスの制限事項です。
1つの要因として、Ethernet インターフェイスでパケットをキャプチャして、
IPアドレスを書き換えただけで Layer2 のヘッダを書き換えないことがあります。
loopback インターフェイスは Ethernet の Layer2 ヘッダを使わないため、
OS の IP(Internet Protocol)スタックはパケットを処理できず、
待ち受けているデーモンにパケットを届けられないのです。


<h2><a name="can-i-use-iptablestraffic-control-with-tcpreplay">Q:</a>iptables などでトラフィックコントロールできますか？</h2>
tcpreplay を実行しているのと同じサーバでは、iptables/tc は使えません(機能しません)。
tcpreplay で送信したパケットに IPTables や Traffic Control (tc) を適用するには、
tcpreplay を iptables/tc とは別のサーバで動作させ、
tcpreplay で送信したパケットを iptables/tc のサーバを経由させる方法しかありません。
この制限事項は、Linux カーネルがフレーム(パケット)を挿入する方法と、
iptables/tc のためにフレーム(パケット)を読み出す方法の差異によるものです。
この違いで、tcpreplay が送出するトラフィックは、
iptables/tc からは参照できなくなります。


Tcpreplay のコンパイル／Compiling Tcpreplay
===================
  
<h2><a name="are-there-binaries-available-for-xxx-operating-system">Q:</a>XXX 用のバイナリファイルはありますか？<h2>
たぶんあると思います。
私達は、いかなる OS 用にもバイナリファイルをリリースしません。
Linux や \*BSD や Solaris や OS X などのたくさんの OS では、
*tcpreplay* などのオープンソースなアプリケーションをパッケージにするチームがあり、
それぞれのパッケージフォーマットで(RPM や BSD/Mac ports や SunFreeware などで)
リリースしています。


<h2><a name="what-if-i-ask-you-really-nicely-to-build-a-binary-for-me">Q:</a>お願いしたらバイナリを作ってくれますか？</h2>
いつでも依頼して良いのですが、きっとその依頼は無視されてしまいます。


<h2><a name="unable-to-find-a-supported-method-to-send-packets--please-upgrade-your-libpcap-or-enable-libdnet">Q:</a>パケットを送出するための方法が見つかりません。libpcap をバージョンアップするか libdnet を有効化してください</h2>
Tcpreplay は、パケットを送信するために多くの API やライブラリを使えます。
BSD の BPF や、Linux の PF\_PACKET や libpcap や libdnet です。
もし BPF や PF\_PACKET がサポートされていないプラットフォームで使いたい場合は、
最新バージョンの [libpcap][tcpdump] や [libdnet][libdnet] が必要になります。
libpcap を使うのであれば、最新バージョンにアップグレードすることを強く勧めます。
libpcap にはバグがあるし、頻繁にアップデートされているからです。
それに対して、libdnet は数年間アップデートが無い状態ですが、
古いバージョンの libnet ライブラリよりは新しいバージョンの方が安定しています。

ここで、libdnet が必要なケースを紹介します:
Linux や \*BSD 以外で *tcpbridge* を使いたい場合です。

注意: Tcpreplay はもう libnet はサポートしません! (libdnet はサポートされます)


<h2><a name="tcpedit_stubdef-command-not-found">Q:</a> tcpedit_stub.def: Command not found</h2>

```
Making all in tcpedit
make[2]: Entering directory `/home/acferen/tcpreplay-trunk/src/tcpedit'
tcpedit_stub.def
make[2]: tcpedit_stub.def: Command not found
make[2]: *** [tcpedit_stub.h] Error 127
make[2]: Leaving directory `/home/acferen/tcpreplay-trunk/src/tcpedit'
make[1]: *** [all-recursive] Error 1
make[1]: Leaving directory `/home/acferen/tcpreplay-trunk/src'
make: *** [all] Error 2
```

[GitHub][github] のソースコードをビルドしようとしている時のみ、
上記のエラーが発生するはずです。(普通の tarball では発生しません)
この問題は [GNU Autogen][autogen] がインストールされていないことが原因です。
autogen をインストールするか、tarball をダウンロードしてください。


<h2><a name="tcpreplay_optsh723-error-#error-option-template-version-mismatches-autooptsoptionsh-header">Q:</a> tcpreplay_opts.h:72:3: error: #error option template version mismatches autoopts/options.h header</h2>
[GitHub][github] のソースコードをビルドしていて、
インストールされている Autogen/AutoOpts が libopts のバージョンと異なるためです。
結果として、OS の Autogen が生成する src/*_opts.[ch] が、
バージョンミスマッチとなってしまいます。

この問題を解決するには: `./configure --disable-local-libopts --disable-libopts-install`


<h2><a name="issues-with-autogenlibopts">Q:</a>autogen や libopts に関する話題</h2>
Tcpreplay のツール群は GNU の Autogen/libops ライブラリを使います。
これによりとても便利に開発できるようになりますが、コストもかかります。
一般に、Autogen/libopts ライブラリはあまり後方互換が良くないので、
ソースコードを書いた人とは異なる Autogen/libopts を使っていると、
tcpreplay のコンパイル時に問題となりえます。
例えば、下記のようなエラーになります:

```
make[3]: Entering directory
`/root/services_from_source/tcpreplay-3.0.beta11/src/tcpedit'
if gcc -DHAVE_CONFIG_H -I. -I. -I../../src    -I.. -I../common -I../..   -g
-O2 -Wall -O2 -funroll-loops -std=gnu99 -MT tcpedit.o -MD -MP -MF
".deps/tcpedit.Tpo" -c -o tcpedit.o tcpedit.c; \
then mv -f ".deps/tcpedit.Tpo" ".deps/tcpedit.Po"; else rm -f
".deps/tcpedit.Tpo"; exit 1; fi
In file included from tcpedit.c:46:
tcpedit_stub.h:18:30: autoopts/options.h: No such file or directory
In file included from tcpedit.c:46:
tcpedit_stub.h:50: error: syntax error before "const"
tcpedit_stub.h:50: warning: type defaults to `int' in declaration of
`tcpedit_tcpedit_optDesc_p'
tcpedit.c:58: error: syntax error before "const"
tcpedit.c:58: warning: type defaults to `int' in declaration of
`tcpedit_tcpedit_optDesc_p'
make[3]: *** [tcpedit.o] Error 1
```
良い方法があって、下記で簡単に解決できます: `./configure --enable-local-libopts`


<h2><a name="problems-with-linking-under-recent-fedora-coreredhat">Q:</a>Fedora Core/RedHat で発生するリンクできない問題</h2>
新しいバージョンの Fedora Core や Red Hat ES/WS では、
static library は同梱されず dynamic library だけになります。
もしソースコードから *tcpreplay* をコンパイルする場合は、
*tcpreplay* が library をリンクできるよう、configure を実行する時に
`--enable-dynamic-link` を指定する必要があります。


一般的なエラー／Common Errors
=============

<h2><a name="unable-to-send-packet-error-with-pcap_injectpacket-#10-send-message-too-long">Q:</a> Unable to send packet: Error with pcap_inject(packet #10): send: Message too long</h2>
OS X (少なくとも 10.4.9 までの) バグです。
(tcpreplay や tcpbridge が動作する時もそうですが)送信元の MAC アドレスを偽って、
1500 バイト以上の Ethernet フレームを送信する時に発生します。
アップル社は 10.5(Leopard) でこの問題を修正しました。
現時点で、ワークアラウンド(回避方法)はありません。


<h2><a name="can't-open-eth0-libnet_select_device-can't-find-interface-eth0">Q:</a> Can't open eth0: libnet_select_device(): Can't find interface eth0</h2>
たいていの場合は、(例えば eth0 などの)インターフェイスが UP していなかったり、
あるいは IPアドレスが設定されていない場合に発生します。

<h2><a name="can't-open-eth0-uid-0">Q:</a> Can't open eth0: UID != 0</h2>
Tcpreplay は root(管理者)権限で実行する必要があります。


<h2><a name="100000-write-attempts-failed-from-full-buffers-and-were-repeated">Q:</a> 100000 write attempts failed from full buffers and were repeated</h2>
*tcpreplay* が
"100000 write attempts failed from full buffers and were repeated"
のようなメッセージを出力する時は、
たいていカーネルバッファが一杯になっており、
メモリに書き込めるようになるまで処理を停止する必要があります。
`-t` オプションを指定して最大速度で送信している時に発生します。
この文章の「OS のチューニング」のセクションを読めば問題を解決できます。


<h2><a name="unable-to-process-testcache-cache-file-version-mismatch">Q:</a>test.cache を処理できない／cache ファイルのバージョンミスマッチ</h2>
*tcpprep* で生成され *tcpreplay* で利用される cache ファイルのフォーマットは、
機能拡張のためにいくつかのバージョンがあります。
(Tcpreplay の)バージョンアップの際に、
cache ファイルのフォーマットは変更されるかもしれません。
フォーマットは滅多に変更されませんが、いつでも変更されうるものです。
この場合、以前の別のバージョンで生成された cache ファイルと互換性が無くなります。
同じバージョンの *tcpreplay* と *tcpprep* であれば、
cache ファイルを読み書きできます。
cache ファイルの各バージョンと、tcpprep/tcpreplay のバージョンの対応表は下記です:

* バージョン 1／Version 1:
 * 1.3.beta1 以前／Prior to 1.3.beta1
* バージョン 2／Version 2:
 * 1.3.beta2 から 1.3.1/1.4.beta1
* バージョン 3／Version 3:
 *1.3.2/1.4.beta2 から 2.0.3
* バージョン 4／Version 4:
 * 2.1.0 以降

2.3.0 以前のバージョンの tcpprep には、
big endian と little endian のシステム間で互換性が無い問題があるので注意してください。


<h2><a name="skipping-sll-loopback-packet">Q:</a>SLL loopback パケットがスキップされる</h2>
Linux でパケットをキャプチャし、インターフェイスを 'any' 指定するなどしたため、
loopback のインターフェイスでもキャプチャされました。
(これらのパケットは) tcpreplay が実際にパケットを送信するだけの十分な情報が無いので、
パケットはスキップされるのです。
宛先または送信元の MAC アドレスを (-D や -S で) 指定すれば、
この問題は解決します。


<h2><a name="packet-length-8892-is-greater-then-mtu-skipping-packet">Q:</a>8892 バイトのパケットが MTU より大きいくてスキップされる</h2>
(下記の例では 8892 バイトの) パケット長が、
送信するインターフェイスの MTU(Maximum Transmition Unit)よりも大きいです。
Tcpreplay は、このパケットは破棄せざるをえません。
代わりに、*tcpreplay-edit* で パケットサイズを MTU に切り詰める
`--mtu-trunc` オプションを指定することで、
フレームのチェックサムも再計算され送信できるようになります。
ただし、パフォーマンスに影響するので注意してください。


<h2><a name="tcpreplay-doesn't-send-entire-packettcprewrite-truncates-packets">Q:</a>tcpreplay がパケットの一部しか送信しない／tcprewrite がパケットをトランケートする</h2>
*tcpreplay* ではエラーが発生していないように見えると思います。
pcap ファイルを Ethereal/Wireshark で見てみると 400バイトのパケットなのに、
tcpreplay では 100バイトしか送信していないことに気付くかもしれません。

このようなエラーはこれまでに 2-3回遭遇したことがありますが全て同じ原因で、
pcap ファイルが破損していました。
どうやら、Red Hat に同梱された古い libpcap のバグが原因のようです。

pcap ファイルの破損が疑われるような場合は、
エラーを検出するために *tcpcapinfo* を実行してみてください。

まず初めに、pcap ファイルのフォーマットの背景から記述します。
それぞれの pcap ファイルの先頭には、
(色々な情報がありますが)とりわけ *snaplen* のファイルヘッダがあります。
snaplen の情報は、パケットごとの最大データサイズです。
もしパケットがこの値よりも大きい場合には、
そのパケットはトランケートされます。
それぞれのパケットには、*len* と *caplen* を含むパケットヘッダを含んでいます。
*len* はもとのパケットサイズで、*caplen* は実際に記録されているデータサイズです。

正しいフォーマットで記録された pcap ファイルでは、
caplen > snaplen になるようなことはありえません。
残念なことにいくつかのツールやアプリケーションでは、
このような問題が発生してしまいます。
従って libpcap ライブラリがこのようなファイルを読み込んだ場合に、
実際のデータ長として snaplen の値を使用してしまいます。
Ethereal/Wireshark は pcap ファイルの読み込みに libpcap ライブラリを使わないため、
もとのパケットサイズを表示することになるのです。

この問題を解決するためには、
16 バイト(0x10)オフセットした先の 2バイトを 0xFFFF に変更します。
これにより pcap ファイルが修正され、
libpcap や tcprewrite や tcpreplay などで正しく読み込めるようになります。

破損した pcap ファイルの例:

```
xxd broken.pcap | head -3
0000000: d4c3 b2a1 0200 0400 0000 0000 0000 0000  ................
0000010: 6000 0000 0100 0000 1b6f 954b ca25 0e00  `........o.K.%..
0000020: 4a00 0000 4a00 0000 0000 5e00 0101 0015  J...J.....^.....
```

正しく修正された pcap ファイルの例:

```
xxd fixed.pcap | head -3
0000000: d4c3 b2a1 0200 0400 0000 0000 0000 0000  ................
0000010: ffff 0000 0100 0000 1b6f 954b ca25 0e00  .........o.K.%..
0000020: 4a00 0000 4a00 0000 0000 5e00 0101 0015  J...J.....^.....
```


<h2><a name="tcpreplay-is-sending-packets-out-of-order">Q:</a>tcpreplay が順番通りにパケットを送信しない</h2>
これは tcpreplay の問題ではありません。
受信したネットワークカードやドライバの問題です。
この問題が発生した 1つの例として、
Broadcom の 10GbE の NIC で "multi\_mode" を有効にする場合が挙げられます。
bnx2x のドライバで "multi\_mode=0" を設定すれば問題が解決します。
Linux カーネルの v2.6.24 で、このデフォルト値が ON になったようです。

以前は、NIC がハードウェアで時刻情報を付与したような時に、
この問題が発生していました。


Tcpreplay のユースケース／Use Cases for Tcpreplay
======================= 

<h2><a name="external-use-cases">Q:</a>ユースケース(外部サイト)</h2>
* Renaud Bidou の文章 [How to Test an IDS][howtoids] に *tcpreplay* が登場します

<h2><a name="other-documents">Q:</a>他のドキュメント</h2>
* [RFC 2544][rfc2544] - ネットワークデバイスのベンチマーク方法論／Benchmarking Methodology for Network Interconnect Devices


[gplv3]:                http://www.gnu.org/licenses/gpl-3.0.html
[howtoids]:             http://www.iv2-technologies.com/HowToTestAnIPS.pdf
[nm]:                   http://info.iet.unipi.it/~luigi/netmap/
[tomahawk]:             http://tomahawk.sourceforge.net/
[mergcap]:              http://wiki.wireshark.org/Tools
[endace]:               http://www.emulex.com/solutions/endace-network-visibility-solutions/
[wireshark]:            http://www.wireshark.org/
[tcpdump]:              http://www.tcpdump.org/
[napi]:                 http://www.linuxfoundation.org/collaborate/workgroups/networking/napi
[libdnet]:              http://libdnet.sourceforge.net/
[github]:               https://github.com/appneta/tcpreplay
[autogen]:              http://autogen.sourceforge.net/
[mark_wagner]:          http://www.redhat.com/promo/summit/2008/downloads/pdf/Thursday/Mark_Wagner.pdf
[rfc2544]:              http://www.faqs.org/rfcs/rfc2544.html
[installation]:         {{ site.url }}/content/installation.html
