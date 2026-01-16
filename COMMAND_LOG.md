# Command Log

A running list of useful commands and locations discovered during our sessions.

## Home Assistant SSH Access

- **Command:** `ssh -i ~/.ssh/id_ed25519 hassio@<YOUR_HA_IP_ADDRESS>`
- **Purpose:** Use this command to gain remote access to the Home Assistant terminal from a WSL (Windows Subsystem for Linux) terminal or any other Linux-based terminal.
- **Note:** Remember to replace `<YOUR_HA_IP_ADDRESS>` with the actual IP address of your Home Assistant instance.

## GitHub SSH Authentication (Home Assistant)

- **Location:** Home Assistant machine (Raspberry Pi 5)
- **SSH Key Location:** `~/.ssh/id_ed25519` (private key) and `~/.ssh/id_ed25519.pub` (public key)
- **Public Key (already added to GitHub):**
  ```
  ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIDHccsHu5gT5sXiz38Wikgeblw3FDh+HoBgmd7QYpFOo homeassistant-ha-instance
  ```
- **Test Connection:** `ssh -T git@github.com`
- **Expected Response:** `Hi PhantomBJ! You've successfully authenticated, but GitHub does not provide shell access.`

## Repository Locations

### Development Environment (WSL/Linux)
- **Path:** `/root/AIpro`
- **Branch:** `master`
- **Auto-navigation:** `.bashrc` configured to cd to this directory on shell startup

### Home Assistant Environment (Raspberry Pi 5)
- **Path:** `/config/AI_HApro`
- **Branch:** `master`
- **Purpose:** Deployment environment where Node-RED flows are imported

## Common Git Workflow Commands

### On Development Environment (/root/AIpro)
```bash
# Make changes, then commit and push
git add <files>
git commit -m "descriptive message"
git push origin master
```

### On Home Assistant (/config/AI_HApro)
```bash
# Pull latest changes from GitHub
cd /config/AI_HApro
git pull origin master

# Check current status
git status

# View available files
ls -la
```

## Node-RED Environment Export Commands

### Find Node-RED package.json
```bash
find /config -name "package.json" -path "*node-red*" 2>/dev/null
# OR
ls -la ~/.node-red/package.json
# OR
ls -la /config/.node-red/package.json
```

### Copy to Repository
```bash
cp ~/.node-red/package.json /config/AI_HApro/nodered_package.json
# Then commit and push to GitHub
```

## Important SSH Keys

### Development Environment
- **Location:** `~/.ssh/`
- **Purpose:** This directory stores your local user's SSH private and public keys. The private key (`id_ed25519`) is used to authenticate to remote systems like Home Assistant.

### Home Assistant Environment
- **Location:** `~/.ssh/`
- **Public Key:** `ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIDHccsHu5gT5sXiz38Wikgeblw3FDh+HoBgmd7QYpFOo homeassistant-ha-instance`
- **Purpose:** Authenticates with GitHub for git operations (clone, pull, push)
