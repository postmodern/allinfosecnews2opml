# allinfosecnews2opml (Ruby version)

So there used to be this website called [allinfosecnews.com] which aggregated a
ton of Security RSS feeds. Then they announced they were shutting down, but were
nice enough to publish their curated list of RSS feeds on
[GitHub][allinfosecnews_sources]. Only problem is they only gave us a markdown
file. So a community member, [4rv0], hacked together a
[Python script][allinfosecnews2opml-python] to parse the markdown file and
generate an [OPML] file which one could import into their choice of RSS feed
reader. Cool! So then I got to thinking, why not try rewriting the script in
[Ruby], and maybe add better filtering. While I ended up using an ugly regex to
parse the markdown list entries, it came out alright.

**Edit:** you might want to also look at
[ShellShark's InfoSec Blogs List][shellsharks-feed],
which has *far more* entries; although it's missing some feeds from
allinfosecnews.

[shellsharks-feed]: https://shellsharks.com/infosec-blogs

**Edit 2:** I attempted to merge [ShellShark's InfoSec Blog List][shellsharks-feed]
with the `allinfosecnews.opmp` file. See [allinfosec-combined.opml](allinfosec-combined.opml).

## Features

* Only includes the news RSS feeds.
* Filters out spammy websites (ex: Hakin9 and KitSploit) and
  non-InfoSec keywords (ex: blockchain, "crypto", "AI", etc).
* Checks if the RSS feed URLs are still alive.

## Requirements

* [Ruby] >= 3.2.0

## Install

```
$ bundle install
```

## Synopsis

```
./allinfosecnews2opml
```

## Dead Feeds

While testing my script I noticed that many of the feeds could not be loaded
in my RSS feed reader. So I added code to check the feed URLs. Appears that many
of the feeds are either 404 (moved without a redirect) or their TLS certificates
have expired.

```
https://www.akamai.com/blog: Net::ReadTimeout with #<TCPSocket:(closed)>
https://www.bankinfosecurity.com/rss-feeds: SSL_connect returned=1 errno=0 peeraddr=50.56.167.254:443 state=error: certificate verify failed (unable to get local issuer certificate)
https://www.offensive-security.com/blog/feed: 404 => Net::HTTPNotFound for https://www.offsec.com/blog/feed -- unhandled response
https://www.zerofox.com/blog/feed: 500 => Net::HTTPInternalServerError for https://www.zerofox.com/blog/feed/ -- unhandled response
https://cloud7.news/feed/gn: SSL_connect returned=1 errno=0 peeraddr=23.111.175.212:443 state=error: certificate verify failed (hostname mismatch)
https://cofense.com/feed/: 404 => Net::HTTPNotFound for https://cofense.com/feed/ -- unhandled response
https://www.lawfareblog.com/taxonomy/term/5798/all/feed: 403 => Net::HTTPForbidden for https://www.lawfaremedia.org/taxonomy/term/5798/all/feed -- unhandled response
https://www.darkreading.com/rss_simple.asp: 404 => Net::HTTPNotFound for https://www.darkreading.com/rss_simple.asp -- unhandled response
https://www.databreachtoday.co.uk/rss-feeds: SSL_connect returned=1 errno=0 peeraddr=50.56.167.254:443 state=error: certificate verify failed (unable to get local issuer certificate)
https://www.forbes.com/cybersecurity/feed/: 404 => Net::HTTPNotFound for https://www.forbes.com/cybersecurity/feed/ -- unhandled response
https://www.govinfosecurity.com/rss-feeds: SSL_connect returned=1 errno=0 peeraddr=50.56.167.254:443 state=error: certificate verify failed (unable to get local issuer certificate)
https://hackingvision.com/feed/: 522 =>  for https://hackingvision.com/feed/ -- unhandled response
https://danielmiessler.com/category/information-security/feed: 404 => Net::HTTPNotFound for https://danielmiessler.com/category/information-security/feed -- unhandled response
https://www.lawfareblog.com/rss.xml: 404 => Net::HTTPNotFound for https://www.lawfaremedia.org/rss.xml -- unhandled response
https://www.lunasec.io/docs/blog/rss.xml: Failed to open TCP connection to www.lunasec.io:443 (getaddrinfo: No address associated with hostname)
https://resources.infosecinstitute.com/topics/malware-analysis/feed: 404 => Net::HTTPNotFound for https://www.infosecinstitute.com/topics/malware-analysis/feed/ -- unhandled response
https://resources.infosecinstitute.com/topics/news/feed: 404 => Net::HTTPNotFound for https://www.infosecinstitute.com/topics/news/feed/ -- unhandled response
https://www.rtcsec.com/index.xml: 404 => Net::HTTPNotFound for https://www.enablesecurity.com/index.xml/ -- unhandled response
https://www.reddit.com/r/netsec: 504 => Net::HTTPGatewayTimeout for https://www.reddit.com/r/netsec -- unhandled response
https://www.secureworks.com/rss?feed=research: 404 => Net::HTTPNotFound for https://www.secureworks.com/rss?feed=research -- unhandled response
https://shellsharks.github.io/feed.xml: 404 => Net::HTTPNotFound for https://shellsharks.com/feed.xml -- unhandled response
https://www.theinformation.com/feed: 403 => Net::HTTPForbidden for https://www.theinformation.com/feed -- unhandled response
https://therecord.media/feed/: 404 => Net::HTTPNotFound for https://therecord.media/feed -- unhandled response
https://catless.ncl.ac.uk/risksrss2.xml: SSL_connect returned=1 errno=0 peeraddr=128.240.212.26:443 state=error: certificate verify failed (self-signed certificate in certificate chain)
http://www.rssmix.com/: 522 =>  for http://www.rssmix.com/ -- unhandled response
https://www.vice.com/en/rss/topic/hacking: 404 => Net::HTTPNotFound for https://www.vice.com/en/r/ss/tag/hacking -- unhandled response
https://xploitlab.com/feed: connection refused: xploitlab.com:80
https://zetter.substack.com/feed: 404 => Net::HTTPNotFound for https://zetter.substack.com/feed -- unhandled response
```

### shellsharks-feedly-rss.opml

Attempting to normalize and merge [ShellShark's InfoSec Blog List][shellsharks-feed] epic RSS feed list resulted in these dead feed URLs and website URLs being discovered:

```
https://www.akamai.com/blog: Net::ReadTimeout with #<TCPSocket:(closed)>
http://www.rssmix.com/: 522 =>  for http://www.rssmix.com/ -- unhandled response
https://www.hbgary.com/feed/rss/: Failed to open TCP connection to www.hbgary.com:443 (execution expired)
http://blog.mimecast.com/feed/: Failed to open TCP connection to blog.mimecast.com:80 (getaddrinfo: Name or service not known)
http://blog.deepinstinct.com/feed/: Failed to open TCP connection to blog.deepinstinct.com:80 (getaddrinfo: Name or service not known)
http://digital-forensics.sans.org/blog/feed/: 404 => Net::HTTPNotFound for https://www.sans.org/digital-forensics-incident-response/blog/feed/ -- unhandled response
http://radare.today/index.xml: 404 => Net::HTTPNotFound for https://radare.today/index.xml -- unhandled response
https://dolosgroup.io/welcome?format=rss: SSL_connect returned=1 errno=0 peeraddr=198.49.23.144:443 state=error: certificate verify failed (hostname mismatch)
http://vrt-sourcefire.blogspot.com/feeds/posts/default: 404 => Net::HTTPNotFound for http://vrt-sourcefire.blogspot.com/feeds/posts/default -- unhandled response
http://www.offensive-security.com/feed/: cannot deflate brotli-encoded response. Please install and require the 'brotli' gem.
http://www.sensepost.com/blog/index.rss: 404 => Net::HTTPNotFound for https://sensepost.com/blog/index.rss -- unhandled response
http://samcurry.net/feed/: 404 => Net::HTTPNotFound for https://samcurry.net/feed -- unhandled response
http://feeds.feedburner.com/netsparker: 404 => Net::HTTPNotFound for http://feeds.feedburner.com/netsparker -- unhandled response
http://blog.csnc.ch/feed/: Failed to open TCP connection to blog.csnc.ch:80 (getaddrinfo: Name or service not known)
https://www.signalscorps.com/feed.xml: Failed to open TCP connection to www.signalscorps.com:443 (getaddrinfo: Name or service not known)
http://pouyadarabi.blogspot.com/feeds/posts/default: 404 => Net::HTTPNotFound for http://pouyadarabi.blogspot.com/feeds/posts/default -- unhandled response
https://musings.konundrum.org/feed.xml: 404 => Net::HTTPNotFound for https://musings.konundrum.org/feed.xml -- unhandled response
https://microsoftedge.github.io: 404 => Net::HTTPNotFound for https://microsoftedge.github.io/ -- unhandled response
http://localhost:4000/: connection refused: localhost:80
https://maxwelldulin.invades.space/blog-rss.xml: Failed to open TCP connection to maxwelldulin.invades.space:443 (getaddrinfo: Name or service not known)
https://darrenmartyn.ie/feed/: SSL_connect returned=1 errno=0 peeraddr=192.0.78.24:443 state=error: certificate verify failed (hostname mismatch)
https://oxagast.org/feed.xml: Failed to open TCP connection to oxagast.org:443 (getaddrinfo: Name or service not known)
https://www.revblock.dev/rss/: SSL_connect returned=1 errno=0 peeraddr=151.101.195.7:443 state=error: certificate verify failed (hostname mismatch)
http://blog.ahmednabeel.com/rss/: Failed to open TCP connection to blog.ahmednabeel.com:80 (getaddrinfo: Name or service not known)
http://blog.justinsteven.com/atom.xml: 404 => Net::HTTPNotFound for https://www.justinsteven.com/atom.xml -- unhandled response
http://172.31.17.250/: Failed to open TCP connection to 172.31.17.250:80 (execution expired)
http://blog.sipvicious.org/feeds/posts/default: 404 => Net::HTTPNotFound for https://www.enablesecurity.com/index.xml/ -- unhandled response
https://www.rffuste.com/feed/: SSL_connect returned=1 errno=0 peeraddr=162.159.153.4:443 state=error: ssl/tls alert handshake failure (SSL alert number 40)
https://anthok.com/posts/index.xml: Failed to open TCP connection to anthok.com:443 (execution expired)
https://www.noob2pro4n6.com/feeds/posts/default?alt=rss: Failed to open TCP connection to www.noob2pro4n6.com:443 (getaddrinfo: Name or service not known)
https://cyber.wtf/feed/: SSL_connect returned=1 errno=0 peeraddr=165.227.246.193:443 state=error: certificate verify failed (unable to get local issuer certificate)
https://opensourceweekly.org/index.xml: 403 => Net::HTTPForbidden for https://kerkour.com/ -- unhandled response
http://www.securityartwork.es/feed/?lang=en: Failed to open TCP connection to www.securityartwork.es:80 (execution expired)
https://bugra.ninja/index.xml: 522 =>  for https://bugra.ninja/index.xml -- unhandled response
https://showipintbri.github.io/feed.xml: 404 => Net::HTTPNotFound for https://showipintbri.github.io/feed.xml -- unhandled response
https://medium.com/feed/@interc3pt3r: 404 => Net::HTTPNotFound for https://medium.com/feed/@interc3pt3r -- unhandled response
http://dnlongen.blogspot.com/feeds/posts/default: 404 => Net::HTTPNotFound for http://dnlongen.blogspot.com/feeds/posts/default -- unhandled response
https://medium.com/feed/@vflexo: 404 => Net::HTTPNotFound for https://medium.com/feed/@vflexo -- unhandled response
https://zero-s4n.hashnode.dev/rss.xml: 404 => Net::HTTPNotFound for https://zero-s4n.hashnode.dev/rss.xml -- unhandled response
https://www.neil-fox.github.io: SSL_connect returned=1 errno=0 peeraddr=185.199.109.153:443 state=error: certificate verify failed (hostname mismatch)
https://attackshipsonfi.re/feed: Failed to open TCP connection to attackshipsonfi.re:443 (getaddrinfo: No address associated with hostname)
https://cyberkitties.github.io/: 404 => Net::HTTPNotFound for https://cyberkitties.github.io/ -- unhandled response
http://touchmymalware.blogspot.com/feeds/posts/default: 404 => Net::HTTPNotFound for http://touchmymalware.blogspot.com/feeds/posts/default -- unhandled response
https://courk.fr/index.php/feed/: Failed to open TCP connection to courk.fr:443 (getaddrinfo: Name or service not known)
http://hulkvision.gitlab.io/: 404 => Net::HTTPNotFound for http://hulkvision.gitlab.io/ -- unhandled response
https://blog.xpnsec.com/rss/: 404 => Net::HTTPNotFound for https://blog.xpnsec.com/rss/ -- unhandled response
https://www.codereversing.com/blog: 404 => Net::HTTPNotFound for https://www.codereversing.com/blog -- unhandled response
https://www.secu.ninja/feed/: 404 => Net::HTTPNotFound for https://www.secu.ninja/feed/ -- unhandled response
https://rw.md/feed.xml: Failed to open TCP connection to rw.md:443 (getaddrinfo: No address associated with hostname)
https://doubleagent.net/feed.xml: 404 => Net::HTTPNotFound for https://doubleagent.net/feed.xml/ -- unhandled response
https://medium.com/feed/@hetroublemakr: 404 => Net::HTTPNotFound for https://medium.com/feed/@hetroublemakr -- unhandled response
https://emily.id.au/blog: 404 => Net::HTTPNotFound for https://emily.id.au/blog -- unhandled response
https://bmcder.com/blog?format=rss: 404 => Net::HTTPNotFound for https://bmcder.com/blog?format=rss -- unhandled response
https://blog.notso.pro/feed.xml: 404 => Net::HTTPNotFound for https://blog.notso.pro/feed.xml -- unhandled response
https://medium.com/feed/@noob.assassin: 404 => Net::HTTPNotFound for https://medium.com/feed/@noob.assassin -- unhandled response
https://neonsea.uk/blog/feed.xml: SSL_connect returned=1 errno=0 peeraddr=150.230.23.190:443 state=error: tlsv1 alert internal error (SSL alert number 80)
https://tbhaxor.com/rss/: 403 => Net::HTTPForbidden for https://tbhaxor.com/rss/ -- unhandled response
https://dfworks.com: Failed to open TCP connection to dfworks.com:443 (execution expired)
https://nareshlamgade.com.np/feed/: 500 => Net::HTTPInternalServerError for https://nareshlamgade.com.np/feed/ -- unhandled response
https://linxz.tech/post/index.xml: Failed to open TCP connection to linxz.tech:443 (getaddrinfo: Name or service not known)
https://ben.the-collective.net/feed/: 500 => Net::HTTPInternalServerError for https://ben.the-collective.net/feed/ -- unhandled response
https://blog.haboob.sa/blog?format=rss: 404 => Net::HTTPNotFound for https://blog.haboob.sa/blog?format=rss -- unhandled response
https://exact.realty/blog/posts/index.xml: Failed to open TCP connection to exact.realty:443 (getaddrinfo: Name or service not known)
https://blog.0xrishabh.dev/feed.xml: 404 => Net::HTTPNotFound for https://blog.0xrishabh.dev/feed.xml -- unhandled response
http://www.terminal23.net/atom.xml: 404 => Net::HTTPNotFound for http://www.terminal23.net/atom.xml -- unhandled response
https://pardonmynoot.com/index.xml: 530 =>  for https://pardonmynoot.com/index.xml -- unhandled response
https://medium.com/feed/@nmochea: 404 => Net::HTTPNotFound for https://medium.com/feed/@nmochea -- unhandled response
https://zer0tru5t.com/feed/: Failed to open TCP connection to zer0tru5t.com:443 (execution expired)
https://www.omerlh.info/feed/: 404 => Net::HTTPNotFound for https://www.omerlh.info/feed/ -- unhandled response
https://saajan.bhujel.cyou/feed.xml: SSL_connect returned=1 errno=0 peeraddr=50.18.142.31:443 state=error: certificate verify failed (hostname mismatch)
http://0.0.0.0:4000/: connection refused: 0.0.0.0:80
https://cybercrimetech.com/feed.xml: 404 => Net::HTTPNotFound for https://cybercrimetech.com/feed.xml -- unhandled response
https://scumjr.github.io/feed.xml: 404 => Net::HTTPNotFound for https://scumjr.github.io/feed.xml -- unhandled response
https://laststandsecurity.co.uk/: Failed to open TCP connection to laststandsecurity.co.uk:443 (getaddrinfo: No address associated with hostname)
http://www.syfuhs.net/syndication.axd: SSL_connect returned=1 errno=0 peeraddr=40.118.235.113:443 state=error: certificate verify failed (certificate has expired)
https://ledz1996.gitlab.io/blog/blog/: 404 => Net::HTTPNotFound for https://ledz1996.gitlab.io/blog/blog/ -- unhandled response
http://niiconsulting.com/checkmate/feed/: 500 => Net::HTTPInternalServerError for https://niiconsulting.com/checkmate/feed/ -- unhandled response
https://cube0x0.github.io/feed.xml: 404 => Net::HTTPNotFound for https://cube0x0.github.io/feed.xml -- unhandled response
https://breakdawn.substack.com/feed: 404 => Net::HTTPNotFound for https://breakdawn.substack.com/feed -- unhandled response
https://jackfromeast.site/atom.xml: 404 => Net::HTTPNotFound for https://jackfromeast.site/atom.xml -- unhandled response
https://mechanicalsympathy.nl/index.xml: SSL_connect returned=1 errno=0 peeraddr=13.107.246.67:443 state=error: certificate verify failed (certificate has expired)
http://localhost:4000/: connection refused: localhost:80
https://redteamer.tips/feed/: SSL_connect returned=1 errno=0 peeraddr=92.222.139.190:443 state=error: certificate verify failed (hostname mismatch)
http://www.securelist.com/en/rss/allupdates: Failed to open TCP connection to www.securelist.com:80 (getaddrinfo: Name or service not known)
https://blog.isiraadithya.com/feed/: 404 => Net::HTTPNotFound for https://blog.isiraadithya.com/feed/ -- unhandled response
https://warandcode.com/index.xml: Failed to open TCP connection to warandcode.com:443 (getaddrinfo: Name or service not known)
https://nickbloor.co.uk/feed/: 403 => Net::HTTPForbidden for https://nickbloor.co.uk/feed/ -- unhandled response
https://blog.joeware.net: 500 => Net::HTTPInternalServerError for https://blog.joeware.net/ -- unhandled response
https://blog.fletchto99.com/: Failed to open TCP connection to blog.fletchto99.com:443 (getaddrinfo: No address associated with hostname)
https://medium.com/@mrnikhilsri?source=rss-e8316c785b9f------2: Failed to open TCP connection to blog.niksthehacker.com:443 (getaddrinfo: Name or service not known)
```

[allinfosecnews.com]: https://allinfosecnews.com/
[allinfosecnews_sources]: https://github.com/foorilla/allinfosecnews_sources
[4rv0]: https://github.com/4rv0
[allinfosecnews2opml-python]: https://github.com/4rv0/allinfosecnews2opml
[OPML]: https://opml.org/
[Ruby]: https://www.ruby-lang.org/
