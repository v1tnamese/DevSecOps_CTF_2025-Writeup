



# 🔐 Password Cracking - 1 (20 points)

> **Category:** DevOps / Crypto  
> **Description:**  
> The flag is the password for this Ansible Vault file.



## 📁 Challenge Files

- `vault?token=...` (downloaded file, actually an Ansible Vault file)
- Contains ASCII header:


\$ANSIBLE\_VAULT;1.1;AES256

````

---

## 🧠 Objective

Recover the password used to encrypt the Ansible Vault file in order to retrieve the flag.

---

## 🛠️ Tools Used

- `strings`
- `ansible2john` (from [John the Ripper jumbo edition](https://github.com/openwall/john))
- `hashcat` (for GPU-accelerated cracking)
- `rockyou.txt` wordlist

---

## 🔍 Steps to Solve

### 1. Inspect the Vault File

```bash
strings vault?token=... 
````

Found:

```
$ANSIBLE_VAULT;1.1;AES256
...
(hex-encoded ciphertext)
```

→ Confirmed: This is an **Ansible Vault** file encrypted with AES256.

---

### 2. Convert Vault File to Hashcat Format

```bash
ansible2john vault?token=... > credentials.hash
```

Output:

```bash
$ansible$0*0*<salt>*<HMAC>*<ciphertext>
```

---

### 3. Crack the Vault Password with Hashcat

Run Hashcat using mode `16900` (Ansible Vault):

```bash
hashcat -m 16900 -O -a 0 -w 4 credentials.hash /usr/share/wordlists/rockyou.txt
```

After a few minutes:

```
Hash Cracked: zebracakes
```

---

## 🏁 Flag

```
zebracakes
```

Use this password to decrypt the vault and retrieve the content if needed.

---

## 📚 Notes

* Hashcat mode **16900** is specific for Ansible Vaults.
* Make sure `hashcat` has GPU support properly configured for optimal speed.
* Always preprocess targets with `ansible2john` before cracking.

---



Dưới đây là nội dung writeup bằng tiếng Anh, theo chuẩn định dạng `README.md` dành cho GitHub, trình bày quá trình phân tích và bẻ khóa hash trong bài tập CTF thuộc mảng `password_cracking`:

---



---

# 🔐 Password Cracking - HMAC-SHA512


## 🧩 Challenge Description

You are given a single hash and asked to find the password and salt used to generate it. 

Given hash:

d8e5d901a23c7d3023eedf501b626bfdc4a3b243635491e6d2abd39c0ec7cf9dff0c677383a7558e066d1417b08a3311d0ebcdc5f8b9f219477839dcb0ebfbfe

````

## 🔍 Step 1: Identify the Hash Type

We started by using two common hash identification tools:

### Using `hashid`:
```bash
hashid 'd8e5...fbfe'
````

Output:

```
[+] SHA-512 
[+] Whirlpool 
[+] Salsa10 
[+] Salsa20 
[+] SHA3-512 
[+] Skein-512 
[+] Skein-1024(512)
```

### Using `hash-identifier`:

```bash
hash-identifier 'd8e5...fbfe'
```

Output:

```
Possible Hashs:
[+] SHA-512
[+] Whirlpool

Least Possible Hashs:
[+] SHA-512(HMAC)
[+] Whirlpool(HMAC)
```

## 🧠 Step 2: Find Hashcat Mode

To confirm this is an **HMAC-SHA512**, we searched for supported hash modes using:

```bash
hashcat --help | grep '512' | grep 'salt'
```

We determined the hash mode to be:

```
1760 | HMAC-SHA512 (key = $salt)
```

## 🧨 Step 3: Crack the Hash with Hashcat

We assumed the format used in the hash file is:

```
<hash>:<salt>
```

We placed the target hash and salt (`PunkCTF2025`) in `crack3.txt` in the format required by hashcat:

```
d8e5...fbfe:PunkCTF2025
```

Then we ran the following command:

```bash
hashcat -m 1760 -a 0 -w 3 -r /usr/share/hashcat/rules/best64.rule crack3.txt zebra_rockyou.txt
```

### Parameters:

* `-m 1760`: Specifies HMAC-SHA512 with salt as key.
* `-a 0`: Straight attack mode.
* `-r best64.rule`: Applies mutation rules.
* `zebra_rockyou.txt`: Custom wordlist based on zebra-themed passwords.

## ✅ Result

```bash
Status...........: Cracked
Hash.Mode........: 1760 (HMAC-SHA512 (key = $salt))
Hash.Target......: d8e5...fbfe:PunkCTF2025
Recovered........: 1/1 (100.00%)
Candidates.......: zebras -> *as**a
Password.........: zanyzebra9
Salt.............: PunkCTF2025
```

## 🧠 Lessons Learned

* `hashid` and `hash-identifier` are useful for narrowing down possible hash types.
* Always consider HMAC when SHA-512 is suggested with a salt.
* Hashcat mode 1760 (`HMAC-SHA512 (key = $salt)`) works with format `hash:salt`.
* Rule-based cracking (`best64.rule`) improves success rate significantly.

## 🛠️ Tools Used

* [`hashid`](https://github.com/psypanda/hashID)
* [`hash-identifier`](https://github.com/blackploit/hash-identifier)
* [`hashcat`](https://github.com/hashcat/hashcat)

## 📁 Files

* `crack3.txt`: contains the hash and salt
* `zebra_rockyou.txt`: custom password list
* `best64.rule`: mutation rules for hashcat

