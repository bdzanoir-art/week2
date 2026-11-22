# Day 9 - DNS lab: dig and nslookup

## Record types
- A = IPv4 address
- AAAA = IPv6 address
- MX = mail server (lowest number = highest priority)
- NS = name servers responsible for the domain
- TXT = free text (SPF, domain verification)

## Key ideas
- TTL = how many seconds the answer can be cached
- "Non-authoritative answer" = answer came from a cache, not from the domain's own server
- Large TXT answers do not fit in UDP, so nslookup retries in TCP mode
- Timeouts on the local DNS server: test with a public one (dig @8.8.8.8 google.com)

## dig +trace github.com
1. Root servers (.)
2. .com TLD servers (a.gtld-servers.net ...)
3. github.com name servers (awsdns, nsone)
4. Final answer: github.com A 140.82.121.3 (TTL 60)

DS / RRSIG / NSEC3 lines are DNSSEC records.

## Security lesson
Mistyping a domain (authropic.com instead of anthropic.com) can lead to a
different, real domain. This is typosquatting, used in phishing.

## Commands used
    dig github.com MX
    dig wikipedia.org AAAA
    dig anthropic.com TXT
    nslookup google.com
    nslookup -type=MX github.com
    nslookup -type=NS wikipedia.org
    nslookup -type=TXT anthropic.com
    nslookup github.com 8.8.8.8
    dig +trace github.com
