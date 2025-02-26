### **ADHD-Friendly Notes: Host Discovery with Nmap** 🚀

## **🔍 Why is Host Discovery Important?**

Before starting an **internal penetration test**, you need to:  
✔️ Identify **which systems are online**  
✔️ Determine **what you can work with**  
✔️ Use different **Nmap host discovery techniques**

📌 **ICMP echo requests** (ping scans) are the most effective way to check if a target is alive.

---

## **📁 Always Save Scan Results!**

✅ Useful for **comparison, documentation, and reporting**  
✅ Different tools can produce **different results**  
✅ Helps identify **patterns and anomalies**

---

## **🌎 Scanning a Network Range**

To scan an entire network (without port scanning):

```bash
sudo nmap 10.129.2.0/24 -sn -oA tnet | grep for | cut -d" " -f5
```

🛠 **Explanation:**

- `10.129.2.0/24` → Network range
- `-sn` → Disables port scanning, only checks if hosts are alive
- `-oA tnet` → Saves results in all formats (tnet.xml, tnet.gnmap, tnet.nmap)
- `grep for | cut -d" " -f5` → Filters out active IPs

⚠ **Firewalls can block ICMP pings**, so some hosts may not appear!

---

## **📜 Scanning a Predefined IP List**

If you have a **list of specific IPs** to check:

```bash
sudo nmap -sn -oA tnet -iL hosts.lst | grep for | cut -d" " -f5
```

📌 **Example `hosts.lst` file:**

```
10.129.2.4
10.129.2.10
10.129.2.11
10.129.2.18
10.129.2.19
10.129.2.20
10.129.2.28
```

🛠 **Explanation:**

- `-iL hosts.lst` → Reads target IPs from a file
- Only **3 out of 7** hosts were detected as **alive** (firewalls may block pings)

---

## **🛠 Scanning Multiple IPs**

For scanning **specific IPs manually**:

```bash
sudo nmap -sn -oA tnet 10.129.2.18 10.129.2.19 10.129.2.20
```

📌 **Shortcut for sequential IPs:**

```bash
sudo nmap -sn -oA tnet 10.129.2.18-20
```

💡 **Use this when IPs are in sequence!**

---

## **🔍 Scanning a Single IP**

To check if a single host is alive:

```bash
sudo nmap 10.129.2.18 -sn -oA host
```

📌 **Example Output:**

```
Host is up (0.087s latency).
MAC Address: DE:AD:00:00:BE:EF
```

🛠 **Explanation:**

- `10.129.2.18` → Target IP
- `-sn` → No port scan, just checks if host is alive
- `-oA host` → Saves results

---

## **💡 ICMP Echo Requests & ARP Pings**

📌 By default, **Nmap first sends an ARP request** to check if a host is alive.

To **force ICMP Echo Requests** instead:

```bash
sudo nmap 10.129.2.18 -sn -oA host -PE --packet-trace
```

🛠 **Explanation:**

- `-PE` → Sends **ICMP Echo Requests**
- `--packet-trace` → Shows packets sent/received

📌 **Example Output (ARP Reply Identified Host as Alive):**

```
SENT ARP who-has 10.129.2.18 tell 10.10.14.2
RCVD ARP reply 10.129.2.18 is-at DE:AD:00:00:BE:EF
```

⚠ **ARP replies alone can confirm if a host is alive!**

---

## **🔎 Finding Why a Host is "Alive"**

To check **why** Nmap detected a system as alive:

```bash
sudo nmap 10.129.2.18 -sn -oA host -PE --reason
```

📌 **Example Output:**

```
Host is up, received arp-response (0.028s latency).
```

🛠 **Explanation:**

- `--reason` → Shows why a host was marked **alive**

---

## **🚀 Disabling ARP Pings**

To **disable ARP pings** and **force ICMP Echo Requests**:

```bash
sudo nmap 10.129.2.18 -sn -oA host -PE --packet-trace --disable-arp-ping
```

📌 **Example Output:**

```
SENT ICMP Echo request
RCVD ICMP Echo reply
Host is up (0.086s latency).
```

🛠 **Explanation:**

- `--disable-arp-ping` → Stops ARP-based detection

---

## **🎯 Key Takeaways**

✅ **Host Discovery is crucial** before scanning ports  
✅ **ICMP Echo Requests** are **effective but can be blocked** by firewalls  
✅ **ARP pings** are used by default, but can be disabled  
✅ **Always save scan results** for comparison & documentation  
✅ **Use lists (-iL) for large-scale scans**

📌 **More Host Discovery Strategies:**  
🔗 [Nmap Host Discovery Guide](https://nmap.org/book/host-discovery-strategies.html)

## **📝  What is the Operating System?**

💡 **Based on the last scan result, the OS is most likely:**  
**Windows** 🖥️ (TTL = 128 is a strong indicator of Windows OS)

🚀 **Let me know if you need any modifications!** 😊

To find all **TCP ports** on the target and enumerate the **hostname**, follow these steps:

### **Step 1: Scan All TCP Ports**

Run the following Nmap command to scan **all** 65535 TCP ports:

```bash
sudo nmap -p- -sS <target-IP> -Pn
```

📌 **Explanation:**

- `-p-` → Scans **all TCP ports (1-65535)**
- `-sS` → Uses **SYN scan** for speed & stealth
- `-Pn` → **Disables ICMP ping** (assumes host is up)

✅ **Submit the total number of open TCP ports** from the scan.

---

### **Step 2: Find the Hostname**

Run the following Nmap command to **enumerate the hostname** of the target:

```bash
sudo nmap -sL <target-IP>
```

OR  
Use service scanning (`-sV`) with host discovery:

```bash
sudo nmap -sV --script=dns-nsid,dns-service-discovery <target-IP>
```

📌 **Explanation:**

- `-sL` → **Lists hostnames** without scanning
- `-sV` → **Service detection**
- `--script=dns-nsid,dns-service-discovery` → Uses **DNS-based** enumeration

