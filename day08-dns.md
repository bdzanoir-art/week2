# Day 8: DNS records

| Record | Purpose | Example |
|---|---|---|
| A | Domain to IPv4 | site.com -> 93.184.216.34 |
| AAAA | Domain to IPv6 | site.com -> 2606:2800::1 |
| CNAME | Alias to another domain | www.site.com -> site.com |
| MX | Mail server, with priority | 10 mail.site.com |
| TXT | Free text: SPF, DKIM, domain verification | "v=spf1 include:_spf.google.com ~all" |

## Resolution steps
1. Browser/OS cache and hosts file
2. Recursive resolver (ISP or 8.8.8.8)
3. Root server -> gives the TLD servers
4. TLD server (.com) -> gives the authoritative nameserver
5. Authoritative server -> gives the final record
6. Resolver caches the answer for the TTL

## Check yourself
1. A maps a name to an IP. CNAME maps a name to another name, which is then resolved.
2. MX.
3. The authoritative nameserver.
4. How long a resolver may cache the answer.
5. SPF and DKIM are plain text published in the domain's DNS, and mail servers read them to verify the sender.
