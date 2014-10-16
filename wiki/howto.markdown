---
layout: content
title:  "How To..."
categories: tcpreplay wiki
description: "How-to usage information"
---

- [Overview](#overview)
- [How to do a Performance Test for an IP Flow Appliance](#how-to-do-a-performance-test-for-an-ip-flow-appliance)
	- [Reducing Flows Per Second](#reducing-flows-per-second)
	- [Increasing Flows Per Second with netmap](#increasing-flows-per-second-with-netmap)
- [How to do a Performance Test for Packet Capture Device](#how-to-do-a-performance-test-for-a-packet-capture-device)

<h2 id="How To"><a name="overview"></a>Overview</h2>
This section is designed to get you up and running quickly and easily by supplying
step-by-step examples of various scenarios. 

The following how-to examples use one of provided [sample captures][captures].
Two identical machines are used, each with i7 processors and multi-port Intel 82599 10GigE 
adapters. The 10GigE adapters are connected back-to-back with patch cables. LAN connectivity with
SSH is via one of the GigE interfaces. GigE interfaces are numbered eth0 
through eth5. The 10GigE interfaces are eth6 and eth7.

Both systems are running Ubuntu 10.04 with a Linux version 3.1.10 kernel.

There are several screencast videos available that are related this section. The entire play list
of screencasts are available [here][playlist], or you can watch all screencasts below. If you are more
interested in detailed descriptions, skip the screencasts and proceed to the next section.

<iframe width="620" height="460" 
    src="//www.youtube.com/embed/videoseries?list=PLFjjcN5EvTP0Hsxq1AEh7FhvaFH__Vd83" 
    frameborder="0" 
    allowfullscreen>
</iframe>

## <a name="how-to-do-a-performance-test-for-an-ip-flow-appliance"></a>How to do a Performance Test for an IP Flow Appliance
Use this example if you need to test the performance of a device that 
captures and reports [IP Flow][flow]/[NetFlow] statistics.
In this example we are sending to a device under test (DUT) running
[nprobe]. We will use the *nprobe* logging features to measure how many
packets, bytes and flows per second are captured.
 
The ability to generate wire-rate traffic and extremely high flows-per-second
is new in Tcpreplay v4.0.0. The physical setup is very simple. In our experience 
Tcpreplay will greatly outperform the receiving station. Therefore it is 
important to track the amount of traffic sent and received, and determine
the amount of data lost.

* Download and install the latest release of [Tcpreplay][download] on the test machine
* Download [bigFlows.pcap] onto the test machine (see [captures] wiki for details)
* Download and install [nprobe] on the DUT
* Start nprobe on DUT ...

```
# nprobe -T "%IPV4_SRC_ADDR %IPV4_DST_ADDR %IPV4_NEXT_HOP %INPUT_SNMP \
%OUTPUT_SNMP %IN_PKTS %IN_BYTES %FIRST_SWITCHED %LAST_SWITCHED %L4_SRC_PORT \
%L4_DST_PORT %TCP_FLAGS %PROTOCOL %SRC_TOS %SRC_AS %DST_AS %IPV4_SRC_MASK \
%IPV4_DST_MASK" -i eth7
03/Jan/2014 20:50:24 [nprobe.c:2822] Welcome to nprobe v.6.7.3 for x86_64
03/Jan/2014 20:50:24 [plugin.c:143] No plugins found in ./plugins
03/Jan/2014 20:50:24 [plugin.c:143] No plugins found in /usr/local/lib/nprobe/plugins
03/Jan/2014 20:50:24 [nprobe.c:4004] Welcome to nprobe v.6.7.3 for x86_64
03/Jan/2014 20:50:24 [plugin.c:665] 0 plugin(s) enabled
03/Jan/2014 20:50:24 [nprobe.c:3145] Using packet capture length 128
03/Jan/2014 20:50:24 [nprobe.c:4222] Flows ASs will not be computed 
03/Jan/2014 20:50:24 [nprobe.c:4298] Capturing packets from interface eth7
03/Jan/2014 20:50:24 [util.c:3262] nProbe changed user to 'nobody'
```

* On the test station, send some traffic ...
    * The `--unique-ip` option will ensure that IP addresses are unique for every loop
iteration, which will result in unique flows for every iteration
    * The `-K` option will preload the PCAP file into memory before testing.
Omit if you do not have enough memory for this option. 
    * The `-t` option will send as quickly as possible. If you want to reduce the rate, replace `-t` with `--mbps` option
    * In this example we are only sending for about
20 seconds, but we recommending increasing `--loop` option to generate several
minutes of traffic

```
tcpreplay -i eth7 -tK --loop 50 --unique-ip bigFlows.pcap  
File Cache is enabled
Actual: 39580750 packets (17770889200 bytes) sent in 20.02 seconds.
Rated: 876866137.5 Bps, 7014.92 Mbps, 1953026.60 pps
Flows: 2034300 flows, 100378.13 fps, 39558950 flow packets, 21800 non-flow
Statistics for network device: eth7
	Attempted packets:         39580750
	Successful packets:        39580750
	Failed packets:            0
	Truncated packets:         0
	Retried packets (ENOBUFS): 0
	Retried packets (EAGAIN):  0

```

* On the DUT observe the flow statistics received ...
    * Notice that when the DUT is receiving 100K flows/sec it is not able
to process everything. This is common for many flow products.
    * Loss will most likely increase with the amount of processing that
flow tool is asked to do. For example, we would expect loss to increase 
if we were to enable deep packet inspection (DPI) to classify the 
application type.
    * Industry expects flow tools to process 5 fps per Mbps. Therefore
this 10GigE link should be able to perform at 50Kfps (50,000 flows per second)

```
03/Jan/2014 20:52:30 [nprobe.c:1557] ---------------------------------
03/Jan/2014 20:52:30 [nprobe.c:1558] Average traffic: [1.870 M pps][6 Gb/sec]
03/Jan/2014 20:52:30 [nprobe.c:1563] Current traffic: [569.089 K pps][2 Gb/sec]
03/Jan/2014 20:52:30 [nprobe.c:1568] Current flow export rate: [0.0 flows/sec]
03/Jan/2014 20:52:30 [nprobe.c:1571] Flow drops: [export queue too long=0]
[too many flows=0]
03/Jan/2014 20:52:30 [nprobe.c:1575] Export Queue: 0/512000 [0.0 %]
03/Jan/2014 20:52:30 [nprobe.c:1582] Flow Buckets: [active=749448][allocated=749448]
[toBeExported=0][frags=0]
03/Jan/2014 20:52:30 [nprobe.c:1465] Processed packets: 13089188 (max bucket search: 162)
03/Jan/2014 20:52:30 [nprobe.c:1468] Flow export stats: [0 bytes][0 pkts][0 flows]
03/Jan/2014 20:52:30 [nprobe.c:1477] Flow drop stats:   [0 bytes][0 pkts][0 flows]
03/Jan/2014 20:52:30 [nprobe.c:1482] Total flow stats:  [0 bytes][0 pkts][0 flows]
03/Jan/2014 20:52:30 [nprobe.c:235] Packet stats (pcap): 13100072/234355 pkts 
rcvd/dropped [1.8%] [Last 13334427/234355 pkts rcvd/dropped]
03/Jan/2014 20:53:00 [nprobe.c:1557] ---------------------------------
03/Jan/2014 20:53:00 [nprobe.c:1558] Average traffic: [626.567 K pps][2 Gb/sec]
03/Jan/2014 20:53:00 [nprobe.c:1563] Current traffic: [336.465 K pps][1 Gb/sec]
03/Jan/2014 20:53:00 [nprobe.c:1568] Current flow export rate: [0.0 flows/sec]
03/Jan/2014 20:53:00 [nprobe.c:1571] Flow drops: [export queue too long=0]
[too many flows=0]
03/Jan/2014 20:53:00 [nprobe.c:1575] Export Queue: 0/512000 [0.0 %]
03/Jan/2014 20:53:00 [nprobe.c:1582] Flow Buckets: [active=1542197][allocated=1542197]
[toBeExported=0][frags=0]
03/Jan/2014 20:53:00 [nprobe.c:1465] Processed packets: 23183007 (max bucket search: 336)
03/Jan/2014 20:53:00 [nprobe.c:1468] Flow export stats: [0 bytes][0 pkts][0 flows]
03/Jan/2014 20:53:00 [nprobe.c:1477] Flow drop stats:   [0 bytes][0 pkts][0 flows]
03/Jan/2014 20:53:00 [nprobe.c:1482] Total flow stats:  [0 bytes][0 pkts][0 flows]
03/Jan/2014 20:53:00 [nprobe.c:235] Packet stats (pcap): 23183007/11039913 pkts 
rcvd/dropped [32.3%] [Last 20888493/10805558 pkts rcvd/dropped]
```

### <a name="reducing-flows-per-second"></a>Reducing Flows Per Second

To reduce the flow rate to 50Kfps you adjust the `--mbps` option ...

```
tcpreplay -i eth7 -K --mbps 3500 --loop 50 --unique-ip bigFlows.pcap 
File Cache is enabled
Actual: 39580750 packets (17770889200 bytes) sent in 40.06 seconds.
Rated: 437499777.1 Bps, 3499.99 Mbps, 974434.59 pps
Flows: 2034300 flows, 50082.23 fps, 39558950 flow packets, 21800 non-flow
Statistics for network device: eth7
	Attempted packets:         39580750
	Successful packets:        39580750
	Failed packets:            0
	Truncated packets:         0
	Retried packets (ENOBUFS): 0
	Retried packets (EAGAIN):  0
```

### <a name="increasing-flows-per-second-with-netmap"></a>Increasing Flows Per Second with netmap
We have already shown that the maximum rate that we can send is just over 7 Gbps.
To increase flow rate to maximum possible with selected capture file you must install
[netmap], which will allow Tcpreplay to write directly to the network adapters TX 
buffers.

**Note:** Using *netmap* is invasive and is not for the faint of heart. You should be
somewhat familiar with compiling kernel modules before proceeding. Also, be aware that
the network stack will be disconnected from the selected network adapter for the duration
of the test. If you use SSH, do not connect via the same interface that you are testing
on.

* Download [netmap] tarball and extract. Note that you will have to eventually compile
*netmap* but it isn't necessary to do so if you are simply compiling Tcpreplay with
*netmap* capabilities. If you are able to extract into `/usr/src` or
`/usr/local/src` you can simply go to your Tcpreplay source directory and enter ...

```
./configure

 ...

##########################################################################
             TCPREPLAY Suite Configuration Results (4.0.0)
##########################################################################
libpcap:                    /usr (>= 0.9.6)
libdnet:                    no ()
autogen:                    /usr/local/bin/autogen (5.16.2)
Use libopts tearoff:        yes
64bit counter support:      yes
tcpdump binary path:        /usr/sbin/tcpdump
fragroute support:          no
tcpbridge support:          yes
tcpliveplay support:        yes

Supported Packet Injection Methods (*):
Linux TX_RING:              no
Linux PF_PACKET:            yes
BSD BPF:                    no
libdnet:                    no
pcap_inject:                yes
pcap_sendpacket:            yes **
Linux/BSD netmap:           yes /usr/src/netmap-release
```

* If you extract elsewhere you will need to supply the location of the *netmap* source,
for example ...

```
./configure --with-netmap=/home/fklassen/git/netmap/

 ...

##########################################################################
             TCPREPLAY Suite Configuration Results (4.0.0)
##########################################################################
libpcap:                    /usr (>= 0.9.6)
libdnet:                    no ()
autogen:                    /usr/local/bin/autogen (5.16.2)
Use libopts tearoff:        yes
64bit counter support:      yes
tcpdump binary path:        /usr/sbin/tcpdump
fragroute support:          no
tcpbridge support:          yes
tcpliveplay support:        yes

Supported Packet Injection Methods (*):
Linux TX_RING:              no
Linux PF_PACKET:            yes
BSD BPF:                    no
libdnet:                    no
pcap_inject:                yes
pcap_sendpacket:            yes **
Linux/BSD netmap:           yes /home/fklassen/git/netmap/
```

* You will need to compile *netmap* using instructions in its *README* file.
Specifics on how to install the driver is beyond the scope of this document.
However, here is a script that we developed which will allow us to temporarily
install the *netmap_lin* module and the netmap-enabled *ixgbe* 10GigE driver ...

``` sh
#!/bin/sh
ifdown eth6
ifdown eth7
rmmod ixgbe
rmmod netmap_lin
insmod ./netmap_lin.ko
mknod /dev/netmap c 10 59
modprobe mdio
insmod ./ixgbe.ko
ifup eth6
ifup eth7
```
* When you have the *netmap* drivers installed, *tcpreplay* will be able to exploit
this feature when using the `--netmap` option. Otherwise the drivers run in normal
mode and applications will run normally ...
    * Notice that the link is saturated and therefore 135 Kfps is the
maximum we can achieve with this pcap file and this 10GigE link

```
tcpreplay -i eth7 -tK --loop 50 --unique-ip --netmap bigFlows.pcap 
Switching network driver for eth7 to netmap bypass mode... done!
File Cache is enabled
Actual: 39580750 packets (17770889200 bytes) sent in 15.00 seconds.
Rated: 1182219090.4 Bps, 9457.75 Mbps, 2633133.19 pps
Flows: 2034300 flows, 135333.03 fps, 39558950 flow packets, 21800 non-flow
Statistics for network device: eth7
	Attempted packets:         39580750
	Successful packets:        39580750
	Failed packets:            0
	Truncated packets:         0
	Retried packets (ENOBUFS): 0
	Retried packets (EAGAIN):  0
Switching network driver for eth7 to normal mode... done!
```

* If you download [smallFlows.pcap] you may see even higher flows per second. This
capture has smaller flows and therefore is able to generate up to 157 Kfps before the 
link is saturated.
   * When using *netmap*, the size of the flows within the capture file is
the limiting factor in determining maximum fps. We have been able to generate 18 Mfps using a
capture with only 64 byte packets

```
tcpreplay -i eth7 -tK --loop 50 --unique-ip --netmap smallFlows.pcap 
Switching network driver for eth7 to netmap bypass mode... done!
File Cache is enabled
Actual: 713050 packets (460826550 bytes) sent in 0.385312 seconds.
Rated: 1195982865.8 Bps, 9567.86 Mbps, 1850578.23 pps
Flows: 60450 flows, 156885.84 fps, 712150 flow packets, 900 non-flow
Statistics for network device: eth7
	Attempted packets:         713050
	Successful packets:        713050
	Failed packets:            0
	Truncated packets:         0
	Retried packets (ENOBUFS): 0
	Retried packets (EAGAIN):  0
Switching network driver for eth7 to normal mode... done!
```

## <a name="how-to-do-a-performance-test-for-a-packet-capture-device"></a>How to do a Performance Test for a Packet Capture Device
When testing to a packet capture device or a capture application such as *tcpdump*, the
setup is similar to the *IP Traffic Flow Appliance* example above. Instead of
*nprobe* run your packet capture application.
 

[bigFlows.pcap]:  https://dl.dropboxusercontent.com/u/98306176/captures/bigFlows.pcap
[smallFlows.pcap]:    https://dl.dropboxusercontent.com/u/98306176/captures/smallFlows.pcap
[netmap]:         http://info.iet.unipi.it/~luigi/netmap/
[captures]: captures.html
[download]: installation.html
[nprobe]:   http://www.ntop.org/products/nprobe/
[flow]:     https://ietf.org/wg/ipfix/
[NetFlow]:  http://www.cisco.com/go/netflow
[playlist]:   http://www.youtube.com/playlist?list=PLFjjcN5EvTP0Hsxq1AEh7FhvaFH__Vd83
