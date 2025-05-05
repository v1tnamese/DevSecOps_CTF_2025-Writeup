
---

```markdown
# 🔐 Password Cracking - 1 (20 points)

> **Category:** DevOps / Crypto  
> **Description:**  
> The flag is the password for this Ansible Vault file.

---

## 📁 Challenge Files

- `vault?token=...` (downloaded file, actually an Ansible Vault file)
- Contains ASCII header:
```

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

