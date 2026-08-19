# MAM Connection Troubleshooter 🌐

### AI Agent Skill
This is a specialized AI Skill designed for agents such as **Hermes, OpenClaw, and Claude**. It provides a structured diagnostic framework for resolving the common "Client cannot accept incoming connections" error on MyAnonamouse (MAM).

### ⚠️ CRITICAL WARNING: No Direct Interaction
To prevent account bans, **AI agents must NOT interact with the MyAnonamouse website directly.** 
The agent's role is to perform network diagnostics and then walk the user through the manual steps required on the website. Direct AI interaction with the site is a violation of terms and may lead to a permanent ban.

### Why this happens
MAM requires that your torrent client be reachable from the outside world to verify your account. Failure usually occurs in one of three places:
1. **The Client:** Not listening on the specified port.
2. **The Network:** Port is blocked by a Firewall, Router, or VPN.
3. **The Profile:** The port entered in MAM settings doesn't match your actual external port.

### 🚀 AI-Powered Automation
This skill is designed for agents with network tools. If the agent has access to your firewall or router API, it can:
- **Detect** the current open external port.
- **Update** firewall rules to allow incoming traffic.
- **Verify** the connection in real-time.
*Note: All final updates to the MAM website must be performed by the human user.*

### Especially Useful for VPN Users
If you use a VPN (e.g., ProtonVPN), your external port often changes upon reconnection. This skill helps agents track those dynamic shifts and ensure your MAM profile stays aligned.

### How to use this tool
**Installation:**
```bash
git clone https://github.com/curator8888/mam-connection-troubleshooter.git
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
