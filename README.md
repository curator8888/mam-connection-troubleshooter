# MAM Connection Troubleshooter 🌐

### Overview
This repository provides a structured diagnostic framework for resolving the common **"Client cannot accept incoming connections"** error on MyAnonamouse (MAM). 

### Why this happens
MAM requires that your BitTorrent client be reachable from the outside world to verify your account. Failure usually occurs in one of three places:
1. **The Client:** Not listening on the specified port.
2. **The Network:** Port is blocked by a Firewall, Router, or VPN.
3. **The Profile:** The port entered in MAM settings doesn't match your actual external port.

### How to use this tool
The included diagnostic script allows you to test external port reachability instantly.

**Installation:**
```bash
git clone https://github.com/davidajtyler/mam-connection-troubleshooter.git
cd mam-connection-troubleshooter
```

**Running the Check:**
```bash
python3 scripts/check_mam_port.py <your_external_ip> <your_port>
```

### Diagnostic Matrix
| Local Listening | External Open | Result | Action |
| :--- | :--- | :--- | :--- |
| ❌ | ❌ | **Client Issue** | Check client settings; ensure it's running. |
| ✅ | ❌ | **Network Issue** | Check Port Forwarding / Firewall / VPN. |
| ✅ | ✅ | **Profile Issue** | Update the port in your MAM Client Settings. |
