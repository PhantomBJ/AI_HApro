# Project Overview

This project appears to be a Python application designed to analyze GitHub repositories. Its main function is to fetch and parse `README.md` files from GitHub, extracting structured information such as project descriptions, code examples, and other relevant details. The extracted data is then organized into a structured format, likely for documentation or analysis purposes.

The application uses a modular architecture, with separate components for interacting with the GitHub API, parsing content, and defining data structures.

**Technologies:**
*   **Language:** Python
*   **Key Libraries:**
    *   `requests` or a similar HTTP client for GitHub API interaction.
    *   `pytest` for unit testing.
    *   Pydantic (`schemas.py`) for data validation and schema definition.

# Building and Running

### Installation

To install the necessary dependencies, run:

```bash
pip install -r requirements.txt
```
*(Note: A `requirements.txt` file is assumed but was not found. This command is a placeholder.)*

### Execution

The main entry point of the application seems to be `main.py`. To run the application, you would typically execute:

```bash
python main.py <arguments>
```
*(Note: The specific command-line arguments are not defined. A TODO is to document the CLI usage.)*

### Testing

The project includes a `tests` directory, indicating the use of `pytest`. To run the tests, execute:

```bash
pytest
```

# Development Conventions

*   **Code Style:** The code likely follows standard Python conventions (PEP 8). The presence of `schemas.py` and type hints suggests a focus on code quality and data integrity.
*   **Testing:** Unit tests are present and should be maintained to ensure functionality and prevent regressions. New features should be accompanied by corresponding tests.
*   **Modularity:** The project is divided into logical modules (`github_service.py`, `utils.py`, `schemas.py`), and this convention should be followed.

# Version Control

This project is version-controlled using Git. The central "source of truth" is a remote repository hosted on GitHub, and authentication is managed via SSH keys.

*   **Remote URL (SSH):** `git@github.com:PhantomBJ/AI_HApro.git`

### Synchronization Workflow
- **Development** occurs in this local environment. Changes are committed and pushed to the remote repository.
- **Deployment** on the live Home Assistant environment is done by pulling the latest changes from the remote repository.
- The use of SSH keys should prevent the need for repeated password or token entry.

# Session Updates (January 16, 2026)

## Claude Code Session:
*   **CLAUDE.md Created:** Generated comprehensive documentation for future Claude Code instances, including project overview, architecture, common commands, and custom agent information.
*   **NodeRED_Steps.md Created:** homelab-mentor agent generated a 1,168-line beginner-friendly guide for Git setup and Node-RED workflow imports (via homelab-mentor agent).
*   **Custom Agents Configured:**
    - `homelab-mentor`: For Home Assistant, Node-RED, Raspberry Pi 5 guidance
    - `gemini-researcher`: For web research and documentation lookup
*   **Documentation Updates:** Updated CLAUDE.md with references to new guides and agent information.

## Home Assistant Setup Completed:
*   **SSH Key Authentication:** Successfully generated SSH key pair on Home Assistant (Raspberry Pi 5) and added to GitHub account.
    - Location: `~/.ssh/id_ed25519` (private) and `~/.ssh/id_ed25519.pub` (public)
    - Public key: `ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIDHccsHu5gT5sXiz38Wikgeblw3FDh+HoBgmd7QYpFOo homeassistant-ha-instance`
    - SSH connection to GitHub verified and working
*   **Repository Cloned on Home Assistant:** Successfully cloned AI_HApro repository to `/config/AI_HApro` on the Home Assistant machine.
    - Branch: `master` (contains latest docs)
    - All files present: CLAUDE.md, NodeRED_Steps.md, HAnodeRED_MQTT.md, NodeRED_GitHub_Workflow.md, GEMINI.md
*   **Git Workflow Operational:** Can now push/pull between development environment (/root/AIpro) and Home Assistant (/config/AI_HApro) via GitHub.

# Current Project Status (As of Last Session)

*   **Git Repository Setup:** Successfully initialized a Git repository, connected it to your GitHub remote, and configured SSH key authentication. This provides a robust version control and synchronization mechanism.
*   **Documentation Created:**
    *   `HAnodeRED_MQTT.md`: Contains a basic Node-RED flow JSON for monitoring MQTT button presses.
    *   `NodeRED_GitHub_Workflow.md`: Provides a comprehensive guide on how to import flows from GitHub into Node-RED and how to export your Node-RED environment details.
*   **Home Assistant Access:** You successfully regained access to your Home Assistant web interface, rendering the password reset unnecessary for now. We also debugged and enabled SSH access to your HA instance, which is now ready for future CLI interactions.

**Current Status - Where We Left Off:**
User is on Home Assistant machine at `/config/AI_HApro` directory and ready to export Node-RED environment details.

**Next Steps to Resume:**
1. **Export Node-RED Package List:** Find and copy the Node-RED `package.json` file to document installed palettes/modules.
   - Check locations: `~/.node-red/package.json` or `/config/.node-red/package.json`
   - Command to find: `find /config -name "package.json" -path "*node-red*" 2>/dev/null`
   - Copy to: `/config/AI_HApro/nodered_package.json`
   - Commit and push to GitHub

2. **Optional: Export Current Flows:** If any flows exist in Node-RED UI:
   - Export from Node-RED UI: Menu → Export → All Flows → Download
   - Save to `/config/AI_HApro/flows.json`
   - Commit and push to GitHub

This will allow AI assistants to see what Node-RED modules are available when creating custom flows for your setup.

