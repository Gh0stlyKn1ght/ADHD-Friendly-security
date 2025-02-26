### **🔍 Identifying the Operating System of the Target Machine**

To find the **operating system (OS)** running on the target machine **without triggering IDS/IPS alerts**, follow these steps:

---

## **Step 1: Perform a Stealthy OS Detection Scan**

Run a **low-profile OS detection scan** using **Nmap’s OS detection (`-O`)** with **fragmentation (`-f`)**:

```bash
sudo nmap <target-IP> -O -Pn -T2 -f
```

📌 **Why?**  
✅ **`-O`** → Enables OS detection  
✅ **`-Pn`** → Disables ping to avoid firewall blocks  
✅ **`-T2`** → Slows the scan to **reduce IDS/IPS alerts**  
✅ **`-f`** → Fragments packets to **bypass simple firewall rules**

---

## **Step 2: Use Decoys to Hide Your Scan**

If the target has an **aggressive IDS/IPS**, disguise your scan using **decoy IPs**:

```bash
sudo nmap <target-IP> -O -Pn -T2 -D RND:5
```

📌 **Why?**  
✅ Injects **random IP addresses** as decoys  
✅ **Confuses logs**, making detection harder

---

## **Step 3: Check Open Ports for OS Clues**

Run **a version scan (`-sV`)** to gather more info about services & OS:

```bash
sudo nmap <target-IP> -p- -sV -O -Pn -T2
```

📌 **Look for**:  
✅ **Web Server Banners** (Apache on Ubuntu, IIS on Windows, etc.)  
✅ **SSH Version** (OpenSSH often runs on Linux, FreeBSD)  
✅ **SMB Version** (Windows uses SMBv2, v3)

---

## **Step 4: Analyze Results & Submit the OS Name**

✅ If Nmap shows:

- **Linux Kernel 3.x, 4.x** → **Ubuntu/Debian**
- **Windows NT 10.0** → **Windows 10/Server**
- **macOS X 10.x** → **Apple macOS**


