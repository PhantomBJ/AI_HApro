# Node-RED and GitHub Synchronization Workflow

This document outlines the process for synchronizing Node-RED flows and environment details between the development environment (Gemini AI) and your live Home Assistant / Node-RED instance using our shared GitHub repository.

---

## Part 1: Importing Flows from GitHub into Node-RED

Follow these steps to pull a new or updated flow that I have pushed to the GitHub repository and deploy it in your Node-RED editor.

### Prerequisites
- `git` must be installed on a machine that can access your Node-RED instance.
- You must have already cloned our shared repository (`AI_HApro`) to that machine.

### The Import Workflow
1.  **Open a Terminal:** On your machine, open a terminal or command prompt.
2.  **Navigate to the Repository:** Change into the project's directory.
    ```bash
    cd path/to/your/AI_HApro
    ```
3.  **Pull Latest Changes:** Fetch the latest updates from the GitHub repository.
    ```bash
    git pull origin main
    ```
4.  **Access the Flow File:** The repository will now contain the latest files. I will typically save new flows in a file, for example `flows.json` or within a `flows/` directory. Open this file in a text editor.
5.  **Copy the JSON Content:** Select and copy the entire contents of the JSON file.
6.  **Import into Node-RED:**
    - Open your Node-RED editor in your web browser.
    - Click the main menu (☰) in the top-right corner and select **Import**.
    - Paste the copied JSON code into the import dialog.
    - Choose whether to import to a new flow tab or your current one.
    - Click the **Import** button.
7.  **Configure and Deploy:**
    - The new nodes will appear in your editor.
    - **Crucially, you may need to configure them.** For example, an MQTT node I create will have a placeholder for the server. You will need to double-click the node and select your pre-configured MQTT broker from the dropdown menu.
    - Once configured, click the **Deploy** button.

---

## Part 2: Exporting Your Environment for Me to Analyze

To ensure the code I write is compatible with your setup, you can export details about your environment and add them to our repository.

**IMPORTANT:** Never commit sensitive information like passwords, API keys, or secret tokens to the Git repository.

### 1. Exporting Your Current Flows
This is the most important piece of information.
1.  In the Node-RED editor, go to the main menu (☰) and select **Export**.
2.  In the dialog, select the **All Flows** tab.
3.  Click the **Download** button. This will save a `flows.json` file to your computer.
4.  **Action:** Move this `flows.json` file into our `AI_HApro` repository directory, commit it, and push it to GitHub. This will allow me to see your entire current setup.

### 2. Listing Your Installed Node-RED Modules
This tells me which nodes you have available.
1.  Find the `package.json` file located in your Node-RED user directory. This is typically at `~/.node-red/package.json` on Linux systems.
2.  **Action:** Copy this file into our `AI_HApro` repository. To avoid confusion, you can rename it to `nodered_package.json`. Commit and push this file.

### 3. Documenting Your Configuration (MQTT, etc.)
Share non-sensitive details about your setup.
1.  Create a new text file in our repository named `environment_setup.md`.
2.  In this file, describe your setup. **DO NOT INCLUDE PASSWORDS.**
    - **Good Example:** "I am running the Mosquitto MQTT broker add-on in Home Assistant. Its IP address is `192.168.1.100` and the port is `1883`. I have a user named `mqtt_user` for authentication."
    - **Bad Example:** "My MQTT password is `SuperSecret123`."
3.  **Action:** Commit and push this `environment_setup.md` file.

By following these steps, we can maintain a clear and synchronized understanding of the project, which will make our collaboration much more efficient.
