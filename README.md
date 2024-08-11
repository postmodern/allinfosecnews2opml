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
https://www.bankinfosecurity.com/rss-feeds: SSL_connect returned=1 errno=0 peeraddr=50.56.167.254:443 state=error: certificate verify failed (unable to get local issuer certificate)
https://www.offensive-security.com/blog/feed: 404 Not Found                     
https://cloud7.news/feed/gn: SSL_connect returned=1 errno=0 peeraddr=23.111.175.212:443 state=error: certificate verify failed (hostname mismatch)                         
https://www.lawfareblog.com/taxonomy/term/5798/all/feed: 403 Forbidden          
https://www.cshub.com/rss/articles: 403 Forbidden                               
https://www.cshub.com/rss/news: 403 Forbidden                                   
https://www.darkreading.com/rss_simple.asp: 404 Not Found                       
https://www.databreachtoday.co.uk/rss-feeds: SSL_connect returned=1 errno=0 peeraddr=50.56.167.254:443 state=error: certificate verify failed (unable to get local issuer certificate)
http://www.exploitalert.com/feed/: 405 Method Not Allowed
https://www.forbes.com/cybersecurity/feed/: 404 Not Found
https://www.govinfosecurity.com/rss-feeds: SSL_connect returned=1 errno=0 peeraddr=50.56.167.254:443 state=error: certificate verify failed (unable to get local issuer certificate)
https://hackingvision.com/feed/: HTTP status code 522
https://danielmiessler.com/category/information-security/feed: 404 Not Found
https://www.lawfareblog.com/rss.xml: 403 Forbidden
https://www.lunasec.io/docs/blog/rss.xml: Failed to open TCP connection to www.lunasec.io:443 (getaddrinfo: No address associated with hostname)
https://resources.infosecinstitute.com/topics/malware-analysis/feed: 404 Not Found
https://api.msrc.microsoft.com/update-guide/rss: 404 Not Found
https://resources.infosecinstitute.com/topics/news/feed: 404 Not Found
https://portswigger.net/blog/rss: 404 Not Found
https://portswigger.net/research/rss: 404 Not Found
https://www.rtcsec.com/index.xml: 404 Not Found
https://www.secureworks.com/rss?feed=research: 404 Not Found
https://shellsharks.github.io/feed.xml: 404 Not Found
https://www.theinformation.com/feed: 403 Forbidden
https://catless.ncl.ac.uk/risksrss2.xml: SSL_connect returned=1 errno=0 peeraddr=128.240.212.26:443 state=error: certificate verify failed (self-signed certificate in certificate chain)
https://www.vice.com/en/rss/topic/hacking: 404 Not Found
https://xploitlab.com/feed: Failed to open TCP connection to xploitlab.com:443 (Connection refused - connect(2) for "xploitlab.com" port 443)
https://zetter.substack.com/feed: 404 Not Found
https://7ms.us/rss/: 404 Not Found
https://surveillancereport.tech/feed: 404 Not Found
https://feeds.simplecast.com/vRu9TC9N: 404 Not Found
```

[allinfosecnews.com]: https://allinfosecnews.com/
[allinfosecnews_sources]: https://github.com/foorilla/allinfosecnews_sources
[4rv0]: https://github.com/4rv0
[allinfosecnews2opml-python]: https://github.com/4rv0/allinfosecnews2opml
[OPML]: https://opml.org/
[Ruby]: https://www.ruby-lang.org/
