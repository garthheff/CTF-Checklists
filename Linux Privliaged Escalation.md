## Check what current user can run as sudo 
- sudo -l
- [gtfobins](https://gtfobins.github.io/)

## Find files that have SUID or SGID bits set.  
find / -type f -perm -04000 -ls 2>/dev/null

# Can we increase the privilege level of a process or binary with “Capabilities”? 
getcap -r / 2>/dev/null  

## Modify path? 
Add folders to PATH:  export PATH=/tmp:$PATH
Create a root bash script: echo "/bin/bash" > thm

# Do we have access to /etc/password and /etc/shadow ? 
hashcat the password hash? 

## Privilege Escalation tools
- LinPeas: https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/linPEAS
- LinEnum: https://github.com/rebootuser/LinEnum
- LES (Linux Exploit Suggester): https://github.com/mzet-/linux-exploit-suggester
- Linux Smart Enumeration: https://github.com/diego-treitos/linux-smart-enumeration
- Linux Priv Checker: [https://github.com/linted/linuxprivchecker](https://github.com/linted/linuxprivchecker)

