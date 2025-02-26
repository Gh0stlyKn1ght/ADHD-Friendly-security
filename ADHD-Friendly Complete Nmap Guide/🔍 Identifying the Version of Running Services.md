
### **🔍 Identifying the Version of Running Services**

To determine **which services are running and their versions**, follow these steps:

---

## **Step 1: Perform a Version Detection Scan (`-sV`)**

Run an **Nmap service version scan** to gather information:

```bash
sudo nmap <target-IP> -p- -sV -Pn -T2
```

📌 **Why?**  
✅ **`-p-`** → Scans **all ports (1-65535)**  
✅ **`-sV`** → Enables **service version detection**  
✅ **`-Pn`** → Disables ping to avoid **firewall blocks**  
✅ **`-T2`** → Slows the scan to **reduce IDS/IPS alerts**

---

## **Step 2: Check for a Specific Service Version**

🔥 If the **full scan takes too long**, try scanning **common service ports**:

```bash
sudo nmap <target-IP> -p 22,25,53,80,443,3306 -sV -Pn
```

📌 **Look for:**

- **Web servers** (`Apache`, `Nginx`, `IIS`)
- **SSH version** (OpenSSH, Dropbear)
- **Database servers** (`MySQL`, `PostgreSQL`)
- **FTP services** (vsftpd, ProFTPd)
- **Mail servers** (`Postfix`, `Exim`)

---

## **Step 3: Use Nmap Scripting Engine (NSE)**

🔥 Some services **hide** their versions. Use NSE to dig deeper:

```bash
sudo nmap <target-IP> -p <specific-port> --script=banner -Pn
```

📌 Example for **port 80 (HTTP)**:

```bash
sudo nmap <target-IP> -p 80 --script=http-server-header -Pn
```

✅ **Finds web server version from HTTP response headers**


