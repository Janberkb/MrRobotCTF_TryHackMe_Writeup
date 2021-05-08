# Nmap Scan Output

# Nmap 7.91 scan initiated Thu May 6 13:37:12 2021 as: nmap -sS -sV -O --script vuln -oN ./output_vuln.txt 10.10.104.30

Nmap scan report for 10.10.104.30
Host is up (0.064s latency).
Not shown: 997 filtered ports
PORT    STATE  SERVICE  VERSION
22/tcp  closed ssh
80/tcp  open   http     Apache httpd
| http-csrf:
| Spidering limited to: maxdepth=3; maxpagecount=20; withinhost=10.10.104.30
|   Found the following possible CSRF vulnerabilities:
|
|     Path: [http://10.10.104.30:80/js/vendor/null,this.tags.length=0},t.get=function(){if(0==this.tags.length)return](http://10.10.104.30/js/vendor/null,this.tags.length=0%7d,t.get=function()%7bif(0==this.tags.length)return)
|     Form id:
|     Form action: [http://10.10.104.30/](http://10.10.104.30/)
|
|     Path: [http://10.10.104.30:80/js/vendor/null,this.tags.length=0},t.get=function(){if(0==this.tags.length)return](http://10.10.104.30/js/vendor/null,this.tags.length=0%7d,t.get=function()%7bif(0==this.tags.length)return)
|     Form id:
|     Form action: [http://10.10.104.30/](http://10.10.104.30/)
|
|     Path: [http://10.10.104.30:80/js/BASE_URL+"/live/"](http://10.10.104.30/js/BASE_URL+%22/live/%22));this.firstBoot?(this.firstBoot=!1,this.track.omni("Email
|     Form id:
|     Form action: [http://10.10.104.30/](http://10.10.104.30/)
|
|     Path: [http://10.10.104.30:80/js/BASE_URL+"/live/"](http://10.10.104.30/js/BASE_URL+%22/live/%22));this.firstBoot?(this.firstBoot=!1,this.track.omni("Email
|     Form id:
|     Form action: [http://10.10.104.30/](http://10.10.104.30/)
|
|     Path: [http://10.10.104.30:80/js/u;c.appendChild(o);'+(n?'o.c=0;o.i=setTimeout(f2,100)':'')+'](http://10.10.104.30/js/u;c.appendChild(o);'+(n?%27o.c=0;o.i=setTimeout(f2,100)%27:%27%27)+%27)}}catch(e){o=0}return
|     Form id:
|     Form action: [http://10.10.104.30/](http://10.10.104.30/)
|
|     Path: [http://10.10.104.30:80/js/u;c.appendChild(o);'+(n?'o.c=0;o.i=setTimeout(f2,100)':'')+'](http://10.10.104.30/js/u;c.appendChild(o);'+(n?%27o.c=0;o.i=setTimeout(f2,100)%27:%27%27)+%27)}}catch(e){o=0}return
|     Form id:
|     Form action: [http://10.10.104.30/](http://10.10.104.30/)
|
|     Path: [http://10.10.104.30:80/js/rs;if(s.useForcedLinkTracking||s.bcf){if(!s."+"forcedLinkTrackingTimeout)s.forcedLinkTrackingTimeout=250;setTimeout('if(window.s_c_il)window.s_c_il['+s._in+'].bcr()',s.forcedLinkTrackingTimeout);}else](http://10.10.104.30/js/rs;if(s.useForcedLinkTracking%7C%7Cs.bcf)%7Bif(!s.%22+%22forcedLinkTrackingTimeout)s.forcedLinkTrackingTimeout=250;setTimeout('if(window.s_c_il)window.s_c_il%5B'+s._in+'%5D.bcr()',s.forcedLinkTrackingTimeout);%7Delse)
|     Form id:
|     Form action: [http://10.10.104.30/](http://10.10.104.30/)
|
|     Path: [http://10.10.104.30:80/js/rs;if(s.useForcedLinkTracking||s.bcf){if(!s."+"forcedLinkTrackingTimeout)s.forcedLinkTrackingTimeout=250;setTimeout('if(window.s_c_il)window.s_c_il['+s._in+'].bcr()',s.forcedLinkTrackingTimeout);}else](http://10.10.104.30/js/rs;if(s.useForcedLinkTracking%7C%7Cs.bcf)%7Bif(!s.%22+%22forcedLinkTrackingTimeout)s.forcedLinkTrackingTimeout=250;setTimeout('if(window.s_c_il)window.s_c_il%5B'+s._in+'%5D.bcr()',s.forcedLinkTrackingTimeout);%7Delse)
|     Form id:
|     Form action: [http://10.10.104.30/](http://10.10.104.30/)
|
|     Path: [http://10.10.104.30:80/wp-login.php](http://10.10.104.30/wp-login.php)
|     Form id: loginform
|_    Form action: [http://10.10.104.30/wp-login.php](http://10.10.104.30/wp-login.php)
|_http-dombased-xss: Couldn't find any DOM based XSS.
|_http-server-header: Apache
|_http-stored-xss: Couldn't find any stored XSS vulnerabilities.
443/tcp open   ssl/http Apache httpd
|_http-csrf: Couldn't find any CSRF vulnerabilities.
|_http-dombased-xss: Couldn't find any DOM based XSS.
|_http-server-header: Apache
|_http-stored-xss: Couldn't find any stored XSS vulnerabilities.
|_sslv2-drown:
Device type: general purpose|specialized|storage-misc|WAP|broadband router|printer
Running (JUST GUESSING): Linux 3.X|5.X|4.X|2.6.X (91%), Crestron 2-Series (89%), HP embedded (89%), Asus embedded (88%)
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:5.4 cpe:/o:linux:linux_kernel:4 cpe:/o:crestron:2_series cpe:/h:hp:p2000_g3 cpe:/o:linux:linux_kernel:2.6.22 cpe:/o:linux:linux_kernel:2.6 cpe:/h:asus:rt-n56u
Aggressive OS guesses: Linux 3.10 - 3.13 (91%), Linux 5.4 (91%), Linux 3.10 - 4.11 (90%), Linux 3.12 (90%), Linux 3.13 (90%), Linux 3.13 or 4.2 (90%), Linux 3.2 - 3.5 (90%), Linux 3.2 - 3.8 (90%), Linux 4.2 (90%), Linux 4.4 (90%)
No exact OS matches for host (test conditions non-ideal).

OS and Service detection performed. Please report any incorrect results at [https://nmap.org/submit/](https://nmap.org/submit/) .

# Nmap done at Thu May 6 13:39:09 2021 -- 1 IP address (1 host up) scanned in 117.27 seconds