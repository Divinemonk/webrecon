# Subdomain enumeration

### 1. **Amass (OWASP)**
[Amass](https://github.com/OWASP/Amass) is one of the most comprehensive and widely used tools for subdomain enumeration. It can use multiple techniques like brute-forcing, DNS queries, passive gathering, and more.

#### Install Amass:
```bash
sudo apt install amass
```

#### Basic Usage:
```bash
amass enum -d example.com
```
This will gather subdomains for the domain `example.com` using passive techniques (like scraping public DNS records, certificates, etc.).

#### Advanced Usage with Active Techniques:
You can use Amass to also perform active scans to discover more subdomains by including brute-force and DNS resolution.
```bash
amass enum -active -brute -d example.com -o amass_subdomains.txt
```

- `-active`: Use active scanning techniques (more aggressive).
- `-brute`: Perform brute-forcing using wordlists.

You can also specify a custom wordlist:
```bash
amass enum -d example.com -w /path/to/wordlist -o amass_subdomains.txt
```

Amass can also do:
- **OSINT-based passive scanning** (using services like `crt.sh`, `shodan.io`, etc.).
- **Reverse DNS queries**.
- **DNS enumeration** for deeper scans.

#### To get subdomains from third-party data sources:
```bash
amass enum -d example.com -src
```
This pulls data from sources like public DNS logs, Shodan, and more.

---

### 2. **Sublist3r**
[Sublist3r](https://github.com/aboul3la/Sublist3r) is a fast subdomain enumeration tool that can use both passive and active techniques to discover subdomains. It uses various search engines, DNS, and other public sources.

#### Install Sublist3r:
```bash
git clone https://github.com/aboul3la/Sublist3r.git
cd Sublist3r
pip3 install -r requirements.txt
```

#### Basic Usage:
```bash
python3 sublist3r.py -d example.com
```
This will discover subdomains using public sources like search engines and DNS.

#### Advanced Usage with Additional Options:
```bash
python3 sublist3r.py -d example.com -o sublist3r_subdomains.txt
```
This will save the results to `sublist3r_subdomains.txt`.

You can also use the `-b` flag to include brute-forcing with a wordlist:
```bash
python3 sublist3r.py -d example.com -b -w /path/to/wordlist -o sublist3r_subdomains.txt
```

---

### 3. **Subfinder**
[Subfinder](https://github.com/projectdiscovery/subfinder) is another excellent tool for discovering subdomains. It's part of the [Project Discovery](https://github.com/projectdiscovery) tools collection, which is a set of highly efficient and actively maintained tools.

#### Install Subfinder:
```bash
GO111MODULE=on go get -u github.com/projectdiscovery/subfinder/v2/cmd/subfinder
```

#### Basic Usage:
```bash
subfinder -d example.com
```

#### Advanced Usage:
You can add passive and active sources using the `-o` flag for output:
```bash
subfinder -d example.com -o subfinder_subdomains.txt
```

Subfinder is designed to be extremely fast and to work with various public sources, like certificate transparency logs, public DNS records, and more.

---

### 4. **DNSdumpster**
[DNSdumpster](https://dnsdumpster.com/) is a free online tool that passively collects DNS records for a domain. It’s not a tool you install, but you can use it to gather subdomains directly from their web interface.

#### Usage:
1. Go to [DNSdumpster](https://dnsdumpster.com/).
2. Enter the domain you want to search for subdomains (e.g., `example.com`).
3. Review the discovered subdomains and export the results.

---

### 5. **Shodan**
[Shodan](https://www.shodan.io/) is a search engine for Internet-connected devices. You can search for subdomains or even related services (e.g., HTTP/HTTPS, FTP, etc.) exposed on the web.

#### Basic Search:
1. Go to [Shodan.io](https://www.shodan.io/).
2. Type `hostname:example.com` in the search bar to look for subdomains associated with `example.com`.

You can also use Shodan's API to automate the process using a Python script:
```bash
pip install shodan
```

```python
import shodan

API_KEY = 'YOUR_SHODAN_API_KEY'
api = shodan.Shodan(API_KEY)

results = api.search('hostname:example.com')
for result in results['matches']:
    print(result['hostnames'])
```

---

### 6. **Findomain**
[Findomain](https://github.com/Findomain/Findomain) is another fast and efficient tool for subdomain enumeration that integrates with multiple sources for data collection.

#### Install Findomain:
```bash
cargo install findomain
```

#### Basic Usage:
```bash
findomain -t example.com
```

#### Advanced Usage:
```bash
findomain -t example.com -o findomain_subdomains.txt
```

Findomain can also run in **brute-force** mode if you use a custom wordlist.

---

### 7. **Knockpy**
[Knockpy](https://github.com/guelfoweb/knock) is a Python-based subdomain discovery tool that supports brute-force scanning, but it’s particularly effective for discovering subdomains using common names (or a custom wordlist).

#### Install Knockpy:
```bash
git clone https://github.com/guelfoweb/knock.git
cd knock
python3 setup.py install
```

#### Basic Usage:
```bash
knockpy example.com
```

This will use a list of common subdomain names to try to identify subdomains for `example.com`.

---

### 8. **DNSRecon**
[DNSRecon](https://github.com/darkoperator/dnsrecon) is a DNS enumeration tool that can perform both zone transfers and brute-force DNS lookups.

#### Install DNSRecon:
```bash
sudo apt install dnsrecon
```

#### Basic Usage:
```bash
dnsrecon -d example.com
```

#### Advanced Usage (Brute-Forcing Subdomains):
```bash
dnsrecon -d example.com -t brt -w /path/to/wordlist
```

This will perform a DNS brute force using the provided wordlist.

---

### 9. ** crt.sh (Certificate Transparency Logs)**
You can use [crt.sh](https://crt.sh/) to search for subdomains by querying the Certificate Transparency logs. These logs list certificates issued for domains and can reveal subdomains.

#### Search for Subdomains:
Go to `https://crt.sh/` and type in the domain:
```
example.com
```

It will show you all the subdomains that have SSL/TLS certificates registered for them. This method is great for finding subdomains that might not be listed publicly.

---

### 10. **Google Dorks**
Google Dorks can be used to find subdomains by querying Google's search engine with specific search parameters.

#### Example Google Dork:
```bash
site:*.example.com
```

This will list subdomains indexed by Google. You can also use `inurl` to search for specific subdomains:
```bash
inurl:example.com
```

---

### 11. **Reverse DNS Lookup (PTR Records)**
You can query reverse DNS entries for a specific IP range to identify associated subdomains. This can be done using `dig` or `nslookup`.

Example command:
```bash
dig -x 192.168.1.1
```

This will return any associated PTR (reverse DNS) records, which might reveal subdomains linked to that IP address.

---

### 12. **Automated Recon Tools (Recon-ng, SpiderFoot, etc.)**
There are also more comprehensive automated reconnaissance frameworks, such as [Recon-ng](https://github.com/lanmaster53/recon-ng) and [SpiderFoot](https://github.com/smicallef/spiderfoot), which can automate subdomain discovery along with other reconnaissance tasks.

---

### Additional Tips:
- **Rate Limiting**: Be careful when using brute-force tools (like Sublist3r with `-b`) as they can trigger rate limits or CAPTCHAs.
- **Filter Results**: Use tools like `grep`, `sort`, or `uniq` to clean up your output (e.g., removing duplicates).
- **Third-party APIs**: Many tools like Amass and Subfinder support integrating with third-party APIs (e.g., VirusTotal, Shodan, etc.) for even more subdomains.
- **Always Use Proper Authorization**: Be sure you're authorized to perform any kind of scanning or enumeration on a domain, especially when doing active enumeration.
