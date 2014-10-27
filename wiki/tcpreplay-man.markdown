---
layout: content
title:  "tcpreplay man page"
categories: tcpreplay wiki
description: "tcpreplay のマニュアル／tcpreplay manual"
---

<!-- Creator     : groff version 1.20.1 -->
<!-- CreationDate: Fri Jan 10 00:00:00 2014 -->
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
<title>tcpreplay</title>

</head>
<body>

<h1 align="center">tcpreplay</h1>

<a href="#NAME">名前／NAME</a><br>
<a href="#SYNOPSIS">書式／SYNOPSIS</a><br>
<a href="#DESCRIPTION">概要／DESCRIPTION</a><br>
<a href="#OPTIONS">オプション／OPTIONS</a><br>
<a href="#OPTION PRESETS">オプションの事前設定／OPTION PRESETS</a><br>
<a href="#FILES">ファイル／FILES</a><br>
<a href="#EXIT STATUS">exit コード／EXIT STATUS</a><br>
<a href="#AUTHORS">AUTHORS／AUTHORS</a><br>
<a href="#COPYRIGHT">COPYRIGHT／COPYRIGHT</a><br>
<a href="#BUGS">バグ／BUGS</a><br>
<a href="#NOTES">注意／NOTES</a><br>

<hr>


<h2>名前／NAME
<a name="NAME"></a>
</h2>


<p style="margin-left:11%; margin-top: 1em">tcpreplay
&minus; pcap ファイルに保存されたネットワークトラフィックを再送信</p>

<h2>書式／SYNOPSIS
<a name="SYNOPSIS"></a>
</h2>



<p style="margin-left:11%; margin-top: 1em"><b>tcpreplay</b>
[<b>&minus;</b><i>flag</i> [<i>value</i>]]...
[<b>&minus;&minus;</b><i>opt&minus;name</i> [[=|
]<i>value</i>]]...<b>&lt;pcap_file(s)&gt;</b></p>

<p style="margin-left:11%; margin-top: 1em">
tcpreplay は tcpdump や他の pcap(3) ファイルに保存された
ネットワークトラフィックを再送信するツールです。</p>

<h2>概要／DESCRIPTION
<a name="DESCRIPTION"></a>
</h2>


<p style="margin-left:11%; margin-top: 1em">
tcpreplay の基本的な使い方は、指定されたファイル群を
キャプチャされた時と同じ速度で再送信したり、
あるいは、ハードウェアが許す限りの速度の範囲内であれば、
指定したデータ速度で再送信します。
オプションとして、2つの NIC にトラフィックを分割したり、
ファイルに保存したり、様々な方法でフィルタしたり編集したり、
FireWall や NIDS や他のネットワークデバイスのテスト環境を提供します。
Tcpreplay Manual http://tcpreplay.appneta.com のページに、
もっと詳細な情報が記載されています。</p>

<h2>オプション／OPTIONS
<a name="OPTIONS"></a>
</h2>



<p style="margin-left:11%; margin-top: 1em"><b>&minus;d</b>
<i>number</i>, <b>&minus;&minus;dbug</b>=<i>number</i></p>

<p style="margin-left:22%;">
デバック出力を有効にします。
このオプションは 1回だけ指定できます。
オプションには整数を指定します。
<i>number</i> は下記の範囲で指定できます:</p>

<p style="margin-left:28%;">0 から 5</p>

<p style="margin-left:22%;">
<i>number</i> のデフォルト値は:<br>
0</p>

<p style="margin-left:22%; margin-top: 1em">
&minus;-enable-debug オプションをつけて configure された場合は、
デバッグ出力のレベルを指定できます。
数値が大きくなるとたくさんの情報を出力します。</p>

<p style="margin-left:11%;"><b>&minus;q</b>,
<b>-&minus;quiet</b></p>

<p style="margin-left:22%;">出力抑制モード</p>

<p style="margin-left:22%; margin-top: 1em">
実行後の統計情報以外は出力しません。</p>

<p style="margin-left:11%;"><b>&minus;T</b> <i>string</i>,
<b>&minus;&minus;timer</b>=<i>string</i></p>

<p style="margin-left:22%;">
パケットの送信タイミングを下記から指定:
select, ioport, gtod, nano.
このオプションは 1回だけ指定できます。
デフォルトの <i>string</i> は:<br>
gtod</p>

<p style="margin-left:22%; margin-top: 1em">
パケットの送信タイミングの方法を選択できます:</p>

<p style="margin-left:22%; margin-top: 1em"><i>nano</i> -
nanosleep() API を使います</p>

<p style="margin-left:22%; margin-top: 1em"><i>select</i> -
select() API を使います</p>

<p style="margin-left:22%; margin-top: 1em"><i>ioport</i> -
Write to i386 IO Port 0x80</p>

<p style="margin-left:22%; margin-top: 1em"><i>gtod
[default]</i> - gettimeofday() ループを使います</p>


<p style="margin-left:11%;"><b>&minus;&minus;maxsleep</b>=<i>number</i></p>

<p style="margin-left:22%;">
パケット間隔を X [ms/milliseconds] より長くしません。
オプションには整数を指定します。
デフォルトの <i>number</i> は:<br>
0</p>

<p style="margin-left:22%; margin-top: 1em">
tcpreplay がパケットを送信する間にスリープする最大時間を
ミリ秒(milliseconds)で指定します。
観測対象となる大多数のパケットの間に、
無意味な空白時間がある場合に使用すると効果的です。
デフォルトでは無効(disabled)です。</p>

<p style="margin-left:11%;"><b>&minus;v</b>,
<b>-&minus;verbose</b></p>

<p style="margin-left:22%;">
パケットを tcpdump を使ってデコードし標準出力(STDOUT)に出力します。
このオプションは 1回だけ指定できます。</p>

<p style="margin-left:11%;"><b>&minus;A</b> <i>string</i>,
<b>&minus;&minus;decode</b>=<i>string</i></p>

<p style="margin-left:22%;">
tcpdump のデコーダに渡す引数です。
このオプションは 1回だけ指定できます。
このオプションは、下記のオプションと一緒に使用する必要があります:
verbose.</p>

<p style="margin-left:22%; margin-top: 1em">
デバッグ出力モード (<b>-v</b>) を使う場合、
パケットのデコード方法を指定するために
1つ以上の引数を <b>tcpdump</b> に渡す必要があります。
デフォルトでは &minus;n と &minus;l が指定されます。
tcpreplay が引数を解釈してしまわないように、
&minus;A や &quot;-axxx&quot; のように '&quot;' でくくってください。
その他のオプションは tcpdump(1) のマニュアルページを見てください。</p>

<p style="margin-left:11%;"><b>&minus;K</b>,
<b>-&minus;preload&minus;pcap</b></p>

<p style="margin-left:22%;">
送信する前に RAM(メモリ)に読み込みます。</p>

<p style="margin-left:22%; margin-top: 1em">
再送信のパフォーマンスを向上させるために、
指定した pcap ファイル群を送信する前に RAM(メモリ)に読み込みます。
事前読み込み機能は、<b>--loop</b> を指定してもしなくても使えます。
このオプションを使う場合は、メモリ使用量を削減するために、
ループごとの統計情報は出力されません。
統計情報は与えられたオプションをベースに予想され、
また、最初のループから収集されます。</p>

<p style="margin-left:11%;"><b>&minus;c</b> <i>string</i>,
<b>&minus;&minus;cachefile</b>=<i>string</i></p>

<p style="margin-left:22%;">
tcpprep のキャッシュファイルを元にトラフィックを
(NIC ごとに)分割します。
このオプションは 1回だけ指定できます。
このオプションを使う場合は オプション:intf2 の指定が必要です。
このオプションは下記のオプションと一緒には使用できません: dualfile</p>

<p style="margin-left:22%; margin-top: 1em">
(FireWall やルータや IDS などの)デバイスを経由させて
双方向にパケットを送信する pcap ファイルを利用する場合、
tcpprep コマンドを使ってキャッシュファイルを作ることで、
トラフィックを 2つの NIC ごとに区別して送信できます。</p>

<p style="margin-left:11%;"><b>&minus;2</b>,
<b>-&minus;dualfile</b></p>

<p style="margin-left:22%;">
ネットワークタップから、一度に 2つのファイルを送信します。
このオプションは 1回だけ指定できます。
このオプションを使う場合は オプション:intf2 の指定が必要です。
このオプションは下記のオプションと一緒には使用できません: cachefile</p>

<p style="margin-left:22%; margin-top: 1em">
ネットワークタップを使ってトラフィックをキャプチャした場合、
それぞれの方向ごとに 2つの pcap ファイルが保存されます。
このオプションを使うことで、
それぞれの NIC のどちらかから 1つのパケットを、また、
それぞれの pcap ファイルのタイムスタンプをベースに
2つのファイルをから順にパケットを取り出して、
2つのファイルを同時に送信できます。</p>

<p style="margin-left:11%;"><b>&minus;i</b> <i>string</i>,
<b>&minus;&minus;intf1</b>=<i>string</i></p>

<p style="margin-left:22%;">
クライアントから、サーバ/RX/プライマリに対して送信する
インターフェイスを指定します。
このオプションは 1回だけ指定できます。</p>

<p style="margin-left:22%; margin-top: 1em">
全てのトラフィックを送信するインターフェイス、または、
tcpprep コマンドで &rsquo;primary&rsquo; とマークされたトラフィックを
送信するインターフェイスです。
プライマリトラフィックは、通常はクライアントからサーバへの、
あるいは仮想インターフェイスの inbound(RX) になります。</p>

<p style="margin-left:11%;"><b>&minus;I</b> <i>string</i>,
<b>&minus;&minus;intf2</b>=<i>string</i></p>

<p style="margin-left:22%;">
サーバから、クライアント/TX/セカンダリに対して送信する
インターフェイスを指定します。
このオプションは 1回だけ指定できます。</p>

<p style="margin-left:22%; margin-top: 1em">
tcpprep コマンドで &rsquo;secondary&rsquo; とマークされたトラフィックを
送信するインターフェイスです。
セカンダリトラフィックは、通常はサーバからクライアントへの、
あるいは仮想インターフェイスの outbound(TX) になります。</p>
一般的には、
このオプションは &minus;-cachefile と一緒に使用しないと意味がありません</p>

<p style="margin-left:11%;"><b>&minus;&minus;listnics</b></p>

<p style="margin-left:22%;">
使用できる NIC のリストを表示して終了します。</p>

<p style="margin-left:11%;"><b>&minus;l</b> <i>number</i>,
<b>&minus;&minus;loop</b>=<i>number</i></p>

<p style="margin-left:22%;">
キャプチャファイルを X回ループします。
このオプションは 1回だけ指定できます。</p>
オプションには整数を指定します。
<i>number</i> の指定には下記の制限があります:</p>

<p style="margin-left:28%;">0 以上</p>

<p style="margin-left:22%;"><i>number</i> のデフォルト値は:<br>
1</p>


<p style="margin-left:11%;"><b>&minus;&minus;pktlen</b></p>

<p style="margin-left:22%;">
snaplen を上書きして、実際のパケット長を使用します。
このオプションは 1回だけ指定できます。</p>

<p style="margin-left:22%; margin-top: 1em">
デフォルトの動作では、tcpreplay は 
pcap ファイルに保存されている &quot;snaplen&quot; をベースに
パケットを送信します。この値はたいていの場合は正しく動作します。
しかし、時折、
その値より大きなサイズのパケットが保存されていることもあります。
このオプションを指定することで、
pcap ファイルに保存されている &quot;snaplen&quot; を無視して、
オリジナルのパケット長をベースに送信するようになります。
このオプションを使用すると、想定外の事象が発生するかもしれません。</p>

<p style="margin-left:11%;"><b>&minus;L</b> <i>number</i>,
<b>&minus;&minus;limit</b>=<i>number</i></p>

<p style="margin-left:22%;">
パケットの送信数を制限します。
このオプションは 1回だけ指定できます。
オプションには整数を指定します。
<i>number</i> の値は下記が想定されています:</p>

<p style="margin-left:28%;">1 以上</p>

<p style="margin-left:22%;"><i>number</i> のデフォルト値は:<br>
-1</p>

<p style="margin-left:22%; margin-top: 1em">
tcpreplay は、デフォルトでは全てのパケットを送信します。
このオプションを指定することで、送信パケット数を制限できます。</p>

<p style="margin-left:11%;"><b>&minus;x</b> <i>string</i>,
<b>&minus;&minus;multiplier</b>=<i>string</i></p>

<p style="margin-left:22%;">
再送信速度を n倍速で指定できます。
このオプションは 1回だけ指定できます。
オプションには整数を指定します。
このオプションは下記のオプションと一緒には使用できません。
pps, mbps, oneatatime, topspeed</p>

<p style="margin-left:22%; margin-top: 1em">
パケットの再送信速度を指定します。指定例:<br>
2.0 と指定するとキャプチャされた速度の二倍速で送信します <br>
0.7 と指定するとキャプチャされた速度の 0.7倍速で送信します</p>

<p style="margin-left:11%;"><b>&minus;p</b> <i>number</i>,
<b>&minus;&minus;pps</b>=<i>number</i></p>

<p style="margin-left:22%;">
1秒あたりのパケット数(packets/sec)を指定してパケットを送信します。
このオプションは 1回だけ指定できます。
このオプションは下記のオプションと一緒には使用できません。</p>
multiplier, mbps, oneatatime, topspeed. 
オプションには整数を指定します。</p>

<p style="margin-left:11%;"><b>&minus;M</b> <i>string</i>,
<b>&minus;&minus;mbps</b>=<i>string</i></p>

<p style="margin-left:22%;">
指定された Mbps でパケットを送信します。
このオプションは 1回だけ指定できます。
このオプションは下記のオプションと一緒には使用できません。
multiplier, pps, oneatatime, topspeed.</p>

<p style="margin-left:22%; margin-top: 1em">
tcpreplay が送信する速度(Mbps) を小数点で指定します。</p>

<p style="margin-left:11%;"><b>&minus;t</b>,
<b>-&minus;topspeed</b></p>

<p style="margin-left:22%;">
できる限り速い速度で送信します。
このオプションは下記のオプションと一緒には使用できません。
mbps, multiplier, pps, oneatatime.</p>

<p style="margin-left:11%;"><b>&minus;o</b>,
<b>-&minus;oneatatime</b></p>

<p style="margin-left:22%;">
ユーザからの入力があるたびに 1つパケットを送信します。
このオプションは下記のオプションと一緒には使用できません。
mbps, pps, multiplier, topspeed.</p>

<p style="margin-left:22%; margin-top: 1em">
ステップごとに、1パケットあるいは複数パケットを送信できます。</p>

<p style="margin-left:11%;"><b>&minus;&minus;pps&minus;multi</b>=<i>number</i></p>

<p style="margin-left:22%;">
(pps で指定した)時間間隔ごとに、
パケットを何発送信するかを指定します。
このオプションは下記のオプションと一緒に使用します: pps.
オプションには整数を指定します。
<i>number</i> の値は下記が想定されています:</p>

<p style="margin-left:28%;">1 以上</p>

<p style="margin-left:22%;"><i>number</i> のデフォルト値は:<br>
1</p>

<p style="margin-left:22%; margin-top: 1em">
高速にパケットを送信しようとすると、
送信するパケットの間隔は非常に短くなり、
パケットの間隔の sleep 時間の精度を保つのが困難になります。
このオプションを指定することで、
一度に(1回の sleep の間隔に)複数パケットを送信できるようになり、
パケットの間隔の sleep 時間を長くでき高い精度で速度を指定できます。</p>

<p style="margin-left:11%;"><b>&minus;&minus;unique&minus;ip</b></p>

<p style="margin-left:22%;">
個別の flow を作成するために、
ループごとに異なる IPアドレスで送信します。
このオプションは下記のオプションと一緒に使用します: loop </p>

<p style="margin-left:22%; margin-top: 1em">
IPv4 と IPv6 のパケットにおいて、<b>--loop</b> ごとに、
個別の IPアドレスで送信することを保障します。
パケットごとの CRC を変更しない仕組みを使うので、
パフォーマンスには影響しません。
このオプションは、ループごとの flows/sec をかなり向上させます。</p>

<p style="margin-left:11%;"><b>&minus;&minus;netmap</b></p>

<p style="margin-left:22%;">
netmap が有効化された NIC に直接パケットを書き込みます。</p>

<p style="margin-left:22%; margin-top: 1em">
この機能は、Linux と BSD において、
netmap が有効になっている NIC のドライバを検出します。
検出された場合には、
パケットを送信している間は(通常の)ネットワークドライバはバイパスされ、
ネットワークバッファは直接 netmap の NIC に書き込まれます。
netmap を使うことで、市販されている一般的な NIC でも、
商用のネットワークジェネレータと同程度の速度を実現できます。
通常のネットワークドライバはバイパスされてしまうので、
その NIC を使ったほかのアプリケーションは通信できなくなります。
詳細情報は(付属の) INSTALL を確認してください。</p>

<p style="margin-left:11%;"><b>&minus;&minus;no&minus;flow&minus;stats</b></p>

<p style="margin-left:22%;">
flow の数や通信レートや満了時間などを、表示せず追跡しません。</p>

<p style="margin-left:22%; margin-top: 1em">
flow の統計情報を収集したり表示するのを抑制します。
このオプションは、
<b>--preload-pcap</b> を使わない場合にパフォーマンスを向上できます。
flow の機能は、送信した flow の統計情報を収集し表示します。
flow はおおまかに 5つの要素で判別されます。すなわち、
送信元IPアドレス、送信先IPアドレス、
送信元port番号、送信先port番号、プロトコルの 5つです。
もし、<b>--loop</b> を使う場合、
<b>--unique-ip</b> が指定されていたり、
<b>tcpreplay-edit</b> でループごとにパケットが書き換えられていない限り、
ループごとに異なる flow にはなりません。</p>

<p style="margin-left:11%;"><b>&minus;&minus;flow&minus;expiry</b>=<i>number</i></p>

<p style="margin-left:22%;">
flow が終了すると判断されるまでの無通信時間を指定します。
このオプションは下記のオプションと一緒には使用できません: no-flow-stats
オプションには整数を指定します。
<i>number</i> の値は下記が想定されています:</p>

<p style="margin-left:28%;">0 以上</p>

<p style="margin-left:22%;"><i>number</i> のデフォルト値は: <br>
0</p>

<p style="margin-left:22%; margin-top: 1em">
このオプションは、無通信時間をベースにして、
flow の終了を追跡したりレポートします。
pcap ファイルに含まれるタイムスタンプは、
flow の終了を決定するために用いられます。
パケットが送信された実際のタイムスタンプは関係しません。
例えば 30 が指定された場合、
30秒間その flow の通信が無かった場合、
その flow の後の通信は新規の flow と認識されます。
従って、単位時間あたりの flow 数(fps) の統計情報は上昇します。
このオプションは、
flow を取り扱う製品の flow のタイムアウトの設定を最適化するために使われます。
実際に flow の応答が遅くなって flow をドロップする場合には、
このタイムアウト時間を小さく設定します。
タイムアウト時間を大きくし過ぎると、
flow を取り扱う製品のリソースの上昇につながります。
キャプチャ時よりも速い速度で再送信する時にこのオプションを使うと、
flow や fps の数をつり上げることになるので注意してください。
デフォルト値は 0(終了しない)で、一般的な値は 30～120秒程度です。</p>

<p style="margin-left:11%;"><b>&minus;P</b>,
<b>-&minus;pid</b></p>

<p style="margin-left:22%;">tcpreplay を実行した時のプロセス番号(PID)を表示します。</p>

<p style="margin-left:11%;"><b>&minus;&minus;stats</b>=<i>number</i></p>

<p style="margin-left:22%;">
X 秒ごとに統計情報を表示します。
オプションには整数を指定します。
<i>number</i> の値は下記が想定されています:</p>

<p style="margin-left:28%;">1 以上</p>

<p style="margin-left:22%; margin-top: 1em">
それほど厳密でなく &quot;best effort&quot; で表示されます。
パケット送信間隔が長い場合は、統計情報の表示も間隔が長くなります。</p>

<p style="margin-left:11%;"><b>&minus;V</b>,
<b>-&minus;version</b></p>

<p style="margin-left:22%;">バージョン情報を表示します。</p>

<p style="margin-left:11%;"><b>&minus;h</b>,
<b>-&minus;less&minus;help</b></p>

<p style="margin-left:22%;">簡単な使用方法を表示して終了します。</p>

<p style="margin-left:11%;"><b>&minus;H</b>,
<b>&minus;&minus;help</b></p>

<p style="margin-left:22%;">使用方法を表示して終了します。</p>

<p style="margin-left:11%;"><b>&minus;!</b>,
<b>&minus;&minus;more-help</b></p>

<p style="margin-left:22%;">ページャーで拡張使用方法を表示します。</p>

<p style="margin-left:11%;"><b>&minus;</b> [<i>rcfile</i>],
<b>&minus;&minus;save-opts</b>[=<i>rcfile</i>]</p>

<p style="margin-left:22%;">オプションの情報を <i>rcfile</i> に保存します。
デフォルトでは後述される <b>オプションの事前設定／OPTION PRESETS</b> の
最後の(<i>last</i>) 設定になります。</p>

<p style="margin-left:11%;"><b>&minus;</b> <i>rcfile</i>,
<b>&minus;&minus;load-opts</b>=<i>rcfile</i>,
<b>&minus;&minus;no-load-opts</b></p>

<p style="margin-left:22%;">オプションの情報を <i>rcfile</i> から読み込みます。
<i>no-load-opts</i> オプションを指定すると、
以前に読み込んだ RC/INI ファイルを無効化できます。
<i>&minus;&minus;no-load-opts</i> の指定は最初に読み込まれ、指定する順番は無関係です。</p>

<h2>オプションの事前設定／OPTION PRESETS
<a name="OPTION PRESETS"></a>
</h2>


<p style="margin-left:11%; margin-top: 1em">
事前設定不可(<i>not presettable</i>) となっているオプション以外は、
コンフィグレーションファイル
(&quot;RC&quot; または &quot;.INI&quot; ファイル) から読み出して
事前にセットできます。
<i>homerc</i> ファイルは、
それがディレクトリでなければ &quot;<i>$$/</i>&quot; になり、
この場合はディレクトリ内の &quot;<i>.tcpreplayrc</i>&quot; が検索されます。</p>

<h2>ファイル／FILES
<a name="FILES"></a>
</h2>


<p style="margin-left:11%; margin-top: 1em">
コンフィグファイルは <b>OPTION PRESETS</b> を参照してください。</p>

<h2>exit コード／EXIT STATUS
<a name="EXIT STATUS"></a>
</h2>


<p style="margin-left:11%; margin-top: 1em">
下記の 1つが返ります:<b><br>
0</b> (EXIT_SUCCESS)</p>

<p style="margin-left:22%;">実行成功</p>

<p style="margin-left:11%;"><b>1</b> (EXIT_FAILURE)</p>

<p style="margin-left:22%;">実行失敗または文法エラー</p>

<p style="margin-left:11%;"><b>66</b> (EX_NOINPUT)</p>

<p style="margin-left:22%;">指定された設定ファイルの読み込み失敗</p>

<p style="margin-left:11%;"><b>70</b> (EX_SOFTWARE)</p>

<p style="margin-left:22%;">libopts の内部エラー．
autogen-users@lists.sourceforge.net に報告してください。</p>


<h2>AUTHORS／AUTHORS
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


<p style="margin-left:11%; margin-top: 1em">
バグは tcpreplay-users@lists.sourceforge.net に報告してください。</p>

<h2>NOTES
<a name="NOTES"></a>
</h2>


<p style="margin-left:11%; margin-top: 1em">This manual
page was <i>AutoGen</i>-erated from the <b>tcpreplay</b>
option definitions.</p>
<hr>
</body>
</html>
