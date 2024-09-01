
---

**Gobuster**
- [ ] `gobuster dns -d domain.local -t 25 -w /opt/Seclist/Discovery/DNS/subdomain-top2000.txt`
---

**NO CRED ENUM**
- [ ] `enum4linux -a -u "" -p "" <DC IP> && enum4linux -a -u "guest" -p "" <DC IP>`
- [ ] `smbmap -u "" -p "" -P 445 -H <DC IP> && smbmap -u "guest" -p "" -P 445 -H <DC IP>`
- [ ] `smbclient -U '%' -L //<DC IP> && smbclient -U 'guest%' -L //`
---

**BLOODHOUND**
- [ ] `bloodhound-python -d domain.local -dc DC01 -c All -u username -p password`
- [ ] `bloodhound-python -u username -p password -d domain.local -dc DC01 -ns <name-server> --zip  -c All`
- [ ] `bloodhound-python -d domain.local -dc DC01 -c LoggedOn -u username -p password`
---

** SMB Signing:False **
- [ ] `nxc smb 192.168.1.0/24 --gen-relay-list relay_list.txt`
---

** COMMANDS **
- [ ] `net group /domain`
- [ ] `whoami /groups`
- [ ] `gpresult /V`
- [ ] `net user TheUser /domain`
- [ ] `net group MyGroup /domain`
- [ ] `net view Hostname /domain`
---