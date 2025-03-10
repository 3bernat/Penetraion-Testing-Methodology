## Meterpreter

Use only if you have a meterpreter shell and you need to pivot to another network.

### Portfwd

`meterpreter > portfwd add -l 80 -r 172.21.0.0 -p 80`

### Autoroute

In Metasploit:

1. `use post/multi/manage/autoroute`

```
msf5 post(multi/manage/autoroute) > options

Module options (post/multi/manage/autoroute):

   Name     Current Setting  Required  Description
   ----     ---------------  --------  -----------
   CMD      autoadd          yes       Specify the autoroute command (Accepted: add, autoadd, print, delete, default)
   NETMASK  255.255.255.0    no        Netmask (IPv4 as "255.255.255.0" or CIDR as "/24"
   SESSION                   yes       The session to run this module on.
   SUBNET                    no        Subnet (IPv4, for example, 10.10.10.0)

msf5 post(multi/manage/autoroute) > 
```

2. `set session <avaliable session>`
3. `run`

### Metasploit Socks Proxy

1  auxiliary/server/socks4a                                  normal  No     Socks4a Proxy Server
2  auxiliary/server/socks5                                   normal  No     Socks5 Proxy Server
3  auxiliary/server/socks_unc                                normal  No     SOCKS Proxy UNC Path Redirection

## Cntlm

```
apt install cntlm
cntlm -u username@breakme.local -I proxy
export http://127.0.0.1:3128, export https://127.0.0.1:3128
chromium --proxy-server="http://127.0.0.1:3128"
```


## Other Tools

- ssf: https://github.com/securesocketfunneling/ssf
- rpivot: https://github.com/klsecservices/rpivot
- hans (ICMP Tunneling): http://code.gerade.org/hans/
- Iodine (ICMP Tunneling over DNS): https://code.kryo.se/iodine/
- Dnscat2: https://github.com/iagox86/dnscat2
- Chisel: https://github.com/jpillora/chisel
- httptunnel: In Kali apt install httptunnel
- ligolo: https://github.com/sysdream/ligolo
- ligolo-ng: https://github.com/nicocha30/ligolo-ng/releases
- reGeorg: https://github.com/sensepost/reGeorg

## Other Resources

- https://0xdf.gitlab.io/2019/01/28/pwk-notes-tunneling-update1.html
- https://highon.coffee/blog/ssh-meterpreter-pivoting-techniques/
- https://medium.com/maverislabs/proxyjump-the-ssh-option-you-probably-never-heard-of-2d7e41d43464
- https://book.hacktricks.xyz/generic-methodologies-and-resources/tunneling-and-port-forwarding