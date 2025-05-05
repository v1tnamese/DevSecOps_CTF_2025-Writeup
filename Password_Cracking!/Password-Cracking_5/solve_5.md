```bash
┌──(kali㉿kali)-[~/CTF/DevOps/password_cracking]
└─$ hashid '92d7dcb3b27551277307d46856325798'                                                                          
Analyzing '92d7dcb3b27551277307d46856325798'
[+] MD2 
[+] MD5 
[+] MD4 
[+] Double MD5 
[+] LM 
[+] RIPEMD-128 
[+] Haval-128 
[+] Tiger-128 
[+] Skein-256(128) 
[+] Skein-512(128) 
[+] Lotus Notes/Domino 5 
[+] Skype 
[+] Snefru-128 
[+] NTLM 
[+] Domain Cached Credentials 
[+] Domain Cached Credentials 2 
[+] DNSSEC(NSEC3) 
[+] RAdmin v2.x 


┌──(kali㉿kali)-[~/CTF/DevOps/password_cracking]
└─$ john --format=raw-md5 --wordlist=/usr/share/wordlists/rockyou.txt crack5.txt 

Using default input encoding: UTF-8
Loaded 1 password hash (Raw-MD5 [MD5 128/128 AVX 4x3])
Warning: no OpenMP support for this hash type, consider --fork=4
Press 'q' or Ctrl-C to abort, almost any other key for status
3greenzebras     (?)     
1g 0:00:00:00 DONE (2025-05-05 14:54) 1.428g/s 17810Kp/s 17810Kc/s 17810KC/s 3groogu..3gina8
Use the "--show --format=Raw-MD5" options to display all of the cracked passwords reliably
Session completed. 
                           
```