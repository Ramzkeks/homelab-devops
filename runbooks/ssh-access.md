# SSH Access (Homelab)

## Target

| Field    | Value                        |
|----------|------------------------------|
| Hostname | `homelab-debian`             |
| LAN IP   | `192.168.x.x`                |
| User     | `autumn_falls`               |

---

## Prerequisites

- OpenSSH server installed and running on the VM
- VirtualBox network adapter set to **Bridged** mode
- SSH client available on the host machine (Windows: built-in OpenSSH or PuTTY)

---

## Key Generation (Windows)

Generate an ED25519 key pair on your host machine:

```powershell
ssh-keygen -t ed25519 -C "homelab"
```

Default key location:
- Private key: `C:\Users\<USER>\.ssh\id_ed25519`
- Public key: `C:\Users\<USER>\.ssh\id_ed25519.pub`

---

## Install Public Key on VM

### Option A — Manual (recommended)

1. Print your public key on Windows:

```powershell
type $env:USERPROFILE\.ssh\id_ed25519.pub
```

2. On the VM, open `authorized_keys`:

```bash
mkdir -p ~/.ssh
nano ~/.ssh/authorized_keys
```

3. Paste the public key line (`ssh-ed25519 AAAA...`), save and exit.

4. Set correct permissions:

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

### Option B — One-liner

```powershell
type $env:USERPROFILE\.ssh\id_ed25519.pub | ssh autumn_falls@192.168.x.x "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys && chmod 700 ~/.ssh && chmod 600 ~/.ssh/authorized_keys"
```

---

## Verify Access

```powershell
ssh autumn_falls@192.168.x.x
```

With explicit key:

```powershell
ssh -i $env:USERPROFILE\.ssh\id_ed25519 autumn_falls@192.168.x.x
```

Expected result: login without password prompt.

---

## Get Current VM IP

If IP changed (DHCP), check it inside the VM via VirtualBox console:

```bash
ip a
```

Look for the `192.168.x.x` address on the bridged interface (usually `enp0s3` or `eth0`).