┌──(kali㉿kali)-[~/CTF/DevOps/password_cracking]
└─$ strings vault\?token=eyJ1c2VyX2lkIjo0MjMsInRlYW1faWQiOjI3MiwiZmlsZV9pZCI6Mn0.aBdJJA.byv2j8mtkuf1HqdNmdzql-iE19Q 
$ANSIBLE_VAULT;1.1;AES256
61643339343131623937633938363634353838633532643166353162343034353764623238313134
3664373637333039343265613130643661303132376537620a353362643664353463636338653830
65366434356566643865326635643533373736346535343433666462643634383137346334363737
3735393963326566360a653463616465653934393365663535393361356337376438633864376431
36646333356330363139653830346261326239313363636431666139353939376364

┌──(kali㉿kali)-[~/CTF/DevOps/password_cracking]
└─$ ansible2john credentials.vault > credentials.hash  

┌──(kali㉿kali)-[~/CTF/DevOps/password_cracking]
└─$ cat credentials.hash 
credentials.vault:$ansible$0*0*ad39411b97c98664588c52d1f51b40457db281146d76730942ea10d6a0127e7b*e4cadee9493ef5593a5c77d8c8d7d16dc35c0619e804ba2b913ccd1fa95997cd*53bd6d54ccc8e80e6d45efd8e2f5d537764e5443fdbd648174c46777599c2ef6
                  
┌──(kali㉿kali)-[~/CTF/DevOps/password_cracking]
└─$ cat credentials.hash
$ansible$0*0*ad39411b97c98664588c52d1f51b40457db281146d76730942ea10d6a0127e7b*e4cadee9493ef5593a5c77d8c8d7d16dc35c0619e804ba2b913ccd1fa95997cd*53bd6d54ccc8e80e6d45efd8e2f5d537764e5443fdbd648174c46777599c2ef6

┌──(kali㉿kali)-[~/CTF/DevOps/password_cracking]
└─$ hashcat -m 16900 -O -a 0 -w 4 credentials.hash /usr/share/wordlists/rockyou.txt

$ansible$0*0*ad39411b97c98664588c52d1f51b40457db281146d76730942ea10d6a0127e7b*e4cadee9493ef5593a5c77d8c8d7d16dc35c0619e804ba2b913ccd1fa95997cd*53bd6d54ccc8e80e6d45efd8e2f5d537764e5443fdbd648174c46777599c2ef6:zebracakes
                                                          
Session..........: hashcat
Status...........: Cracked
Hash.Mode........: 16900 (Ansible Vault)
Hash.Target......: $ansible$0*0*ad39411b97c98664588c52d1f51b40457db281...9c2ef6
Time.Started.....: Sun May  4 10:53:35 2025 (3 mins, 43 secs)
Time.Estimated...: Sun May  4 10:57:18 2025 (0 secs)
Kernel.Feature...: Pure Kernel
Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........:     2067 H/s (195.93ms) @ Accel:1024 Loops:1024 Thr:1 Vec:8
Recovered........: 1/1 (100.00%) Digests (total), 1/1 (100.00%) Digests (new)
Progress.........: 454656/14344385 (3.17%)
Rejected.........: 0/454656 (0.00%)
Restore.Point....: 450560/14344385 (3.14%)
Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:9216-9999
Candidate.Engine.: Device Generator
Candidates.#1....: 111813 -> yobabz
Hardware.Mon.#1..: Util: 50%
