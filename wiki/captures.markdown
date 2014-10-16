---
layout: content
title:  "Sample Captures"
categories: tcpreplay wiki
description: "List of sample network capture files for use with Tcpreplay suite"
---

- [Overview](#overview)
    - [smallFlows.pcap](#smallflows-pcap)
    - [bigFlows.pcap](#bigflows-pcap)
    - [test.pcap](#test-pcap)
- [Contributions](#contributions "Project Contributions")


<h2 id="Sample Captures"><a name="overview"></a>Overview</h2>
Tcpreplay Suite can read nearly any packet capture file. If you prefer to get up and
running quickly, we have provided some sample captures. They are specially
designed to test [IP Flow][flow]/[NetFlow], but they are also useful for
testing performance of switches and network adapters.

Pcap files are available [here][pcaps].

### <a name="smallflows-pcap"></a>[smallFlows.pcap][small]
This is a synthetic capture which is a combination of several captures containing a few different applications.
 It is designed to create large number of flows utilizing various protocols at relatively low network traffic rate.
If you want to have many flows in a small file and are not concerned about how realistic
the combination of flows is, select this capture

Size:			9.4 MB   
Packets:		14261   
Flows:			1209   
Average packet size:	646 bytes   
Duration:		5 minutes   
Number Applications:	28


### <a name="bigflows-pcap"></a>[bigFlows.pcap][big]
This is a capture of real network traffic on a busy private network's access point to the Internet.
The capture is much larger and has a smaller average packet size than the previous capture.
It also has many more flows and different applications. If the large size
of this file isn't a problem, you may want to select it for your tests.   


Size:			368 MB   
Packets:		791615    
Flows:			40686  
Average packet size:	449 bytes   
Duration:		5 minutes   
Number Applications:	132

### <a name="test-pcap"></a>[test.pcap][test]
This is a small capture that is included in the source tarball and is used to test
accuracy of *tcpreplay* results during `sudo make test`. It is also good for demonstrating
*tcpprep* capbablilites

Size:           0.07 MB   
Packets:        141    
Flows:          37  
Average packet size:    445 bytes   
Duration:       3 seconds   
Number Applications:    1


## <a name="contributions"></a>Contributions
If you have other good examples of pcap files that work well with Tcpreplay please contact the
maintainer.

[flow]:     https://ietf.org/wg/ipfix/
[NetFlow]:  http://www.cisco.com/go/netflow
[pcaps]:    https://www.dropbox.com/sh/j8i8v1nujvbumyb/ujd3biK6Fk/captures
[small]:    https://dl.dropboxusercontent.com/u/98306176/captures/smallFlows.pcap
[big]:      https://dl.dropboxusercontent.com/u/98306176/captures/bigFlows.pcap
[test]:     https://dl.dropboxusercontent.com/u/98306176/captures/test.pcap
