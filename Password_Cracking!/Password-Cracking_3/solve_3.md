```bash
┌──(kali㉿kali)-[~/CTF/DevOps/password_cracking]
└─$ hashid 'd8e5d901a23c7d3023eedf501b626bfdc4a3b243635491e6d2abd39c0ec7cf9dff0c677383a7558e066d1417b08a3311d0ebcdc5f8b9f219477839dcb0ebfbfe'
Analyzing 'd8e5d901a23c7d3023eedf501b626bfdc4a3b243635491e6d2abd39c0ec7cf9dff0c677383a7558e066d1417b08a3311d0ebcdc5f8b9f219477839dcb0ebfbfe'
[+] SHA-512 
[+] Whirlpool 
[+] Salsa10 
[+] Salsa20 
[+] SHA3-512 
[+] Skein-512 
[+] Skein-1024(512) 


┌──(kali㉿kali)-[~/CTF/DevOps/password_cracking]
└─$ hash-identifier 'd8e5d901a23c7d3023eedf501b626bfdc4a3b243635491e6d2abd39c0ec7cf9dff0c677383a7558e066d1417b08a3311d0ebcdc5f8b9f219477839dcb0ebfbfe'

   #########################################################################
   #     __  __                     __           ______    _____           #
   #    /\ \/\ \                   /\ \         /\__  _\  /\  _ `\         #
   #    \ \ \_\ \     __      ____ \ \ \___     \/_/\ \/  \ \ \/\ \        #
   #     \ \  _  \  /'__`\   / ,__\ \ \  _ `\      \ \ \   \ \ \ \ \       #
   #      \ \ \ \ \/\ \_\ \_/\__, `\ \ \ \ \ \      \_\ \__ \ \ \_\ \      #
   #       \ \_\ \_\ \___ \_\/\____/  \ \_\ \_\     /\_____\ \ \____/      #
   #        \/_/\/_/\/__/\/_/\/___/    \/_/\/_/     \/_____/  \/___/  v1.2 #
   #                                                             By Zion3R #
   #                                                    www.Blackploit.com #
   #                                                   Root@Blackploit.com #
   #########################################################################
--------------------------------------------------

Possible Hashs:
[+] SHA-512
[+] Whirlpool

Least Possible Hashs:
[+] SHA-512(HMAC)
[+] Whirlpool(HMAC)


┌──(kali㉿kali)-[~/CTF/DevOps/password_cracking]
└─$ hashcat --help |grep '512' | grep 'salt'
   1710 | sha512($pass.$salt)                                        | Raw Hash salted and/or iterated
   1720 | sha512($salt.$pass)                                        | Raw Hash salted and/or iterated
   1740 | sha512($salt.utf16le($pass))                               | Raw Hash salted and/or iterated
   1730 | sha512(utf16le($pass).$salt)                               | Raw Hash salted and/or iterated
   1760 | HMAC-SHA512 (key = $salt)                                  | Raw Hash authenticated

┌──(kali㉿kali)-[~/CTF/DevOps/password_cracking]
└─$ hashcat -m 1760 -a 0 -w 3 -r /usr/share/hashcat/rules/best64.rule crack3.txt zebra_rockyou.txt

d8e5d901a23c7d3023eedf501b626bfdc4a3b243635491e6d2abd39c0ec7cf9dff0c677383a7558e066d1417b08a3311d0ebcdc5f8b9f219477839dcb0ebfbfe:PunkCTF2025:zanyzebra9
                                                          
Session..........: hashcat
Status...........: Cracked
Hash.Mode........: 1760 (HMAC-SHA512 (key = $salt))
Hash.Target......: d8e5d901a23c7d3023eedf501b626bfdc4a3b243635491e6d2a...TF2025
Time.Started.....: Mon May  5 13:52:51 2025 (0 secs)
Time.Estimated...: Mon May  5 13:52:51 2025 (0 secs)
Kernel.Feature...: Pure Kernel
Guess.Base.......: File (zebra_rockyou.txt)
Guess.Mod........: Rules (/usr/share/hashcat/rules/best64.rule)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........:  3909.2 kH/s (9.03ms) @ Accel:1024 Loops:77 Thr:1 Vec:4
Recovered........: 1/1 (100.00%) Digests (total), 1/1 (100.00%) Digests (new)
Progress.........: 42889/42889 (100.00%)
Rejected.........: 0/42889 (0.00%)
Restore.Point....: 0/557 (0.00%)
Restore.Sub.#1...: Salt:0 Amplifier:0-77 Iteration:0-77
Candidate.Engine.: Device Generator
Candidates.#1....: zebras -> *as**a
Hardware.Mon.#1..: Util: 29%
```