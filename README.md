# OSINT Investigation: hackthissite.org

## Overview

This project documents a passive Open-Source Intelligence (OSINT) investigation of `hackthissite.org`.

The investigation used publicly available information from multiple OSINT and security-intelligence sources to examine the target's domain information, DNS infrastructure, subdomains, certificate data, passive DNS records, DNSSEC configuration, reputation observations, typosquatting possibilities, and technology-related information.

The purpose of the investigation was to develop practical experience in collecting, comparing, and interpreting publicly available information while maintaining a passive reconnaissance approach.

## Objectives

* Gather publicly available information about the target domain.
* Identify DNS records and associated infrastructure.
* Discover subdomains using multiple independent sources.
* Examine certificate and Certificate Transparency information.
* Review passive and historical DNS observations.
* Examine DNSSEC-related information.
* Review domain reputation observations from security-intelligence sources.
* Identify potential typosquatting or look-alike domains.
* Compare information from multiple sources to improve confidence in findings.
* Document observations, limitations, and lessons learned.

## Scope and Rules

### Scope

The investigation focused on publicly available information related to `hackthissite.org`, including:

* Domain and WHOIS information
* DNS records and DNS infrastructure
* Publicly discoverable subdomains
* Certificate and Certificate Transparency records
* Passive and historical DNS data
* DNSSEC information
* Public reputation observations
* Potential typosquatting domains
* Publicly available technology-related information

### Rules

* The investigation was conducted using publicly available information.
* No authentication or private accounts were accessed.
* No exploitation or vulnerability testing was performed.
* No attempts were made to gain unauthorized access to systems or services.
* Active enumeration was avoided during this OSINT investigation.
* Findings were compared across multiple sources where possible.
* Historical and passive data was treated as evidence of past or recorded observations, not automatic proof of current activity.

## Methodology

The investigation followed a passive reconnaissance workflow:

1. **Domain and WHOIS Research**
   Collected domain registration, registrar, nameserver, and IP registration information.

2. **DNS and Infrastructure Analysis**
   Examined A, MX, NS, TXT, SOA, and CAA records to understand the domain's DNS and hosting configuration.

3. **Subdomain Discovery**
   Used multiple passive sources to identify publicly associated subdomains and compared results between sources.

4. **Certificate Analysis**
   Examined the current TLS certificate and Certificate Transparency records to identify certificate-associated hostnames and historical observations.

5. **Passive DNS Analysis**
   Reviewed passive DNS data to identify current and historical DNS observations associated with the domain.

6. **DNSSEC Analysis**
   Examined publicly available DNSSEC information using DNSSEC analysis tools.

7. **Reputation Analysis**
   Reviewed security-intelligence observations from VirusTotal and recorded the results without treating vendor classifications as definitive proof of malicious activity.

8. **Typosquatting Analysis**
   Used a typosquatting analysis tool to identify domains that resemble the target domain.

9. **Technology Profiling**
   Reviewed publicly available technology-related information using BuiltWith.

10. **Cross-Verification**
    Compared findings from different sources to identify information that was independently observed by more than one source.

## Key Findings

### WHOIS and Domain Information
WHOIS information showed that `hackthissite.org` is registered through **Porkbun LLC**. 

[View WHOIS evidence](./screenshots/whois-domain-information.png) 
The domain was created on **August 10, 2003**, updated on **August 15, 2026**, and is currently listed with an expiration date of **August 10, 2027**.

The registrant's personal identity was not exposed in the WHOIS results, which is consistent with the use of privacy or redaction mechanisms.

The domain uses multiple authoritative nameservers under `buddyns.com`:

* `c.ns.buddyns.com`
* `f.ns.buddyns.com`
* `g.ns.buddyns.com`
* `h.ns.buddyns.com`
* `j.ns.buddyns.com`

A WHOIS lookup of the associated IP range `137.74.0.0/16` showed RIPE registry information. The lookup itself noted that registry information does not necessarily identify the current holder of the address space.

A reverse IP lookup for `137.74.187.100` returned `hackthissite.org` as the hostname identified in the source's dataset. [View reverse IP evidence](./screenshots/reverse-ip-results.png)


#### Observations

* The domain has been registered since 2003.
* Porkbun LLC is listed as the registrar.
* Multiple nameservers provide DNS delegation.
* WHOIS privacy/redaction prevents identification of the registrant from the public record.
* IP registration information should not automatically be interpreted as proof of current ownership or hosting.
* Reverse IP results should be treated as dataset observations rather than proof that an IP hosts only one website.

### DNS and Infrastructure

DNS analysis identified five IPv4 addresses associated with `hackthissite.org`:

* `137.74.187.101`
* `137.74.187.100`
* `137.74.187.102`
* `137.74.187.104`
* `137.74.187.103`
  
[View DNS records evidence](./screenshots/dns-records.png)

The domain uses five authoritative nameservers under `buddyns.com`, consistent with the nameservers observed in the WHOIS information.

The MX records point to Google's mail infrastructure, indicating that Google-hosted mail servers are configured to receive email for the domain.

The DNS results also contained several TXT records, including an SPF policy and domain-verification values. The SPF record specifies permitted sending sources and ends with `-all`, indicating that mail from unauthorized sources should fail the SPF check.

CAA records were also present. These records specify certificate authorities that are permitted to issue certificates for the domain.

The WHOIS DNS snapshot identified **OVHcloud (AS16276)** as the hosting network and reported that IPv6 was not enabled in its current view.

#### Observations

* Multiple A records were associated with the main domain.
* DNS delegation uses multiple authoritative nameservers.
* Email delivery is configured through Google's mail infrastructure.
* TXT records provide additional DNS-based configuration and verification information.
* CAA records provide certificate-issuance restrictions.
* DNS information can change over time, so these findings represent observations from the time of analysis.

### Subdomain Discovery

Subdomain discovery was performed using passive sources to identify hostnames publicly associated with `hackthissite.org`.

A Google search using the `site:hackthissite.org` operator identified five subdomains:

* `www.hackthissite.org`
* `forum.hackthissite.org`
* `legal.hackthissite.org`
* `ctf.hackthissite.org`
* `mirror.hackthissite.org`
[View Google subdomain evidence](./screenshots/google-subdomain-discovery.png)

Pentest-Tools' passive subdomain discovery returned **88 records** associated with the domain.

[View Pentest-Tools evidence](./screenshots/pentest-tools-subdomain-discovery.png)
 The five subdomains identified through Google were also present in those results.

Examples of IP associations observed in the results included:

* `forum.hackthissite.org` → `137.74.187.101`
* `ctf.hackthissite.org` → `185.199.109.153`

Netlas DNS Search independently displayed the domain's DNS information, including A, NS, MX, and TXT records, with the main A and NS records consistent with the other sources reviewed.

The OSINT Framework also listed several additional subdomain-discovery tools. Active enumeration tools were not used during this phase because the purpose of this project was to maintain a passive reconnaissance approach.

#### Observations

* Multiple independent sources identified overlapping subdomain information.
* Pentest-Tools reported more records than the Google search, demonstrating the value of using multiple sources.
* The presence of a hostname in a passive dataset does not necessarily mean that the host is currently active.
* IP associations may change over time and should be treated as observations from the source at the time of collection.
* `ctf.hackthissite.org` and `forum.hackthissite.org` were independently observed with different IP addresses.

### Certificate Transparency and TLS Analysis

The current TLS certificate for `hackthissite.org` was examined using publicly available certificate information.

The certificate was issued by the **Hellenic Academic and Research Institutions CA (HARICA)** and was valid from **March 25, 2026 to October 10, 2026** at the time of analysis. The certificate uses a **4096-bit RSA key**.[View current TLS certificate evidence](./screenshots/ssl-certificate-analysis.png)


The certificate's Subject field contained an `.onion` hostname, while the Subject Alternative Names (SANs) included:

* `hackthissite.org`
* `www.hackthissite.org`
* The observed `.onion` hostname

Certificate Transparency data from `crt.sh` revealed additional certificate-associated hostnames, including:

* `ctf.hackthissite.org`
* `h5ai.hackthissite.org`
* `status.hackthissite.org`
* `email.hackthissite.org`
* `irc.hackthissite.org`
* `wolf.irc.hackthissite.org`
* `lille.irc.hackthissite.org`
* `www.irc.hackthissite.org`
* `mta-sts.hackthissite.org`
  
[View Certificate Transparency evidence](./screenshots/crtsh-certificate-transparency.png)

Some identities returned by the Certificate Transparency search were unrelated to the target domain and were excluded from the findings.

#### Observations

* Certificate Transparency provided additional hostnames that were not all identified during the initial Google search.
* Certificate-associated hostnames may represent historical or current certificate usage and should not automatically be interpreted as currently active services.
* The presence of an `.onion` hostname in the certificate identifies it as a certificate-associated identity; it does not, by itself, prove that an active Tor service was verified during this investigation.
* The certificate was approaching its stated expiration date at the time the information was collected.
* Certificate information provided another source for cross-checking the domain's publicly observable infrastructure.

### Passive DNS Analysis

Passive DNS sources were used to examine DNS observations associated with `hackthissite.org`. The investigation used DNSDumpster and Mnemonic to compare recorded DNS information.

DNSDumpster identified several hostnames with associated A records, including:

* `api.hackthissite.org`
* `git.hackthissite.org`
* `email.hackthissite.org`
* `hp.hackthissite.org`
* `irc.hackthissite.org`
* `irc-hub.hackthissite.org`
* `irc-www.hackthissite.org`
* `lille.irc.hackthissite.org`
* `wolf.irc.hackthissite.org`
* `www.irc.hackthissite.org`
* `psep.hackthissite.org`
* `qdb.hackthissite.org`
* `stats.hackthissite.org`
* `status.hackthissite.org`

Mnemonic's passive DNS results included:

* 5 A records
* 12 AAAA records
* 2 CNAME records
* 6 PTR records
[View Mnemonic passive DNS evidence](./screenshots/mnemonic-aaaa-records.png)

The AAAA records were particularly useful for comparison because the current WHOIS DNS view did not show IPv6 records. This demonstrates that passive DNS datasets may contain historical observations that are not necessarily reflected in a current DNS lookup.

#### Observations

* Passive DNS revealed additional hostnames beyond those identified during the initial subdomain-discovery stage.
* DNSDumpster and Mnemonic provided different types of DNS observations.
* Historical passive DNS data should not automatically be interpreted as proof of current DNS configuration.
* Differences between current DNS results and passive DNS records can occur because DNS configurations and infrastructure change over time.
* Hostnames such as `api`, `git`, `irc`, and `status` were recorded as observations; their names alone were not used to assume the function of each host.

[View DNSDumpster evidence](./screenshots/dnsdumpster-overview.png)

### DNSSEC Analysis

DNSSEC information was examined using DNSSEC Analyzer and DNSViz.

DNSSEC Analyzer reported that no **DS, DNSKEY, or RRSIG records** were found for `hackthissite.org` at the time of analysis.[View DNSSEC Analyzer evidence](./screenshots/dnssec-analyzer-results.png)


DNSViz provided a visual representation of the domain's DNS configuration and reported a mixture of secure and insecure statuses across different parts of the DNS hierarchy.

#### Observations

* DNSSEC Analyzer did not observe DS, DNSKEY, or RRSIG records for the target domain.
* DNSViz showed both secure and insecure statuses within its analysis.
* The results indicate that DNSSEC signing was not observed for the domain during this investigation.
* DNSSEC status is a configuration observation and was not treated as evidence of a vulnerability.
* DNSSEC analysis provided another way to examine the domain's DNS security configuration.

### Reputation Analysis

VirusTotal was used to review publicly available reputation observations associated with `hackthissite.org`.

The domain detection results showed that two security vendors classified the domain as **suspicious**, while other vendors returned clean or unrated results.

VirusTotal's passive DNS and subdomain relationship data was also reviewed. The passive DNS replication results showed no detections across the engines represented in the results.

For the subdomain relationships, most observed subdomains had no detections. Two entries showed a single detection out of 91 engines:

* `email.hackthissite.org` → 1/91
* `psrp.hackthissite.org` → 1/91

The associated IP addresses displayed in the VirusTotal results were also recorded as part of the observation.

#### Observations

* Reputation results varied between security vendors.
* Two vendors classified the main domain as suspicious, while other vendors did not make the same classification.

[View VirusTotal evidence](./screenshots/virustotal-detection.png)

* The VirusTotal results were treated as vendor observations rather than definitive evidence that the domain or its infrastructure is malicious.
* Low detection counts associated with individual subdomains require additional context before drawing conclusions.
* The target is a security-training website, so reputation classifications should be interpreted in the context of its intended security-related activities.

### Typosquatting Analysis

DNSTwister was used to identify domains that resemble `hackthissite.org` and could potentially be confused with the legitimate domain.

The tool reported:

* **Found:** 4
* **Available:** 523
* **Errors:** 1

The four domains listed under the results were:

* `hackthissite.org`
* `hackthisite.org`
* `hackthisist.e.org`
* `hackthisis.te.org`

The first entry is the legitimate target domain. The remaining three were identified by the tool as look-alike domains.

[View DNSTwister evidence](./screenshots/dnstwister-results.png)


#### Observations

* Three look-alike domains were identified in addition to the legitimate target.
* A domain being identified as a typosquatting candidate does not establish that it is malicious or being used for impersonation.
* The results demonstrate how small changes to spelling or domain structure can produce domains that resemble a legitimate target.
* Typosquatting analysis can help identify domains that may warrant further monitoring.

### Technology Profiling

BuiltWith was used to review publicly available technology-related information associated with `hackthissite.org`.

The results included several categories and detections, including:

* **Framework:** Onion Location
* **Language:** English (inferred)
* **Widgets / Data Sources:** Apple Whitelist, CrUX Dataset, Cloudflare Radar, and Common Crawl

The BuiltWith results were treated as technology-profile observations rather than definitive confirmation of how each detected item is currently used by the website.

The Onion Location detection was consistent with the `.onion` identity observed in the TLS certificate analysis. However, this was not treated as independent proof that an active Tor service was verified during the investigation.

#### Observations

* Technology profiling provided additional context about the domain.
* Some results were inferred or categorized by the profiling service.
* Technology-detection tools can produce results based on available public signals and should therefore be interpreted alongside other evidence.
* The BuiltWith findings were used as supporting information rather than as standalone proof of a specific technology deployment.

## Cross-Verification

Information was compared across multiple OSINT sources to improve confidence in the observations.

Several findings were independently supported:

| Finding                      | Sources Compared                           | Observation                                                                                                               |
| ---------------------------- | ------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------- |
| Main A records               | WHO.IS, Netlas, Mnemonic                   | Multiple sources recorded the same primary IPv4 addresses.                                                                |
| Nameservers                  | WHO.IS, Netlas, DNSDumpster                | The `buddyns.com` nameservers were consistently observed.                                                                 |
| Subdomains                   | Google, Pentest-Tools, crt.sh, DNSDumpster | Multiple sources identified overlapping hostnames.                                                                        |
| Certificate-associated hosts | WHO.IS, crt.sh                             | Certificate information provided additional hostnames and identities.                                                     |
| DNS records                  | WHO.IS, Netlas, DNSDumpster, Mnemonic      | Different sources provided overlapping current and historical DNS observations.                                           |
| `.onion` identity            | TLS certificate, BuiltWith                 | Both sources provided evidence associated with an Onion Location identity.                                                |
| Reputation                   | VirusTotal                                 | Vendor results showed differing classifications, demonstrating the importance of comparing security-intelligence results. |

Differences between sources were also recorded rather than treated as errors automatically. In particular, passive DNS data may contain historical observations that differ from a current DNS snapshot.

Cross-verification helped distinguish information that appeared consistently across sources from information that required additional context or could not be confirmed as current.

## Limitations

This investigation was based on publicly available information and therefore has several limitations:

* OSINT data can become outdated as domain registrations, DNS records, certificates, hosting arrangements, and infrastructure change.
* Passive DNS and Certificate Transparency records may contain historical information that does not represent currently active hosts or services.
* Subdomain discovery results from different sources may vary in coverage and accuracy.
* Reverse IP results depend on the underlying dataset and do not necessarily represent all domains associated with an IP address.
* Security reputation classifications may differ between vendors and should not be treated as definitive evidence of malicious activity.
* Technology profiling results may include inferred or historical detections.
* No active vulnerability scanning, exploitation, authentication testing, or unauthorized access attempts were performed.
* The investigation provides an OSINT snapshot of publicly observable information rather than a complete assessment of the target's infrastructure.

## Evidence

Screenshots captured during the investigation are available in the [`screenshots`](./screenshots) directory.

### WHOIS and Domain Information

* [WHOIS domain information](./screenshots/whois-domain-information.png)
* [WHOIS IP information](./screenshots/whois-ip-information.png)
* [WHOIS history](./screenshots/whois-history.png)
* [Reverse IP results](./screenshots/reverse-ip-results.png)

### DNS and Infrastructure

* [DNS records](./screenshots/dns-records.png)
* [Uptime analysis](./screenshots/uptime-analysis.png)
* [Ping and traceroute diagnostics](./screenshots/diagnostics-ping-traceroute.png)

### Subdomain Discovery

* [Google subdomain discovery](./screenshots/google-subdomain-discovery.png)
* [Pentest-Tools subdomain discovery](./screenshots/pentest-tools-subdomain-discovery.png)
* [Netlas DNS search](./screenshots/netlas-dns-search.png)

### Certificate Analysis

* [Current TLS certificate](./screenshots/ssl-certificate-analysis.png)
* [Certificate Transparency results](./screenshots/crtsh-certificate-transparency.png)

### Passive DNS

* [DNSDumpster overview](./screenshots/dnsdumpster-overview.png)
* [DNSDumpster A records](./screenshots/dnsdumpster-a-records.png)
* [DNSDumpster MX records](./screenshots/dnsdumpster-mx-records.png)
* [DNSDumpster NS records](./screenshots/dnsdumpster-ns-records.png)
* [Mnemonic A records](./screenshots/mnemonic-a-records.png)
* [Mnemonic AAAA records](./screenshots/mnemonic-aaaa-records.png)
* [Mnemonic CNAME and PTR records](./screenshots/mnemonic-cname-ptr-records.png)

### DNSSEC

* [DNSSEC Analyzer results](./screenshots/dnssec-analyzer-results.png)
* [DNSViz DNSSEC visualization](./screenshots/dnsviz-dnssec-visualization.png)

### Reputation Analysis

* [VirusTotal detection results](./screenshots/virustotal-detection.png)
* [VirusTotal passive DNS and subdomain results](./screenshots/virustotal-passive-dns-subdomains.png)

### Typosquatting

* [DNSTwister results](./screenshots/dnstwister-results.png)

## Conclusion

This investigation demonstrated how publicly available information can be collected and correlated to build a structured picture of a domain's observable infrastructure.

The investigation identified domain registration information, DNS records, associated IP addresses, publicly discoverable subdomains, certificate-associated hostnames, passive DNS observations, DNSSEC information, reputation classifications, and potential typosquatting candidates.

Using multiple sources made it possible to cross-check several findings and identify differences between current and historical observations. The investigation also demonstrated the importance of interpreting OSINT data carefully rather than assuming that every historical record, hostname, IP association, or security classification represents current activity.

The project provided practical experience in passive reconnaissance, DNS analysis, certificate research, information verification, and evidence-based reporting.

No exploitation or unauthorized access was performed. The investigation remained focused on publicly available information and passive OSINT techniques.
