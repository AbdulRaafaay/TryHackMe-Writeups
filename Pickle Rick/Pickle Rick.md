# 🥒 Pickle Rick — TryHackMe CTF Writeup

**Platform:** TryHackMe
**Difficulty:** Easy
**Tags:** Web Exploitation · Linux Privilege Escalation · Command Injection
**Author of this writeup:** Abdul Rafay

---

## 🧠 Challenge Description

> This Rick and Morty-themed challenge requires you to exploit a web server and find **three ingredients** to help Rick make his potion and transform himself back into a human from a pickle.
>
> Your mission: Get inside the server, look around, and recover all three hidden ingredients.

---

## 🚩 Task Name

**Pickle Rick**

---

## 🌐 Recon

### 🔍 View Page Source

* Found a hardcoded username in the HTML source:

  ```
  Username: --------
  ```

### 🧪 Gobuster

Command used:

```bash
gobuster dir -u http://10.10.89.30/ -w /usr/share/wordlists/SecLists/Discovery/Web-Content/directory-list-2.3-medium.txt
```

Discovered paths:

* `/robots.txt` → Contains string -----------
* `/login.php` → Login panel available

---

## 🔐 Gaining Access

* Used the discovered credentials:

  ```
  Username: ---------
  Password: ---------
  ```
* Successfully logged into the web application.

---

## 🖥️ Command Panel Access

After login, a command panel was accessible where Linux commands could be executed.

### Directory contents after running `ls`:

```
--------
--------
--------
denied.php
index.html
login.php
portal.php
robots.txt
```

---

## 🧪 Ingredient #1

* `cat` command was disabled, so used `less`:

  ```bash
  less Sup----------
  ```
* ✅ Found **first ingredient**

---

## 🔐 Privilege Escalation

* Ran `sudo -l` and found the user can run **all commands** with `sudo`.

### 🧪 Ingredient #2

* Listed Rick’s home directory:

  ```bash
  ls /home/rick
  ```
* Found a file named ------------
* Viewed it with:

  ```bash
  less /home/rick/---------
  ```
* ✅ Found **second ingredient**

### 🧪 Ingredient #3

* Accessed root’s directory with sudo:

  ```bash
  sudo ls /root
  ```
* Found -------
* Viewed it with:

  ```bash
  sudo less /root/------
  ```
* ✅ Found **third ingredient**

---

## 🧪 Summary of Ingredients

* ✅ Ingredient #1: `Sup-------`
* ✅ Ingredient #2: `/home/rick/------`
* ✅ Ingredient #3: `/root/------`

---

## 🧠 Lessons Learned

* Always check HTML source for hardcoded creds.
* Gobuster is useful for brute-forcing directories.
* Alternative commands like `less`, `more`, or `head` can bypass `cat` restrictions.
* Always run `sudo -l` to check privilege escalation possibilities.

---