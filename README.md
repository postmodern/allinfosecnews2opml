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

[allinfosecnews.com]: https://allinfosecnews.com/
[allinfosecnews_sources]: https://github.com/foorilla/allinfosecnews_sources
[4rv0]: https://github.com/4rv0
[allinfosecnews2opml-python]: https://github.com/4rv0/allinfosecnews2opml
[OPML]: https://opml.org/
[Ruby]: https://www.ruby-lang.org/
