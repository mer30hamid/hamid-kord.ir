---
published: true
title: استفاده از DoH در ویندوز با کمک نرم افزار Acrylic DNS Proxy
layout: post
tags: [Proxy, شبکه, DNS, DoH]
date: 2022-12-09T19:45:02+04:30
tweet: https://twitter.com/mer30hamid/status/1601310172965896198
---

قبلا در مورد [راه اندازی سرور DoH اختصاصی فقط با PHP](https://hamid-kord.ir/posts/doh-using-php/) نوشته بودم، پیدا کردن این مدل سرور خیلی آسونه ولی استفاده ازش داخل ویندوز بصورتی که نیاز به نصب نرم افزار نباشه هنوز زوده. در ویندوز 10 باید نسخه های آخرش رو دریافت کرده باشید و نصب ویندوز 11 هم برای همه امکانپذیر نیست. در این نوشته توضیح می دهم نرم افزار Acrylic Dns Proxy چطور می تواند بین درخواست های اپلیکیشن های ویندوز و سرور های DNS یا DoH در اینترنت قرار بگیره و درخواست های عادی همه اپلیکیشن های ویندوز رو به اون سرورها هدایت کند. این نرم افزار ابتدا در سال 2010 به زبان دلفی نوشته شده و اول قرار بوده فقط کار یک پروکسی برای دی ان اس ها انجام بده و کارهایی مثل کش کردن درخواستها را هم انجام بدهد ولی از [سال 2019](https://mayakron.altervista.org/support/acrylic/ChangeLog.htm) امکان DNS-over-HTTPS protocol یا همون DoH بهش اضافه شد. مراحل کار را به شرح زیر باید انجام بدهیم تا از شر Do53 (DNS over UDP port 53) که مثل سرطان می مونه خلاص بشیم!

1. نصب نرم افزار
   
   برای این کار اول به [سایت رسمی نرم افزار](https://mayakron.altervista.org/) رفته، اون رو دانلود و سپس نصبش کنید. نصب نرم افزار خیلی ساده هست. این نرم افزار یک "ویندوز سرویس" به اسم Acrylic DNS Proxy ایجاد می کنه و یک رابط کاربری گرافیکی به ما میده به اسم "Acrylic UI" که در منوی استارت ویندوز در دسترس هست. ما فقط با همین کار داریم. عنوان پنجره این رابط خیلی مهمه و وضعیت فعلی "ویندوز سرویس" رو داخل پرانتز به ما نشون میده.

2. تنظیم فایل کانفیگ
   
   در Acrylic UI از منوی File گزینه Open Acrylic Configuration رو بزنید تا فایل کانفیگ نرم افزار برای ویرایش باز بشه. این نرم افزار امکانات زیادی دارد، من فقط نحوه ی تنظیم کردن DoH را اینجا شرح می دهم. فایل باز شده از [فرمت شناخته شده INI](https://en.wikipedia.org/wiki/INI_file) پیروی می کنه بنابراین بسیار خوانا و ساده است.
   
   برای تنظیم DoH باید بلوک مربوط به تنظیم هر کدوم رو در قسمت [GlobalSection] اضافه کنید. جالب است بدانید که امکان تنظیم DNS هم وجود داره ولی اینجا تنظیم DNS رو توضیح نمیدهم چون می خواهیم از شر آن خلاص بشیم و فقط از DoH استفاده کنیم!
   هر بلوک تنظیم مربوط به دی او اچ (یا دی ان اس، فرقی نمی کنه) به ترتیب اولویت دارای کلمات Primary و Secondary و Tertiary و ... هستند. یعنی "اولا" و "ثانیا" و "ثالثا" و ... من نمونه های Primary و Secondry را به صورت زیر برای خودم تنظیم کردم:

```ini
[GlobalSection]
PrimaryServerAddress=78.142.193.50
PrimaryServerPort=443
PrimaryServerProtocol=DOH
PrimaryServerDoHProtocolPath=dns-query
PrimaryServerDoHProtocolHost=nl-ams.doh.sb
PrimaryServerDoHProtocolConnectionType=System
PrimaryServerDoHProtocolReuseConnections=Yes
PrimaryServerDoHProtocolUseWinHttp=Yes
IgnoreFailureResponsesFromPrimaryServer=No
IgnoreNegativeResponsesFromPrimaryServer=No

SecondaryServerAddress=89.38.131.38
SecondaryServerPort=443
SecondaryServerProtocol=DOH
SecondaryServerDoHProtocolPath=dns-query
SecondaryServerDoHProtocolHost=dnsnl-noads.alekberg.net
SecondaryServerDoHProtocolConnectionType=System
SecondaryServerDoHProtocolReuseConnections=Yes
SecondaryServerDoHProtocolUseWinHttp=Yes
IgnoreFailureResponsesFromSecondaryServer=No
IgnoreNegativeResponsesFromSecondaryServer=No
```

هر قسمت از اسمش معلوم هست چه کاربردی دارد. در سایت رسمی نرم افزار خط به خط هر یک از تنظیمات بالا توضیح داده شده:

[مستندات کانفیگ نرم افزار](https://mayakron.altervista.org/support/acrylic/Configuration.htm)


در نمونه بالا به کلمات Primary و Secondary توجه کنید. مابقی محتوای کانفیگ نیازی به تغییر ندارد. کانفیگی که من الان دارم استفاده می کنم این هست، می توانید کامل کپی کنید:

```ini
[GlobalSection]
PrimaryServerAddress=78.142.193.50
PrimaryServerPort=443
PrimaryServerProtocol=DOH
PrimaryServerDoHProtocolPath=dns-query
PrimaryServerDoHProtocolHost=nl-ams.doh.sb
PrimaryServerDoHProtocolConnectionType=System
PrimaryServerDoHProtocolReuseConnections=Yes
PrimaryServerDoHProtocolUseWinHttp=Yes
IgnoreFailureResponsesFromPrimaryServer=No
IgnoreNegativeResponsesFromPrimaryServer=No

SecondaryServerAddress=89.38.131.38
SecondaryServerPort=443
SecondaryServerProtocol=DOH
SecondaryServerDoHProtocolPath=dns-query
SecondaryServerDoHProtocolHost=dnsnl-noads.alekberg.net
SecondaryServerDoHProtocolConnectionType=System
SecondaryServerDoHProtocolReuseConnections=Yes
SecondaryServerDoHProtocolUseWinHttp=Yes
IgnoreFailureResponsesFromSecondaryServer=No
IgnoreNegativeResponsesFromSecondaryServer=No


TertiaryServerAddress=149.255.57.130
TertiaryServerPort=443
TertiaryServerProtocol=DOH
TertiaryServerDoHProtocolPath=dns-query
TertiaryServerDoHProtocolHost=hamid.cybranceehost.com
TertiaryServerDoHProtocolConnectionType=System
TertiaryServerDoHProtocolReuseConnections=Yes
TertiaryServerDoHProtocolUseWinHttp=Yes
IgnoreFailureResponsesFromTertiaryServer=No
IgnoreNegativeResponsesFromTertiaryServer=No

; You can specify below whether Acrylic should sinkhole IPv6 lookups (also known as DNS requests of AAAA type) or not.
SinkholeIPv6Lookups=No
;
; You can direct Acrylic to forward reverse lookups (also known as DNS requests of PTR type) for private IP ranges to
; your DNS servers by choosing Yes instead of No. Aside from protecting you and your DNS servers from the traffic of
; these usually needless queries, choosing No is usually a better choice also to avoid leaking information about your
; private address space.
;
ForwardPrivateReverseLookups=No
;
; THE ACRYLIC DNS PROXY CACHING MECHANISM EXPLAINED
;
; When Acrylic receives a DNS request from a client the hosts cache (an in-memory static cache derived from the
; AcrylicHosts.txt file) is searched first. If nothing is found there the request is then searched in the address cache
; (an in-memory dynamic cache backed up by the AcrylicCache.dat file). At this point one of the following three cases
; can happen:
;
; [1] The request is not found in the address cache or its corresponding response is older than
; "AddressCacheScavengingTime" minutes: In this case the original request is forwarded to all of the configured DNS
; servers simultaneously. The response to the client is delayed until the first one of the DNS servers comes out with a
; valid response. All the other responses coming from the other DNS servers will be discarded.
;
; [2] The request is found in the address cache and its corresponding response is older than
; "AddressCacheSilentUpdateTime" minutes but not older than "AddressCacheScavengingTime minutes": In this case the
; response to the client is sent immediately from the address cache and the original request is also forwarded to all of
; the configured DNS servers simultaneously like in the previous case. The first valid response coming from one of the
; DNS servers will be used to silently update the address cache, while all the other responses coming from the other DNS
; servers will be discarded.
;
; [3] The request is found in the address cache and its corresponding response is younger than
; "AddressCacheSilentUpdateTime" minutes: In this case the response to the client is sent immediately from the address
; cache and no network activity with any of the configured DNS servers will occur.
;
; Be aware that to minimize disk activity the address cache is flushed from memory to disk only when Acrylic is stopped
; or the system is shut down.
;
; And now about the caching parameters:
;
; The time to live (in minutes) of a failure response in the address cache.
;
AddressCacheFailureTime=0
;
; The time to live (in minutes) of a negative response in the address cache.
;
AddressCacheNegativeTime=60
;
; The time to live (in minutes) of a positive response in the address cache.
;
AddressCacheScavengingTime=5760
;
; The time (in minutes) elapsed which an item in the address cache must be silently updated should a request occur.
;
AddressCacheSilentUpdateTime=1440
;
; The time (in minutes) elapsed which the address cache is pruned of obsolete items. A value of 0 indicates that no
; pruning of the address cache is ever done.
;
AddressCachePeriodicPruningTime=360
;
; The address cache domain name affinity mask is a list of semicolon separated values or wildcards that allows to
; restrict DNS responses for which domain names are to be cached in the address cache.
;
AddressCacheDomainNameAffinityMask=^dns.msftncsi.com;^ipv6.msftncsi.com;^www.msftncsi.com;*
;
; The address cache query type affinity mask is list of semicolon separated values that allows to restrict DNS responses
; for which query types are to be cached in the address cache.
;
; All DNS query types are supported, either explicitly using A, AAAA, CNAME, MX, NS, PTR, SOA, SRV and TXT or implicitly
; using their decimal values.
;
AddressCacheQueryTypeAffinityMask=A;AAAA;CNAME;MX;NS;PTR;SOA;SRV;TXT
;
; You can disable any disk activity related to the address cache by choosing Yes instead of No. If you do that Acrylic
; will use the address cache only in memory.
;
AddressCacheInMemoryOnly=No
;
; You can disable the address cache altogether by choosing Yes instead of No. If you do that Acrylic will work as a
; forwarding-only DNS proxy.
;
AddressCacheDisabled=No
;
; The local IPv4 address to which Acrylic binds. A value of 0.0.0.0 indicates that Acrylic should bind to all available
; addresses and as such it will be able to receive DNS requests coming from all of your network interfaces. A value
; corresponding to the IPv4 address of one of your network interfaces instead will allow Acrylic to receive DNS requests
; only from that specific network interface. An empty value instead indicates that no binding should occur on IPv4.
;
LocalIPv4BindingAddress=0.0.0.0
;
; The local UDPv4 port to which Acrylic binds. The default value of 53 is the standard port for DNS resolution. You
; should change this value only if you are using a non standard DNS client.
;
LocalIPv4BindingPort=53
;
; The local IPv6 address to which Acrylic binds. A value of 0:0:0:0:0:0:0:0 indicates that Acrylic should bind to all
; available addresses and as such it will be able to receive DNS requests coming from all of your network interfaces. A
; value corresponding to the IPv6 address of one of your network interfaces instead will allow Acrylic to receive DNS
; requests only from that specific network interface. An empty value instead indicates that no binding should occur on
; IPv6.
;
LocalIPv6BindingAddress=0:0:0:0:0:0:0:0
;
; The local UDPv6 port to which Acrylic binds. The default value of 53 is the standard port for DNS resolution. You
; should change this value only if you are using a non standard DNS client.
;
LocalIPv6BindingPort=53
;
; On Windows versions prior to Windows Vista or Windows Server 2008 the IPv6 protocol is usually not installed by
; default. For Windows 2000 there is a Microsoft IPv6 Technology Preview package available for download while for
; Windows XP the IPv6 protocol must be added to the list of available network protocols in your network connection
; Properties window.
;
; If you want to enable local IPv6 binding for Acrylic on Windows versions prior to Windows Vista or Windows Server 2008
; you can choose Yes below after having installed all the necessary prerequisites.
;
LocalIPv6BindingEnabledOnWindowsVersionsPriorToWindowsVistaOrWindowsServer2008=No
;
; The time to live (in seconds) set for DNS responses generated by Acrylic (e.g. the ones generated from mappings
; contained in the AcrylicHosts.txt file).
;
GeneratedResponseTimeToLive=300
;
; The maximum time (in milliseconds) to wait for a response coming from a DNS server configured with the UDP protocol.
;
ServerUdpProtocolResponseTimeout=4999
;
; The maximum time (in milliseconds) to wait for the first byte of a response coming from a DNS server configured with
; the TCP protocol.
;
ServerTcpProtocolResponseTimeout=4999
;
; The maximum time (in milliseconds) to wait for the other bytes of a response coming from a DNS server configured with
; the TCP protocol.
;
ServerTcpProtocolInternalTimeout=2477
;
; The maximum times (in milliseconds) to wait for the below events when communicating with an intermediary SOCKS 5 proxy
; server on behalf of a DNS server configured with the SOCKS5 protocol.
;
ServerSocks5ProtocolProxyFirstByteTimeout=2477
ServerSocks5ProtocolProxyOtherBytesTimeout=2477
ServerSocks5ProtocolProxyRemoteConnectTimeout=2477
ServerSocks5ProtocolProxyRemoteResponseTimeout=4999
;
; The hit log is a text file into which every DNS request and DNS response received by Acrylic can be logged.
;
; It is activated by specifying a non-empty value for the HitLogFileName parameter and contains lines with the following
; TAB-separated fields:
;
; [01] The timestamp of the DNS request or response in the format YYYY-MM-DD HH:MM:SS.FFF (local time).
; [02] The IP address from where the DNS request originates from or the DNS response is destined to.
; [03] The status code of the DNS request or response:
;        X => Resolved directly by Acrylic
;        H => Resolved using the hosts cache
;        C => Resolved using the address cache
;        F => Forwarded to at least one of your DNS servers
;        R => Response accepted from one of your DNS servers
;        U => Silent update accepted from one of your DNS servers
; [04] The index of the DNS server the DNS response is coming from.
; [05] The time it took (in milliseconds) for the DNS server to produce a DNS response.
; [06] The dissected DNS request or response.
;
; A dissected DNS request looks like:
;
; OC=0;RD=1;QDC=1;Q[1]=x.com;T[1]=A
;
; Where:
;
; [01] OC=0 means that the DNS operation code (OPCODE) is 0. Possible values are: 0 = a standard query (QUERY), 1 = an
; inverse query (IQUERY), 2 = a server status request (STATUS).
; [02] RD=1 means that the DNS response recursion desired bit (RD) is 1. If RD is set, it directs the name server to
; pursue the query recursively.
; [03] QDC=1 means that the number of queries (QDCOUNT) contained in the DNS request is 1.
; [04] Q[1]=x.com means that DNS query 1 refers to the "x.com" domain name.
; [05] T[1]=A means that DNS query 1 is of type A (IPv4).
;
; A dissected DNS response looks like:
;
; OC=0;RC=0;TC=0;RD=1;RA=1;AA=0;QDC=1;ANC=2;NSC=0;ARC=0;Q[1]=x.com;T[1]=CNAME;A[1]=x.com>y.com;T[2]=A;A[2]=y.com>1.2.3.4
;
; Where:
;
; [01] OC=0 means that the DNS operation code (OPCODE) is 0. Possible values are: 0 = a standard query (QUERY), 1 = an
; inverse query (IQUERY), 2 = a server status request (STATUS).
; [02] RC=0 means that the DNS response code (RCODE) is 0. Possible values are: 0 = no error condition, 1 = format error
; (the name server was unable to interpret the query), 2 = server failure (the name server was unable to process this
; query due to a problem with the name server), 3 = name error (meaningful only for responses from an authoritative name
; server, this code signifies that the domain name referenced in the query does not exist), 4 = not implemented (the
; name server does not support the requested kind of query), 5 = refused (the name server refuses to perform the
; specified operation for policy reasons).
; [03] TC=0 means that the DNS response truncated bit (TC) is 0. This bit specifies that this message was truncated due
; to length greater than that permitted on the transmission channel.
; [04] RD=1 means that the DNS response recursion desired bit (RD) is 0. If RD is set, it directs the name server to
; pursue the query recursively.
; [05] RA=1 means that the DNS response recursion available bit (RA) is 0. This bit denotes whether recursive query
; support is available in the name server.
; [06] AA=0 means that the DNS response authoritative answer bit (AA) is 0. This bit specifies that the responding name
; server is an authority for the domain name in question section.
; [07] QDC=1 means that the number of queries (QDCOUNT) contained in the DNS response is 1.
; [08] ANC=2 means that the number of answers (ANCOUNT) contained in the DNS response is 2.
; [09] NSC=0 means that the number of nameserver records (NSCOUNT) contained in the DNS response is 0.
; [10] ARC=0 means that the number of additional records (ARCOUNT) contained in the DNS response is 0.
; [11] Q[1]=x.com means that the DNS query 1 refers to the "x.com" domain name.
; [12] T[1]=CNAME means that the DNS answer 1 is of type CNAME (canonical name).
; [13] A[1]=x.com>y.com means that the DNS answer 1 that refers to the "x.com" domain name is "y.com".
; [14] T[2]=A means that the DNS answer 2 is of type A (IPv4).
; [15] A[2]=y.com>1.2.3.4 means that the DNS answer 2 that refers to the "y.com" domain name is "1.2.3.4".
;
; Regarding the HitLogFileName you can use an absolute or a relative path and a kind of daily log rotation can be
; achieved by including the %DATE% template within the file name. A complete list of all the templates you can use
; within the file name is shown below:
;
; %DATE%
; The current date in YYYYMMDD format.
;
; %TEMP%
; The current value of the TEMP environment variable.
;
; %APPDATA%
; The current value of the APPDATA environment variable.
;
; %LOCALAPPDATA%
; The current value of the LOCALAPPDATA environment variable.
;
; Examples:
;
; HitLogFileName=HitLog.%DATE%.txt
; HitLogFileName=%TEMP%\AcrylicDNSProxyHitLog.%DATE%.txt
;
HitLogFileName=
;
; The filter (a combination of one or more of the status codes explained above) which controls what gets written into
; the hit log.
;
HitLogFileWhat=XHCF
;
; You can enable the full dump (in addition to the DNS format dissections explained above) of DNS requests and responses
; into the hit log by choosing Yes instead of No.
;
HitLogFullDump=No
;
; The maximum number of hit log items that can be kept in memory before they are flushed to disk. For performance
; reasons the hit log is flushed to disk only when the hit log memory buffer is full, when Acrylic is stopped or when
; the system is shutdown, therefore you might experience a delay from when a DNS request or response is received to when
; its details get written into the hit log.
;
HitLogMaxPendingHits=512
;
; ALLOWING REQUESTS FROM OTHER COMPUTERS
;
; Although for security reasons the default behaviour of Acrylic is to refuse to handle requests coming from other
; computers, it is possible to specify below in the AllowedAddressesSection a list of IP addresses (wildcards are
; allowed) from which can come requests that Acrylic is allowed to handle. You have to specify a different key name for
; each entry, like in the following example:
;
; [AllowedAddressesSection]
; IP1=192.168.45.254 -- A single IP address
; IP2=192.168.44.100 -- Another single IP address
; IP3=192.168.100.* -- All addresses starting with 192.168.100
; IP4=172.16.* -- All addresses starting with 172.16
;
; Although not recommended for security reasons you can also allow Acrylic to handle requests coming from any IP
; address, like in the following example:
;
; [AllowedAddressesSection]
; IP1=*
;
; You must also create a firewall rule to allow incoming traffic directed to the two Acrylic executables:
; "AcrylicService.exe" and "AcrylicConsole.exe".
;
[AllowedAddressesSection]
```

بلوک Tertiary سروری هست که خودم با PHP ساختم. همونطور که می بینید در همه بلوک های کد بالا DNS رو پاک کردم و همه DoH هستند. از منوی File گزینه Save رو بزنید ازتون می پرسه که می خواهید ویندوز سرویس ریستارت بشه، گزینه yes رو بزنید و به عنوان Acrylic UI دقت کنید که یک لحظه می نویسه Not Running و بعدش می نویسه Running

3. تنظیم شبکه برای استفاده از دی ان اس لوکال اکریلیک
   
   فقط کافیه در تنظیمات دی ان اس کارتهای شبکه ویندوز همانی که با آن به اینترنت وصل می شوید (وای فای، یا ...) یک آی پی قرار بدین:
   
   ```text
   127.0.0.1
   ```
   
   که خیلی ساده است. در هر صورت برای تمامی ویندوزها در سایت رسمی آموزش گام به گام تصویری هم وجود داره:
   
   - [Windows 10](https://mayakron.altervista.org/support/acrylic/Windows10Configuration.htm)
   - [Windows 8](https://mayakron.altervista.org/support/acrylic/Windows8Configuration.htm)
   - [Windows 7](https://mayakron.altervista.org/support/acrylic/Windows7Configuration.htm)
   - [Windows Vista](https://mayakron.altervista.org/support/acrylic/WindowsVistaConfiguration.htm)
   - [Windows XP](https://mayakron.altervista.org/support/acrylic/WindowsXPConfiguration.htm)
   - [Windows 2000](https://mayakron.altervista.org/support/acrylic/Windows2000Configuration.htm)

در حالت عادی وقتی که اکریلیک نصب **نباشه** و تنظیم هر دی ان اس مستقیما داخل تنظیمات شبکه وجود داشته باشه،  [درخواستهای دی ان اس کاملا آسیب پذیر هستند](https://en.wikipedia.org/wiki/DNS_hijacking). در این حالت فقط به یک یا دو آدرس سرور وصل می شویم،  به این شکل:

```text
                             ||
                             ||
                             ||
  WINDOWS MACHINE            ||          INTERNET    
                             ||
+-----------------+          ||
|APPLICATION      |          ||
|(Chome, IE, ...) |          ||
+-----------------+          ||                 --DNS Hijacking 
     |                       ||               -/     
     |   DNS Request (Do53)  ||            --/       
     |                       ||          -/          
     v                       ||       --/            
+------------------+         ||    --/               
| Network Adaptor  |         ||  -/    +-----------+ 
| (WIFI, Lan, ...) |---------||</----->|DNS server | 
|                  |DNS Request (Do53) |8.8.8.8    | 
+------------------+         ||        |1.1.1.1    | 
                             ||        |...        | 
                             ||        +-----------+ 
                             ||
                             ||
```

اما حالا درخواست DNS همه نرم افزارها در حضور اکریلیک و طی مراحل گفته شده به این شکل امن می شود:

```text
                                  ||
                                  ||
    WINDOWS MACHINE               ||             INTERNET     
                                  ||
                                  ||
                                  ||
                                  ||
+-------------------+             ||         +---------------+ 
| Application       |             ||    |--->|  DoH SERVER 1 | 
| (Chrome, IE, ...) |             ||    |    +---------------+ 
+-------------------+             ||    |    +---------------+ 
       |                          ||    |--->|  DoH SERVER 2 | 
       |Dns Request(Do53)         ||    |    +---------------+ 
       v                          ||    |            .         
+-------------------+             ||    |            .         
| Network Adapter   |             ||    |            .         
| (Wifi, Lan, ...)  |             ||    |    +---------------+ 
+-------------------+             ||    |--->|  DoH SERVER N | 
       |                          ||    |    +---------------+ 
       |Dns Request(Do53)         ||    | 
       v                          ||    | 
+-------------------+             ||    | 
| Acrylic DNS Proxy |             ||    | 
| Local DNS server  | DoH Request ||    | 
| port:53           |-------------||--->| 
| IP:127.0.0.1      |             ||
+-------------------+             ||
                                  ||
```

4. تست درست بودن تنظیمات
   
   این کار ساده است. کافیست داخل مرورگرتان به یک سایت مراجعه کنید! اما توصیه می کنم قبل از اینکار ابتدا کارهای زیر را انجام دهید:
   
   کش اکریلیک و سیستم را پاک کنید. برای پاک کردن کش اکریلیک در Acrylic UI از منوی Actions گزینه Purge Acrylic Cache Data را بزنید. برای پاک کردن کش شبکه دستور زیر را بصورت ادمین اجرا کنید:
   
   ```bash
   ipconfig /flushdns
   ```
   
   داخل مرورگر هم از حالت Private browsing یا Incognito استفاده کنید (از ترکیب کلید های Ctrl+shift+n در بیشتر مرورگرها میشه استفاده کرد)
   
   توصیه می کنم از [نرم افزار  Dns Jumper](https://www.sordum.org/7952/dns-jumper-v2-2/) استفاده کنید چرا که کارها را خیلی راحت تر می کند، هم برای تست کردن، هم خالی کردن کش و هم برای تنظیم دی ان اس روی تمامی اینترفیس های شبکه در ویندوز.

   ## تداخل با داکر
   اگر اول روی سیستم خودتون داکر داشته باشید، اکریلیک کار نمی کند ولی خطا هم نشان نمی دهد، اگر اول اکریلیک داشته باشید و بخواهید داکر نصب کنید، داکر نصب نمی شود و با خطا مواجه می شوید. راه حل این مشکل تنظیم پورت 53 روی یک آی پی خاص روی سیستم شماست، در کانفیگی که در این مطلب مشاهده می کنید، اکریلیک روی تمامی آی پی ها با تنظیم زیر به پورت 53 گوش می دهد:
   
   ```ini
   LocalIPv4BindingAddress=0.0.0.0
   ```
   باید بجای 0.0.0.0 یکی از آی پی های ویندوز خودتون جایگزینش کنید. برای اینکه ببینید چه آی پی هایی امکان تنظیم دارند از دستور زیر استفاده کنید:
   
   ```bash
   ipconfig
   ```
   
   یا اینکه کارت های شبکه را در بخش تنظیمات شبکه ویندوز مشاهده کنید و از آنجا به دنبال آدرس مناسب بگردید. همچنین باید تنظیم گوش دادن به آی پی نسخه 6 را در کانفیگ غیر فعال کنید. می توانید خط مربوطه را پاک کنید ولی بهتر است ابتدای آن ; قرار بدهید و آن را اصطلاحا "کامنت کنید" یعنی به شکل زیر:
   
   ```ini
   ; LocalIPv6BindingAddress=0:0:0:0:0:0:0:0
   ```
   
   در آخر باید آی پی که ست کردین رو بجای 127.0.0.1 توی تنظیمات دی ان اس های شبکه استفاده کنید. 