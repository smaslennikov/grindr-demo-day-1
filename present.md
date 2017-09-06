---
author: ISRE - Slava Maslennikov
title: Storing secrets in the wild
---
# Storing secrets in the wild

---
## Turn something like

```bash
Pizza is delicious and tasty
```

---
## Into this

```bash
-----BEGIN PGP MESSAGE-----

hQIMA1oDd0D7XIibAQ/+N+9OvH1qmrzId4Q4EoittaXuZ2xR
MgzPKmkjm8jhpg6rmem6Tpww3q5eqy/LFxHG/jKzbti5mTT5
thbg+Xa9MZhiUIjgjjjv8Sn2afb51JLGWOzm+x3lM32Xap7c
E7f/snDngmn+a481LovCdNPp3tSyCjObM4Pp5OwVrHzVGmgi
...
```

---
## Requirements

* GnuPG [keys](https://grindr.atlassian.net/wiki/spaces/ISRE/pages/149133925/Yubikey+setup+and+configuration) for all
* Public keys shared

---
## Set up GnuPG

```bash
pub   rsa4096 2017-08-07 [SC]
      10932C55A4BE1E3484F374D9E540F5AE106445F5
uid           [ultimate] Svyatoslav I. Maslennikov
< slava.maslennikov@grindr.com>
uid           [ultimate] Svyatoslav I. Maslennikov
< me@smaslennikov.com>
sub   rsa4096 2017-08-07 [S]
sub   rsa4096 2017-08-07 [E]
sub   rsa4096 2017-08-07 [A]
```

---
## Import your team's keys with [Keybase](https://keybase.io/)

```bash
pub   rsa3744 2017-04-27 [SC] [expires: 2017-11-12]
      AC59126516ABC4A09D50534215548CEA38B03AFD
uid           [  full  ] joshua lai < joshuajlai@gmail.com>
uid           [  full  ] joshua lai < joshua.lai@grindr.com>
sub   rsa2048 2017-04-27 [S]
sub   rsa2048 2017-04-27 [E]
sub   rsa2048 2017-04-27 [A]

pub   rsa4096 2016-06-06 [SC] [expires: 2017-09-06]
      6D63865D1C6EEB0F92C394A15D21FFA27D8DCC66
uid           [  full  ] Naftuli Kay < me@naftuli.wtf>
uid           [  full  ] Naftuli Kay < rfkrocktk@gmail.com>
uid           [  full  ] Naftuli Kay < naftuli.kay@grindr.com>
sub   rsa4096 2016-06-06 [E]
sub   rsa4096 2016-06-06 [S]
sub   rsa4096 2016-06-06 [A]
```

---
## Generate a secret

```bash
$ randomness=$( \
    gtr -dc 'A-Za-z0-9!%()+,-:<=>?@[\]^_`{|}~' \
    < /dev/urandom | head -c 512)

$ gpg2 --encrypt -r naftuli.kay \
                -r joshua.lai  \
                -r slava.maslennikov \
            -o bin/vault_passphrase.gpg
```

---
## Upload to your local VCS

```bash
$ git add bin/vault_passphrase.gpg
$ git commit -m "Pizza has arrived! But don't tell anyone"
$ git push origin pizza_days
```

---
## Decrypt the secret

```bash
$ gpg --decrypt bin/vault_passphrase.gpg
gpg: encrypted with 2048-bit RSA key, ID 48671D9C66D275F8,
created 2017-04-27
      "joshua lai < joshuajlai@gmail.com>"
gpg: encrypted with 4096-bit RSA key, ID 82742D54722793E5,
created 2016-06-06
      "Naftuli Kay < me@naftuli.wtf>"
gpg: encrypted with 4096-bit RSA key, ID 5A037740FB5C889B,
created 2017-08-07
      "Svyatoslav I. Maslennikov < slava.maslennikov@grindr.com>"
Pizza is delicious and tasty
```

---
## Use in the wild

* [Ansible: PR 102](https://github.com/grindrllc/ansible/pull/102)
* [Ansible: PR 109](https://github.com/grindrllc/ansible/pull/109)

---
## Thank you!

Don't forget to present pizza to your local ISRE team!
