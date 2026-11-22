# Day 9 - DNS lab: dig and nslookup

## Record types

| Type | Stores | Example |
|---|---|---|
| A | IPv4 address | github.com -> 140.82.121.3 |
| AAAA | IPv6 address | wikipedia.org -> 2a02:ec80:600:ed1a::1 |
| MX | Mail server (lowest number = first) | github.com -> 0 github-com.mail.protection.outlook.com |
| NS | Name servers of the domain | wikipedia.org -> ns0/ns1/ns2.wikimedia.org |
| TXT | Free text (SPF, domain verification) | v=spf1 include:_spf.google.com ~all |

## Commands

| Command | Purpose |
|---|---|
| dig github.com MX | Ask for one record type |
| dig +short google.com | Show only the answer |
| dig @8.8.8.8 google.com | Ask a specific DNS server |
| nslookup -type=NS wikipedia.org | Same idea with nslookup |
| nslookup github.com 8.8.8.8 | Use Google DNS instead of the default |
| dig +trace github.com | Follow the full lookup from the root |

## dig +trace github.com

1. Root servers (.)
2. .com servers (a.gtld-servers.net ...)
3. github.com name servers (awsdns, nsone)
4. Final answer: A 140.82.121.3, TTL 60

Full output: [day-09-dig-trace.txt](day-09-dig-trace.txt)

## Key ideas

- TTL = how many seconds an answer can be cached
- "Non-authoritative answer" = answer came from a cache
- Big TXT answers do not fit in UDP, so nslookup retries in TCP
- Timeout on the local DNS server: test with dig @8.8.8.8
- Typosquatting: authropic.com is a different domain from anthropic.com

## Check yourself

1. What does an MX record store?
   The mail server of the domain. The lowest number has the highest priority.
2. What is TTL?
   The number of seconds an answer can be kept in cache.
3. What does "Non-authoritative answer" mean?
   The answer came from a cache, not from the domain's own name server.
4. What is the order of dig +trace?
   Root, then .com, then the domain's name servers, then the final answer.
5. Why is typosquatting dangerous?
   A similar domain can be used for phishing when someone mistypes.
