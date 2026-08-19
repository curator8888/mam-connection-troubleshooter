---
name: mam-connection-troubleshooter
description: Use when diagnosing MAM "incoming connection" errors.
version: 1.0.0
tags: [mam, torrents, networking, troubleshooting]
---

# MAM Connection Troubleshooter 🌐

This skill helps users resolve the common "Client cannot accept incoming connections" error on MyAnonamouse. This error occurs when the MAM system cannot verify that your BitTorrent client is reachable from the outside world.

## The Logic of the Failure

For MAM to verify your connection, three things must be true:
1. **The Client is Listening:** Your torrent client (Deluge, qBittorrent, etc.) must be actively listening on a specific port.
2. **The Path is Open:** Your router, firewall, or VPN must be forwarding external traffic on that port to your client.
3. **The Profile Matches:** The port you have entered into your MAM Client Settings must match the actual port currently open on your external IP.

## Diagnostic Workflow

When a user reports a connection error, follow these steps in order:

### Step 1: Verify Local Listening
Check if the client is actually listening on the port the user thinks it is.
**Command:**
`netstat -tulpn | grep <port>` or `ss -tulpn | grep <port>`
- **Success:** You see a process (e.g., `deluged`) listening on the port.
- **Failure:** The client isn't listening. The user must check their client's "Network" or "Connection" settings.

### Step 2: Verify External Reachability
Check if the port is open to the internet. Since the agent often has an external IP, it can test this for the user.
**Command:**
`nc -zv <external_ip> <port>`
- **Success:** `Connection to <ip> <port> succeeded!`. The networking layer is correct.
- **Failure:** `Connection refused` or `Timeout`. This indicates a Firewall, Router, or VPN issue.

### Step 3: Verify Profile Alignment
If Step 1 and 2 are successful, the issue is almost certainly a mismatch in the MAM profile.
- **The Check:** Compare the port used in Step 2 with the port listed in the user's **MAM Settings $\rightarrow$ Client Settings**.
- **The Fix:** Update the MAM profile port to match the active external port.

## The "VPN Trap" (Critical Warning)

If the user is using a VPN (especially those with dynamic port forwarding like ProtonVPN), the external port **changes every time the VPN reconnects**.

**The Symptom:** The connection works today, but fails tomorrow despite no changes to the client.
**The Solution:** The user must check their VPN's current forwarded port and update their MAM profile accordingly.

## Diagnostic Matrix

| Local Listening | External Open | Result | Action |
| :--- | :--- | :--- | :--- |
| ❌ | ❌ | **Client Issue** | Check client settings; ensure it's running. |
| ✅ | ❌ | **Network Issue** | Check Port Forwarding / Firewall / VPN. |
| ✅ | ✅ | **Profile Issue** | Update port in MAM Client Settings. |

## Troubleshooting Script (Python)

Save as `scripts/check_mam_port.py`. This script allows the agent to perform a quick external check for a user.

```python
import socket
import sys

def check_port(ip, port):
    try:
        with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
            s.settimeout(5)
            result = s.connect_ex((ip, int(port)))
            if result == 0:
                return True
            return False
    except Exception as e:
        print(f"Error: {e}")
        return False

if __name__ == "__main__":
    if len(sys.argv) < 3:
        print("Usage: python check_mam_port.py <ip> <port>")
        sys.exit(1)
    
    target_ip = sys.argv[1]
    target_port = sys.argv[2]
    
    if check_port(target_ip, target_port):
        print(f"SUCCESS: Port {target_port} is OPEN on {target_ip}")
    else:
        print(f"FAILURE: Port {target_port} is CLOSED on {target_ip}")
```
