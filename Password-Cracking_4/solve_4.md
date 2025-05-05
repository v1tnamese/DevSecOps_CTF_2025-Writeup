┌──(kali㉿kali)-[~/CTF/DevOps/password_cracking]
└─$ echo '$6$Q9/shQzQf6xlQyKr$bfHWQDlkwfvrJTBU0itN6kJeyEwQKfvviQ3buIDDNG1S/77a52unKnEssSw340AOMoGzUiyQ.l60wfho28Ay41' > crack4.txt

┌──(kali㉿kali)-[~/CTF/DevOps/password_cracking]
└─$ hashid '$6$Q9/shQzQf6xlQyKr$bfHWQDlkwfvrJTBU0itN6kJeyEwQKfvviQ3buIDDNG1S/77a52unKnEssSw340AOMoGzUiyQ.l60wfho28Ay41'
Analyzing '$6$Q9/shQzQf6xlQyKr$bfHWQDlkwfvrJTBU0itN6kJeyEwQKfvviQ3buIDDNG1S/77a52unKnEssSw340AOMoGzUiyQ.l60wfho28Ay41'
[+] SHA-512 Crypt 

┌──(kali㉿kali)-[~/CTF/DevOps/password_cracking]
└─$ john --format=sha512crypt --wordlist=/usr/share/wordlists/rockyou.txt crack4.txt
Using default input encoding: UTF-8
Loaded 1 password hash (sha512crypt, crypt(3) $6$ [SHA512 128/128 AVX 2x])
No password hashes left to crack (see FAQ)
                                                                                                                    
┌──(kali㉿kali)-[~/CTF/DevOps/password_cracking]
└─$ john --show --format=sha512crypt crack4.txt                                     

?:pinkzebra

1 password hash cracked, 0 left
