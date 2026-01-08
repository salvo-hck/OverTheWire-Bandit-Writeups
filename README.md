# OverTheWire-Bandit-Writeups
My solutions and technical notes for the Bandit levels on OverTheWire

OverTheWire Bandit Completion 🚩
This repository contains my technical notes and solutions for the OverTheWire Bandit wargame.

🛡️ Key Skills Demonstrated:
Linux PrivEsc: Exploiting SUID binaries and cronjobs.

Network Security: Using nc, and openssl for service discovery.

Automation: Writing bash and python scripts to automate brute-force attacks.

Shell Escaping: Breaking out of restricted shells ($0, vim, etc.).


# Bandit Notes

**Tags**: #Bandit #Linux #CTF

---

## Level 0

**Tags**: #SSH

**Goal**: Log in to the Bandit server via SSH.

**Solution**:

bash

```
ssh -p 2220 bandit0@bandit.labs.overthewire.org
```

**Password**: bandit0

**How It Works**:

- ssh -p 2220: Connects to the server on port 2220.
- User: bandit0, password: bandit0 (given).

**Other Commands**:

- man ssh: Check SSH options.
- ssh -v: Debug connection.

**Notes**: Learned SSH basics. Will try verbose mode next time.

---

## Level 0-1

**Tags**: #FileAccess

**Goal**: Read password from readme in home directory.

**Solution**:

bash

```
cat readme
```

**Password**: ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If

**How It Works**:

- cat readme: Prints the file’s content (password).

**Other Commands**:

- less readme: View interactively.
- file readme: Check file type.

**Notes**: Simple file reading. Will practice less for big files.

---

## Level 1

**Tags**: #SpecialCharacters

**Goal**: Read password from file named -.

**Solution**:

bash

```
cat ./-
```

**Password**: 263JGJPfgU6LtdEvgfWU1XP5yac29mFx

**How It Works**:

- ./-: Uses path to avoid treating - as an option.
- cat ./-: Reads the file.

**Other Commands**:

- cat -- -: Use -- to handle dashes.
- ls -a: List all files.

**Notes**: Dashes in filenames are tricky. Will try -- next time.

---

## Level 2
**Tags**: #Spaces

**Goal**: Read password from spaces in this filename.

**Solution**:

bash

```
cat "spaces in this filename"
```

**Password**: MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx

**How It Works**:

- "spaces in this filename": Quotes handle spaces in the filename.
- cat: Reads the file.

**Other Commands**:

- cat spaces\ in\ this\ filename: Escape spaces.
- ls: Confirm filename.

**Notes**: Quoting is key for spaces. Will use tab completion to avoid errors.

---

## Level 3

**Tags**: #HiddenFiles

**Goal**: Read password from .hidden in inhere directory.

**Solution**:

bash

```
ls -a inhere
cat inhere/.hidden
```

**Password**: 2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ

**How It Works**:

- ls -a: Shows hidden files (starting with .).
- cat inhere/.hidden: Reads the password.

**Other Commands**:

- find inhere -type f: Find all files.
- ls -la: Show file details.

**Notes**: Hidden files are common. Will use ls -a to check for them.

---

## Level 4

**Tags**: #Find

**Goal**: Find human-readable file among -file00 to -file09 in inhere.

**Solution**:

bash

```
for i in {0..9}; do cat inhere/-file0$i; done
```

**Password**: 4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw

**How It Works**:

- for i in {0..9}: Loops through filenames (-file00 to -file09).
- cat inhere/-file0$i: Prints each file; readable one has the password.
- reset: Fixes terminal if garbled by binary files.

**Other Commands**:

- file inhere/-file0*: Check file types.
- strings inhere/-file0*: Extract readable strings.

**Notes**: Loops are useful. Will try file to identify text files first.

---

## Level 5

**Tags**: #Find

**Goal**: Find file in inhere: 1033 bytes, readable, not executable.

**Solution**:

bash

```
find inhere -type f -size 1033c -readable ! -executable -exec file {} \; | grep text | cut -d: -f1
cat inhere/maybehere07/.file2
```

**Password**: HWasnPhtq9AVKe0dmk45nxy20cvUa6EG

**How It Works**:

- find inhere -type f -size 1033c -readable ! -executable: Finds files matching criteria.
- -exec file {} \; | grep text: Identifies text files.
- cut -d: -f1: Gets file path.
- cat inhere/maybehere07/.file2: Reads password.

**Other Commands**:

- ls -l inhere/*/*: Check sizes.
- strings inhere/maybehere07/.file2: Extract strings.

**Notes**: find is powerful. Piping to grep and cut was new.

---

## Level 6

**Tags**: #Find #Permissions

**Goal**: Find file owned by bandit7 (user), bandit6 (group), 33 bytes.

**Solution**:

bash

```
find / -type f -user bandit7 -group bandit6 -size 33c -readable 2>/dev/null
cat /var/lib/dpkg/info/bandit7.password
```

**Password**: morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj

**How It Works**:

- find / -type f -user bandit7 -group bandit6 -size 33c -readable: Searches filesystem for matching file.
- 2>/dev/null: Hides permission errors.
- cat /var/lib/dpkg/info/bandit7.password: Reads password.

**Other Commands**:

- ls -l /var/lib/dpkg/info: Check ownership.
- stat /var/lib/dpkg/info/bandit7.password: View metadata.

**Notes**: Full filesystem search was cool. Learned to suppress errors.

---

## Level 7

**Tags**: #Grep

**Goal**: Find password in data.txt on line with “millionth”.

**Solution**:

bash

```
grep "millionth" data.txt
```

**Password**: dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc

**How It Works**:

- grep "millionth" data.txt: Finds line with “millionth”.
- Password is after “millionth”.

**Other Commands**:

- awk '/millionth/ {print $2}' data.txt: Extract password.
- sed -n '/millionth/p' data.txt: Print matching line.

**Notes**: grep is simple for text search. Will try awk for field extraction.

---

## Level 8

**Tags**: #Uniq

**Goal**: Find unique line in data.txt.

**Solution**:

bash

```
sort data.txt | uniq -c | grep "^ *1 "
```

**Password**: 4CKMh1JI91bUIZZPXDqGanal4xvAg0JM

**How It Works**:

- sort data.txt: Sorts lines.
- uniq -c: Counts line occurrences.
- grep "^ *1 ": Finds line with count 1 (unique).
- Password follows the count.

**Other Commands**:

- sort -u data.txt: List unique lines.
- uniq -u: Show unique lines (sorted input).

**Notes**: sort and uniq are great together. Quoting grep pattern was key.

---

## Level 9

**Tags**: #Strings

**Goal**: Find password in binary data.txt, after several = characters.

**Solution**:

bash

```
strings data.txt | grep '==' | less
```

**Password**: FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey

**How It Works**:

- strings data.txt: Extracts readable strings from binary.
- grep '==': Finds strings with ==.
- less: Scroll to find password after = characters.

**Other Commands**:

- strings data.txt: View all strings.
- xxd data.txt: Hex dump for inspection.

**Notes**: strings is perfect for binaries. Will try xxd for binary analysis.

---

## Level 10

**Tags**: #Base64

**Goal**: Decode base64 password in data.txt.

**Solution**:

bash

```
base64 -d data.txt
```

**Password**: dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr

**How It Works**:

- base64 -d data.txt: Decodes base64 content.
- Output: The password is (password).

---

## Level 11

**Tags**: #ROT13

**Goal**: Decode ROT13 password in data.txt.

**Solution**:

bash

```
cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
```

**Password**: 

**How It Works**:7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4

- cat data.txt: Reads file.
- tr 'A-Za-z' 'N-ZA-Mn-za-m': Shifts letters by 13 positions (ROT13).
- Output: The password is (password>).

**Other Commands**:

- echo (text) | tr 'A-Za-z' 'N-ZA-Mn-za-m': Test ROT13.
- Python: str.translate() for ROT13.

---

# Bandit Level 12

**Tags**: #Bandit #Hexdump #Compression

## Goal

Decode hexdump in data.txt and decompress repeatedly to find the password.

## Solution

```bash
mktemp -d
cp data.txt /tmp/tmp.eV8Og8TFZD
cd /tmp/tmp.eV8Og8TFZD
mv data.txt hexdata.txt
xxd -r hexdata.txt data.bin
file data.bin  # bzip2
mv data.bin data.bz2
bunzip2 data.bz2
file data  # gzip
mv data data.gz
gunzip data.gz
file data  # tar
mv data data.tar
tar xf data.tar
file data5.bin  # tar
mv data5.bin data2.tar
tar xf data2.tar
file data6.bin  # bzip2
mv data6.bin data.bz2
bunzip2 data.bz2
file data  # tar
mv data datanew.tar
tar xf datanew.tar
file data8.bin  # gzip
mv data8.bin datanew1.gz
gunzip datanew1.gz
file datanew1  # ASCII text
cat datanew1
```

## Password

FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn

## How It Works

- mktemp -d: Creates temp directory (/tmp/tmp.eV8Og8TFZD).
    
- cp and mv: Copy and rename data.txt to hexdata.txt.
    
- xxd -r: Converts hexdump to binary (data.bin).
    
- file: Identifies compression type (bzip2, gzip, tar).
    
- mv: Renames files to match extensions (.bz2, .gz, .tar).
    
- bunzip2, gunzip, tar xf: Decompress each layer (bzip2 → gzip → tar → tar → bzip2 → tar → gzip).
    
- cat datanew1: Reads final text file with password.
    

## Other Commands

- ls: List files after each step.
    
- xxd: View hexdump (without -r).
    
- strings: Extract text from files.
    

## Notes

Hexdump is hex representation of binary, reversed with xxd -r. Repeated compression required multiple decompressions. Manual process was fun and taught file types. Might try scripting for practice later.

---
# Bandit Level 13 - SSH Key Authentication

## Problem

- Password for /etc/bandit_pass/bandit14 readable **only by bandit14**
- Found sshkey.private (RSA private key) in /home/bandit13/

## Solution

bash

```
# 1. Download key to local machine
scp -P 2220 bandit13@bandit.labs.overthewire.org:sshkey.private .

# 2. Fix permissions (SSH requires 600)
chmod 600 sshkey.private

# 3. SSH as bandit14 using the key
ssh -i sshkey.private bandit14@bandit.labs.overthewire.org -p 2220

# 4. Read the password
cat /etc/bandit_pass/bandit14
```

## Key Points

- **Can't SSH from localhost** (blocked by OverTheWire)
- **Can't read file as bandit13** (permission denied)
- **Private key needs 600 permissions** or SSH refuses to use it
- **Key-based auth** = no password needed for SSH

## Common Errors

- sshkey.private not accessible → wrong directory
- Permissions too open → chmod 600 sshkey.private
- localhost blocked → connect to actual server domain

**Pro Tip**: Delete the private key when done: rm sshkey.private

p: MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS

---
# Bandit Level 14 → 15 - Submit Password to Port 30000

## Goal
Send **current password** (`bandit14`) to `localhost:30000` → get **bandit15** password.

---

## Solution
```bash
nc localhost 30000
```

**Input:**
```
MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS
```

**Output:**
```
Correct!
8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo
```

> `8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo` = **bandit15 password**

---

## Key Points
- **Port 30000 ≠ SSH** → **Don’t use `ssh`**
- Use `nc` (netcat) to send raw text
- Service expects **one line**: the current password

---

## One-Liner (Auto)
```bash
echo "MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS" | nc localhost 30000
```

---

## Common Mistake
```bash
ssh -p 30000 ...   # Wrong — closes connection
```

---


**Next**: ssh bandit15@bandit.labs.overthewire.org -p 2220
Password: 
         8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo

---

# Bandit 15 → 16: SSL Password Submit

**Goal**: Send password to `localhost:30001` via **SSL/TLS**

```bash
echo "8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo" | \
openssl s_client -connect localhost:30001 -quiet
```

**Output**:
```
Correct!
kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx
```

kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx = **bandit16 password**

**Key Flags**:
- `-connect host:port` → target
- `-quiet` → suppress `RENEGOTIATING`, `DONE`

**Self-signed cert warning** = expected (ignore)

---

# Bandit 16 → 17: SSL Port + RSA Key Auth

**Goal**: Submit `bandit16` password to one SSL port in `31000–32000` → get **RSA private key** for `bandit17`

---

## Step 1: Scan Open Ports
```bash
nc -z localhost 31000-32000
````

**Output**:

text

```
31046, 31518, 31691, 31790, 31960
```

---

## Step 2: Fingerprint with openssl s_client

bash

```
for p in 31046 31518 31691 31790 31960; do
  echo "=== $p ==="
  echo "kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx" | \
  openssl s_client -connect localhost:$p -quiet -ign_eof
done
```

**Key Output** (31790):

text

```
Correct!
-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEAvmOkuifmMg6HL2YPIOjon6iWfbp7c3jx34YkYWqUH57SUdyJ
...
-----END RSA PRIVATE KEY-----
```

> 31790 = **SSL + password checker**

---

## Step 3: Save Key in bandit16

bash

```
cat > /tmp/key17 << 'EOF'
-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEAvmOkuifmMg6HL2YPIOjon6iWfbp7c3jx34YkYWqUH57SUdyJ
imZzeyGC0gtZPGujUSxiJSWI/oTqexh+cAMTSMlOJf7+BrJObArnxd9Y7YT2bRPQ
Ja6Lzb558YW3FZl87ORiO+rW4LCDCNd2lUvLE/GL2GWyuKN0K5iCd5TbtJzEkQTu
DSt2mcNn4rhAL+JFr56o4T6z8WWAW18BR6yGrMq7Q/kALHYW3OekePQAzL0VUYbW
JGTi65CxbCnzc/w4+mqQyvmzpWtMAzJTzAzQxNbkR2MBGySxDLrjg0LWN6sK7wNX
x0YVztz/zbIkPjfkU1jHS+9EbVNj+D1XFOJuaQIDAQABAoIBABagpxpM1aoLWfvD
KHcj10nqcoBc4oE11aFYQwik7xfW+24pRNuDE6SFthOar69jp5RlLwD1NhPx3iBl
J9nOM8OJ0VToum43UOS8YxF8WwhXriYGnc1sskbwpXOUDc9uX4+UESzH22P29ovd
d8WErY0gPxun8pbJLmxkAtWNhpMvfe0050vk9TL5wqbu9AlbssgTcCXkMQnPw9nC
YNN6DDP2lbcBrvgT9YCNL6C+ZKufD52yOQ9qOkwFTEQpjtF4uNtJom+asvlpmS8A
vLY9r60wYSvmZhNqBUrj7lyCtXMIu1kkd4w7F77k+DjHoAXyxcUp1DGL51sOmama
+TOWWgECgYEA8JtPxP0GRJ+IQkX262jM3dEIkza8ky5moIwUqYdsx0NxHgRRhORT
8c8hAuRBb2G82so8vUHk/fur85OEfc9TncnCY2crpoqsghifKLxrLgtT+qDpfZnx
SatLdt8GfQ85yA7hnWWJ2MxF3NaeSDm75Lsm+tBbAiyc9P2jGRNtMSkCgYEAypHd
HCctNi/FwjulhttFx/rHYKhLidZDFYeiE/v45bN4yFm8x7R/b0iE7KaszX+Exdvt
SghaTdcG0Knyw1bpJVyusavPzpaJMjdJ6tcFhVAbAjm7enCIvGCSx+X3l5SiWg0A
R57hJglezIiVjv3aGwHwvlZvtszK6zV6oXFAu0ECgYAbjo46T4hyP5tJi93V5HDi
Ttiek7xRVxUl+iU7rWkGAXFpMLFteQEsRr7PJ/lemmEY5eTDAFMLy9FL2m9oQWCg
R8VdwSk8r9FGLS+9aKcV5PI/WEKlwgXinB3OhYimtiG2Cg5JCqIZFHxD6MjEGOiu
L8ktHMPvodBwNsSBULpG0QKBgBAplTfC1HOnWiMGOU3KPwYWt0O6CdTkmJOmL8Ni
blh9elyZ9FsGxsgtRBXRsqXuz7wtsQAgLHxbdLq/ZJQ7YfzOKU4ZxEnabvXnvWkU
YOdjHdSOoKvDQNWu6ucyLRAWFuISeXw9a/9p7ftpxm0TSgyvmfLF2MIAEwyzRqaM
77pBAoGAMmjmIJdjp+Ez8duyn3ieo36yrttF5NSsJLAbxFpdlc1gvtGCWW+9Cq0b
dxviW8+TFVEBl1O4f7HVm6EpTscdDxU+bCXWkfjuRb7Dy9GOtt9JPsX8MBTakzh3
vBgsyi/sN3RqRBcGU40fOoZyfAMT8s1m/uYv52O6IgeuZ/ujbjY=
-----END RSA PRIVATE KEY-----
EOF

chmod 600 /tmp/key17
```

---

## Step 4: Download Key to Local Machine

**From LOCAL terminal**:

bash

```
cd ~/Desktop/overthewire
scp -P 2220 bandit16@bandit.labs.overthewire.org:/tmp/key17 bandit17_key
chmod 600 bandit17_key
```

---

## Step 5: Login as bandit17

bash

```
ssh -i ~/Desktop/overthewire/bandit17_key bandit17@bandit.labs.overthewire.org -p 2220
```

---

## Key Lessons

- nc **lies** about SSL → always verify with openssl s_client
- Correct! + RSA key = **SSH key auth**
- **Never copy-paste keys** → use scp
- **Ports are fixed** (31790 = winner)

---

**Pro Skill**: SSL service fingerprinting + key-based SSH

---
# Bandit 17 → 18: diff 

```bash
diff passwords.old passwords.new
````

bandit17@bandit:~$ diff passwords.old passwords.new
42c42 (line in wich there is a difference)
 < BMIOFKM7CRSLI97voLp3TD80NAq5exxk(old)
---> x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO(new)

password: x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO

---

# Bandit 18 → 19: .bashrc Trap → Clean Shell

**bandit18 password** (from diff/level17):
 
 x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO
---
**Trap**: `.bashrc` → `exit 0` → logout

**Fix**: Force clean shell
```bash
ssh -t bandit18@bandit.labs.overthewire.org -p 2220 "bash --norc"
````

Then:

bash

```
cat readme
```

**bandit19 password**:

text

```
cGWpMaKXVwDUNgPAVJbWYuGHVn9zl3j8
```

**Next**:

bash

```
ssh bandit19@bandit.labs.overthewire.org -p 2220
```

**Pro skill**: Bypass malicious startup scripts (--norc, --noprofile)

p: cGWpMaKXVwDUNgPAVJbWYuGHVn9zl3j8

---

# Bandit 19 → 20: setuid Privilege Escalation

**Binary**: `./bandit20-do`  
**Owner**: `bandit20`  
**Permission**: `-r-sr-x---` → **setuid**

**Test**:
```bash
./bandit20-do whoami
# → bandit20
````

**Exploit**:

bash

```
./bandit20-do cat /etc/bandit_pass/bandit20
```

**bandit20 password**:

text

```
0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO
```

**Next**:

bash

```
ssh bandit20@bandit.labs.overthewire.org -p 2220
```

p:  0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO

---

# Bandit 20 → 21: suconnect — setuid Network Oracle

**Binary**: `./suconnect <port>`  
**setuid**: `bandit21` → can read next password

---

## Exact Flow

| Terminal | Command | Role |
|---------|--------|------|
| **T1** | `nc -l 12345` | **Server** — you **type here** |
| **T2** | `./suconnect 12345` | **Client** — runs as `bandit21` |

```text
You type in T1 ──► nc sends ──► suconnect receives
                  ◄── suconnect checks & replies ◄──
                  nc in T1 prints both lines
````

---



-
Your Execution


# T1
nc -l 12345
0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO   ← typed
EeoULMCra2q0dSkYj561DX7s1CpBuOBt   ← received

# T2
./suconnect 12345
0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO   ← received
Read: ...
Password matches, sending next password

 bandit21 Password

EeoULMCra2q0dSkYj561DX7s1CpBuOBt



Next

ssh bandit21@bandit.labs.overthewire.org -p 2220
p:  EeoULMCra2q0dSkYj561DX7s1CpBuOBt

----------------------------------------------------------------------------------------------
---
---

# **Bandit 21 → 22: Cronjob Password Leak**

**Goal** Find the bandit22 password written by a scheduled cronjob.

---

### 1. Locate the cronjob

bash

```
cd /etc/cron.d
ls
```

**Relevant file:** cronjob_bandit22

bash

```
cat cronjob_bandit22
```

text

```
@reboot bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
* * * * * bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
```

→ Runs **every minute** as **bandit22**

---

### 2. Inspect the script

bash

```
cat /usr/bin/cronjob_bandit22.sh
```

bash

```
#!/bin/bash
chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
cat /etc/bandit_pass/bandit22 > /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
```

→ **Copies** bandit22 password to /tmp/... → **Makes it readable** by everyone (644)

---

### 3. Read the leaked password

bash

```
cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
```

**Output:**

text

```
tRae0UfB9v0UzbCdn9cY0gQnds9GF58Q
```

---

### 4. Login to Level 22

bash

```
ssh bandit22@bandit.labs.overthewire.org -p 2220
```

**Password:**

text

```
tRae0UfB9v0UzbCdn9cY0gQnds9GF58Q
```

---
---
---
# Bandit 22 → 23: Dynamic Cronjob Password Leak

**Goal**  
Extract the bandit23 password from a cronjob that writes it to a unique temp file based on the username.

---

### 1. Locate the cronjob  
```bash
cd /etc/cron.d
ls
```

**Relevant file:** `cronjob_bandit23`

```bash
cat cronjob_bandit23
```
```
@reboot bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
* * * * * bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
```
→ Runs every minute as bandit23

---

### 2. Analyze the script  
```bash
cat /usr/bin/cronjob_bandit23.sh
```
```bash
#!/bin/bash

myname=$(whoami)
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)

echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"

cat /etc/bandit_pass/$myname > /tmp/$mytarget
```

**Logic:**  
- Gets current user → `bandit23`  
- Hashes `"I am user bandit23"` with MD5  
- Writes password to `/tmp/<hash>`

---

### 3. Compute the target filename  
```bash
myname=$(whoami)
mytarget=$(echo "I am user $myname" | md5sum | cut -d ' ' -f 1)
echo $mytarget
```

**Output:**  
```
8ca319486bfbbc3663ea0fbe81326349
```

---

### 4. Read the leaked password  
```bash
cat /tmp/8ca319486bfbbc3663ea0fbe81326349
```

**Output:**  
```
jc1udXuA1tiHqjIsL8yaapX5XIAI6i0n
```

---

### 5. Login to Level 23  
```bash
ssh bandit23@bandit.labs.overthewire.org -p 2220
```
**Password:**  
jc1udXuA1tiHqjIsL8yaapX5XIAI6i0n

p : tRae0UfB9v0UzbCdn9cY0gQnds9GF58Q

---
---
---

my first exploit:

bandit22@bandit:/tmp$ myname="bandit24"
mytarget=$(echo "I am user $myname" | md5sum | cut -d ' ' -f 1)
echo $mytarget
ee4ee1703b083edac9f8183e4ae70293
bandit22@bandit:/tmp$ ls
ls: cannot open directory '.': Permission denied
bandit22@bandit:/tmp$ cat/tmp/ee4ee1703b083edac9f8183e4ae70293
-bash: cat/tmp/ee4ee1703b083edac9f8183e4ae70293: Not a directory
bandit22@bandit:/tmp$ cat /tmp/ee4ee1703b083edac9f8183e4ae70293
gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8
bandit22@bandit:/tmp$ 




# Bandit 23 to 24: Unintended but Valid Exploit

**Your method (correct, but not intended):**

bash

```
myname="bandit24"
mytarget=$(echo "I am user $myname" | md5sum | cut -d ' ' -f 1)
cat /tmp/$mytarget
# → gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8
```

**Intended method:** Create a script → cronjob runs it → emails password → check mail

**You found a logic reuse vulnerability** **Same code pattern = same exploit**

**Real skill:**

- Pattern recognition
- Logic reuse exploitation
- Blind prediction attack


Login to Level 24 (from **your laptop**)

bash

```
ssh bandit24@bandit.labs.overthewire.org -p 2220
```

**Password:**

text

```
gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8
```

---
---
---

# Bandit 24 → 25: Brute-Force PIN with Persistent TCP (Python)

**Goal**  
A daemon on `localhost:30002` expects:  
`bandit24_password + space + 4-digit PIN (0000–9999)`  
Correct PIN → returns `bandit25` password  
Must use **one TCP connection** for all attempts.

---

## Important Note: Write-Only in `/tmp`
- `/home/bandit24` is **read-only** → cannot save files there  
- All scripts **must be written in `/tmp`** (world-writable)

```bash
nano /tmp/brute.py
````

---

## Final Working Script

python

```
#!/usr/bin/env python3
import socket

HOST = "localhost"
PORT = 30002
PASSWORD = "gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8"

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect((HOST, PORT))

for pin in range(10000):
    pin_str = f"{pin:04d}"
    payload = f"{PASSWORD} {pin_str}\n"
    
    s.send(payload.encode())
    response = s.recv(1024).decode().strip()
    
    if "The password of user bandit25 is" in response:
        print("\nSUCCESS:", response)
        with open('/tmp/bandit25_password.txt', 'w') as f:
            f.write(response + "\n")
        print("Password saved to /tmp/bandit25_password.txt")
        break

s.close()
```

---

## Commands Used

bash

```
# 1. Create and edit script in /tmp
nano /tmp/brute.py

# 2. Run the brute-force
python3 /tmp/brute.py

# 3. (Optional) View saved password
cat /tmp/bandit25_password.txt
```

---

## Pseudocode Explanation

text

```
1. Connect to localhost:30002 (TCP)
2. FOR each PIN from 0000 to 9999:
     → Send: "PASSWORD(bandit24password) PIN\n"
     → Read response
     → IF contains "The password of user bandit25 is":
          → Print & save → BREAK
3. Close connection
```

---

## Result

- **PIN found:**1667
- **bandit25 password:**iCi86ttT4KSNe1armKiwbQNmB3YJP3q4
- Saved to: /tmp/bandit25_password.txt

---

## Login to Level 25

bash

```
ssh bandit25@bandit.labs.overthewire.org -p 2220
```

**Password:**

text

```
iCi86ttT4KSNe1armKiwbQNmB3YJP3q4
```
p: iCi86ttT4KSNe1armKiwbQNmB3YJP3q4

---
---
---

# OverTheWire Bandit Level 25→26 Solution

## Challenge Overview

Logging into bandit26 from bandit25 requires understanding that bandit26 uses a custom shell instead of `/bin/bash`, and we need to break out of it.

---

## Step 1: Investigate the Custom Shell

From bandit25, check what shell bandit26 uses:

bash

```bash
grep bandit26 /etc/passwd
```

**Output:**

```
bandit26:x:11026:11026:bandit level 26:/home/bandit26:/usr/bin/showtext
```

The shell is `/usr/bin/showtext` instead of `/bin/bash`.

---

## Step 2: Examine the Custom Shell Script

bash

```bash
cat /usr/bin/showtext
```

**Script contents:**

bash

```bash
#!/bin/sh
export TERM=linux
exec more ~/text.txt
exit 0
```

**Key observations:**

- The script uses `more` to display `~/text.txt`
- Then immediately exits
- `more` is a pager that shows content page by page
- If content fits in terminal, `more` displays everything and exits immediately

---

## Step 3: Explore Bandit26's Directory

bash

```bash
ls -la /home/bandit26
```

**Important findings:**

- `bandit27-do` - A setuid binary owned by bandit27
- `text.txt` - The file displayed by the showtext script

---

## Step 4: Download SSH Key to Local Machine

From your **local computer** (not from bandit25):

bash

```bash
scp -P 2220 bandit25@bandit.labs.overthewire.org:~/bandit26.sshkey ./bandit26.sshkey
chmod 400 bandit26.sshkey
```

**Why local machine?**

- SSH from bandit25 to bandit26 (localhost→localhost) triggers a blocking mechanism
- Connecting from outside avoids this issue

---

## Step 5: Exploit the `more` Command

### The Strategy

1. Make terminal window extremely small (5-10 lines tall)
2. When `text.txt` doesn't fit, `more` pauses for user input
3. From `more`, we can launch vim
4. From vim, we can get a real shell

### Execute the Attack

**On your local machine with a tiny terminal:**

bash

```bash
ssh -i bandit26.sshkey bandit26@bandit.labs.overthewire.org -p 2220
```

You should see `more` pause with content displayed.

---

## Step 6: Break Out Using Vim

### Inside `more`:

Press `v` to open vim editor

### Inside vim (Normal Mode):

1. If in INSERT mode, press `ESC` to get to normal mode
2. Set shell to bash:

vim

```vim
   :set shell=/bin/bash
```

3. Launch the shell:

vim

```vim
   :shell
```

**Result:** You now have a bash shell as bandit26! 🎉

---
---
---
# OverTheWire Bandit Level 25→26 Solution

## Challenge Overview

Logging into bandit26 from bandit25 requires understanding that bandit26 uses a custom shell instead of `/bin/bash`, and we need to break out of it to access a setuid binary.

---

## Step 1: Investigate the Custom Shell

From bandit25, check what shell bandit26 uses:

```bash
grep bandit26 /etc/passwd
```

**Output:**

```
bandit26:x:11026:11026:bandit level 26:/home/bandit26:/usr/bin/showtext
```

The shell is `/usr/bin/showtext` instead of `/bin/bash`.

---

## Step 2: Examine the Custom Shell Script

```bash
cat /usr/bin/showtext
```

**Script contents:**

```bash
#!/bin/sh
export TERM=linux
exec more ~/text.txt
exit 0
```

**Key observations:**

- The script uses `more` to display `~/text.txt`
- Then immediately exits
- `more` is a pager that shows content page by page
- If content fits in terminal, `more` displays everything and exits immediately

---

## Step 3: Explore Bandit26's Directory

```bash
ls -la /home/bandit26
```

**Important findings:**

```
-rwsr-x---   1 bandit27 bandit26 14884 Oct 14 09:26 bandit27-do
-rw-r-----   1 bandit26 bandit26   258 Oct 14 09:26 text.txt
```

- `bandit27-do` - A **setuid binary** owned by bandit27
- The `s` in `-rwsr-x---` means it runs with bandit27's permissions when executed
- `text.txt` - The file displayed by the showtext script

---

## Step 4: Download SSH Key to Local Machine

From your **local computer** (not from bandit25):

```bash
scp -P 2220 bandit25@bandit.labs.overthewire.org:~/bandit26.sshkey ./bandit26.sshkey
chmod 400 bandit26.sshkey
```

**Why local machine?**

- SSH from bandit25 to bandit26 (localhost→localhost) triggers a blocking mechanism
- Connecting from outside avoids this issue

---

## Step 5: Exploit the `more` Command

### The Strategy

1. Make terminal window extremely small (5-10 lines tall)
2. When `text.txt` doesn't fit, `more` pauses for user input
3. From `more`, we can launch vim
4. From vim, we can get a real shell

### Execute the Attack

**On your local machine with a tiny terminal:**

```bash
ssh -i bandit26.sshkey bandit26@bandit.labs.overthewire.org -p 2220
```

You should see `more` pause with content displayed.

---

## Step 6: Break Out Using Vim

### Inside `more`:

Press `v` to open vim editor

### Inside vim:

**If you accidentally enter INSERT mode, press `ESC` first**

Then run these commands:

1. Set shell to bash:
    
    ```vim
    :set shell=/bin/bash
    ```
    
    Press `Enter`
    
2. Launch the shell:
    
    ```vim
    :shell
    ```
    
    Press `Enter`
    

**Result:** You now have a bash shell as bandit26! 🎉

```bash
bandit26@bandit:~$
```

---

## Step 7: Understanding Setuid Binaries

Check the permissions of `bandit27-do`:

```bash
ls -la bandit27-do
```

**Output:**

```
-rwsr-x---   1 bandit27 bandit26 14884 Oct 14 09:26 bandit27-do
```

**Permission breakdown:**

- `-rwsr-x---` - The `s` replaces `x`, meaning **setuid bit is set**
- Owner: `bandit27` - The file is owned by bandit27
- Group: `bandit26` - Members of bandit26 group can execute it
- When bandit26 executes this file, it **runs as bandit27**

**Test it:**

```bash
./bandit27-do id
```

**Output:**

```
uid=11026(bandit26) gid=11026(bandit26) euid=11027(bandit27) groups=11026(bandit26)
```

Notice `euid=11027(bandit27)` - the **effective user ID** is bandit27!

---

## Step 8: Get the Password

```bash
./bandit27-do cat /etc/bandit_pass/bandit27
```

**Output:**

```
upsNCc7vzaRDx6oZC6GiR6ERwe1MowGB
```

✅ **Password obtained!**

---

## Key Concepts Learned

### 1. Custom Shells

- Users can have custom shells defined in `/etc/passwd`
- These shells execute instead of the standard bash shell
- Located in the 7th field: `username:x:UID:GID:comment:home:shell`

### 2. The `more` Command

- Pager for displaying content page by page
- Interactive when content exceeds terminal size
- Can launch vim with `v` command
- Other commands: `Space` (next page), `Enter` (next line), `q` (quit)

### 3. Vim Power & Modes

- **Normal mode**: Default mode for navigation and commands (press `ESC` to enter)
- **Insert mode**: For typing/editing text (press `i`, `a`, etc.)
- **Command mode**: For running commands (type `:` from normal mode)

**Useful vim commands:**

- `:!command` - Execute shell command
- `:set shell=/path/to/shell` - Change shell
- `:shell` - Spawn a shell
- `:q` - Quit
- `ESC` - Return to normal mode

### 4. Setuid Binaries (SUID)

**What is setuid?**

- Special permission bit that allows a file to execute with the **owner's privileges**
- Indicated by `s` in the owner's execute position: `-rwsr-xr-x`

**Permission bits:**

```
-rwsr-x---
 |||
 ||+-- Setuid bit (s instead of x)
 |+--- Write permission
 +---- Read permission
```

**Real vs Effective User ID:**

- `uid` (User ID): Your actual user (bandit26)
- `euid` (Effective User ID): The user the process runs as (bandit27)
- Setuid changes the `euid` to the file owner

**Security implications:**

- Setuid binaries are powerful privilege escalation tools
- Must be carefully audited for vulnerabilities
- Common in system utilities (like `passwd`, `sudo`)

### 5. Terminal Size Manipulation

- Small terminals force pagination in `more`
- Gives us time to interact before script exits
- Critical for this exploit to work

---

## Common Pitfalls

❌ **Trying to SSH from bandit25 to bandit26**

- Localhost connections are blocked with: `!!! Connecting from localhost is blocked`
- Must connect from external machine

❌ **Terminal too large**

- If `text.txt` fits completely, `more` exits immediately
- No opportunity to press `v`
- Solution: Make terminal 5-10 lines tall

❌ **Not understanding vim modes**

- Typing commands while in INSERT mode won't work
- Must press `ESC` first to get to normal mode
- Commands start with `:` in command mode

❌ **Using bandit27-do incorrectly**

- `./bandit27-do bandit27` ❌ (this tries to run a command called "bandit27")
- `./bandit27-do cat /etc/bandit_pass/bandit27` ✅ (this runs the cat command)

---

## Solution Summary

```mermaid
graph TD
    A[Bandit25] --> B[Find custom shell: /usr/bin/showtext]
    B --> C[Analyze script: uses 'more ~/text.txt']
    C --> D[Check bandit26 directory: find bandit27-do setuid binary]
    D --> E[Download SSH key to local machine]
    E --> F[Make terminal tiny 5-10 lines]
    F --> G[SSH from local machine]
    G --> H[more pauses - press 'v']
    H --> I[Vim opens with text.txt]
    I --> J[Press ESC if in INSERT mode]
    J --> K[:set shell=/bin/bash]
    K --> L[:shell]
    L --> M[Bash shell as bandit26!]
    M --> N[ls -la: see bandit27-do with setuid]
    N --> O[./bandit27-do id: verify euid=bandit27]
    O --> P[./bandit27-do cat /etc/bandit_pass/bandit27]
    P --> Q[Password: upsNCc7vzaRDx6oZC6GiR6ERwe1MowGB]
```

---

## Complete Command Reference

```bash
# === Investigation (from bandit25) ===
grep bandit26 /etc/passwd              # Find custom shell
cat /usr/bin/showtext                  # Examine the shell script
ls -la /home/bandit26                  # Check bandit26's files

# === Download key (from local machine) ===
scp -P 2220 bandit25@bandit.labs.overthewire.org:~/bandit26.sshkey ./
chmod 400 bandit26.sshkey

# === Connect with small terminal (from local machine) ===
ssh -i bandit26.sshkey bandit26@bandit.labs.overthewire.org -p 2220

# === In more ===
# Press: v (to open vim)

# === In vim ===
ESC                    # Exit INSERT mode if needed
:set shell=/bin/bash   # Set shell to bash
:shell                 # Spawn bash shell

# === In bash as bandit26 ===
ls -la                                    # See files and permissions
file bandit27-do                          # Verify it's an executable
./bandit27-do id                          # Test setuid (see euid=bandit27)
./bandit27-do cat /etc/bandit_pass/bandit27  # Get password
```

---

## Additional Learning Resources

**Understanding Setuid:**

```bash
# Find all setuid files on system
find / -perm -4000 2>/dev/null

# Check file permissions in detail
stat bandit27-do

# See numeric permission representation
ls -lan bandit27-do
```

**Vim basics:**

- Tutorial: Run `vimtutor` in terminal
- Help: `:help` in vim
- Exit: `:q!` (quit without saving)

**More command:**

- Manual: `man more`
- Help: `h` while in more

---

## Password

```
upsNCc7vzaRDx6oZC6GiR6ERwe1MowGB
```

---

## Tags

#overthewire #bandit #linux #vim #shell-escape #privilege-escalation #setuid #more-command #restricted-shell #suid-binary
p: upsNCc7vzaRDx6oZC6GiR6ERwe1MowGB


---
---
---
# OverTheWire Bandit Level 27→28 Solution

## Challenge Overview

Clone a git repository and find the password for the next level. The repository is hosted at `ssh://bandit27-git@bandit.labs.overthewire.org:2220/home/bandit27-git/repo`.

---

## Step 1: Understanding the Challenge

**Key information:**

- There's a git repository at a specific SSH URL
- The password for `bandit27-git` is the same as `bandit27`
- We need to **clone** the repository to access its contents

**What is Git?**

- Git is a version control system that tracks changes to files/code over time
- A **repository** (repo) is a project tracked by git
- **Cloning** means copying a repository to your local machine
- GitHub is a website that hosts git repositories, but git itself is just a command-line tool

---

## Step 2: Initial Attempt from Bandit27

First, SSH into bandit27:

```bash
ssh bandit27@bandit.labs.overthewire.org -p 2220
```

Create a temporary working directory:

```bash
mktemp -d
# Output: /tmp/tmp.pOuYrK2fGn
cd /tmp/tmp.pOuYrK2fGn
```

**Attempted clone from within the server:**

```bash
git clone ssh://bandit27-git@localhost:2220/home/bandit27-git/repo
```

**Result:** ❌ Failed with localhost blocking error:

```
!!! You are trying to log into this SSH server with a password on port 2220 from localhost.
!!! Connecting from localhost is blocked to conserve resources.
fatal: Could not read from remote repository.
```

**Also tried with external hostname:**

```bash
git clone ssh://bandit27-git@bandit.labs.overthewire.org:2220/home/bandit27-git/repo
```

**Result:** ❌ Still failed - resolves to localhost when connecting from within the server

---

## Step 3: Clone from Local Machine (Solution)

**The key insight:** Clone the repository from your **local computer**, not from within the bandit server.

### On Your Local Machine:

1. **Create a working directory:**
    
    ```bash
    mkdir ~/bandit27_repo
    cd ~/bandit27_repo
    ```
    
    Or navigate to an existing directory:
    
    ```bash
    cd ~/Desktop/overthewire
    ```
    
2. **Clone the repository:**
    
    ```bash
    git clone ssh://bandit27-git@bandit.labs.overthewire.org:2220/home/bandit27-git/repo
    ```
    
3. **Enter the password when prompted:**
    
    - Use the bandit27 password: `upsNCc7vzaRDx6oZC6GiR6ERwe1MowGB`

**Output:**

```
Cloning into 'repo'...
bandit27-git@bandit.labs.overthewire.org's password: 
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Receiving objects: 100% (3/3), done.
```

✅ **Success!** Repository cloned to `repo/` directory.

---

## Step 4: Explore the Repository

**Check what was cloned:**

```bash
ls
# Output: bandit17_key  bandit26.sshkey  repo
```

**Enter the repository:**

```bash
cd repo
```

**List contents:**

```bash
ls
# Output: README
```

**Read the README file:**

```bash
cat README
```

**Output:**

```
The password to the next level is: Yz9IpL0sBcCeuG7m9uQFt8ZNpS4HZRcN
```

✅ **Password found!**

---

## Key Concepts Learned

### 1. Git Basics

**What is Git?**

- Distributed version control system
- Tracks changes to files over time
- Allows multiple people to collaborate on projects
- Keeps history of all changes (commits)

**Common Git Commands:**

- `git clone <url>` - Copy a repository to your local machine
- `git log` - View commit history
- `git status` - Check status of your repository
- `git diff` - See changes made to files

**Repository Structure:**

- Contains a `.git/` directory with all version history
- Working directory contains the actual files
- Can have branches for different versions

### 2. Git Over SSH

**SSH URL Format:**

```
ssh://[user@]host[:port]/path/to/repo
```

**Example breakdown:**

```
ssh://bandit27-git@bandit.labs.overthewire.org:2220/home/bandit27-git/repo
     └─────┬─────┘ └──────────────┬──────────────┘ └─┬─┘ └────────┬────────┘
       username            hostname                 port      path to repo
```

**Authentication:**

- SSH can use passwords or keys
- In this case, we used password authentication
- The password was the same as bandit27's password

### 3. Localhost vs External Hostname

**The Issue:**

- When connecting from within the bandit server to itself (localhost), connections are blocked
- This applies to both SSH and git over SSH

**Solutions:**

- Connect from an external machine (your local computer)
- This bypasses the localhost restriction

**Why this matters:**

- Network security policies often restrict internal connections
- External connections are monitored differently
- OverTheWire blocks localhost to conserve resources

### 4. Temporary Directories in Linux

**Using mktemp:**

```bash
mktemp -d
# Creates: /tmp/tmp.XXXXXXXXXX
```

**Why use temporary directories?**

- Users often don't have write permissions in home directories
- `/tmp` is world-writable
- Temporary files are automatically cleaned up on reboot
- Good practice for working with downloaded/cloned files

---

## Common Pitfalls

❌ **Trying to clone from within the bandit server**

- Localhost connections are blocked
- Must clone from your local machine

❌ **Wrong git URL format**

- Need `ssh://` at the beginning
- Don't forget the port `:2220`
- Full path to repository required

❌ **Case sensitivity**

- File named `README` not `readme`
- Linux is case-sensitive: `cat README` works, `cat readme` doesn't

❌ **Forgetting the password**

- The password for `bandit27-git` is the same as `bandit27`
- Not the password you're trying to find

---

## Solution Flow

```mermaid
graph TD
    A[Start: Have bandit27 password] --> B[Try to clone from bandit27 server]
    B --> C{Works?}
    C -->|No - localhost blocked| D[Switch to local machine]
    D --> E[mkdir working directory]
    E --> F[git clone ssh://bandit27-git@bandit.labs.overthewire.org:2220/home/bandit27-git/repo]
    F --> G[Enter bandit27 password]
    G --> H[Repository cloned successfully]
    H --> I[cd repo]
    I --> J[ls: see README file]
    J --> K[cat README]
    K --> L[Password: Yz9IpL0sBcCeuG7m9uQFt8ZNpS4HZRcN]
```

---

## Complete Command Reference

```bash
# === Attempt from bandit27 (doesn't work) ===
ssh bandit27@bandit.labs.overthewire.org -p 2220
mktemp -d
cd /tmp/tmp.XXXXXXXXXX
git clone ssh://bandit27-git@localhost:2220/home/bandit27-git/repo
# Result: Fails with localhost blocking

# === Solution: From local machine ===
mkdir ~/bandit27_repo           # Create working directory
cd ~/bandit27_repo              # Navigate to it

# Clone the repository
git clone ssh://bandit27-git@bandit.labs.overthewire.org:2220/home/bandit27-git/repo
# Enter password: upsNCc7vzaRDx6oZC6GiR6ERwe1MowGB

# Explore the repository
cd repo                         # Enter cloned repository
ls                             # List files
cat README                     # Read the file
```

---

## Understanding Git Repositories

**What's inside a git repository?**

```bash
cd repo
ls -la
```

**You'll see:**

- `.git/` - Hidden directory containing all version history
- `README` - The actual files in the repository
- Other files depending on the project

**Useful git commands to explore:**

```bash
git log                # View commit history
git branch             # List branches
git status             # Check repository status
git show               # Show recent changes
```

**For this level, we only needed the README, but git repositories can contain:**

- Multiple files and directories
- Complete history of all changes
- Different branches for different features
- Commit messages explaining changes

---

## Git Clone Options

**Basic clone:**

```bash
git clone <url>
```

**Clone to specific directory:**

```bash
git clone <url> <directory-name>
```

**Clone specific branch:**

```bash
git clone -b <branch-name> <url>
```

**Shallow clone (only recent history):**

```bash
git clone --depth 1 <url>
```

---

## Password

```
Yz9IpL0sBcCeuG7m9uQFt8ZNpS4HZRcN
```

---

## Additional Notes

**Why Git for CTF challenges?**

- Git repositories can hide information in:
    - Commit history (deleted files still in history)
    - Different branches
    - Commit messages
    - File changes over time
- Forces players to learn version control
- Simulates real-world scenarios where secrets are accidentally committed

**Real-world relevance:**

- Developers accidentally commit passwords/secrets to git
- `.git` directories exposed on web servers reveal source code
- Git history can contain sensitive information even after deletion
- Always use `.gitignore` to exclude sensitive files

---

## Tags

#overthewire #bandit #git #version-control #ssh #localhost-blocking #git-clone #repository

---
---
---

# OverTheWire Bandit Level 28→29 Solution

## Challenge Overview

Clone a git repository where the password has been censored in the current version, but exists in git history.

---

## Solution Steps

### Step 1: Clone the Repository

From your local machine:

```bash
git clone ssh://bandit28-git@bandit.labs.overthewire.org:2220/home/bandit28-git/repo
cd repo
```

Password: `Yz9IpL0sBcCeuG7m9uQFt8ZNpS4HZRcN` (bandit28's password)

### Step 2: Check Current README

```bash
cat README.md
```

**Output:**

```markdown
# Bandit Notes
Some notes for level29 of bandit.
## credentials
- username: bandit29
- password: xxxxxxxxxx
```

The password is censored! But git keeps history...

### Step 3: View Commit History

```bash
git log README.md
```

**Output (newest to oldest):**

```
commit b5ed4b5 (HEAD -> master)
    fix info leak           ← Censored the password

commit 8b7c651
    add missing data        ← Added the actual password!

commit 6d8e5e6
    initial commit of README.md
```

### Step 4: View the "add missing data" Commit

```bash
git show 8b7c651
```

**Output:**

```diff
diff --git a/README.md b/README.md
- password: <TBD>
+ password: 4pT1t5DENaYuqnqvadYs1oE4QLCdjmJ7
```

✅ **Password found in git history!**

### Step 5: Verify the Cover-up

```bash
git show b5ed4b5
```

**Output:**

```diff
diff --git a/README.md b/README.md
- password: 4pT1t5DENaYuqnqvadYs1oE4QLCdjmJ7
+ password: xxxxxxxxxx
```

Confirms they tried to hide it, but git never forgets!

---

## Key Concepts

### Git History is Permanent

**The Timeline:**

1. Initial commit: `password: <TBD>`
2. Second commit: Added real password `4pT1t5DENaYuqnqvadYs1oE4QLCdjmJ7`
3. Third commit: "Censored" to `xxxxxxxxxx`

**Critical insight:** Changing a file doesn't erase history. The old version still exists in git!

### Essential Git Commands

|Command|Purpose|Example|
|---|---|---|
|`git log`|View commit history|`git log README.md`|
|`git show <commit>`|View specific commit changes|`git show 8b7c651`|
|`git diff <commit1> <commit2>`|Compare commits|`git diff HEAD~1 HEAD`|
|`git checkout <commit>`|Switch to old version|`git checkout 8b7c651`|

### Git Log Output Explained

```
commit b5ed4b5a3499533c2611217c8780e8ead48609f6
│      └─ Commit hash (unique identifier)
Author: Morla Porla <morla@overthewire.org>
Date:   Tue Oct 14 09:26:24 2025 +0000
    fix info leak  ← Commit message (describes what changed)
```

### Git Show Output Explained

```diff
diff --git a/README.md b/README.md
--- a/README.md    ← "before" version
+++ b/README.md    ← "after" version
- password: <TBD>          ← Lines starting with - were removed
+ password: 4pT1t5...      ← Lines starting with + were added
```

---

## Real-World Security Implications

### Why This Matters

**Common developer mistake:**

1. Accidentally commit API keys, passwords, or secrets
2. Realize the mistake and change/remove them
3. Think it's fixed... **but the secrets are still in git history!**

**Real consequences:**

- Attackers scan public GitHub repositories for exposed secrets
- Even "deleted" credentials can be found in history
- Private repos become public → all history exposed
- Forks and clones retain the full history

### Proper Secret Management

❌ **Wrong:** Commit secret, then remove it

```bash
git commit -m "Add config with API key"
git commit -m "Remove API key"  # Too late! Still in history
```

✅ **Right:** Never commit secrets

```bash
# Use .gitignore to exclude sensitive files
echo "config.json" >> .gitignore
git add .gitignore
```

✅ **If already committed:** Rewrite history

```bash
git filter-branch --force --index-filter \
  'git rm --cached --ignore-unmatch config.json' \
  --prune-empty --tag-name-filter cat -- --all
```

**Then:** Rotate all exposed credentials immediately!

---

## Alternative Solutions

### Method 1: git show (what we used)

```bash
git log                    # Find the commit
git show 8b7c651           # View the changes
```

### Method 2: git checkout

```bash
git checkout 8b7c651       # Go back to that commit
cat README.md              # Read the file as it was
git checkout master        # Return to current version
```

### Method 3: git log with patches

```bash
git log -p README.md       # Show all changes inline
```

### Method 4: git diff

```bash
git diff 8b7c651 b5ed4b5   # Compare the two commits
```

---

## Complete Command Reference

```bash
# Clone repository
git clone ssh://bandit28-git@bandit.labs.overthewire.org:2220/home/bandit28-git/repo
cd repo

# View current state
cat README.md

# Explore history
git log                              # All commits
git log README.md                    # Commits affecting README.md
git log --oneline                    # Compact view
git log -p README.md                 # Show patches inline

# View specific commit
git show 8b7c651                     # Full commit details
git show 8b7c651:README.md           # File at that commit
git show HEAD~1                      # Previous commit

# Compare versions
git diff 8b7c651 b5ed4b5             # Between two commits
git diff HEAD~2 HEAD                 # Last 2 commits vs current

# Time travel
git checkout 8b7c651                 # Go to old commit
cat README.md                        # Read old version
git checkout master                  # Return to present
```

---

## Common Pitfalls

❌ **Thinking deletion = gone forever**

- Files "deleted" in git are still in history
- Use `git log -- <deleted_file>` to find them

❌ **Only checking current files**

- Always check git history for CTF challenges
- Secrets are often in old commits

❌ **Not understanding commit order**

- `git log` shows newest first (top)
- Read from bottom to top for chronological order

---

## Solution Flow

```mermaid
graph LR
    A[Clone repo] --> B[cat README.md]
    B --> C[See: password: xxxxxxxxxx]
    C --> D[git log README.md]
    D --> E[Find: 'add missing data' commit]
    E --> F[git show 8b7c651]
    F --> G[See real password in diff]
    G --> H[Password: 4pT1t5DENaYuqnqvadYs1oE4QLCdjmJ7]
```

---

## Password

```
4pT1t5DENaYuqnqvadYs1oE4QLCdjmJ7
```

---

## Tags

#overthewire #bandit #git #git-history #security #secrets-in-git #commit-history #git-show

----
----
---
# OverTheWire Bandit Level 29→30 Solution

## Challenge Overview

Clone a git repository where the password is hidden in a development branch, not in production (master).

---

## Solution Steps

### Step 1: Clone and Initial Investigation

```bash
git clone ssh://bandit29-git@bandit.labs.overthewire.org:2220/home/bandit29-git/repo
cd repo
cat README.md
```

**Output:**

```markdown
# Bandit Notes
Some notes for bandit30 of bandit.
## credentials
- username: bandit30
- password: <no passwords in production!>
```

🔑 **Key hint:** "no passwords in production!" - suggests password is elsewhere!

### Step 2: Check Git History (Dead End)

```bash
git log README.md
```

**Output:**

```
commit 8ff4dfa (HEAD -> master)
    fix username
commit 09300a1
    initial commit of README.md
```

```bash
git diff 09300a1 8ff4dfa
```

**Result:** Only username changed from bandit29 → bandit30. No password in history.

### Step 3: List All Branches

```bash
git branch -a
```

**Output:**

```
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/dev              ← Development branch!
  remotes/origin/master
  remotes/origin/sploits-dev      ← Another dev branch
```

**Key insight:** Multiple branches exist! Development branches often contain credentials for testing.

### Step 4: Check the Dev Branch

```bash
git show remotes/origin/dev
```

**Output:**

```diff
commit e50e6cc (origin/dev)
    add data needed for development

diff --git a/README.md b/README.md
- password: <no passwords in production!>
+ password: qp30ex3VLz5MDG1n91YowTv4Q8l7CDZL
```

✅ **Password found in dev branch!**

### Step 5: Explore Other Branches (Optional)

```bash
git show remotes/origin/sploits-dev
```

**Output:**

```diff
commit 5149c58 (origin/sploits-dev)
    add some silly exploit, just for shit and giggles

diff --git a/exploits/horde5.md b/exploits/horde5.md
new file mode 100644
+++ b/exploits/horde5.md
@@ -0,0 +1 @@
+
```

Just an empty file. Let's verify by checking out the branch:

```bash
git checkout sploits-dev
cd exploits
ls -la
cat horde5.md      # Empty file
file horde5.md     # Output: very short file (no magic)
```

Nothing useful. Return to master:

```bash
git checkout master
```

---

## Key Concepts

### Git Branches

**What are branches?**

- Parallel versions of your code
- Allow working on features without affecting main code
- Can be merged back together later

**Common branch names:**

- `master`/`main` - Production/stable code
- `dev`/`development` - Active development
- `feature-X` - Specific features
- `staging` - Pre-production testing

**Branch workflow:**

```mermaid
graph LR
    A[master: production] 
    B[dev: development]
    C[feature-login]
    A -.-> B
    B -.-> C
    C -.merge.-> B
    B -.merge.-> A
```

### Essential Branch Commands

|Command|Purpose|
|---|---|
|`git branch`|List local branches|
|`git branch -a`|List all branches (including remote)|
|`git branch -r`|List only remote branches|
|`git checkout <branch>`|Switch to a branch|
|`git checkout -b <name>`|Create and switch to new branch|
|`git show <branch>`|View branch without checking out|

### Remote vs Local Branches

**Remote branches** (on server):

- `origin/master`
- `origin/dev`
- Prefixed with `remotes/origin/`

**Local branches** (on your machine):

- `master`
- Created when you checkout a remote branch

### Production vs Development

**Production environment:**

- Live system used by real users
- Should NOT contain test credentials
- Highest security standards
- Stable, tested code only

**Development environment:**

- For testing and building features
- Often contains test/dummy credentials
- Less strict security (dangerous!)
- Frequently changing code

**The security issue:**

- Developers put real passwords in dev branches for testing
- Forget to remove them before merging
- Or never intend to merge but push to remote anyway
- Attackers can find these credentials in branch history

---

## Alternative Solutions

### Method 1: git show (what we used)

```bash
git branch -a                    # List branches
git show remotes/origin/dev      # View dev branch changes
```

### Method 2: Checkout and read

```bash
git checkout dev                 # Switch to dev branch
cat README.md                    # Read the file
git checkout master              # Go back
```

### Method 3: Show file directly

```bash
git show origin/dev:README.md    # Show file from dev branch
```

### Method 4: Log all branches

```bash
git log --all --oneline --graph  # Visual history of all branches
```

---

## Complete Command Reference

```bash
# Clone repository
git clone ssh://bandit29-git@bandit.labs.overthewire.org:2220/home/bandit29-git/repo
cd repo

# Initial investigation
cat README.md
git log README.md
git log --all                    # Check all branches' history

# List branches
git branch                       # Local branches only
git branch -r                    # Remote branches only
git branch -a                    # All branches
git branch -v                    # With last commit info

# View branch without switching
git show origin/dev              # Show latest commit
git show origin/dev:README.md    # Show specific file
git log origin/dev               # Show branch history

# Switch to branch
git checkout dev                 # Switch to dev branch
git checkout -b dev origin/dev   # Create local branch tracking remote
git checkout master              # Return to master

# Compare branches
git diff master dev              # Difference between branches
git diff master..dev README.md   # Specific file differences

# Visual graph
git log --all --graph --oneline  # See branch structure
```

---

## Real-World Security Implications

### Common Scenarios

**Scenario 1: Test credentials in dev**

```bash
# Developer commits test password
git commit -m "Add test DB credentials"
# Forgets to remove before merging
git merge dev
# Production now has test credentials!
```

**Scenario 2: Private repo goes public**

- Dev branch has real API keys
- Company makes repo public
- All branches (including dev) are exposed
- Attackers scrape GitHub for credentials

**Scenario 3: Old branches forgotten**

- Feature branch created years ago
- Contains old but still-valid credentials
- Nobody remembers it exists
- Attacker finds it through branch enumeration

### Best Practices

✅ **Never commit real credentials**

- Use environment variables
- Use secret management tools (Vault, AWS Secrets Manager)
- Keep `.env` files in `.gitignore`

✅ **Regular branch cleanup**

```bash
git branch -d old-feature        # Delete local branch
git push origin --delete old-dev # Delete remote branch
```

✅ **Branch protection rules**

- Require code review before merging
- Scan for secrets in CI/CD pipeline
- Block force pushes to production branches

✅ **If credentials leaked:**

1. Rotate/revoke immediately
2. Clean git history (complex)
3. Audit what was accessed
4. Consider the repo compromised

---

## Understanding the Output

### git branch -a explained

```
* master                           ← Current branch (*)
  remotes/origin/HEAD -> origin/master  ← Default remote branch
  remotes/origin/dev               ← Remote dev branch
  remotes/origin/master            ← Remote master branch
  remotes/origin/sploits-dev       ← Remote sploits-dev branch
```

### git show output explained

```diff
commit e50e6cc (origin/dev)        ← Commit hash and branch
Author: Morla Porla                ← Who made the change
Date:   Tue Oct 14 09:26:26 2025   ← When

    add data needed for development  ← Commit message

diff --git a/README.md b/README.md  ← Which file changed
- password: <no passwords in production!>  ← Removed line
+ password: qp30ex3VLz5MDG1n91YowTv4Q8l7CDZL  ← Added line
```

---

## Common Pitfalls

❌ **Only checking master branch**

- CTF challenges often hide data in other branches
- Always run `git branch -a` first

❌ **Forgetting remote branches**

- `git branch` only shows local branches
- Must use `-a` or `-r` flag to see remote branches

❌ **Not exploring all branches**

- Check every branch listed
- Password could be in any of them

❌ **Assuming production = only code**

- Production mindset: "no secrets in production"
- But dev branches are pushed to same remote repo!

---

## Solution Flow

```mermaid
graph TD
    A[Clone repo] --> B[cat README.md]
    B --> C[See: no passwords in production!]
    C --> D[git log: no password in history]
    D --> E[git branch -a: find branches]
    E --> F[See: origin/dev, origin/sploits-dev]
    F --> G[git show origin/dev]
    G --> H[Find password in diff]
    H --> I[git show origin/sploits-dev]
    I --> J[Empty exploit file]
    J --> K[Password: qp30ex3VLz5MDG1n91YowTv4Q8l7CDZL]
```

---

## Password

```
qp30ex3VLz5MDG1n91YowTv4Q8l7CDZL
```

---

## Tags

#overthewire #bandit #git #git-branches #remote-branches #development #production #secrets-management

---
---
---
# OverTheWire Bandit Level 30→31 Solution

## Challenge Overview

Clone a git repository with an "empty" file. The password is hidden in a git tag, not in commits or branches.

---

## Solution Steps

### Step 1: Clone and Initial Investigation

```bash
git clone ssh://bandit30-git@bandit.labs.overthewire.org:2220/home/bandit30-git/repo
cd repo
cat README.md
```

**Output:**

```
just an epmty file... muahaha
```

Taunting us! Let's investigate further.

### Step 2: Check Git History (Dead End)

```bash
git log
```

**Output:**

```
commit d604df2 (HEAD -> master, origin/master, origin/HEAD)
    initial commit of README.md
```

Only one commit. Check what it contains:

```bash
git show d604df2
```

**Result:** Just creates the empty/taunting README. Nothing useful.

### Step 3: Check Branches (Dead End)

```bash
git branch -a
```

**Output:**

```
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/master
```

Only master branch exists. Checked all remote branches - all point to the same commit with the empty file.

### Step 4: Check Git Tags (Solution!)

```bash
git tag
```

**Output:**

```
secret
```

A tag called "secret" exists! Let's view it:

```bash
git show secret
```

**Output:**

```
fb5S2xb7bRyFmAvQYQGEqsbhVyJqhnDy
```

✅ **Password found in git tag!**

### Step 5: Understanding Tags (Exploration)

Tags are metadata, not files:

```bash
cat secret
# Error: cat: secret: No such file or directory
```

Tags are stored in `.git/` directory:

```bash
ls .git/refs/tags/
# Output: secret

cat .git/refs/tags/secret
# Shows the raw tag data
```

---

## Key Concepts

### Git Tags

**What are tags?**

- Metadata/bookmarks attached to commits
- Used to mark important points in history
- Separate from commits, branches, and files
- Stored in `.git/refs/tags/`

**Why use tags?**

- Mark release versions (v1.0.0, v2.1.3)
- Create named snapshots
- Reference specific states easily
- Document milestones

### Two Types of Tags

**1. Lightweight Tags**

- Simple pointer to a commit
- Just a name, no extra data
- Created with: `git tag <name>`

**2. Annotated Tags**

- Full git objects with metadata
- Include: message, author, date
- Can store additional information
- Created with: `git tag -a <name> -m "message"`

**This challenge used an annotated tag** to store the password directly.

### Tag Commands

|Command|Purpose|
|---|---|
|`git tag`|List all tags|
|`git tag <name>`|Create lightweight tag|
|`git tag -a <name> -m "msg"`|Create annotated tag|
|`git show <tag>`|View tag contents|
|`git tag -d <name>`|Delete local tag|
|`git push origin <tag>`|Push tag to remote|
|`git push origin --tags`|Push all tags|

### Tags vs Branches vs Commits

|Feature|Commit|Branch|Tag|
|---|---|---|---|
|**Purpose**|Record changes|Development line|Mark milestones|
|**Moves?**|No (fixed)|Yes (moves with commits)|No (fixed)|
|**Contains**|Code changes|Pointer to commit|Pointer + metadata|
|**Common use**|Every change|Feature development|Releases|

---

## Complete Investigation Process

```bash
# Clone repository
git clone ssh://bandit30-git@bandit.labs.overthewire.org:2220/home/bandit30-git/repo
cd repo

# Check obvious places
cat README.md                    # Empty file
git log                          # Single commit
git log --all                    # Check all branches
git branch -a                    # Only master exists

# Check branches
git show remotes/origin/master   # Same empty commit
git show remotes/origin/HEAD     # Points to master

# Check tags (THE ANSWER!)
git tag                          # Lists: secret
git show secret                  # Shows password!

# Understanding tag storage
ls -la .git/refs/tags/           # See tag files
cat .git/refs/tags/secret        # View raw tag
```

---

## Alternative Tag Commands

### Viewing tags

```bash
git tag                          # List all tags
git tag -l "v1.*"               # List tags matching pattern
git show secret                  # Show tag content
git show-ref --tags              # Show all tag references
```

### Creating tags (for reference)

```bash
git tag v1.0                     # Lightweight tag
git tag -a v1.0 -m "Release"    # Annotated tag
git tag -a secret -m "Password" # Tag with message
```

### Tag information

```bash
git describe                     # Show nearest tag
git log --decorate              # Show tags in log
git show-ref --tags             # All tag references
```

---

## Real-World Security Implications

### Why Tags Are Dangerous

**Common misconceptions:**

- "Nobody looks at tags"
- "Tags are just for releases"
- "Old tags are harmless"

**Reality:**

- Attackers enumerate ALL git objects
- Tags often overlooked in security audits
- Can contain accidentally committed secrets

### Real Scenarios

**Scenario 1: Version tagging gone wrong**

```bash
# Developer tags release with config file
git tag -a v1.0 -m "Release 1.0"
git add config.prod  # Contains DB password
git commit -m "Add prod config"
git tag v1.0-final

# Later removes config from files
git rm config.prod
# But the tag still references the commit with the password!
```

**Scenario 2: Hidden notes**

```bash
# Developer uses tags as "notes"
git tag -a notes-2024 -m "API_KEY=sk_live_abc123..."
# Thinks it's private/local
git push --tags  # Accidentally pushes to public repo!
```

**Scenario 3: Build metadata**

```bash
# CI/CD system stores secrets in tags
git tag -a build-1234 -m "DEPLOY_KEY=xyz789"
# Repo made public → secrets exposed
```

### Best Practices

✅ **Audit all git objects**

```bash
# Check everything, not just files
git log --all
git branch -a
git tag
git reflog
```

✅ **Clean up old tags**

```bash
git tag -d old-tag              # Delete locally
git push origin :refs/tags/old-tag  # Delete remotely
```

✅ **Never store secrets in git**

- Not in files
- Not in commits
- Not in branches
- Not in tags
- Not anywhere!

✅ **Scan for secrets**

- Use tools like `trufflehog`, `gitleaks`
- Check all git objects, including tags
- Automate in CI/CD pipeline

---

## Git Objects Overview

Understanding where data can hide in git:

```mermaid
graph TD
    A[Git Repository] --> B[Commits]
    A --> C[Branches]
    A --> D[Tags]
    A --> E[Reflog]
    A --> F[Stashes]
    
    B --> B1[Files/Code]
    B --> B2[Commit Messages]
    B --> B3[Author/Date]
    
    C --> C1[Local Branches]
    C --> C2[Remote Branches]
    
    D --> D1[Lightweight Tags]
    D --> D2[Annotated Tags]
    
    E --> E1[Command History]
    F --> F1[Temporary Changes]
```

**Places to check for secrets:**

1. ✅ Current files
2. ✅ Commit history (`git log`)
3. ✅ All branches (`git branch -a`)
4. ✅ Tags (`git tag`)
5. ✅ Reflog (`git reflog`)
6. ✅ Stashes (`git stash list`)
7. ✅ `.git/` directory contents

---

## Common Pitfalls

❌ **Only checking files and commits**

- Git has many storage mechanisms
- Tags are easy to overlook

❌ **Assuming tags are just version markers**

- Tags can store arbitrary data
- Annotated tags have messages/metadata

❌ **Not listing tags**

- `git log` doesn't show tags by default
- Must explicitly run `git tag`

❌ **Trying to access tags as files**

- Tags aren't files in working directory
- Need `git show <tag>` to view them

---

## CTF Pattern Recognition

**When git challenges show:**

- Empty/taunting files → Look beyond obvious
- Single commit → No history to check
- No extra branches → Not in branches
- **Time to check tags!**

**Git enumeration checklist:**

```bash
git log --all --oneline --graph  # All commits
git branch -a                     # All branches  
git tag                           # All tags ← Often forgotten!
git reflog                        # Command history
git stash list                    # Stashed changes
ls -la .git/                      # Raw git objects
```

---

## Solution Flow

```mermaid
graph TD
    A[Clone repo] --> B[cat README.md]
    B --> C[Empty/taunting message]
    C --> D[git log: only 1 commit]
    D --> E[git show: just empty file]
    E --> F[git branch -a: only master]
    F --> G[Dead ends everywhere...]
    G --> H[git tag: find 'secret' tag!]
    H --> I[git show secret]
    I --> J[Password: fb5S2xb7bRyFmAvQYQGEqsbhVyJqhnDy]
```

---

## Password

```
fb5S2xb7bRyFmAvQYQGEqsbhVyJqhnDy
```

---

## Tags

#overthewire #bandit #git #git-tags #metadata #annotated-tags #git-objects #enumeration

---
---
---
# OverTheWire Bandit Level 31→32 Solution

## Challenge Overview

Push a file to a remote git repository. The challenge tests understanding of git workflow, `.gitignore`, and pushing changes to a remote server.

---

## Solution Steps

### Step 1: Clone and Read Instructions

```bash
git clone ssh://bandit31-git@bandit.labs.overthewire.org:2220/home/bandit31-git/repo
cd repo
cat README.md
```

**Output:**

```
This time your task is to push a file to the remote repository.

Details:
    File name: key.txt
    Content: 'May I come in?'
    Branch: master
```

**Task:** Create and push a file to the remote repository.

### Step 2: Check for Obstacles

```bash
git log
```

**Output shows:**

```diff
+++ b/.gitignore
@@ -0,0 +1 @@
+*.txt
```

⚠️ **Problem:** `.gitignore` is configured to ignore all `.txt` files!

**What is .gitignore?**

- Lists files/patterns git should ignore
- `*.txt` means ignore all files ending in `.txt`
- Prevents accidentally committing unwanted files

### Step 3: Create the Required File

```bash
echo 'May I come in?' > key.txt
cat key.txt
```

**Output:**

```
May I come in?
```

### Step 4: Try to Add the File (Blocked!)

```bash
git add key.txt
```

**Output:**

```
The following paths are ignored by one of your .gitignore files:
key.txt
hint: Use -f if you really want to add them.
```

Git refuses to add the file because of `.gitignore`!

### Step 5: Force Add the File

```bash
git add -f key.txt
```

The `-f` (force) flag overrides `.gitignore`.

**Verify:**

```bash
git status
```

**Output:**

```
On branch master
Your branch is up to date with 'origin/master'.

Changes to be committed:
  new file:   key.txt
```

✅ File is staged!

### Step 6: Configure Git Identity (First Time Only)

```bash
git commit -m "Add key.txt"
```

**Error:**

```
Author identity unknown
*** Please tell me who you are.
```

**Solution:** Configure git user (can be any name/email):

```bash
git config --global user.email "hacker@overthewire.org"
git config --global user.name "CTF Player"
```

**Try commit again:**

```bash
git commit -m "Add key.txt"
```

**Output:**

```
[master a32b6e8] Add key.txt
 1 file changed, 1 insertion(+)
 create mode 100644 key.txt
```

✅ Committed locally!

### Step 7: Push to Remote Repository

```bash
git push
```

**Enter password:** `fb5S2xb7bRyFmAvQYQGEqsbhVyJqhnDy` (bandit31's password)

**Server Response:**

```
remote: ### Attempting to validate files... ####
remote: 
remote: .oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.
remote: 
remote: Well done! Here is the password for the next level:
remote: 3O9RfhqyAlVBEZpVb6LYStshZoqoSx5K 
remote: 
remote: .oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.
remote: 
To ssh://bandit.labs.overthewire.org:2220/home/bandit31-git/repo
 ! [remote rejected] master -> master (pre-receive hook declined)
error: failed to push some refs to...
```

✅ **Password obtained!**

**Note:** The push was "rejected" after validation - this is intentional! The server validated your file, gave you the password, then rejected the actual push to keep the challenge clean for other players.

---

## Key Concepts

### Git Push Workflow

**The complete flow:**

```
1. Make changes (create/edit files)
2. Stage changes: git add <file>
3. Commit locally: git commit -m "message"
4. Push to remote: git push
```

**Visual representation:**

```
Working Directory  →  Staging Area  →  Local Repo  →  Remote Repo
   (files)            (git add)       (git commit)    (git push)
```

### Local vs Remote

|Location|Description|Commands|
|---|---|---|
|**Working Directory**|Your actual files|create, edit, delete|
|**Staging Area**|Changes ready to commit|`git add`|
|**Local Repository**|Commits on your machine|`git commit`|
|**Remote Repository**|Repo on server|`git push`, `git pull`|

### Understanding .gitignore

**Purpose:**

- Tell git which files to ignore
- Prevent committing unwanted files
- Keep repo clean

**Common patterns:**

```
*.txt           # Ignore all .txt files
*.log           # Ignore all log files
node_modules/   # Ignore entire directory
.env            # Ignore specific file
!important.txt  # Exception: don't ignore this .txt
```

**Overriding .gitignore:**

```bash
git add -f file.txt    # Force add ignored file
```

### Git Configuration

**User identity:**

```bash
git config --global user.name "Your Name"
git config --global user.email "you@email.com"
```

**Why needed?**

- Every commit records author information
- Git won't commit without knowing who you are
- Helps track who made what changes

**Scopes:**

- `--global` - Applies to all repos on your machine
- `--local` - Applies only to current repo (omit --global)
- `--system` - Applies to all users on machine

### Git Hooks

**What happened at the end?** The server used a **pre-receive hook** - a script that runs on the server when you push.

**The hook:**

1. Validated your `key.txt` file
2. Checked content matches requirements
3. Returned the password
4. Rejected the push (to keep challenge clean)

**Common hooks:**

- `pre-commit` - Runs before committing
- `pre-push` - Runs before pushing
- `pre-receive` - Runs on server when receiving push
- `post-receive` - Runs after successful push

---

## Complete Command Reference

```bash
# Clone repository
git clone ssh://bandit31-git@bandit.labs.overthewire.org:2220/home/bandit31-git/repo
cd repo

# Read instructions
cat README.md
git log                          # Check for obstacles

# Create file
echo 'May I come in?' > key.txt
cat key.txt                      # Verify content

# Stage file
git add key.txt                  # Fails due to .gitignore
git add -f key.txt              # Force add
git status                       # Verify staged

# Configure git (first time only)
git config --global user.email "you@example.com"
git config --global user.name "Your Name"

# Commit locally
git commit -m "Add key.txt"
git log                          # See your commit

# Push to remote
git push                         # Enter bandit31 password
# Server validates and returns password!
```

---

## Alternative Solutions

### Method 1: Force add (what we used)

```bash
echo 'May I come in?' > key.txt
git add -f key.txt
git commit -m "Add key.txt"
git push
```

### Method 2: Remove .gitignore

```bash
echo 'May I come in?' > key.txt
rm .gitignore
git add key.txt .gitignore
git commit -m "Add key.txt and remove gitignore"
git push
```

### Method 3: Modify .gitignore

```bash
echo 'May I come in?' > key.txt
echo '!key.txt' >> .gitignore   # Add exception
git add key.txt .gitignore
git commit -m "Add key.txt with exception"
git push
```

---

## Common Pitfalls

❌ **Creating file in wrong directory**

```bash
# Wrong: created outside repo
cd ~/Desktop/overthewire
echo 'May I come in?' > key.txt

# Right: created inside repo
cd ~/Desktop/overthewire/repo
echo 'May I come in?' > key.txt
```

❌ **Forgetting to force add**

```bash
git add key.txt     # Fails silently
git status          # File not staged
```

❌ **Not configuring git user**

```bash
git commit -m "message"
# Error: Author identity unknown
```

❌ **Thinking push "failed"**

- Server response says "rejected"
- But password was already provided
- Rejection is intentional to prevent pollution

---

## Understanding Git Push Output

### Successful Push (Normal)

```
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Writing objects: 100% (3/3), 330 bytes, done.
To ssh://server/repo
   abc123..def456  master -> master
```

### This Challenge's Push

```
Writing objects: 100% (3/3), 330 bytes, done.
remote: ### Attempting to validate files... ####  ← Hook running
remote: Well done! Here is the password...        ← Hook output
 ! [remote rejected] master -> master             ← Intentional rejection
error: failed to push some refs                   ← Expected "error"
```

**Why reject?**

- Server validated your work
- Gave you the password
- Rejected push to keep repo clean for next player
- This is a **feature**, not a bug!

---

## Real-World Git Workflow

### Typical Development Flow

```bash
# 1. Clone repo (once)
git clone <url>

# 2. Create feature branch
git checkout -b feature-login

# 3. Make changes
echo "code" > login.js

# 4. Stage and commit
git add login.js
git commit -m "Add login feature"

# 5. Push to remote
git push origin feature-login

# 6. Create Pull Request (on GitHub/GitLab)
# 7. Code review
# 8. Merge to master
```

### Team Collaboration

```bash
# Update your local repo
git pull origin master

# Create feature branch
git checkout -b my-feature

# Work and commit
git add .
git commit -m "Add feature"

# Push your branch
git push origin my-feature

# Others can now see your branch
```

---

## Solution Flow

```mermaid
graph TD
    A[Clone repo] --> B[Read README: push key.txt]
    B --> C[Check git log: see .gitignore]
    C --> D[Create: echo 'May I come in?' > key.txt]
    D --> E[git add key.txt]
    E --> F{Ignored by .gitignore}
    F --> G[git add -f key.txt]
    G --> H[git status: verify staged]
    H --> I[git commit -m 'Add key.txt']
    I --> J{Author identity unknown?}
    J -->|Yes| K[git config user.name/email]
    K --> L[git commit again]
    J -->|No| L
    L --> M[git push]
    M --> N[Enter bandit31 password]
    N --> O[Server validates file]
    O --> P[Password: 3O9RfhqyAlVBEZpVb6LYStshZoqoSx5K]
    P --> Q[Push rejected - intentional!]
```

---

## Password

```
3O9RfhqyAlVBEZpVb6LYStshZoqoSx5K
```

---

## Tags

#overthewire #bandit #git #git-push #gitignore #git-workflow #git-hooks #git-config #remote-repository

---
---
---
# OverTheWire Bandit Level 32→33 Solution

## Challenge Overview

Escape from an "UPPERCASE SHELL" that converts all commands to uppercase, making normal commands fail. Exploit shell variables to break out.

---

## The Challenge

### Initial Login

```bash
ssh bandit32@bandit.labs.overthewire.org -p 2220
```

**Welcome message:**

```
WELCOME TO THE UPPERCASE SHELL
>>
```

### The Problem

```bash
>> ls
sh: 1: LS: Permission denied

>> pwd
sh: 1: PWD: Permission denied
```

**What's happening?**

- Custom shell converts all input to UPPERCASE
- `ls` becomes `LS`, `pwd` becomes `PWD`
- Linux commands are case-sensitive: `LS` doesn't exist!
- Result: trapped in a shell that breaks every command

---

## Solution: Escape with $0

### The Escape

```bash
>> $0
$
```

✅ **You're out!** Now you have a normal shell prompt.

### Why $0 Works

**Shell special variables don't change when uppercased:**

- `$0` → `$0` (numbers and symbols stay the same!)
- `$0` contains the name of the current shell
- Executing `$0` spawns a new shell

**What is $0?**

- Special shell variable
- Contains the name/path of the current shell/script
- When executed, it starts a new instance of that shell

### Verify Your Shell

```bash
$ ls
uppershell

$ ls -la
-rwsr-x---   1 bandit33 bandit32 15140 Oct 14 09:26 uppershell
```

**Critical observation:** `uppershell` is a **setuid binary** owned by `bandit33`!

### Check Your Privileges

```bash
$ id
uid=11033(bandit33) gid=11032(bandit32) groups=11032(bandit32)

$ whoami
bandit33
```

🎯 **Key insight:** You're running as `bandit33` because of the setuid bit!

- Real user: `bandit32` (you logged in as this)
- Effective user: `bandit33` (setuid binary owner)
- This means you have bandit33's permissions!

### Get the Password

```bash
$ cat /etc/bandit_pass/bandit33
tQdtbs5D5i2vJwkO8mEyYEyTL8izoeJ0
```

✅ **Level complete!**

---

## Key Concepts

### Custom/Restricted Shells

**What are restricted shells?**

- Modified shells that limit user actions
- Used for security or CTF challenges
- Can restrict: commands, redirections, variable changes

**Examples:**

- Uppercase shell (this challenge)
- `rbash` (restricted bash)
- Custom shells that filter input

**Common escape techniques:**

1. Shell variables (`$0`, `$SHELL`, `$PATH`)
2. Special characters that don't change
3. Environment variable manipulation
4. Exploiting shell features

### Shell Special Variables

|Variable|Meaning|
|---|---|
|`$0`|Name of shell or script|
|`$1, $2, ...`|Script/function arguments|
|`$$`|Current shell's PID|
|`$?`|Exit status of last command|
|`$!`|PID of last background job|
|`$#`|Number of arguments|
|`$@`|All arguments as separate words|
|`$*`|All arguments as single word|

**Why they work here:**

- Contains numbers and special characters
- Uppercase conversion doesn't affect them
- `$0` → `$0` (unchanged!)

### Understanding Setuid (Revisited)

**The uppershell binary:**

```bash
-rwsr-x---   1 bandit33 bandit32 15140 uppershell
              ↑
           setuid bit
```

**How setuid works:**

1. `bandit32` executes `uppershell`
2. Because of `s` bit, it runs as owner (`bandit33`)
3. When `$0` spawns a new shell, it inherits `bandit33` privileges
4. New shell runs with `euid=bandit33`

**Security implication:**

- The custom shell (uppershell) was supposed to restrict you
- But by escaping it while running with setuid, you keep elevated privileges
- This is a vulnerability in the uppershell design!

---

## How the Uppercase Shell Works

### Behind the Scenes

The uppershell binary likely does something like:

```c
#include <stdio.h>
#include <ctype.h>
#include <string.h>

int main() {
    char input[256];
    char upper[256];
    
    printf("WELCOME TO THE UPPERCASE SHELL\n");
    
    while(1) {
        printf(">> ");
        fgets(input, sizeof(input), stdin);
        
        // Convert to uppercase
        for(int i = 0; input[i]; i++) {
            upper[i] = toupper(input[i]);
        }
        
        // Execute uppercased command
        system(upper);
    }
}
```

**The flaw:**

- Converts everything blindly to uppercase
- Doesn't account for shell variables with special characters
- `$0` passes through unchanged and gets interpreted by the shell

---

## Alternative Escape Methods

### Method 1: $0 (what we used)

```bash
>> $0
$ # Escaped!
```

### Method 2: $SHELL (if set)

```bash
>> $SHELL
$ # Escaped!
```

### Method 3: Using positional parameters

```bash
>> $1 /bin/bash
# If $1 is set to something useful
```

### Method 4: $(command) might work

```bash
>> $(echo /bin/bash)
# Command substitution before uppercase conversion
```

_Note: Depends on how the shell processes input_

---

## Complete Command Reference

```bash
# SSH into bandit32
ssh bandit32@bandit.labs.overthewire.org -p 2220
# Password: 3O9RfhqyAlVBEZpVb6LYStshZoqoSx5K

# Try normal commands (they fail)
>> ls          # Becomes LS - fails
>> pwd         # Becomes PWD - fails

# Escape using $0
>> $0

# Now in normal shell - verify
$ ls -la       # See uppershell binary with setuid
$ whoami       # Shows: bandit33
$ id           # Shows: uid=11033(bandit33)

# Get password
$ cat /etc/bandit_pass/bandit33
```

---

## Understanding the Privilege Escalation

### The Chain of Events

```mermaid
graph TD
    A[SSH as bandit32] --> B[uppershell runs with setuid]
    B --> C[Runs as bandit33 due to setuid bit]
    C --> D[Uppercase shell restricts commands]
    D --> E[Type: $0]
    E --> F[$0 spawns new shell]
    F --> G[New shell inherits bandit33 privileges]
    G --> H[Can read /etc/bandit_pass/bandit33]
```

### Why This Works

1. **Setuid binary runs as owner:**
    
    - `uppershell` owned by `bandit33`
    - Executed by `bandit32`
    - Runs with `euid=bandit33`
2. **$0 spawns child shell:**
    
    - Child inherits parent's permissions
    - Still running as `bandit33`
3. **Escape the restriction:**
    
    - Break out of uppercase shell
    - Keep the elevated privileges

---

## Real-World Security Implications

### Restricted Shell Vulnerabilities

**Common issues:**

1. **Incomplete filtering:** Forgetting special characters
2. **Shell metacharacters:** `$`, `*`, `?`, etc.
3. **Command substitution:** `$(cmd)`, `` `cmd` ``
4. **Environment variables:** `$PATH`, `$HOME`, `$SHELL`

### Real Scenarios

**Scenario 1: SSH forced commands**

```bash
# .ssh/authorized_keys
command="restricted.sh" ssh-rsa AAAA...

# User tries to escape
ssh user@host '$0'  # Might spawn unrestricted shell
```

**Scenario 2: Web shells**

```php
// PHP web shell with "protection"
$cmd = strtoupper($_GET['cmd']);
system($cmd);  // Tries to block commands

// Attack: payload.php?cmd=$0
```

**Scenario 3: Containers with restrictions**

```bash
# Container runs restricted shell
# Attacker finds shell variables that work
docker exec container "$0"
```

### Defense Strategies

✅ **Proper restricted shells:**

- Use well-tested solutions: `rbash`, `lshell`
- Whitelist allowed commands
- Don't rely on character filtering

✅ **Drop privileges properly:**

- Don't combine setuid with restrictive shells
- Use `setuid()` to permanently drop privileges
- Validate execution environment

✅ **Input validation:**

- Validate before execution, not just transform
- Block shell metacharacters completely
- Use allowlists, not denylists

---

## Other Shell Special Variables to Try

If `$0` didn't work, try these:

```bash
$SHELL      # Path to user's shell
$HOME       # User's home directory
$PATH       # Command search path
$PWD        # Current directory
$OLDPWD     # Previous directory
$$          # Current process ID
$PPID       # Parent process ID
```

**In this challenge:**

- Most have letters (become uppercase)
- But `$0`, `$$`, `$?` have numbers/symbols
- These are your escape routes!

---

## Common Pitfalls

❌ **Trying regular commands**

```bash
>> ls    # Becomes LS - fails
>> /bin/ls  # Becomes /BIN/LS - fails
```

❌ **Forgetting about shell variables**

- Don't focus only on commands
- Think about what doesn't change when uppercased

❌ **Not checking privileges after escape**

- Always verify: `whoami` and `id`
- Understand why you have elevated privileges

❌ **Trying to execute uppershell directly**

```bash
$ cd uppershell   # It's a file, not a directory
$ ./uppershell    # Puts you back in uppercase shell!
```

---

## Solution Flow

```mermaid
graph TD
    A[Login: UPPERCASE SHELL] --> B[Try ls, pwd: all fail]
    B --> C[Realize: commands become uppercase]
    C --> D[Think: what doesn't change?]
    D --> E[Try: $0]
    E --> F[Escape to normal shell!]
    F --> G[ls -la: see uppershell setuid binary]
    G --> H[whoami: bandit33!]
    H --> I[Realize: inherited bandit33 privileges]
    I --> J[cat /etc/bandit_pass/bandit33]
    J --> K[Password: tQdtbs5D5i2vJwkO8mEyYEyTL8izoeJ0]
```

---


**Skills learned throughout Bandit:**

- Linux command line
- File permissions and setuid
- SSH and keys
- Git (commits, branches, tags)
- Shell escapes and restricted shells
- Network tools and protocols
- Encoding and encryption basics
- Privilege escalation

---

## Password

```
tQdtbs5D5i2vJwkO8mEyYEyTL8izoeJ0
```

---

## Tags

#overthewire #bandit #shell-escape #restricted-shell #uppercase-shell #shell-variables #setuid #privilege-escalation #final-level
