---
layout: content
title:  "Tcpreplay Overview"
categories: tcpreplay wiki
description: "Tcpreplay is a suite of utilities for editing and replaying network traffic which was previously captured by tools like tcpdump and Wireshark"
---

- [Overview](#overview)
- [Examples](#examples)
- [Screencast Videos](#screencast-videos)
- [Useful Links](#useful-links)

<h2 id="Overview"><a name="overview"></a>Overview</h2>
Tcpreplay is a suite of [GPLv3] licensed utilities for UNIX (and Win32 under
[Cygwin]) operating systems for editing and replaying network traffic which
was previously captured by tools like [tcpdump] and [Wireshark]. 
It allows you to classify traffic as client or server, rewrite Layer 2, 3 and 4 
packets and finally replay the traffic back onto the network and through other
devices such as switches, routers, firewalls, NIDS and IPS's. Tcpreplay supports
both single and dual NIC modes for testing both sniffing and in-line devices.

Tcpreplay is used by numerous firewall, IDS, IPS, NetFlow and other networking
vendors, enterprises, universities, labs and open source projects. If your
organization uses Tcpreplay, please let us know who you are and what you use
it for so that we can continue to add features which are useful.

Tcpreplay is designed to work with network hardware and normally does not
penetrate deeper than Layer 2. Yazan Siam with sponsorship from [Cisco] developed
*tcpliveplay* to replay TCP pcap files directly to servers. Use this utility
if you want to test the entire network stack and into the application.

As of version 4.0, Tcpreplay has been enhanced to address the complexities of
testing and tuning [IP Flow][flow]/[NetFlow] hardware. Enhancements include:

* Support for [netmap] modified network drivers for 10GigE wire-speed performance
* Increased accuracy for playback speed
* Increased accuracy of results reporting
* Flow statistics including Flows Per Second (fps)
* Flow analysis for analysis and fine tuning of flow expiry timeouts
* Hundreds of thousands of flows per second (dependent on flow sizes in pcap file) 

Version 4.0 is the first version delivered by Fred Klassen and sponsored by 
[AppNeta]. Many thanks to the author of Tcpreplay, Aaron Turner who has supplied
the world with a a solid and full-featured test product this far. The new author
strives to take Tcpreplay performance to levels normally only seen in commercial
network test equipment.

<h2 id="Examples"><a name="examples"></a>Examples</h2>
The following are examples of *tcpreplay* being used to generated various
patterns of traffic to a *IP Flow* appliance:

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

## <a name="screencast-videos"></a>Screencast Videos
A playlist of Tcpreplay screencasts is available [here][playlist], or you can watch all screencasts below.

<iframe width="620" height="460" 
     src="//www.youtube.com/embed/videoseries?list=PLFjjcN5EvTP0Hsxq1AEh7FhvaFH__Vd83" 
     frameborder="0" 
     allowfullscreen>
</iframe>

## <a name="useful-links"></a>Useful Links

* [Download and installation instructions][installation]
* [Support]
* [Tcpreplay Users Mailing List][maillist]
* Tcpreplay on [Twitter]
* [Live chat]
* [FAQ]
* [How To]
* [Sample Captures]
* [The history of Tcpreplay][history]
* [Screencast Videos][playlist]
* Legacy
    * [Version 3.x wiki][legacywiki]
    * [Tcpreplay Blog][legacyblog]
    * [RSS Feed][rss]
    * [Podcasts]
    


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
