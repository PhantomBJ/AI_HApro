# Complete Beginner's Guide to Git and Node-RED Workflow

This guide will walk you through everything you need to set up Git on your Home Assistant machine and import Node-RED workflows from GitHub. No prior experience required beyond basic Home Assistant usage and knowing how to run terminal commands.

---

## Table of Contents

1. [What We're Going to Do](#what-were-going-to-do)
2. [Part 1: Prerequisites Setup](#part-1-prerequisites-setup)
3. [Part 2: Importing a Node-RED Workflow](#part-2-importing-a-node-red-workflow)
4. [Common Problems and Solutions](#common-problems-and-solutions)
5. [Quick Reference Commands](#quick-reference-commands)

---

## What We're Going to Do

Think of GitHub as a cloud storage service specifically designed for code and configuration files. In our case, we're using it to share Node-RED workflows (called "flows") between different systems.

Here's the big picture:
1. We'll install Git (the tool that talks to GitHub) on your Home Assistant machine
2. We'll download (clone) a copy of the AI_HApro repository to your machine
3. Whenever there's a new workflow, you'll download the latest version and import it into Node-RED

Think of it like this: GitHub is the recipe book in the cloud, Git is the tool you use to get recipes from that book, and Node-RED is your kitchen where you actually use those recipes.

---

## Part 1: Prerequisites Setup

### Step 1.1: Access Your Home Assistant Terminal

First, you need to get to a command line on the machine running Home Assistant.

**If you're running Home Assistant OS:**
1. Open your Home Assistant web interface
2. Click on your profile icon (bottom left)
3. Enable "Advanced Mode" if it's not already on
4. Go to Settings → Add-ons
5. Search for and install "Terminal & SSH" or "Advanced SSH & Web Terminal"
6. Start the add-on and click "Open Web UI"

**If you're running Home Assistant in Docker or on bare metal:**
- SSH into your Raspberry Pi using your preferred SSH client (like PuTTY on Windows or the Terminal app on Mac/Linux)
- Log in with your username and password

**What you should see:**
A command prompt that looks something like this:
```
[core-ssh ~]$
```
or
```
pi@raspberrypi:~$
```

The exact look varies, but you'll know you're in when you see a prompt with a blinking cursor.

---

### Step 1.2: Check if Git is Already Installed

Before installing Git, let's see if it's already there.

**Command to run:**
```bash
git --version
```

**Expected output if Git is installed:**
```
git version 2.30.2
```
(Your version number might be different, and that's fine)

**Expected output if Git is NOT installed:**
```
-bash: git: command not found
```
or
```
sh: git: not found
```

**What to do next:**
- If Git is installed, skip to Step 1.4
- If Git is NOT installed, continue to Step 1.3

---

### Step 1.3: Installing Git

The installation command depends on your system. Most Home Assistant setups use a Debian-based Linux.

**For Home Assistant OS (using Terminal & SSH add-on):**

Unfortunately, Home Assistant OS has a read-only root filesystem, so you can't permanently install packages in the traditional way. Instead, we have two options:

**Option A: Install in the add-on container (Recommended for beginners)**

Home Assistant OS add-ons come with Git pre-installed in many cases. Try running this:

```bash
apk add git
```

**Expected output:**
```
OK: 123 MiB in 45 packages
```

**Option B: Use Home Assistant Supervised or Container installation**

If you're running HA Supervised on Raspberry Pi OS or in a Docker container, you'll need to install Git on the host system.

For Raspberry Pi OS (Debian/Ubuntu based):
```bash
sudo apt update
sudo apt install git -y
```

**Expected output:**
```
Reading package lists... Done
Building dependency tree... Done
...
Setting up git (1:2.30.2-1) ...
```

**For other systems:**
- CentOS/RHEL: `sudo yum install git -y`
- Fedora: `sudo dnf install git -y`
- Alpine: `apk add git`

**Verify the installation:**
```bash
git --version
```

You should now see a version number.

---

### Step 1.4: Configure Git (First-Time Setup)

Git needs to know who you are for tracking changes. This is a one-time setup.

**Commands to run:**
```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

**Replace:**
- "Your Name" with your actual name (keep the quotes)
- "your.email@example.com" with your email address (keep the quotes)

**Example:**
```bash
git config --global user.name "John Smith"
git config --global user.email "john.smith@gmail.com"
```

**Expected output:**
(No output means success in this case)

**Verify your configuration:**
```bash
git config --list | grep user
```

**Expected output:**
```
user.name=Your Name
user.email=your.email@example.com
```

---

### Step 1.5: Choose Where to Store the Repository

You need to decide where on your system to keep the AI_HApro files. This is important because you'll need to remember this location.

**Recommended locations based on your setup:**

**For Home Assistant OS (Terminal & SSH add-on):**
```bash
/config/ai_hapro
```
This stores it in your Home Assistant config folder, which persists across updates.

**For Raspberry Pi OS or other systems:**
```bash
/home/pi/ai_hapro
```
or
```bash
~/ai_hapro
```
(The `~` symbol means "my home directory")

**Why this matters:**
The `/config` directory in Home Assistant OS is persistent and backed up. Other directories might not survive updates or restarts.

**Let's navigate there:**

For Home Assistant OS:
```bash
cd /config
```

For Raspberry Pi OS:
```bash
cd ~
```

**Expected output:**
(No output, but your prompt might change to show the new directory)

**Verify where you are:**
```bash
pwd
```

**Expected output:**
```
/config
```
or
```
/home/pi
```

---

### Step 1.6: Clone the Repository

Now we'll download the AI_HApro repository from GitHub to your machine.

**The command:**
```bash
git clone git@github.com:PhantomBJ/AI_HApro.git ai_hapro
```

**What this command does:**
- `git clone` - Downloads a repository
- `git@github.com:PhantomBJ/AI_HApro.git` - The repository address (using SSH)
- `ai_hapro` - The folder name to create locally (lowercase, easier to type)

**Wait, SSH authentication!**

If this is your first time, Git will ask you to authenticate. You might see:

**Scenario A: SSH Key Prompt**
```
The authenticity of host 'github.com (140.82.121.4)' can't be established.
RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```

**What to do:** Type `yes` and press Enter

**Scenario B: Permission Denied**
```
git@github.com: Permission denied (publickey).
fatal: Could not read from remote repository.
```

**What this means:** You need to set up SSH keys for GitHub. This is a separate process that involves:
1. Generating an SSH key pair on your machine
2. Adding the public key to your GitHub account

Since this is a common issue, see the "SSH Key Setup" section in [Common Problems](#common-problems-and-solutions) below.

**Scenario C: Success!**
```
Cloning into 'ai_hapro'...
remote: Enumerating objects: 42, done.
remote: Counting objects: 100% (42/42), done.
remote: Compressing objects: 100% (28/28), done.
remote: Total 42 (delta 10), reused 38 (delta 8), pack-reused 0
Receiving objects: 100% (42/42), 12.34 KiB | 1.23 MiB/s, done.
Resolving deltas: 100% (10/10), done.
```

**What success looks like:** You'll see progress messages about receiving objects.

---

### Step 1.7: Verify the Clone Was Successful

Let's make sure everything downloaded correctly.

**List the contents of your current directory:**
```bash
ls -la
```

**You should see a folder called `ai_hapro` in the output:**
```
drwxr-xr-x  3 user user  4096 Jan 16 10:30 ai_hapro
```

**Navigate into the repository:**
```bash
cd ai_hapro
```

**List the files inside:**
```bash
ls -la
```

**You should see files like:**
```
-rw-r--r--  1 user user  1234 Jan 16 10:30 GEMINI.md
-rw-r--r--  1 user user  2345 Jan 16 10:30 NodeRED_GitHub_Workflow.md
-rw-r--r--  1 user user  3456 Jan 16 10:30 README.md
drwxr-xr-x  8 user user  4096 Jan 16 10:30 .git
```

**Check the Git status:**
```bash
git status
```

**Expected output:**
```
On branch master
Your branch is up to date with 'origin/master'.

nothing to commit, working tree clean
```

**What this means:**
- You're on the "master" branch (the main version)
- You're in sync with GitHub
- Everything is ready to go

---

### Step 1.8: Remember This Location

This is crucial: You need to remember where you cloned the repository.

**Save this command for future use:**

If you cloned to `/config/ai_hapro`:
```bash
cd /config/ai_hapro
```

If you cloned to `/home/pi/ai_hapro`:
```bash
cd /home/pi/ai_hapro
```

Or simply:
```bash
cd ~/ai_hapro
```

**Pro Tip:** Write this down or save it in a note. You'll need it every time you want to pull updates.

---

## Part 2: Importing a Node-RED Workflow

Now that Git is set up, let's walk through the process of importing a workflow into Node-RED.

### Step 2.1: Open Your Terminal

Follow the same process as Step 1.1 to get to a command line.

---

### Step 2.2: Navigate to the Repository

Change into your AI_HApro directory:

```bash
cd /config/ai_hapro
```

or

```bash
cd ~/ai_hapro
```

(Use whichever path you chose in Step 1.5)

**Verify you're in the right place:**
```bash
pwd
```

**Expected output:**
```
/config/ai_hapro
```

---

### Step 2.3: Pull the Latest Changes from GitHub

This is the magic command that downloads any new or updated files:

```bash
git pull origin master
```

**What this command does:**
- `git pull` - Download and merge changes
- `origin` - From the GitHub repository (the "origin" of the code)
- `master` - From the master branch (the main version)

**Possible outputs:**

**Scenario A: New updates available**
```
remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 3 (delta 2), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), done.
From github.com:PhantomBJ/AI_HApro
   7d60510..abc1234  master     -> origin/master
Updating 7d60510..abc1234
Fast-forward
 flows/button_monitor.json | 45 +++++++++++++++++++++++++++++++++++++++++++++
 1 file changed, 45 insertions(+)
 create mode 100644 flows/button_monitor.json
```

**What this means:** New files were downloaded. In this example, a file called `button_monitor.json` was added.

**Scenario B: Already up to date**
```
Already up to date.
```

**What this means:** No changes since last time. You already have the latest version.

**Scenario C: Merge conflict or local changes**
```
error: Your local changes to the following files would be overwritten by merge:
    flows/button_monitor.json
Please commit your changes or stash them before you merge.
Aborting
```

**What this means:** You've modified files locally. See the troubleshooting section for how to handle this.

---

### Step 2.4: Locate the Flow JSON File

Now you need to find the workflow file. These are typically JSON files stored in a specific location.

**List the files to see what's new:**
```bash
ls -lah
```

**Common locations for flow files:**
- `flows/` directory
- Root directory with names like `button_flow.json`, `mqtt_monitor.json`
- Check any documentation files for mentions of the flow name

**Example:**
```bash
ls flows/
```

**Expected output:**
```
button_monitor.json
light_automation.json
```

**In this example, the flow files are in the `flows/` directory.**

**If you're not sure which file to use:**
- Look for a README or documentation file that mentions the workflow
- The file will typically have a descriptive name matching its purpose
- Ask in your project communication channel which file to import

---

### Step 2.5: View and Copy the JSON Content

You need to copy the entire contents of the JSON file. There are several ways to do this:

**Method A: Display in terminal and copy (Recommended for beginners)**

```bash
cat flows/button_monitor.json
```

**What this does:** Displays the file contents in your terminal.

**Expected output:**
```json
[
    {
        "id": "abc123",
        "type": "mqtt in",
        "z": "flow1",
        "name": "Button Press",
        "topic": "home/button/living_room",
        "qos": "1",
        "broker": "mqtt_broker_id",
        "x": 130,
        "y": 80,
        "wires": [["xyz789"]]
    },
    ...
]
```

**How to copy:**
1. **In the Terminal & SSH add-on web interface:** Click and drag to select all the text, then right-click and choose Copy (or Ctrl+C)
2. **In an SSH client:** Same as above, or use your SSH client's copy function
3. **If the file is very long:** It might be easier to use Method B below

**Method B: Download the file to your computer**

If you're running Home Assistant OS, the repository is in `/config/ai_hapro`, which you can access through Samba or the File Editor add-on:

1. In Home Assistant, go to Settings → Add-ons
2. Install "File editor" if not already installed
3. Open File editor
4. Navigate to `ai_hapro/flows/` (or wherever your JSON file is)
5. Click on the JSON file
6. Click the "Select All" button or press Ctrl+A
7. Copy with Ctrl+C or right-click → Copy

**Method C: Use SCP to download (Advanced)**

If you're comfortable with command-line file transfers:
```bash
scp user@homeassistant.local:/config/ai_hapro/flows/button_monitor.json ~/Downloads/
```

Then open the file on your computer with any text editor.

---

### Step 2.6: Open Node-RED

Now we switch to the Node-RED web interface.

**How to access Node-RED:**

**If Node-RED is a Home Assistant add-on:**
1. Go to Settings → Add-ons
2. Find Node-RED in your installed add-ons
3. Click on it
4. Click "Open Web UI" at the top

**If Node-RED is standalone:**
- Open a web browser
- Navigate to `http://homeassistant.local:1880` (or your Pi's IP address)
- Example: `http://192.168.1.100:1880`

**What you should see:**
The Node-RED editor interface with:
- A menu icon (three horizontal lines) in the top-right corner
- A workspace in the center (might have existing flows or be empty)
- A palette of nodes on the left side
- Deploy button in the top-right area

---

### Step 2.7: Import the Flow into Node-RED

Now we'll bring that JSON code into Node-RED.

**Step-by-step:**

1. **Click the hamburger menu** (three horizontal lines) in the top-right corner

2. **Select "Import"** from the dropdown menu

3. **A dialog box will appear** with a text area and some options

4. **Paste the JSON code** you copied earlier into the text area
   - Click in the text area
   - Press Ctrl+V (or Cmd+V on Mac)
   - The text area should fill with JSON code

5. **Choose import options:**
   - **"Import to:" dropdown** - Choose one of:
     - **"new flow"** (Recommended for first time) - Creates a new tab for this workflow
     - **"current flow"** - Adds to your current tab (can get messy)

6. **Click the "Import" button** (usually red)

**What you should see:**
- The dialog closes
- New nodes appear in the Node-RED workspace
- If you selected "new flow," a new tab appears at the top with the flow name
- The nodes might be clustered together (you can drag them apart for better visibility)

**Visual description:**
You'll see rectangular boxes (nodes) connected by lines (wires). Each node has:
- A colored left edge (indicating the node type)
- A name in the center
- Input dots on the left (if applicable)
- Output dots on the right (if applicable)
- Some nodes might have a red triangle warning icon (this is expected - we'll fix it next)

---

### Step 2.8: Identify Configuration Requirements

This is the most important step. Imported flows often have placeholder configurations that need to be updated for your specific system.

**Common things that need configuration:**

1. **MQTT broker connections**
2. **Home Assistant server settings**
3. **Device names or entity IDs**
4. **API keys or tokens** (if applicable)

**How to identify what needs configuration:**

**Look for red warning triangles:**
- Nodes with a small red triangle in the top-right corner need attention
- These indicate missing or invalid configuration

**Look for yellow warning triangles:**
- These indicate the node is deployed but might have issues
- Usually means the node is trying to connect to something that's not available yet

**Example: MQTT nodes typically need configuration**

---

### Step 2.9: Configure MQTT Nodes (Most Common Scenario)

Let's walk through configuring an MQTT node, as this is the most common requirement.

**Find an MQTT node that needs configuration:**
- Look for nodes labeled "mqtt in" or "mqtt out" with red triangles

**Double-click the node to open its configuration:**

**What you'll see:**
- A properties panel opens on the right side
- Various fields for configuration
- A "Server" dropdown that likely shows "Add new mqtt-broker..." or a broken reference

**Configure the MQTT broker:**

1. **Click the pencil icon** next to the "Server" field
   - This opens the broker configuration

2. **In the broker configuration panel:**
   - **Connection tab:**
     - **Name:** Give it a descriptive name like "Home Assistant MQTT" or "Mosquitto Broker"
     - **Server:** Enter your MQTT broker IP address or hostname
       - Example: `192.168.1.100` or `homeassistant.local`
     - **Port:** Typically `1883` for standard MQTT, `8883` for SSL/TLS
     - **Protocol:** Select "MQTT V3.1.1" (most common)

   - **Security tab** (if authentication is required):
     - **Username:** Your MQTT username (e.g., `mqtt_user`)
     - **Password:** Your MQTT password
     - Leave other fields as default unless you know you need to change them

3. **Click "Add" or "Update"** in the top-right of the broker config panel

4. **Back in the node properties:**
   - The "Server" field should now show your broker name
   - Verify the "Topic" field is correct (this should be pre-filled from the imported flow)
   - Check other settings like QoS (Quality of Service, usually "1" or "2")

5. **Click "Done"** in the top-right of the node properties panel

**Repeat this process for all MQTT nodes:**
- The good news: Once you configure the broker once, you can reuse it
- For subsequent MQTT nodes, just select your existing broker from the "Server" dropdown

---

### Step 2.10: Configure Other Node Types

**Home Assistant nodes:**
- Double-click the node
- Select your Home Assistant server from the "Server" dropdown
- If you don't have one configured:
  - Click the pencil icon
  - Enter your Home Assistant URL (e.g., `http://homeassistant.local:8123`)
  - Enter a long-lived access token (generate one in Home Assistant: Profile → Security → Long-Lived Access Tokens)
  - Click "Add"

**Function nodes:**
- Usually don't need configuration
- But you can double-click to view the JavaScript code
- Only modify if you know what you're doing

**Trigger nodes, Delay nodes, etc.:**
- These typically have pre-configured settings from the import
- You can adjust timings if needed

**Debug nodes:**
- These are for troubleshooting and usually work as-is
- They show output in the Debug sidebar (click the bug icon on the right sidebar to open it)

---

### Step 2.11: Deploy the Flow

Once all nodes are configured (no more red triangles), it's time to activate the flow.

**Click the "Deploy" button** in the top-right corner

**What happens:**
1. Node-RED processes the flow
2. Connections are established (MQTT, Home Assistant, etc.)
3. The flow becomes active

**Deploy options (if you click the dropdown arrow next to Deploy):**
- **Full:** Deploys all flows and restarts Node-RED (default, safest option)
- **Modified Flows:** Only deploys tabs that changed (faster but riskier)
- **Modified Nodes:** Only deploys individual nodes that changed (advanced)

**For beginners, always use "Full" deployment.**

**What you should see after deploying:**

**Success indicators:**
- The "Deploy" button turns gray (was red before)
- No error messages appear
- MQTT nodes show "connected" status underneath them (green dot and text)
- Home Assistant nodes show "connected" status

**Potential issues:**
- **Red text under nodes:** Connection failed
  - Example: "mqtt in: Connection to MQTT broker lost"
  - **Solution:** Check your broker configuration, ensure the broker is running
- **Yellow warning triangle:** Node is deployed but has issues
  - Check the Debug sidebar (bug icon) for error messages
  - Common cause: Wrong topic name, incorrect entity ID

---

### Step 2.12: Test the Flow

Now that it's deployed, let's make sure it works.

**Open the Debug sidebar:**
- Click the bug icon on the right side of the Node-RED interface
- This shows output from Debug nodes in your flow

**Trigger the flow:**
- The exact method depends on what the flow does
- Examples:
  - If it monitors a button press, press the physical button
  - If it's triggered by an MQTT message, send a test message
  - If it's time-based, wait for the scheduled time or adjust it for testing

**Check for output:**
- Look in the Debug sidebar for messages
- Debug nodes in your flow will output information here
- Example output: `{"topic":"home/button/living_room","payload":"pressed"}`

**Interpreting results:**

**Success:**
- You see messages in the Debug sidebar
- Messages match what you expect (button press detected, automation triggered, etc.)
- No error messages

**Partial success:**
- Some nodes work, others don't
- Check individual nodes for red or yellow indicators
- Look for error messages in the Debug sidebar

**Failure:**
- No output in Debug sidebar
- Error messages appear
- Nodes show disconnected status

See the [Troubleshooting section](#common-problems-and-solutions) for help with specific errors.

---

### Step 2.13: Save Your Work (Optional but Recommended)

Node-RED auto-saves your flows, but it's good practice to export a backup.

**Export your flows:**
1. Click the hamburger menu (three lines)
2. Select "Export"
3. Select the "All Flows" tab
4. Click "Download"
5. Save the `flows.json` file to a safe location (e.g., a USB drive, cloud storage)

**Why this matters:**
- If something breaks, you can restore your working configuration
- You can share your setup with others or move it to a different system
- It's a snapshot of your working configuration

---

## Common Problems and Solutions

### Problem 1: SSH Authentication Failed

**Error message:**
```
git@github.com: Permission denied (publickey).
fatal: Could not read from remote repository.
```

**What this means:**
GitHub doesn't recognize your SSH key. You need to set up SSH authentication.

**Solution - Generate and Add SSH Key:**

**Step 1: Check if you already have an SSH key**
```bash
ls -la ~/.ssh
```

If you see files like `id_rsa` and `id_rsa.pub` or `id_ed25519` and `id_ed25519.pub`, you already have a key. Skip to Step 3.

**Step 2: Generate a new SSH key**
```bash
ssh-keygen -t ed25519 -C "your.email@example.com"
```

Press Enter to accept the default file location.
Press Enter twice to skip setting a passphrase (or set one if you prefer security).

**Step 3: Display your public key**
```bash
cat ~/.ssh/id_ed25519.pub
```

or

```bash
cat ~/.ssh/id_rsa.pub
```

Copy the entire output (it starts with `ssh-ed25519` or `ssh-rsa` and ends with your email).

**Step 4: Add the key to GitHub**
1. Go to GitHub.com and log in
2. Click your profile picture → Settings
3. Click "SSH and GPG keys" in the left sidebar
4. Click "New SSH key"
5. Give it a title (e.g., "Home Assistant Pi")
6. Paste your public key into the "Key" field
7. Click "Add SSH key"

**Step 5: Test the connection**
```bash
ssh -T git@github.com
```

Expected output:
```
Hi PhantomBJ! You've successfully authenticated, but GitHub does not provide shell access.
```

**Step 6: Try cloning again**
```bash
git clone git@github.com:PhantomBJ/AI_HApro.git ai_hapro
```

---

### Problem 2: Local Changes Preventing Pull

**Error message:**
```
error: Your local changes to the following files would be overwritten by merge:
    flows/button_monitor.json
Please commit your changes or stash them before you merge.
```

**What this means:**
You've modified files in the repository, and Git won't overwrite your changes.

**Solution A: Discard your local changes (if you don't need them)**
```bash
git reset --hard HEAD
git pull origin master
```

Warning: This permanently deletes your local modifications.

**Solution B: Save your changes temporarily (if you want to keep them)**
```bash
git stash
git pull origin master
git stash pop
```

This saves your changes, pulls new updates, then reapplies your changes.

**Solution C: Commit your changes (advanced)**
```bash
git add .
git commit -m "My local changes"
git pull origin master
```

This saves your changes as a commit, then merges with the new updates.

---

### Problem 3: Node-RED Shows "Connection Lost" for MQTT

**Symptom:**
MQTT nodes show red "disconnected" status after deploying.

**Possible causes:**

**Cause 1: Wrong broker address**
- **Solution:** Double-click the node → Edit broker → Verify the IP address/hostname
- Try pinging the broker: `ping 192.168.1.100`

**Cause 2: Wrong port**
- **Solution:** Standard MQTT uses port 1883, not 8883 (that's for SSL)
- Verify in your broker configuration

**Cause 3: Authentication failed**
- **Solution:** Check username and password in broker security settings
- Verify credentials match your MQTT broker configuration

**Cause 4: Broker not running**
- **Solution:** Check if your MQTT broker (e.g., Mosquitto) is running
- In Home Assistant: Settings → Add-ons → Mosquitto broker → Check status
- Restart if necessary

**Cause 5: Firewall blocking connection**
- **Solution:** Ensure port 1883 is open
- Check router/firewall settings if Node-RED and MQTT are on different machines

---

### Problem 4: Red Triangle on Node After Import

**Symptom:**
Node has a red triangle warning icon and shows "unknown type" or missing configuration.

**Possible causes:**

**Cause 1: Missing Node-RED palette (module)**
- **Solution:** Install the required node package
- Go to Node-RED → Menu → Manage palette → Install tab
- Search for the missing node type (e.g., `node-red-contrib-home-assistant`)
- Click install

**Cause 2: Outdated node version**
- **Solution:** Update the node package
- Go to Manage palette → Nodes tab
- Find the node and click "update"

**Cause 3: Missing configuration**
- **Solution:** Double-click the node and fill in required fields
- Look for empty dropdowns or fields marked with asterisks

---

### Problem 5: "Already Up to Date" But I Know There Are Updates

**Symptom:**
`git pull` says "Already up to date" but you know new files were pushed to GitHub.

**Possible causes:**

**Cause 1: Wrong branch**
- **Solution:** Check which branch you're on
```bash
git branch
```
If it doesn't show `* master`, switch to it:
```bash
git checkout master
```

**Cause 2: Not connected to the right remote**
- **Solution:** Verify your remote URL
```bash
git remote -v
```
Should show `git@github.com:PhantomBJ/AI_HApro.git`

**Cause 3: Local cache issue**
- **Solution:** Force fetch from remote
```bash
git fetch origin
git reset --hard origin/master
```

Warning: This discards local changes.

---

### Problem 6: Can't Find the JSON File After Pulling

**Symptom:**
`git pull` succeeded, but you can't find the flow file.

**Solutions:**

**Step 1: List all files including hidden ones**
```bash
ls -la
```

**Step 2: Search for JSON files**
```bash
find . -name "*.json"
```

**Step 3: Check Git log to see what was added**
```bash
git log --name-only -1
```

This shows files changed in the last commit.

**Step 4: Look in subdirectories**
```bash
ls flows/
ls src/
ls node-red/
```

**Step 5: Ask for help**
Check any README or documentation files for the location, or reach out to your project collaborator.

---

### Problem 7: Deploy Button Stays Red or Shows Error

**Symptom:**
Clicking Deploy doesn't work, or error messages appear.

**Possible causes:**

**Cause 1: Syntax error in function node**
- **Solution:** Check the Debug sidebar for JavaScript errors
- Open function nodes and look for red syntax highlighting
- Fix or remove the problematic code

**Cause 2: Circular flow (infinite loop)**
- **Solution:** Check that wires don't create a loop back to the same node
- Break the loop by removing a wire or adding a gate node

**Cause 3: Node-RED service issue**
- **Solution:** Restart Node-RED
- In Home Assistant: Settings → Add-ons → Node-RED → Restart
- Or via command line: `systemctl restart nodered` (if running as service)

---

## Quick Reference Commands

### Git Commands

**Navigate to repository:**
```bash
cd /config/ai_hapro
```

**Check status:**
```bash
git status
```

**Pull latest changes:**
```bash
git pull origin master
```

**View recent changes:**
```bash
git log --oneline -5
```

**Discard local changes:**
```bash
git reset --hard HEAD
```

**View differences:**
```bash
git diff
```

### File Management

**List files:**
```bash
ls -la
```

**Display file contents:**
```bash
cat filename.json
```

**Search for files:**
```bash
find . -name "*.json"
```

**Check current directory:**
```bash
pwd
```

### Node-RED Access

**Home Assistant add-on:**
- Settings → Add-ons → Node-RED → Open Web UI

**Standalone:**
- `http://homeassistant.local:1880`
- `http://192.168.1.100:1880`

### Node-RED Workflow

1. **Import:** Menu → Import → Paste JSON → Import to new flow
2. **Configure:** Double-click nodes with red triangles → Configure settings
3. **Deploy:** Click Deploy button
4. **Debug:** Click bug icon → View debug messages
5. **Test:** Trigger the flow and check debug output

---

## Tips for Success

1. **Always pull before importing:** Run `git pull` to get the latest files before trying to import
2. **One flow at a time:** Import and test one workflow before moving to the next
3. **Keep notes:** Document your broker addresses, ports, and credentials for easy reference
4. **Backup regularly:** Export your flows after getting something working
5. **Check the Debug sidebar:** It's your best friend for troubleshooting
6. **Don't skip deployment:** Flows don't activate until you click Deploy
7. **Name your brokers clearly:** Use descriptive names like "HA MQTT" instead of "mqtt-broker-1"
8. **Test incrementally:** After configuring each node, deploy and test before moving on

---

## Next Steps

Once you're comfortable with this workflow:
1. Learn about creating your own flows in Node-RED
2. Explore the Node-RED palette for additional node types
3. Study how existing flows work by examining the nodes and connections
4. Experiment with modifying flows to customize behavior
5. Consider setting up automated backups of your Node-RED flows to GitHub

Remember: The Node-RED community is very helpful, and most problems you encounter have been solved before. Don't hesitate to search for error messages or ask for help in forums like the Home Assistant community or Node-RED forums.

---

**You've got this! Take it one step at a time, and you'll be importing flows like a pro in no time.**
