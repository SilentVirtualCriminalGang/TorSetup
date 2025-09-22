# Tor Setup and Basic Testing

![Follow Now!](kazi.png)


## Overview
This README explains a minimal set of commands used to install and start the Tor service on a Debian/Ubuntu-based system, how to install `pip` for Python tools, and how to test whether Tor is working correctly. It also explains a couple of commonly used checks (Tor Project check and DNS leak test) and shows how to upload the README to GitHub.

> **Note:** There appears to be a small typo in the original commands: `128.0.0.1` should be `127.0.0.1` (the localhost IPv4 address). The examples below use `127.0.0.1`.

---

## Prerequisites
- A Debian/Ubuntu-based Linux system (Ubuntu, Debian, Kali, etc.).
- `sudo` privileges (able to run commands as root).
- Internet access to download packages.

---

## Commands and what they do

### 1. Install Tor
```bash
sudo apt update
sudo apt install tor
```
- `sudo apt update` updates the package index.
- `sudo apt install tor` installs the Tor package from your distribution's repositories.

### 2. Start Tor service
```bash
sudo systemctl start tor
```
- Starts the Tor background service (`systemd` unit named `tor`).
- To enable Tor at boot, run: `sudo systemctl enable tor`.
- To check status: `sudo systemctl status tor`.

### 3. Install Python pip
```bash
sudo apt install python3-pip
```
- Installs `pip` for Python 3 so you can install Python tools that might use Tor (for example, `requests[socks]`, `stem`, etc.).

### 4. SOCKS proxy address
The Tor client runs a SOCKS proxy you can use to route application traffic through Tor.
- Default local SOCKS5 proxy address: `127.0.0.1:9050` (not `128.0.0.1`).
- Configure applications to use SOCKS5 at that address (for example, a browser or a Python HTTP client).

### 5. Example: `tornet` command
```bash
sudo tornet --interval 1 --count 0
```
- The original line `Sudo tornet --interval 1 --count 0` looks like it calls a program named `tornet` with some options.
- **Important:** `tornet` is not a standard package installed by `apt` and may not exist on your system. If `tornet` is a custom script or third-party tool, install it first or provide the package name.
- If you meant a tool that repeatedly tests Tor connectivity or rotates circuits, clarify which tool you installed. If you don't have such a tool, you can use `curl` through Tor (example below).

---

## Quick testing examples

### 1. Test HTTP through Tor using `curl`
Install `curl` if you don't have it:
```bash
sudo apt install curl
```
Test via Tor's SOCKS5 proxy:
```bash
curl --socks5 127.0.0.1:9050 https://check.torproject.org/
```
If Tor is working, the response HTML from `check.torproject.org` will indicate whether your connection is coming from a Tor exit node.

### 2. Visit web checks in a browser
- Open a browser configured to use SOCKS5 `127.0.0.1:9050` (or use Tor Browser instead).
- Visit: `https://check.torproject.org/` — shows whether you are using Tor.
- Visit: `https://www.dnsleaktest.com/` or `https://dnsleaktest.com/` — to check DNS leaks (choose the standard or extended test).

**Browser note:** If you configure a regular browser to use the Tor SOCKS proxy, be careful: some browsers leak DNS by default. Tor Browser is preconfigured to avoid common leaks and is recommended for casual use.

---

## Common systemctl commands
- Start: `sudo systemctl start tor`
- Stop: `sudo systemctl stop tor`
- Enable at boot: `sudo systemctl enable tor`
- Disable at boot: `sudo systemctl disable tor`
- Check status: `sudo systemctl status tor`
- View logs: `journalctl -u tor --no-pager -e`

---

## Example: Configure a Python script to use Tor (requests + socks)
```bash
python3 -m pip install requests[socks]
```
Sample Python snippet:
```python
import requests
proxies = {
  'http': 'socks5h://127.0.0.1:9050',
  'https': 'socks5h://127.0.0.1:9050'
}
print(requests.get('https://check.torproject.org/', proxies=proxies, timeout=30).text)
```
- `socks5h://` forces DNS resolution over the SOCKS proxy (avoids DNS leaks).

---

## Security & privacy notes
- Tor provides anonymity at the network level but does not make all applications anonymous by default.
- Use Tor Browser for the best out-of-the-box protection.
- Avoid logging into personal accounts or sending identifying information while using Tor if you need anonymity.
- Make sure any additional tools you use fully route DNS and other traffic through the SOCKS proxy.

---

## Fixing the likely typos from original commands
- Replace `128.0.0.1` with `127.0.0.1` (local loopback IP).
- `Sudo tornet` -> likely `sudo tornet` (lowercase `sudo`) and confirm that `tornet` exists or provide instructions to install it.

---

## Uploading this README to GitHub (quick steps)
1. Initialize a git repo in the folder with this README:
```bash
git init
git add README.md
git commit -m "Add Tor setup README"
```
2. Create a repo on GitHub (via web UI) and copy the remote URL.
3. Add remote and push:
```bash
git remote add origin git@github.com:yourusername/yourrepo.git
git branch -M main
git push -u origin main
```

---

## Author
Prepared by: Kazi Ashrafuzzaman

---



---

## Practical Video Tutorial
For a hands-on demonstration, watch this practical video:  
[Instagram Reel](https://www.instagram.com/reel/DO6QV0eER3u/?igsh=NnN2aTd3bGcyaHA3)

---

## Credit
Credit: **Kazi Ashrafuzzaman**


## License
You may add any license you prefer (MIT recommended for simple README files). Example:
```
MIT License
```
