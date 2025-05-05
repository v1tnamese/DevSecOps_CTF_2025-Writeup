



# 🔐 Password Cracking - 1 (20 points)

> **Category:** DevOps / Crypto  
> **Description:**  
> The flag is the password for this Ansible Vault file.



## 📁 Challenge Files

- `vault?token=...` (downloaded file, actually an Ansible Vault file)
- Contains ASCII header:


\$ANSIBLE\_VAULT;1.1;AES256

````

## 🧠 Objective

Recover the password used to encrypt the Ansible Vault file in order to retrieve the flag.


## 🛠️ Tools Used

- `strings`
- `ansible2john` (from [John the Ripper jumbo edition](https://github.com/openwall/john))
- `hashcat` (for GPU-accelerated cracking)
- `rockyou.txt` wordlist

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

### 2. Convert Vault File to Hashcat Format

```bash
ansible2john vault?token=... > credentials.hash
```

Output:

```bash
$ansible$0*0*<salt>*<HMAC>*<ciphertext>
```

### 3. Crack the Vault Password with Hashcat

Run Hashcat using mode `16900` (Ansible Vault):

```bash
hashcat -m 16900 -O -a 0 -w 4 credentials.hash /usr/share/wordlists/rockyou.txt
```

After a few minutes:

```
Hash Cracked: zebracakes
```

## 🏁 Flag

```
zebracakes
```

## 📚 Notes

* Hashcat mode **16900** is specific for Ansible Vaults.
* Make sure `hashcat` has GPU support properly configured for optimal speed.
* Always preprocess targets with `ansible2john` before cracking.



---

---

# 🔐 Password Cracking - 3 (20 points)

## Challenge Description

You're given the following hash and a salt:

- **Hash**:

d8e5d901a23c7d3023eedf501b626bfdc4a3b243635491e6d2abd39c0ec7cf9dff0c677383a7558e066d1417b08a3311d0ebcdc5f8b9f219477839dcb0ebfbfe

````
- **Salt**: `PunkCTF2025`

📝 **Note**: The flag is the cracked password.

## 🔎 Step-by-Step Solution

### 1. Hash Type Identification

We first identify the hash type using tools like `hashid` and `hash-identifier`:

```bash
$ hashid <hash>
````

**Result**:

```
[+] SHA-512
[+] Whirlpool
...
```

This suggests a SHA-512 based hash, possibly HMAC.

```bash
$ hash-identifier <hash>
```

**Result**:

```
Possible Hashs:
[+] SHA-512
[+] Whirlpool
Least Possible Hashs:
[+] SHA-512(HMAC)
[+] Whirlpool(HMAC)
```

The result confirms it's very likely to be **HMAC-SHA512**.


### 2. Matching Hashcat Mode

We search Hashcat's help menu for salt-based SHA-512 variants:

```bash
$ hashcat --help | grep '512' | grep 'salt'
```

Relevant match:

```
1760 | HMAC-SHA512 (key = $salt)
```

We'll proceed with Hashcat mode `1760`.


### 3. Hash Cracking

We prepare our inputs:

* **Wordlist**: `zebra_rockyou.txt` (custom wordlist)
* **Rules**: `best64.rule`
* **Hash** file: `crack3.txt` (contains only the hash)

Run Hashcat:

```bash
$ hashcat -m 1760 -a 0 -w 3 -r /usr/share/hashcat/rules/best64.rule crack3.txt zebra_rockyou.txt
```

**Output**:

```
...
Recovered........: 1/1 (100.00%)
Hash.............: d8e5...fbfe:PunkCTF2025:zanyzebra9
...
```


## 🏁 Flag

```
zanyzebra9
```



## 🔧 Tools Used

* [Hashcat](https://hashcat.net/hashcat/)
* `hashid` / `hash-identifier`
* Wordlist: Custom `zebra_rockyou.txt`
* Ruleset: `best64.rule`


---
---

# 🔐 Password Cracking - 4

**Points**: 20
**Category**: Password Cracking
**Challenge Description**:
You are given a password hash in the format used by `/etc/shadow`. Your task is to recover the original password (the flag).


## 🧩 Challenge Details

**Hash:**

```
$6$Q9/shQzQf6xlQyKr$bfHWQDlkwfvrJTBU0itN6kJeyEwQKfvviQ3buIDDNG1S/77a52unKnEssSw340AOMoGzUiyQ.l60wfho28Ay41
```

This hash uses the **SHA-512 Crypt** format (common on Unix/Linux systems), as indicated by the `$6$` prefix.


## 🔍 Step-by-Step Solution

### 🔹 Step 1: Identify the Hash Type

We use `hashid` to confirm the hash type.

```bash
hashid '$6$Q9/shQzQf6xlQyKr$bfHWQDlkwfvrJTBU0itN6kJeyEwQKfvviQ3buIDDNG1S/77a52unKnEssSw340AOMoGzUiyQ.l60wfho28Ay41'
```

**Result:**

```
[+] SHA-512 Crypt
```

✅ Confirmed: This is a Linux SHA512-crypt hash.


### 🔹 Step 2: Crack the Hash Using John the Ripper

We save the hash into a file called `crack4.txt`:

```bash
echo '$6$Q9/shQzQf6xlQyKr$bfHWQDlkwfvrJTBU0itN6kJeyEwQKfvviQ3buIDDNG1S/77a52unKnEssSw340AOMoGzUiyQ.l60wfho28Ay41' > crack4.txt
```

Run John with the correct format (`sha512crypt`) and a popular wordlist (`rockyou.txt`):

```bash
john --format=sha512crypt --wordlist=/usr/share/wordlists/rockyou.txt crack4.txt
```

John successfully cracks the password!


### 🔹 Step 3: Retrieve the Cracked Password

To view the cracked password:

```bash
john --show --format=sha512crypt crack4.txt
```

**Output:**

```
?:pinkzebra
```

✅ Cracked Password (Flag): `pinkzebra`


## 🛠️ Tools Used

* [`hashid`](https://github.com/psypanda/hashID) – to identify the hash type.
* [`john`](https://www.openwall.com/john/) – for password cracking.
* `rockyou.txt` – classic wordlist from Kali Linux.
