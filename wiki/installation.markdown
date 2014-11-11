---
layout: content
title:  "ダウンロードとインストール／Download and Installation"
categories: tcpreplay wiki
description: "Tcpreplay のダウンロードとインストール／Tcpreplay download and installation information"
---

- [ダウンロード／Downloads](#downloads)
    - [一般ユーザ向け／Download Releases for Users](#download-releases)
    - [開発者向け／Download Source for Developers](#download-source)
- [インストール／Installation](#installation)
	- [Unix ユーザ向けの簡単な手順／Simple directions for Unix users](#simple-directions-for-unix-users)
    - [インストールビデオ／Installation Video](#installation-video)
	- [Netmap のビルド／Build netmap feature](#build-netmap-feature)
    - [Netmap のインストールビデオ／Netmap Installation Video](#netmap-installation-video)
- [オプション／Advanced Options](#advanced-options)
- [Windows 向けの説明／Special Instructions for Windows](#special-instructions-for-windows)
- [ヘルプが必要ですか？／Need Help?](#need-help)


<h2 id="Downloads"><a name="downloads"></a>ダウンロード／Downloads</h2>

### <a name="download-releases"></a>一般ユーザ向け／Download Releases for Users

* 最新のステーブル版／Latest stable release:
 - [tcpreplay-4.0.5.tar.gz][tcpreplay-4.0.5]
 - [PGP signature][tcpreplay-4.0.5.asc]
 - [Release notes][4.0.5-release]
 - [Version 4.0.5 announcement][4.0.5-announce]

* 以前のリリース／Previous release:
 - [tcpreplay-4.0.4.tar.gz][tcpreplay-4.0.4]
 - [PGP signature][tcpreplay-4.0.4.asc]
 - [Release notes][4.0.4-release]
 - [Version 4.0.4 announcement][4.0.4-announce]

* [古い Win32 向け／Old Win32 release][win32]
 - メンテナは Windows 版をサポートしていません
* [古いバージョン／Complete list of past releases][old_stuff]

### <a name="download-source"></a>開発者向け／Download Source for Developers
[GitHub][repo] のソースコードをコンパイルするには、
リリース版よりも必要要件がたくさんあります。
具体的に言うと、[AutoGen][autogen] が必要です。
[AutoGen version 5.16.2][autogen-download] に対応していないと、
[pull requests][pullreq] はリジェクト(拒否)されてしまいます。

Tcpreplay の開発に参加したい場合は、
[Developer Wiki][dev-wiki] にアクセスしてみてください。

* [GitHub][repo] からダウンロードする
 - `git clone https://github.com/appneta/tcpreplay`
* 開発に参加したい場合は、
単に [レポジトリ／repo][repo] を[フォーク／fork][fork] して、
変更点の準備ができたら
[プルリクエスト／pull request][pullreq] を送ってください
* [最新版のソースコード／master tarball][tarball] をダウンロードしてください
* *master* ブランチは常にリリースできる状態ですので、最新のステーブル版が必要という訳ではありません。[GitHub network][network] を確認してください。


<h2 id="Installation"><a name="installation"></a>インストール／Installation</h2>

### <a name="simple-directions-for-unix-users"></a>Unix ユーザ向けの簡単な手順／Simple directions for Unix users
ソースコードをコンパイルする必要がありますが、
まずはコンパイルするためのツールなどがインストールされていることを確認してください。
例えば、Ubuntu や Debian の場合は下記を実行します:

```
sudo apt-get install build-essential libpcap-dev
```  

次に tarball(tcpreplay-x.x.x.tar.gz) を展開し、
ディレクトリを移動し、下記を実行します:

```
./configure 
make
sudo make install
```

インストールされた環境が全て動作することを確認するために、
オプションで下記を実行しても良いです:

```
sudo make test
```

### <a name="installation-video"></a>インストールビデオ／Installation Video
<iframe width="620" height="460" src="//www.youtube.com/embed/5iK-0MiHZmI?rel=0" frameborder="0" allowfullscreen></iframe>

### <a name="build-netmap-feature"></a>Netmap のビルド／Build netmap feature
この機能は、Linux や BSD において
[netmap][nm] に対応したネットワークドライバを検出します。
もし対応したネットワークドライバが検出された場合は、
netmap はテストのための duration のネットワークドライバをバイパスし、
tcpreplay や tcpreplay-edit がネットワークカードに直接書き込むことを許可します。
この機能は、通常は商用のネットワークテスト装置でしか
得られないようなパフォーマンスレベルになります。

**注意** この機能を使用すると、
このインターフェイス(NIC) を使用している他のアプリケーションは切断されます。
例えば、同じインターフェイス(NIC) で SSH ログインしていると切断されてしまいます。

FreeBSD 10 以降だと標準で Netmap 機能が有効なので、
*configure* を実行するだけで Netmap が有効になります。
それ以外のシステムの場合、
*device netmap* を有効にしたカーネルのコンパイルが必要かもしれません。

Linux の場合、<http://info.iet.unipi.it/~luigi/netmap/> から
最新の Netmap をダウンロードしインストールしてください。
Netmap を */usr/src/* や */usr/local/src* に展開すれば、
*configure* にオプションの追加は不要です。
上記以外に展開する場合はディレクトリの指定が必要です。例えば:

```
./configure --with-netmap=/home/fklassen/git/netmap
make
sudo make install
```

<http://code.google.com/p/netmap/> で Netmap のソースコードも入手できます。

### <a name="netmap-installation-video"></a>Netmap のインストールビデオ／Netmap Installation Video
<iframe width="620" height="460" src="//www.youtube.com/embed/hswh90iIw9s?rel=0" frameborder="0" allowfullscreen></iframe>

## <a name="advanced-options"></a>オプション／Advanced Options

tcpreplay を制御するための configure オプションはたくさんあります。
役に立つオプションの例:

* *--enable-debug* -- デバッグ時に有用です／useful for debugging bugs and crashes.
* *--enable-64bits* -- 巨大な pcap ファイルやたくさんのループをまわす時に 64bit カウンタを利用できます／use 64 bit counters to handle large pcap files & looping
* *--enable-libnet* -- libnet にリンクします。1年以上放置されたバグがあることに注意してください／link to libnet. Note that libnet support has been deprecated due to
various bugs which have not been fixed in over a year.
* *--with-libnet* -- libnet の PATH を指定します／specify root path to libnet (something like --with-libnet=/usr/local)
* *--with-libpcap* -- libpcap の PATH を指定します／specify root path to libpcap
* *--with-netmap* -- netmap の PATH を指定します／specify root path to netmap
* *--with-tcpdump* -- tcpdump の PATH を指定します／specify path to tcpdump executable
* *--enable-tcpreplay-edit* -- パケットを編集しながら再送信する tcpreplay-edit を有効にします／compile tcpreplay with packet editing support

パケットを投入する方法を手動で指定することもできます:

* *--enable-force-pf* -- Linux の PF_PACKET を指定します／force tcpreplay to use Linux's PF_PACKET to send packets
* *--enable-force-bpf* -- BPF インターフェイスを指定します／force tcpreplay to use Free/Net?/OpenBSD or OS X's BPF interface
to send packets
* *--enable-force-libnet* -- Libnet を使ってパケットを送信します／force tcpreplay to use Libnet to send packets
* *--enable-force-inject* -- Libpcap の pcap_inject() API を使ってパケットを送信します／force tcpreplay to use Libpcap's pcap_inject() API to send packets
* *--enable-force-sendpacket* -- Libpcap の pcap_sendpacket() API を使ってパケットを送信します／force tcpreplay to use Libpcap's pcap_sendpacket() API
to send packets

GNU Autogen がインストールされているのであれば、
下記のオプションを指定しても良いです:

* *--disable-local-libopts* -- libopts のtearoff を使用しません／Don't use the libopts tearoff supplied with tcpreplay 
(default is enabled)
* *--disable-libopts-install* -- libopts ライブラリをインストールしません／don't install the libopts library files

## <a name="special-instructions-for-windows"></a>Windows 向けの説明／Special Instructions for Windows

Tcpreplay は、実験的に Windows をサポートしています。
いうなれば、β(ベータ)版のクオリティです。
[Tcpreplay のサポートを受けるには／how to get support for Tcpreplay][Support]
のページを読むことを強くお勧めします。

上記ページによると、
Tcpreplay をコンパイルして実行するには [Cygwin] が必要です。
libpcap の Windows 版である [Winpcap] のインストールも必要です。
Winpcap は、
Cygwin の最上位ディレクトリ(/Wpdpack) にインストールする必要があります。

[ドライバと DLL／driver and DLL][dll] と [開発キット／developer pack][devpack]
の両方をインストールしてください。その後 ./configure を実行します。
`--with-libpcap` オプションで Winpcap の場所を指定する必要がありますが、
`./configure --with-libpcap=/wpdpack` のように全て小文字で指定してください。

上記を実行すれば、ほぼ全ての機能が利用できます。いくつかの注意事項があります。
`make test` は機能しませんが、機能しない部分のほとんどはマイナーな機能です。

[Win32Readme.txt] に詳細が記述されています。

Cygwin のパッケージが壊れているという情報があります。
これは GNU の Autogen の一部を壊します。
特に、GitHub から Tcpreplay をビルドする時に利用する部分です。
従って、tarball(tcpreplay-x.x.x.tar.gz) を使うことを強くお勧めします。


## <a name="need-help"></a>ヘルプが必要ですか？／Need Help?

トラブルですか？
[tcpreplay-users メーリングリスト][maillist] に質問してみてください。
あるいは、[サポート／Support][Support] のセクションを見てください。

[maillist]: maillist.html
[Support]:  support.html
[nm]:       http://info.iet.unipi.it/~luigi/netmap/
[Cygwin]:   http://www.cygwin.com/
[Winpcap]:  http://www.winpcap.org
[dll]:      http://www.winpcap.org/install/default.htm
[devpack]:  http://www.winpcap.org/devel.htm
[tcpreplay-3.4.4]:           http://prdownloads.sourceforge.net/tcpreplay/tcpreplay-3.4.4.tar.gz?download
[tcpreplay-3.4.4.asc]:       http://tcpreplay.synfin.net/raw-attachment/wiki/Download/tcpreplay-3.4.4.tar.gz.asc
[tcpreplay-3.4.4.changelog]: http://tcpreplay.synfin.net/browser/tags/3.4.4/docs/CHANGELOG
[tcpreplay-4.0.0beta2]:      https://drive.google.com/folderview?id=0Bwy1iN_ElthJWi14Mnh5YUJTUWs&usp=docslist_api#
[tcpreplay-4.0.0beta2.asc]:  https://drive.google.com/file/d/0Bwy1iN_ElthJeXdmT05jME4yc3M/edit?usp=sharing
[tcpreplay-4.0.0]:           http://sourceforge.net/projects/tcpreplay/files/tcpreplay/4.0.0/
[tcpreplay-4.0.0.asc]:       https://drive.google.com/file/d/0Bwy1iN_ElthJZk1IVUNCUzEzVEU/edit?usp=sharing
[4.0.0-release]:             https://github.com/appneta/tcpreplay/releases/tag/v4.0.0
[4.0.0beta1_announce]:       {{ site.url }}/tcpreplay/news/2013/12/20/4-0-beta1.html
[4.0.0beta2_announce]:       {{ site.url }}/tcpreplay/news/2013/12/22/4-0-beta2.html
[4.0.0-announce]:            {{ site.url }}/tcpreplay/news/2014/01/05/4-0-0.html
[win32]:                     http://sourceforge.net/projects/tcpreplay/files/tcpreplay-win32/
[old_stuff]:                 http://sourceforge.net/projects/tcpreplay/files/
[stats]:                     http://sourceforge.net/projects/tcpreplay/files/stats/timeline
[tarball]:                   https://github.com/appneta/tcpreplay/tarball/master
[network]:                   https://github.com/appneta/tcpreplay/network
[fork]:                      https://help.github.com/articles/fork-a-repo
[repo]:                      https://github.com/appneta/tcpreplay
[pullreq]:                   https://help.github.com/articles/fork-a-repo#pull-requests
[autogen]:                   http://autogen.sourceforge.net/
[autogen-download]:          http://ftp.gnu.org/gnu/autogen/rel5.16.2/
[dev-wiki]:                  https://github.com/appneta/tcpreplay/wiki
[Win32Readme.txt]:           https://github.com/appneta/tcpreplay/blob/master/docs/Win32Readme.txt
[tcpreplay-4.0.1]:           http://sourceforge.net/projects/tcpreplay/files/tcpreplay/4.0.1/
[tcpreplay-4.0.1.asc]:       https://drive.google.com/file/d/0Bwy1iN_ElthJQlRkWmRWWmtzYUk/edit?usp=sharing
[4.0.1-release]:             https://github.com/appneta/tcpreplay/releases/tag/v4.0.1
[4.0.1-announce]:            {{ site.url }}/tcpreplay/news/2014/01/16/4-0-1.html
[tcpreplay-4.0.2]:           http://sourceforge.net/projects/tcpreplay/files/tcpreplay/4.0.2/
[tcpreplay-4.0.2.asc]:       https://drive.google.com/file/d/0Bwy1iN_ElthJM3JCTXRtUXBadVU/edit?usp=sharing
[4.0.2-release]:             https://github.com/appneta/tcpreplay/releases/tag/v4.0.2
[4.0.2-announce]:            {{ site.url }}/tcpreplay/news/2014/01/17/4-0-2.html
[tcpreplay-4.0.3]:           http://sourceforge.net/projects/tcpreplay/files/tcpreplay/4.0.3/
[tcpreplay-4.0.3.asc]:       https://drive.google.com/file/d/0Bwy1iN_ElthJaUZJS3lONjY0dEU/edit?usp=sharing
[4.0.3-release]:             https://github.com/appneta/tcpreplay/releases/tag/v4.0.3
[4.0.3-announce]:            {{ site.url }}/tcpreplay/news/2014/02/04/4-0-3.html
[tcpreplay-4.0.4]:           http://sourceforge.net/projects/tcpreplay
[tcpreplay-4.0.4.asc]:       https://drive.google.com/file/d/0Bwy1iN_ElthJbzRrbl9rczFyaG8/edit?usp=sharing
[4.0.4-release]:             https://github.com/appneta/tcpreplay/releases/tag/v4.0.4
[4.0.4-announce]:            {{ site.url }}/tcpreplay/news/2014/03/22/4-0-4.html
[tcpreplay-4.0.5beta1]:      https://github.com/appneta/tcpreplay/releases/download/v4.0.5beta1/tcpreplay-4.0.5-beta1.tar.gz
[tcpreplay-4.0.5beta1.asc]:  https://github.com/appneta/tcpreplay/releases/download/v4.0.5beta1/tcpreplay-4.0.5-beta1.tar.gz.asc
[4.0.5beta1-release]:        https://github.com/appneta/tcpreplay/releases/tag/v4.0.5beta1
[4.0.5beta1-announce]:       {{ site.url }}/tcpreplay/news/2014/07/27/4-0-5beta1.html
[tcpreplay-4.0.5]:      https://github.com/appneta/tcpreplay/releases/download/v4.0.5/tcpreplay-4.0.5.tar.gz
[tcpreplay-4.0.5.asc]:  https://github.com/appneta/tcpreplay/releases/download/v4.0.5/tcpreplay-4.0.5.tar.gz.asc
[4.0.5-release]:        https://github.com/appneta/tcpreplay/releases/tag/v4.0.5
[4.0.5-announce]:       {{ site.url }}/tcpreplay/news/2014/09/05/4-0-5.html
