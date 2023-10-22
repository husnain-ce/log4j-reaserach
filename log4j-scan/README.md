<h1 align="center"><b>LOG4J-Scan</b></h1>
<h4 align="center">A fully automated, accurate, and extensive scanner for finding vulnerable Log4J hosts</h4>

![Log4j](https://user-images.githubusercontent.com/90478603/198680895-056b27f9-43a1-4780-83f2-6e6eb3905f41.png)

# __Features__

- Support for lists of URLs.
- Fuzzing for more than 60 HTTP request headers (not only 3-4 headers as previously seen tools).
- Fuzzing for HTTP POST Data parameters.
- Fuzzing for JSON data parameters.
- Supports DNS callback for vulnerability discovery and validation.
- WAF Bypass payloads.

---

# 🚨 Annoucement (October 20th, 2022)

Apache Commons Text RCE (CVE-2022-42889) is highly similar to Log4J RCE, and we recommend patching it as soon as possible. Vulnerable applications allow full remote-code execution.

![Log4j-Exploit](https://user-images.githubusercontent.com/90478603/198680895-056b27f9-43a1-4780-83f2-6e6eb3905f41.png)

# 🚨 Announcement (December 17th, 2021)

There is a patch bypass on Log4J v2.15.0 that allows a full RCE. We added support for log4j-scan to reliably detect CVE-2021-45046.

![Log4j-Patch-Exploit](https://user-images.githubusercontent.com/90478603/198681531-55249f02-b3a3-425e-8815-b5cef0e2bfdb.png)

---

# __Description__

We have been researching the Log4J RCE (CVE-2021-44228) since it was released, and we worked in preventing this vulnerability with our customers. We build this detection and scanning tool for discovering and fuzzing for Log4J RCE CVE-2021-44228 vulnerability. This shall be used by security teams to scan their infrastructure for Log4J RCE, and also test for WAF bypasses that can result in achieving code execution on the organization's environment.

It supports DNS OOB callbacks out of the box, there is no need to set up a DNS callback server.

# __Usage__

```python
python3 log4j-scan.py -h

[•] CVE-2021-44228 - Apache Log4j RCE Scanner

usage: log4j-scan.py [-h] [-u URL] [-p PROXY] [-l USEDLIST] [--request-type REQUEST_TYPE] [--headers-file HEADERS_FILE] [--run-all-tests] [--exclude-user-agent-fuzzing]
                     [--wait-time WAIT_TIME] [--waf-bypass] [--custom-waf-bypass-payload CUSTOM_WAF_BYPASS_PAYLOAD] [--test-CVE-2021-45046] [--test-CVE-2022-42889]
                     [--custom-dns-callback-host CUSTOM_DNS_CALLBACK_HOST] [--disable-http-redirects]

optional arguments:
  -h, --help            show this help message and exit
  -u URL, --url URL     Check a single URL.
  -p PROXY, --proxy PROXY
                        send requests through proxy
  -l USEDLIST, --list USEDLIST
                        Check a list of URLs.
  --request-type REQUEST_TYPE
                        Request Type: (get, post) - [Default: get].
  --headers-file HEADERS_FILE
                        Headers fuzzing list - [default: headers.txt].
  --run-all-tests       Run all available tests on each URL.
  --exclude-user-agent-fuzzing
                        Exclude User-Agent header from fuzzing - useful to bypass weak checks on User-Agents.
  --wait-time WAIT_TIME
                        Wait time after all URLs are processed (in seconds) - [Default: 5].
  --waf-bypass          Extend scans with WAF bypass payloads.
  --custom-waf-bypass-payload CUSTOM_WAF_BYPASS_PAYLOAD
                        Test with custom WAF bypass payload.
  --test-CVE-2021-45046
                        Test using payloads for CVE-2021-45046 (detection payloads).
  --test-CVE-2022-42889
                        Test using payloads for Apache Commons Text RCE (CVE-2022-42889).
  --custom-dns-callback-host CUSTOM_DNS_CALLBACK_HOST
                        Custom DNS Callback Host.
  --disable-http-redirects
                        Disable HTTP redirects. Note: HTTP redirects are useful as it allows the payloads to have a higher chance of reaching vulnerable systems.
```

__Scan a Single URL__

```shell
python3 log4j-scan.py -u https://localhost:8080
```

__Scan a Single URL using all Request Methods: GET, POST (url-encoded form), POST (JSON body)__

```shell
python3 log4j-scan.py -u https://localhost:8080 --run-all-tests
```

__Discover WAF bypasses against the environment.__

```shell
python3 log4j-scan.py -u https://localhost:8080 --waf-bypass
```

__Scan a list of URLs__

```shell
python3 log4j-scan.py -l urls.txt
```

__Installation__

```
pip3 install -r requirements.txt
```

# __Docker Support__

```shell
cd log4j-scan
sudo docker build -t log4j-scan .
sudo docker run -it --rm log4j-scan

# With URL list "urls.txt" in current directory
docker run -it --rm -v $PWD:/data log4j-scan -l /data/urls.txt
```