---
layout: content
title:  "FAQ"
categories: tcpreplay wiki
description: "Frequently Asked Questions"
---


- 一般的な質問／General Questions
	- [どうして大文字の Tcpreplay なの？／How should Tcpreplay be capitalized?](#how-should-tcpreplay-be-capitalized)
	- [Tcpreplay の入手方法／Where do I get Tcpreplay?](#where-do-i-get-tcpreplay)
	- [Tcpreplay のインストール方法／How do I install Tcpreplay?](#how-do-i-install-tcpreplay)
	- [Windows 版はありますか？／Is there a Microsoft Windows port?  ](#is-there-a-microsoft-windows-port)
	- [Tcpreplay のライセンスは？／How is Tcpreplay licensed?](#how-is-tcpreplay-licensed)
- Tcpreplay の実行／Running Tcpreplay
	- [tcpreplay はサーバにトラフィックを送信できますか？／Does tcpreplay support sending traffic to a server?  ](#does-tcpreplay-support-sending-traffic-to-a-server)
	- [なぜ Tcpreplay は指定した速度でトラフィックを送信できないのですか？／Why doesn't Tcpreplay send traffic as fast as I told it to?](#why-doesnt-tcpreplay-send-traffic-as-fast-as-i-told-it-to)
	- [tcpreplay を実行しているマシンでパケットを送出できますか？／Can I send packets on the same computer running tcpreplay?](#can-i-send-packets-on-the-same-computer-running-tcpreplay)
	- [古いバージョンの tcpreplay はパケットを書き換えできたのに(今はできません)／Older versions of tcpreplay allowed me to edit packets. What happened?](#older-versions-of-tcpreplay-allowed-me-to-edit-packets-what-happened)
	- [tcpprep でキャッシュファイルを作る必要性は？ tcprewrite だけでは動かない？／Why do I need to use tcpprep to create cache files? Can't this be done in tcprewrite?](#why-do-i-need-to-use-tcpprep-to-create-cache-files-can't-this-be-done-in-tcprewrite)
	- [tcpreplay が全てのパケットを送出しない理由は？／Why is tcpreplay not sending all the packets?](#why-is-tcpreplay-not-sending-all-the-packets)
	- [tcpreplay の送信タイミングがめちゃくちゃなのはなぜ？／Why are tcpreplay timings all messed up?](#why-are-tcpreplay-timings-all-messed-up)
	- [tcpreplay は複数のネットワークカード(NIC)で使えますか？(Tomahawk (というアプリケーション)のように)／Does tcpreplay support dual NIC's like Tomahawk?](#does-tcpreplay-support-dual-nic's-like-tomahawk)
	- [tcpreplay は gzip/bzip2 圧縮されたファイルを読み込めますか？／Can tcpreplay read gzip/bzip2 compressed files?](#can-tcpreplay-read-gzipbzip2-compressed-files)
	- [tcpreplay はどのくらいの速度でパケットを送出できますか？／How fast can tcpreplay send packets?](#how-fast-can-tcpreplay-send-packets)
	- [どうやったらもっと速くパケットを送出できますか？／How can I make tcpreplay run even faster?](#how-can-i-make-tcpreplay-run-even-faster)
	- [tcpreplay は Endace DAG カードで使えますか？／Does tcpreplay support Endace DAG cards?](#does-tcpreplay-support-endace-dag-cards)
	- [pcap ファイル以外のキャプチャファイルを使えますか？／Can I use non-pcap capture files?](#can-i-use-non-pcap-capture-files)
	- [Tcpreplay は Pcap-Ng/NTAR ファイルを読み込めますか？／Does Tcpreplay support Pcap-Ng/NTAR files?](#does-tcpreplay-support-pcap-ngntar-files)
	- [tcpreplay は Wi-Fi からパケットを送出できますか？／Can tcpreplay send packets over WiFi?](#can-tcpreplay-send-packets-over-wifi)
	- [loopback インターフェイスから送出したパケットが見えないのはなぜ？／Why doesn't my application see packets replayed over loopback?](#why-doesnt-my-application-see-packets-replayed-over-loopback)
	- [iptables などでトラフィックコントロールできますか？／Can I use IPTables/Traffic Control with tcpreplay?](#can-i-use-iptablestraffic-control-with-tcpreplay)
- Tcpreplay のコンパイル／Compiling Tcpreplay
	- [XXX 用のバイナリファイルはありますか？／Are there binaries available for XXX operating system?](#are-there-binaries-available-for-xxx-operating-system)
	- [お願いしたらバイナリを作ってくれますか？／What if I ask you really nicely to build a binary for me?](#what-if-i-ask-you-really-nicely-to-build-a-binary-for-me)
	- [パケットを送出するための方法が見つかりません。libpcap をバージョンアップするか libdnet を有効化してください／Unable to find a supported method to send packets.  Please upgrade your libpcap or enable libdnet  ](#unable-to-find-a-supported-method-to-send-packets--please-upgrade-your-libpcap-or-enable-libdnet)
	- [tcpedit_stub.def: Command not found   ](#tcpedit_stubdef-command-not-found)
	- [tcpreplay_opts.h:72:3: error: #error option template version mismatches autoopts/options.h header](#tcpreplay_optsh723-error-#error-option-template-version-mismatches-autooptsoptionsh-header)
	- [Issues with autogen/libopts](#issues-with-autogenlibopts)
	- [Problems with linking under recent Fedora Core/RedHat](#problems-with-linking-under-recent-fedora-coreredhat)
- Common Errors
	- [Unable to send packet: Error with pcap_inject(packet #10): send: Message too long](#unable-to-send-packet-error-with-pcap_injectpacket-#10-send-message-too-long)
	- [Can't open eth0: libnet\_select\_device(): Can't find interface eth0](#can't-open-eth0-libnet_select_device-can't-find-interface-eth0)
	- [Can't open eth0: UID != 0](#can't-open-eth0-uid-0)
	- [100000 write attempts failed from full buffers and were repeated](#100000-write-attempts-failed-from-full-buffers-and-were-repeated)
	- [Unable to process test.cache: cache file version mismatch](#unable-to-process-testcache-cache-file-version-mismatch)
	- [Skipping SLL loopback packet](#skipping-sll-loopback-packet)
	- [Packet length (8892) is greater then MTU; skipping packet](#packet-length-8892-is-greater-then-mtu-skipping-packet)
	- [tcpreplay doesn't send entire packet/tcprewrite truncates packets](#tcpreplay-doesn't-send-entire-packettcprewrite-truncates-packets)
	- [tcpreplay is sending packets out of order](#tcpreplay-is-sending-packets-out-of-order)
- Use Cases for Tcpreplay
	- [External Use Cases](#external-use-cases)
	- [Other Documents](#other-documents)


General Questions
=================

<h2 id="FAQ"><a name="how-should-tcpreplay-be-capitalized">Q:</a> How should Tcpreplay be capitalized?</h2>
When refering to the Tcpreplay suite of tools (tcpreplay, tcpprep, tcprewrite, etc)
then the 'T' is capitalized. When only referring to the tcpreplay standalone utility,
then it's not. There is no such thing as 'TCPreplay' or 'TCP replay'.


<h2><a name="where-do-i-get-tcpreplay">Q:</a> Where do I get Tcpreplay?</h2>
Visit our [Download and Installation][installation] page.


<h2><a name="how-do-i-install-tcpreplay">Q:</a> How do I install Tcpreplay?</h2>
You must compile. Refer to [Download and Installation][installation] page.

<h2><a name="is-there-a-microsoft-windows-port">Q:</a> Is there a Microsoft Windows port?</h2>  
Windows 2000 and better are supported under Cygwin. 
In the near future expect native support for Windows (volunteers please). For more information, 
please read the Win32Readme.txt file.


<h2><a name="how-is-tcpreplay-licensed">Q:</a> How is Tcpreplay licensed?</h2>
Tcpreplay is licensed under [GPLv3][gplv3]. For details see the docs/LICENSE file included 
with the source code.

Running Tcpreplay
=================

<h2><a name="does-tcpreplay-support-sending-traffic-to-a-server">Q:</a> Does tcpreplay support sending traffic to a server?</h2>  
If by server you mean a daemon (Unix) or service (Windows) which listens on a port
(a web or mail server would be common examples), then try *tcpliveplay*.
Other Tcpreplay products don't understand the state of 
common protocols like TCP. This means it is unable to synchronize Syn/Ack's to 
create valid TCP sessions. *tcpliveplay* will read a single TCP stream pcap file,
establish a connection to a server then play the contents of the pcap file to
the server.

For ICMP and UDP based protocols *tcpliveplay* will not work, 
however other replay products should work fine
as long as the MAC and IP addresses are set correctly. 
You can edit them using *tcprewrite* or *tcpreplay-edit*.
Be aware that some ICMP and UDP based protocols also carry Layer 3/4 protocol 
information in the protocol itself (SIP is one example), so if you change the 
IP addresses, it may not be a valid SIP packet anymore. In such cases, 
you may want to try using *NetDude* to edit the payload of the packets.


<h2><a name="why-doesnt-tcpreplay-send-traffic-as-fast-as-i-told-it-to">Q:</a> Why doesn't Tcpreplay send traffic as fast as I told it to?</h2>
Try using [netmap][nm] drivers and the `--netmap` option.

Usually this occurs when trying a pcap of small packets.
For example, let's look at a pcap file with an average of 646 byte packets and
we attempt to send at 9 Gbps...

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

... and compare that with a similar test using a 77 byte average packet size.


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

Notice that with smaller packet *pcap* files the *packets per second (pps)* and the 
*flows per second (fps)* rates are much higher which in turn affects the total *Mbps* rate. 

Here are some tricks/ideas you can use to make *tcpreplay* send traffic as fast as you wanted:
   
* Make sure you're running the latest stable release of Tcpreplay.  
* Try installing [netmap][nm] network drivers and using the `--netmap` option   
* Use `--preload-pcap` to load the pcap file into RAM before sending.   
* Try a different timing mechanism. `--timer=gtod` or `--timer=nano` for example. OS X users should always use `--timer=abstime`   
* Use `--mbps` in favor of `--pps` option.   
* If you use `--pps` also use `--pps-multi=X*` to cause *tcpreplay* send multiple packets each sleep cycle   
* Use `--topspeed` or `--mbps=0`. This is always the fastest way to send packets.   
* Make sure your network connection is fast enough. Don't try sending 50Mbps over a 10Mbps link for example.   
* Often using a combination of the above options will allow you to reach the speeds you're trying to hit.   


<h2><a name="can-i-send-packets-on-the-same-computer-running-tcpreplay">Q:</a> Can I send packets on the same computer running tcpreplay?</h2>
Generally speaking no. When *tcpreplay* sends packets, it injects 
them between the TCP/IP stack of the system and the device driver of the network card. 
The result is the TCP/IP stack system running *tcpreplay* never sees the packets.

One suggestion that has been made is using something like VMWare, Parallels or Xen. 
Running *tcpreplay* in the virtual machine (guest) would allow packets to be seen by 
the host operating system.


<h2><a name="older-versions-of-tcpreplay-allowed-me-to-edit-packets-what-happened">Q:</a> Older versions of tcpreplay allowed me to edit packets. What happened?</h2>
Use *tcpreplay-edit* or *tcprewrite*.


<h2><a name="why-do-i-need-to-use-tcpprep-to-create-cache-files-can't-this-be-done-in-tcprewrite">Q:</a> Why do I need to use tcpprep to create cache files? Can't this be done in tcprewrite?</h2>
Most of the *tcpprep* modes take a holistic view of a pcap and look at all the packets 
before deciding which IP's are servers and which are clients. This requires the pcap file 
to be processed fully, sometimes twice. This for all practicality disallows including the 
*tcpprep* logic in *tcpreplay*.

Secondly, since *tcpreplay*, *tcpreplay-edit* and *tcprewrite* all use *tcpprep* cache file data, 
it made sense to separate it into a standalone utility so that the processing can 
be leveraged across multiple rewrites and the actual packet sending.


<h2><a name="why-is-tcpreplay-not-sending-all-the-packets">Q:</a> Why is tcpreplay not sending all the packets?</h2>
Every now and then, someone emails the tcpreplay-users list, asking if there is a bug in tcpreplay 
which causes it not to send all the packets. This usually happens when the using the `-t` 
option, which may send packets faster than the system can handle. In this case, the network adapter 
receives the packet but is unable to send it. This problem is highly hardware dependent.

It is important to understand that if the network socket is indicating that it is congested, tcpreplay will wait
until its buffers become available before moving on to the next packet. If packets are lost, it happens after
the network has accepted the packet.

The longer version goes something like this:

If you are running tcpreplay multiple times and are using *tcpdump* or other packet 
sniffer to count the number of packets sent and are getting different numbers, it's not *tcpreplay's* fault. Try investigating:

1. It is well known that *tcpdump* and other sniffers have a problem keeping up with high-speed traffic. 
Furthermore, the OS in many cases lies about how many packets were dropped. *Tcpdump* will repeat this lie to you. In other words, 
*tcpdump* isn't seeing all the packets. Usually this is a problem with the network card, driver or OS kernel 
which may or may not be fixable. Try another network card/driver.   
2. When *tcpreplay* sends a packet, it actually gets copied to a send buffer in the kernel. 
If this buffer is full, the kernel is supposed to tell tcpreplay that it didn't copy the packet to this buffer. 
If the kernel has a bug which squelches this error, tcpreplay will not keep retrying to send the packet and will 
move on to the next one. Currently we are not aware of any OS kernels with this bug, but it is possible that 
it exists. If you find out that your OS has this problem, please let us know so we can list it here.   
3. Tcpreplay can't send packets which are larger than the MTU of the interface. Generally, you 
can increase Ethernet MTU beyond the default 1500 bytes by using the ifconfig utility, but it is not recommended
that you do so in production. MTU mismatch bugs are very illusive, and make networks run about 1/6th normal speeds.   
4. We've been informed by one user that having NIC hardware checksum enabled causes tools like *tcpdump* to drop packets 
even at very low rates (1Mbps). 
Disabling this feature caused the NIC to stop dropping packets. This is not a bug in *tcpreplay*.


<h2><a name="why-are-tcpreplay-timings-all-messed-up">Q:</a> Why are tcpreplay timings all messed up?</h2>
Occasionally someone complains about timings to be "messed up". Usually this seems to
be caused by a pcap which contains packets with non-sensical timestamps. 
More specifically, we have seen cases where a packet has a timestamp before 
the previous packet in the capture file. This usually happens on network adapters that timestamp 
in hardware, but then split traffic into multiple queues. They are presented to the capture software
in a different order than they arrived.


<h2><a name="does-tcpreplay-support-dual-nic's-like-tomahawk">Q:</a> Does tcpreplay support dual NIC's like Tomahawk?</h2>
Yes! *tcpreplay* has for many years supported high-speed playback using two interfaces
(long before [Tomahawk][tomahawk] existed). For more information about how traffic is split between
the two interfaces, see the *tcpprep* manpage.


<h2><a name="can-tcpreplay-read-gzipbzip2-compressed-files">Q:</a> Can tcpreplay read gzip/bzip2 compressed files?</h2>
Yes, but not directly. Since *tcpreplay* can read data via STDIN, you can decompress the file on the fly like this:

```
gzcat myfile.pcap.gz | tcpreplay -i eth0 -
```

Note that decompressing on the fly will require additional CPU time and will likely reduce the overall 
performance of *tcpreplay*.


<h2><a name="how-fast-can-tcpreplay-send-packets">Q:</a> How fast can tcpreplay send packets?</h2>
First, if performance is important to you, then upgrading to tcpreplay 4.x is worthwhile since it is more optimized then
3.x series. After that, there are a number of variables which effect performance, 
including how you measure it (packets/sec or bytes/sec).

Performance will depend on options selected, pcap file and hardware. Adding [netmap][nm] patches to your network 
adapters will dramatically
increase performance, but be careful not to shoot yourself in the foot. When using the `--netmap` the network
driver is bypassed for the duration of the test.

Here is an example of *tcpreplay* on an i7 processor with an Intel 82599 
10GigE NIC. With the `--netmap` option and this pcap file we achieve near wire 
speed and 157K flows/sec.

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

**Note** that the above example is closer to wire speed than it first appears. Average packet size
for this pcap file is (4608265500 ÷ 7130500) = 646 bytes. 10GigE Ethernet will add an 
additional 17 bytes for IFG, preamble, SOF and FCS on the wire, which makes the average frame 
size 663. On wire, speed is (663 ÷ 646) × 9582.85 = 9836 Mbps. Finally,
the manufacturer of this adapter does not claim 100% wire rate because it is front-ended by 
a hardware timestamp feature. You may achieve 100%.

In some cases we have seen performance
higher than the adverstised clock rate (e.g. 1048 Mbps on GigE). You may achieve 100%
with your adapter and maybe even more.

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


<h2><a name="how-can-i-make-tcpreplay-run-even-faster">Q:</a> How can I make tcpreplay run even faster?</h2>
Profiling tcpreplay has shown that a significant amount of time is spent writing packets to the network. Hence, your OS kernel 
implementation of writing to raw sockets is one of the most important aspects since that is where tcpreplay spends most of it's time.

In no particular order:

* Hardware:   
 1. Use a fast enough hardware. *tcpreplay* your network card and CPU.
 2. Ensure that your system has the fastest memory possible. This is almost as important as selecting a fast CPU.
 3. Since tcpreplay is not multi-threaded, SMP or dual-core processors won't help very much.
 However, other processes can run on the other CPU(s).  
 4. Turn off hyperthreading (HT) if your CPU supports it.
 5. A good network card/driver is important. For 10GigE we have seen good results with Intel 82599 adapters.
 6. Motherboard configuration is important. Sometimes if you see 4 or 6 network adapters on a motherboard, you can be assured
 that they are connected to the Southbridge. That's a shame because the Southbridge is reserved for slower devices such
 as USB, serial ports, etc. To optimize speed, plug your adapter into a PCI slot that is connected to the faster
 Northbridge.
 7. Make sure that your PCI bus has enough lanes for your adapter. PCI Express lanes handle 200MB/s of traffic in each direction. For
 10GigE you should plug into an eight lane (x8) PCI Express slot.
 8. You may find this article from [Mark Wagner][mark_wagner] helpful. It is especially helpful in controlling CPU affinity for
 10GigE interrupts. We have seen great performance boosts from recommendations in this paper.
 
* Configuration:
 1. If you have enough RAM to store the pcap files in a RAM disk with the `--preload-pcap` option.
 2. If you don't have enough RAM for a RAM disk, run *tcpreplay* against the file once in order to 
 load parts of your file in your OS's disk cache. That will help improve performance on the subsequent runs.
 3. If your computer is busy running a bunch of other processes (running X, downloading mail, etc) 
 then it will impact *tcpreplay*'s performance and ability to time packets correctly.
 4. Opening a bunch of small files repeatly will reduce performance. Consider using [mergecap][mergcap]
 to generate a single large file.
 5. Like most network based I/O, it is faster to send the same amount of data in a few large packets then many small packets.
 6. On Linux, use a newer kernel version. Usually you want a kernel version higher than 2.6.18, which will 
 have [NAPI][napi] support, which has significantly enhanced I/O.


<h2><a name="does-tcpreplay-support-endace-dag-cards">Q:</a> Does tcpreplay support Endace DAG cards?</h2>
By default, *tcpreplay* does not support DAG cards. However, [Endace][endace] has released 
a custom version of tcpreplay which does support their cards. 
Please note that the Tcpreplay developers do not support this custom version of tcpreplay, 
so if you have any questions, please contact Endace.


<h2><a name="can-i-use-non-pcap-capture-files">Q:</a> Can I use non-pcap capture files?</h2>
It turns out that there are quite a few capture file formats other then pcap.
If you have a capture file created by a tool which uses one of these other formats (like Solaris snoop) 
you can convert it to pcap format by using [Wireshark's][wireshark] tshark tool.

```
tshark -r blah.snoop -w blah.pcap
```

<h2><a name="does-tcpreplay-support-pcap-ngntar-files">Q:</a> Does Tcpreplay support Pcap-Ng/NTAR files?</h2>
Yes. The Tcpreplay suite uses [libpcap][tcpdump] for reading and writing pcap files. 
If you have libpcap 1.1.0 or higher, then *tcpreplay*, *tcprewrite*, etc can read pcap-ng files. If you have an older version of libpcap, you should upgrade to the latest version as earlier versions of libpcap have bugs with pcap-ng files.


<h2><a name="can-tcpreplay-send-packets-over-wifi">Q:</a> Can tcpreplay send packets over WiFi?</h2>
This turns out to be very OS/hardware dependent, but in many cases, the answer is yes. 
In order for things to work, you generally must do the following:

* Put the WiFi card in managed mode   
* Your pcap files need to be DLT_EN10MB (Ethernet) and have a valid 803.2 header
* The source MAC of the packets need to match the MAC of your WiFi card


<h2><a name="why-doesnt-my-application-see-packets-replayed-over-loopback">Q:</a> Why doesn't my application see packets replayed over loopback?</h2>
Most users are surprised, when they try replaying UDP traffic over loopback, that the listening daemon 
never sees the traffic. This turns out to be a limitation of the loopback interface on many operating systems. 
One contributing factor may be capturing traffic on an Ethernet port, rewriting the IP addresses but not the L2 header. 
Since the loopback interface doesn't use an Ethernet L2 header, the IP stack of the
operating system is unable to parse the packet and deliver it to the listening daemon.


<h2><a name="can-i-use-iptablestraffic-control-with-tcpreplay">Q:</a> Can I use IPTables/Traffic Control with tcpreplay?</h2>
You cannot use iptables/tc on the same box as you run tcpreplay. 
The only way to use IPTables or Traffic Control (tc) with tcpreplay is to run tcpreplay on a different 
box and send the traffic *through* the system running iptables/tc. This limitation is due to how the Linux 
kernel injects frames vs. reading frames for iptables/tc which makes traffic sent via tcpreplay to be 
invisible to iptables/tc.


Compiling Tcpreplay
===================
  
<h2><a name="are-there-binaries-available-for-xxx-operating-system">Q:</a> Are there binaries available for XXX operating system?</h2>
Maybe. We do not release binaries for ANY operating system. 
Many operating systems like Linux, \*BSD, Solaris and OS X have teams which package open source applications 
like *tcpreplay* and release them in their package format (RPM, BSD/Mac ports, SunFreeware, etc).  


<h2><a name="what-if-i-ask-you-really-nicely-to-build-a-binary-for-me">Q:</a> What if I ask you really nicely to build a binary for me?</h2>
You can always ask, but we will probably ignore you.


<h2><a name="unable-to-find-a-supported-method-to-send-packets--please-upgrade-your-libpcap-or-enable-libdnet">Q:</a> Unable to find a supported method to send packets.  Please upgrade your libpcap or enable libdnet</h2>
Tcpreplay can use a variety of API's/libraries to send packets: BSD's BPF, Linux's PF\_PACKET, libpcap and libdnet.
If you're not running on a platform which supports BPF or PF\_PACKET, you'll need either a recent version of 
[libpcap][tcpdump] or [libdnet][libdnet]. If you are using libpcap we strongly suggest upgrading to the latest version
since it tends to have fewer bugs and is actively developed. 
Libdnet by contrast hasn't been updated in a few years but is more stable than older libnet libraries.

Right now there is one case where libdnet is necessary: 
you're not running on Linux or \*BSD and you want to use *tcpbridge*. 

Note: Tcpreplay no longer supports libnet!


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
You should only get this error if you're trying to build from [GitHub][github]. 
The problem is that you do not have [GNU Autogen][autogen] installed on your system.
Either install autogen or download one of the source tarballs.


<h2><a name="tcpreplay_optsh723-error-#error-option-template-version-mismatches-autooptsoptionsh-header">Q:</a> tcpreplay_opts.h:72:3: error: #error option template version mismatches autoopts/options.h header</h2>
You're building from [GitHub][github] and the version of Autogen/AutoOpts? installed on your system is
different from the 
version included in the libopts tearoff in the code. 
The result is that the src/*_opts.[ch] files generated by the system Autogen have a
version mismatch with the tearoff.

The solution is to use: `./configure --disable-local-libopts --disable-libopts-install`


<h2><a name="issues-with-autogenlibopts">Q:</a> Issues with autogen/libopts</h2>
The Tcpreplay suite uses the GNU Autogen/libops library. 
This makes development much easier, but not without some cost. Basically, Autogen/libopts 
doesn't always provide good backwards compatibility, 
so if you have a different version of Autogen/libopts than the author does, 
you may have problems compiling tcpreplay. One example error would be:

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
The good news is that there is an easy fix: `./configure --enable-local-libopts`


<h2><a name="problems-with-linking-under-recent-fedora-coreredhat">Q:</a> Problems with linking under recent Fedora Core/RedHat</h2>
Newer versions of Fedora Core and Red Hat ES/WS do not ship static libraries at all and only 
have dynamic libraries. If you wish to compile *tcpreplay* from source, you will need to pass the 
`--enable-dynamic-link` to configure in order for *tcpreplay* to link to them.


Common Errors
=============

<h2><a name="unable-to-send-packet-error-with-pcap_injectpacket-#10-send-message-too-long">Q:</a> Unable to send packet: Error with pcap_inject(packet #10): send: Message too long</h2>
There is a bug in OS X (at least up to 10.4.9) which causes problems for 
Ethernet frames > 1500 bytes from being sent when you spoof source MAC addresses (like tcpreplay/tcpbridge does).
Apple fixed this in 10.5 (Leopard). Currently there is no work around.


<h2><a name="can't-open-eth0-libnet_select_device-can't-find-interface-eth0">Q:</a> Can't open eth0: libnet_select_device(): Can't find interface eth0</h2>
Generally this occurs when the interface (eth0 in this example) is not up or 
doesn't have an IP address assigned to it.


<h2><a name="can't-open-eth0-uid-0">Q:</a> Can't open eth0: UID != 0</h2>
Tcpreplay requires that you run it as root.


<h2><a name="100000-write-attempts-failed-from-full-buffers-and-were-repeated">Q:</a> 100000 write attempts failed from full buffers and were repeated</h2>
When *tcpreplay* displays a message like "100000 write attempts failed from full buffers and were repeated", 
this usually means the kernel buffers were full and it had to wait until memory was available. 
This can occur when replaying files as fast as possible with the `-t` option. 
See the tuning OS section in this document for suggestions on solving this problem.


<h2><a name="unable-to-process-testcache-cache-file-version-mismatch">Q:</a> Unable to process test.cache: cache file version mismatch</h2>
Cache files generated by *tcpprep* and read by *tcpreplay* are versioned to 
allow enhancements to the cache file format.
Anytime the cache file format changes, the version is incremented. Since this occurs on a very rare basis, 
this is generally not an issue; however anytime there is a change, it breaks compatibility 
with previously created cache files. The solution for this problem is to use the same version of *tcpreplay*
and *tcpprep* to read/write the cache files. Cache file versions match the following versions of tcpprep/tcpreplay:

* Version 1:
 * Prior to 1.3.beta1
* Version 2: 1.3.beta2 to 1.3.1/1.4.beta1
* Version 3: 1.3.2/1.4.beta2 to 2.0.3
* Version 4: 2.1.0 and above. Note that prior to version 2.3.0, tcpprep had a 
bug which broke cache file compatibility between big and little endian systems.


<h2><a name="skipping-sll-loopback-packet">Q:</a> Skipping SLL loopback packet</h2>
Your capture file was created on Linux with the 'any' parameter which then captured a packet on the 
loopback interface. However, tcpreplay doesn't have enough information to actually send the packet,
so it skips it. Specifying a destination and source MAC address (-D and -S) will allow tcpreplay to send these packets.


<h2><a name="packet-length-8892-is-greater-then-mtu-skipping-packet">Q:</a> Packet length (8892) is greater then MTU; skipping packet</h2>
The packet length (in this case 8892 bytes) is greater then the maximum transmition unit (MTU) on the outgoing interface. 
Tcpreplay must skip the packet. Alternatively, you can specify the *tcpreplay-edit* `--mtu-trunc`
option - packets will be truncated to the MTU size, the checksums will be fixed and then sent. Note that this
may impact performance.


<h2><a name="tcpreplay-doesn't-send-entire-packettcprewrite-truncates-packets">Q:</a> tcpreplay doesn't send entire packet/tcprewrite truncates packets</h2>
You won't see any error from *tcpreplay*, but sometimes you'll open a pcap file in 
Ethereal/Wireshark? and notice that a packet is something like 400 bytes but tcpreplay says it only sent 100 bytes. 
We've seen this 2 or 3 times and in each case the reason was the same: the pcap file was broken. 
Apparently certain older versions of libpcap which shipped with Red Hat Linux had a bug.

If you suspect a corrupt pcap file, run *tcpcapinfo* to detect errors.

First a little background on the pcap file format. At the beginning of each file there is a file header which
contains, among other things, the *snaplen*. 
The snaplen is the maximum amount of data that is stored in file per packet; 
if a packet is larger than this value, the packet is truncated. 
Each packet is also prefixed with a packet header which contains two values: the *len* and *caplen*. 
The *len* is the original packet size and the *caplen* is the amount of actual data which was stored.

A properly formatted pcap file will never have a caplen > snaplen. Some tools and applications unfortunately
do not seem to enforce this. So when the libpcap library reads these files, it returns the snaplen as the actual
data available. The reason Ethereal/Wireshark? show the entire packet is because they do not use libpcap to 
read pcap files.

To fix the problem, locate the 2 bytes at offset 16 (0x10) and change them to read 0xFFFF. 
This will repair the file so libpcap/tcprewrite/tcpreplay/etc can process it correctly.

example of broken file:

```
xxd broken.pcap | head -3
0000000: d4c3 b2a1 0200 0400 0000 0000 0000 0000  ................
0000010: 6000 0000 0100 0000 1b6f 954b ca25 0e00  `........o.K.%..
0000020: 4a00 0000 4a00 0000 0000 5e00 0101 0015  J...J.....^.....
```

example of fixed file:

```
xxd fixed.pcap | head -3
0000000: d4c3 b2a1 0200 0400 0000 0000 0000 0000  ................
0000010: ffff 0000 0100 0000 1b6f 954b ca25 0e00  .........o.K.%..
0000020: 4a00 0000 4a00 0000 0000 5e00 0101 0015  J...J.....^.....
```


<h2><a name="tcpreplay-is-sending-packets-out-of-order">Q:</a> tcpreplay is sending packets out of order</h2>
This isn't a problem with tcpreplay, but rather with the receiving network card/driver. 
We've seen an example of Broadcom 10G network cards using "multi\_mode" preventing tcpdump/Wireshark 
from seeing the packets in the correct order. The solution is to configure the bnx2x driver with multi\_mode=0. 
Apparently this was turned on by default as of Linux kernel v2.6.24.

As stated earlier, this can also be caused by hardware timestamping network adapters.

Use Cases for Tcpreplay
======================= 

<h2><a name="external-use-cases">Q:</a> External Use Cases</h2>
* Renaud Bidou's paper [How to Test an IDS][howtoids] talks about using *tcpreplay*

<h2><a name="other-documents">Q:</a> Other Documents</h2>
* [RFC 2544][rfc2544] - Benchmarking Methodology for Network Interconnect Devices


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
