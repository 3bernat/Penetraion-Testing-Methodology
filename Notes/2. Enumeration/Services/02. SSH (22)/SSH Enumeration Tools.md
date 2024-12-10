# SSH Enumeration Tools

## Nmap Enumeration

```sh
$ ls -lh /usr/share/nmap/scripts/ | grep ssh
-rw-r--r-- 1 root root 5.3K Oct 12 09:29 ssh2-enum-algos.nse
-rw-r--r-- 1 root root 1.2K Oct 12 09:29 ssh-auth-methods.nse
-rw-r--r-- 1 root root 3.0K Oct 12 09:29 ssh-brute.nse
-rw-r--r-- 1 root root  16K Oct 12 09:29 ssh-hostkey.nse
-rw-r--r-- 1 root root 5.9K Oct 12 09:29 ssh-publickey-acceptance.nse
-rw-r--r-- 1 root root 3.7K Oct 12 09:29 ssh-run.nse
-rw-r--r-- 1 root root 1.4K Oct 12 09:29 sshv1.nse

$ nmap x.x.x.x -p 22 -sV --script=exampleScript1.nse,exampleScript2.nse
$ nmap x.x.x.x -p 22 -sV ssh-hostkey --script-args ssh_hostkey=full
$ nmap x.x.x.x -p 22 -sV ssh-auth-methods --script-args="ssh.user=root"
```

## Manual Connection

```sh
$ nc -nv x.x.x.X 22 # Might give header
```

```sh
$ ssh x.x.x.x -p22
```

## Netexec

```
- Netexec ssh 172.21.0.0 -u root -p password/passwordfile --no-bruteforce
- Netexec ssh 172.21.0.0 -u root -p password/passwordfile --no-bruteforce -x whoami
```

## SSH Audit: 
Source: https://github.com/jtesta/ssh-audit

```
python ssh-audit.py [-1246pbcnjvlt] 172.21.0.0
```

## Metasploit
```
Auxilary Modules:
auxiliary/scanner/ssh/ssh_version
use scanner/ssh/ssh_enumusers
```
