### **ADHD-Friendly Notes: Saving Nmap Scan Results** 🚀

## **📌 Why Save Scan Results?**

✅ Helps compare **different scanning methods**  
✅ Useful for **documentation & reporting**  
✅ Can be converted into **readable reports**

---

## **📁 Nmap Output Formats**

Nmap can save results in **three formats**:

|**Format**|**Option**|**File Extension**|**Usage**|
|---|---|---|---|
|**Normal Output**|`-oN`|`.nmap`|Human-readable text file|
|**Grepable Output**|`-oG`|`.gnmap`|Easily parsed with command-line tools like `grep`|
|**XML Output**|`-oX`|`.xml`|Used for detailed reports & HTML conversion|

🛠 **Save all formats at once using:**

```bash
sudo nmap <target-IP> -p- -oA target
```

📌 **This creates:**

- `target.nmap`
- `target.gnmap`
- `target.xml`

---

## **🔍 Viewing Saved Results**

### **1️⃣ Normal Output (Human-Readable)**

```bash
cat target.nmap
```

📌 **Example Output:**

```
PORT   STATE SERVICE
22/tcp open  ssh
25/tcp open  smtp
80/tcp open  http
```

### **2️⃣ Grepable Output (For Scripts)**

```bash
cat target.gnmap
```

📌 **Example Output:**

```
Host: 10.129.2.28 () Ports: 22/open/tcp//ssh///, 25/open/tcp//smtp///, 80/open/tcp//http///
```

### **3️⃣ XML Output (For Reports & Automation)**

```bash
cat target.xml
```

📌 **Example XML Snippet:**

```xml
<port protocol="tcp" portid="22"><state state="open" reason="syn-ack"/>
<service name="ssh"/>
</port>
```

---

## **📊 Convert XML to HTML Report**

To create a **visual HTML report**:

```bash
xsltproc target.xml -o target.html
```

✅ Open **`target.html`** in a browser for **a clean, structured report**.

---

## **🎯 Key Takeaways**

✅ Always **save scan results** for documentation & comparison  
✅ Use `-oA` to **save all formats at once**  
✅ Convert XML to **HTML for easy reporting**  
✅ Grepable output (`.gnmap`) is useful for **automation & filtering**

🚀 **This makes scans more organized & shareable!** Let me know if you need further clarification. 😊🔥

### **Steps to Perform a Full TCP Port Scan and Create an HTML Report** 🚀

#### **1️⃣ Run a Full TCP Port Scan (-p-)**

Use the following **Nmap command** to scan all **65535 TCP ports**:

```bash
sudo nmap -p- -sS <target-IP> -oA full_scan
```

📌 **Explanation:**

- `-p-` → Scans **all TCP ports (1-65535)**
- `-sS` → **SYN scan** (stealthy & fast)
- `-oA full_scan` → Saves results in **all formats** (`full_scan.nmap`, `.gnmap`, `.xml`)

---

#### **2️⃣ Convert Results into an HTML Report**

Once the scan completes, **convert the XML output to HTML**:

```bash
xsltproc full_scan.xml -o full_scan.html
```

📌 **Explanation:**

- `xsltproc` → Converts XML to **HTML**
- `full_scan.xml` → The **saved Nmap scan results**
- `-o full_scan.html` → Output file is `full_scan.html`

✅ **Open `full_scan.html` in a browser** to view the formatted report.
