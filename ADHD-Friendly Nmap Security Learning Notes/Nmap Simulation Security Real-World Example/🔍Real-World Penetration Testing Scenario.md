### **🔍 Real-World Penetration Testing Scenario Based on the Nmap Scan**

---

## **Scenario:**

A security researcher (you) has been hired to **test the security** of a **company web server** (example.com). The company wants to identify **vulnerabilities in their public-facing services** and determine if they are at risk.

### **🛠 Step 1: Reconnaissance with Nmap**

#### **Nmap Command Used:**

```bash
nmap -sS -p 22,80,443,3306 -A example.com
```

---

### **📜 Nmap Scan Output with Labels**

```bash
$ nmap -sS -p 22,80,443,3306 -A example.com

# ✅ Scan metadata (Nmap version and timestamp)
Starting Nmap 7.94 ( https://nmap.org ) at 2025-02-23 12:00 UTC  

# ✅ Target domain and resolved IP
Nmap scan report for example.com (192.168.1.10)  
Host is up (0.035s latency).  

# ✅ Only showing 4 ports (996 other ports are filtered by firewall)
Not shown: 996 filtered ports  

# ✅ Open ports and detected services
PORT     STATE  SERVICE    VERSION  
22/tcp   open   ssh        OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (protocol 2.0)  
| ssh-hostkey:  
|   2048 74:5b:4e:da:21:b3:6d:cc:2e:59:ea:ff:34:44:32:6d (RSA)  
|   256 ad:6c:6d:e1:45:ba:b1:bb:72:99:4d:bb:5d:19:43:8e (ECDSA)  
|_  256 45:6b:b3:aa:8b:3d:7c:1e:6a:22:99:51:ff:24:32:10 (ED25519)  

80/tcp   open   http       Apache httpd 2.4.41 ((Ubuntu))  
| http-title: Example Website  
|_http-server-header: Apache/2.4.41 (Ubuntu)  

443/tcp  open   https      Apache httpd 2.4.41 ((Ubuntu))  
| ssl-cert: Subject: commonName=example.com  
| Subject Alternative Name: DNS:example.com, DNS:www.example.com  
| Not valid before: 2024-01-01T00:00:00  
|_Not valid after: 2026-01-01T00:00:00  

3306/tcp closed mysql  

# ✅ MAC Address and vendor (can help identify the target’s hardware)
MAC Address: 00:1A:2B:3C:4D:5E (FakeVendor)  

# ✅ Scan completion message
OS detection performed. Please report any incorrect results at https://nmap.org/submit/.  
Nmap done: 1 IP address (1 host up) scanned in 5.67 seconds  
```

---

## **🔎 Security Analysis & Exploitation Strategy**

### **1️⃣ SSH (Port 22) – Possible Weakness**

**Findings:**

- OpenSSH 8.2p1 is **running on Ubuntu**.
- SSH keys are **exposed** in the scan output.

**Potential Exploits:**

- Check for **weak SSH passwords** using a brute-force attack:
    
    ```bash
    hydra -l admin -P rockyou.txt ssh://192.168.1.10
    ```
    
- Test for **OpenSSH vulnerabilities** (e.g., user enumeration or privilege escalation).

**Defense Strategy:** ✅ **Disable password authentication** and only allow **key-based authentication**.  
✅ **Keep OpenSSH updated** to prevent exploits.

---

### **2️⃣ Web Server (Ports 80 & 443) – Potential Exploits**

**Findings:**

- Apache 2.4.41 is **outdated**.
- SSL certificate is valid **until 2026**.
- The site **may be running WordPress** (based on `http-title`).

**Potential Exploits:**

- **Check for Apache vulnerabilities** using a scanner:
    
    ```bash
    nikto -h https://example.com
    ```
    
- **Identify hidden files/folders**:
    
    ```bash
    gobuster dir -u https://example.com -w /usr/share/wordlists/dirb/common.txt
    ```
    
- **Exploit WordPress if detected**:
    
    ```bash
    wpscan --url https://example.com --enumerate vp
    ```
    
- **Man-in-the-middle attack** if SSL misconfiguration is found.

**Defense Strategy:** ✅ **Upgrade Apache to the latest version**.  
✅ **Use security headers** to prevent attacks (CSP, X-XSS-Protection).  
✅ **Disable directory listing**.

---

### **3️⃣ MySQL (Port 3306) – Closed, But...**

**Findings:**

- MySQL is **closed**, which means it's either firewalled or not running.

**Potential Exploits:**

- If open in internal scans, try **default credentials**:
    
    ```bash
    mysql -h 192.168.1.10 -u root -p
    ```
    
- If misconfigured, check for **SQL injections** in web applications.

**Defense Strategy:** ✅ **Ensure MySQL only listens on localhost**.  
✅ **Use strong database passwords** and **disable root remote login**.

---

### **📌 Key Takeaways**

|**Finding**|**Potential Risk**|**Recommended Fix**|
|---|---|---|
|**OpenSSH 8.2p1**|Weak SSH credentials, outdated version|Use **strong SSH keys**, disable password login|
|**Apache 2.4.41**|Outdated, potential vulnerabilities|Update to latest **Apache version**|
|**SSL Cert Validity**|Secure but may have misconfigurations|Ensure **TLS 1.2+ is enforced**|
|**MySQL Closed**|No immediate risk, but should check internally|Restrict **MySQL access to localhost**|

---

## **📜 Conclusion**

This penetration test shows that **example.com has some vulnerabilities**, including **potential SSH weaknesses, outdated Apache, and a WordPress-based site that may be exploitable**.

