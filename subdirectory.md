# Hidden Directories


### 1. **Gobuster**
You've already mentioned Gobuster, which is fast and efficient. Here's a basic example:

```bash
gobuster dir -w /path/to/wordlist -u http://lookup.thm
```

For better results, you can add a few useful flags:

- `-t`: Number of concurrent threads (higher = faster)
- `-x`: File extensions to try (e.g., `.php`, `.html`, `.js`)
- `-s`: HTTP status code filter (e.g., `-s 200` for only 200 responses)

Example with added flags:
```bash
gobuster dir -w /path/to/wordlist -u http://lookup.thm -t 50 -x php,html,js -s 200
```

---

### 2. **Dirb**
[Dirb](https://github.com/v0re/dirb) is another popular directory scanner that works in a similar way to Gobuster. It has its own built-in wordlists, but you can use your own too.

Example command:
```bash
dirb http://lookup.thm /path/to/wordlist
```

You can specify more advanced options as well:
- `-o`: Output file for results
- `-X`: Extensions to try (e.g., `-X .php,.html`)
- `-r`: Recursively search directories found

Example with options:
```bash
dirb http://lookup.thm /path/to/wordlist -o output.txt -X .php,.html,.js
```

---

### 3. **FFUF (Fuzz Faster U Fool)**
[FFUF](https://github.com/ffuf/ffuf) is a powerful, fast web fuzzing tool. It can also be used to find directories and files by specifying the `-w` option for wordlists.

Example command:
```bash
ffuf -w /path/to/wordlist -u http://lookup.thm/FUZZ
```

Where `FUZZ` is the placeholder that the tool will replace with each word in the wordlist.

You can also add the `-mc` flag to filter by HTTP status codes, e.g., `-mc 200` for 200 responses only.

Example with status code filtering:
```bash
ffuf -w /path/to/wordlist -u http://lookup.thm/FUZZ -mc 200
```

---

### 4. **Nikto**
[Nikto](https://cirt.net/Nikto2) is an open-source web scanner that checks for security vulnerabilities, but it can also be used for discovering hidden directories.

Example command:
```bash
nikto -h http://lookup.thm -C all -w /path/to/wordlist
```

The `-C all` flag tells Nikto to check for all common vulnerabilities, but it can also assist in directory discovery as it tests known paths.

---

### 5. **Wfuzz**
[Wfuzz](https://github.com/xmendez/wfuzz) is another tool designed for fuzzing and directory enumeration. Like FFUF, it's fast and flexible.

Example command:
```bash
wfuzz -c -w /path/to/wordlist -u http://lookup.thm/FUZZ
```

The `-c` flag adds color to the output, which can make it easier to read, and `FUZZ` is the placeholder where the tool will substitute each word from the wordlist.

---

### 6. **Hydra (Web Form Bruteforce)**
If you're interested in brute-forcing specific directories or login pages (with a list of common names), Hydra can be adapted for directory enumeration.

Example command:
```bash
hydra -l admin -P /path/to/wordlist http://lookup.thm http-get
```

This command would attempt to brute-force common paths while performing HTTP GET requests. It's not as fast as Gobuster or FFUF, but it can be useful for testing known login directories or other paths.

---

### 7. **Dirsearch**
[Dirsearch](https://github.com/maurosoria/dirsearch) is a simple yet effective tool designed for brute-forcing directories and files on a web server.

Example command:
```bash
python3 dirsearch.py -u http://lookup.thm -w /path/to/wordlist
```

You can add options like:
- `-t`: Number of threads
- `-e`: Extensions to try (e.g., `-e .php,.html,.js`)
- `-x`: Exclude specific status codes

Example with options:
```bash
python3 dirsearch.py -u http://lookup.thm -w /path/to/wordlist -t 50 -e php,html
```

---

### 8. **Burp Suite (Intruder)**
Burp Suite’s [Intruder](https://portswigger.net/burp/intruder) feature can be used for brute-forcing directories by specifying a list of potential subdirectories or files to try.

1. Set the target to `http://lookup.thm`.
2. Use the Intruder tab to set a custom list of paths (from a wordlist) to fuzz.
3. Burp will send HTTP requests to each URL and display any response codes.

---

### 9. **Amass (for Subdomain Discovery)**
If you're looking for subdomains in addition to subdirectories, [Amass](https://github.com/OWASP/Amass) is a good option. It's used for subdomain enumeration but can also help identify hidden services.

Example command:
```bash
amass enum -d lookup.thm -o amass_results.txt
```

While Amass focuses on subdomains, it's still useful in a larger reconnaissance phase.

---

### 10. **Custom Scripts (using Python or Bash)**
If you prefer writing your own custom scripts for directory enumeration, Python or Bash can be very effective.

Here’s a simple Python example using `requests`:

```python
import requests

url = "http://lookup.thm/"
wordlist = "/path/to/wordlist"

with open(wordlist, "r") as f:
    for line in f:
        path = line.strip()
        full_url = url + path
        response = requests.get(full_url)
        if response.status_code == 200:
            print(f"Found: {full_url}")
```

---

### Additional Tips:
- **Rate Limiting**: Some sites implement rate limiting, so you may need to slow down your scans (e.g., with fewer threads) or use a proxy.
- **Status Codes**: Generally, `200 OK`, `301 Moved Permanently`, and `403 Forbidden` are good to look for. You might also be interested in `404 Not Found` responses to filter out dead links.
- **Content-Type Filtering**: Sometimes, checking the content type of a response (e.g., HTML, JSON) can help identify the correct directory type.
