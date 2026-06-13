# ❤️ Day 10 - Linux Bash Scripts

---

### 💡 Step by step guide

### 🧑‍💻 1: SSH to App Server 3

From the jump host:

```bash
ssh banner@stapp03
```

(Replace `banner` with the actual user for App Server 3 if different.)

---

### 🧑‍💻 2: Install zip package

Check whether `zip` is installed:

```bash
zip -v
```

If not installed:

For CentOS/RHEL:

```bash
sudo yum install zip -y
```

For Rocky/AlmaLinux:

```bash
sudo dnf install zip -y
```

For Ubuntu:

```bash
sudo apt install zip -y
```

**Do NOT put this inside the script.**

---

### 🧑‍💻 3: Create required directories

```bash
mkdir -p /scripts
mkdir -p /archives
```

Verify:

```bash
ls -ld /scripts /archives
```

---

### 🧑‍💻 4: Configure Passwordless SSH to Nautilus Storage Server

Generate SSH keys:

```bash
ssh-keygen -t rsa
```

Press **Enter** for all prompts (leave passphrase empty).

Example:

```text
Enter file in which to save the key (/home/banner/.ssh/id_rsa): [Enter]
Enter passphrase: [Enter]
Enter same passphrase again: [Enter]
```

---

### Copy public key to Storage Server

```bash
ssh-copy-id natasha@ststor01
```

Replace:

* `natasha` → storage server user
* `ststor01` → storage server hostname

Test:

```bash
ssh natasha@ststor01
```

You should log in without entering a password.

Exit:

```bash
exit
```

---

### 🧑‍💻 5: Create the script

```bash
vi /scripts/ecommerce_archive.sh
```

Insert:

```bash
#!/bin/bash

# Create zip archive
zip -r /archives/xfusioncorp_ecommerce.zip /var/www/html/ecommerce

# Copy archive to Nautilus Storage Server
scp /archives/xfusioncorp_ecommerce.zip natasha@ststor01:/archives/
```

Save and exit.

---

### 🧑‍💻 6: Make the script executable

```bash
chmod +x /scripts/ecommerce_archive.sh
```

Verify:

```bash
ls -l /scripts/ecommerce_archive.sh
```

You should see:

```text
-rwxr-xr-x
```

---

### 🧑‍💻 7: Run the script

```bash
/scripts/ecommerce_archive.sh
```

---

### 🧑‍💻 8: Verify local archive

```bash
ls -l /archives/
```

Expected:

```text
xfusioncorp_ecommerce.zip
```

---

### 🧑‍💻 9: Verify archive on Storage Server

```bash
ssh natasha@ststor01
```

Check:

```bash
ls -l /archives/
```

Expected:

```text
xfusioncorp_ecommerce.zip
```

---

### Final script

```bash
#!/bin/bash

zip -r /archives/xfusioncorp_ecommerce.zip /var/www/html/ecommerce

scp /archives/xfusioncorp_ecommerce.zip natasha@ststor01:/archives/
```

### Important

✅ Script location:

```text
/scripts/ecommerce_archive.sh
```

✅ Archive name:

```text
xfusioncorp_ecommerce.zip
```

✅ Local destination:

```text
/archives/
```

✅ Remote destination:

```text
/archives/
```

✅ No `sudo` inside the script.

✅ Passwordless SSH configured.



