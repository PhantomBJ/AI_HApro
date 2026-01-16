# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Home Assistant automation project focused on integrating Node-RED flows with MQTT for home automation. The project uses a Git-based workflow to synchronize Node-RED flows and automation configurations between a development environment and a live Home Assistant instance.

**Primary Technologies:**
- Node-RED for visual automation workflows
- MQTT (Mosquitto) for device messaging and communication
- Home Assistant for smart home orchestration
- Git/GitHub for version control and deployment

## Repository Structure

The project is documentation-driven, serving as a central repository for:
- Node-RED flow definitions (JSON format)
- MQTT topic and workflow documentation
- Development and deployment guides
- Environment configuration details (non-sensitive)

Key documentation files:
- `HAnodeRED_MQTT.md`: Example Node-RED flows for MQTT button monitoring
- `NodeRED_GitHub_Workflow.md`: Import/export workflows for Node-RED flows via Git
- `GEMINI.md`: Project context and status (maintained by another AI assistant)

## Version Control Workflow

This project uses SSH-based Git authentication with the remote repository at `git@github.com:PhantomBJ/AI_HApro.git`.

**Development and Deployment Pattern:**
1. Development occurs in the `/root/AIpro` directory
2. Changes are committed and pushed to the GitHub remote
3. The live Home Assistant environment pulls changes from GitHub
4. Node-RED flows are imported manually via the Node-RED UI

**Important:** When working with this repository, never commit sensitive information (passwords, API keys, tokens, broker credentials). The `.gitignore` is configured for Python projects but always verify before committing configuration files.

## Common Commands

### Version Control
```bash
# Standard workflow
git add <files>
git commit -m "descriptive message"
git push origin master

# Pull updates from remote
git pull origin master
```

### Working with Node-RED Flows

**Creating New Flows:**
- Save flow JSON to the repository (e.g., `flows/new_flow.json` or descriptive filename)
- Include clear documentation about what the flow does
- Note any configuration requirements (MQTT broker settings, topics, etc.)
- Document placeholder values that need user configuration

**Flow JSON Structure:**
- Node-RED flows are JSON arrays containing node definitions
- MQTT nodes reference broker configuration nodes by ID
- Always include a note about required user configuration (broker addresses, topics)

## Architecture Notes

**MQTT Integration:**
- The system uses MQTT topics following the pattern `home/<device>/<location>/state`
- MQTT broker configuration is environment-specific and not committed to the repo
- QoS level 2 is used for button/state monitoring for guaranteed delivery

**Node-RED Configuration Management:**
- Actual broker IPs/credentials are configured in the Node-RED UI
- Repository flows use placeholder values like `YOUR_MQTT_BROKER_ID`
- Users must manually configure broker settings after importing flows

**Deployment Process:**
1. Flows are committed to Git
2. User pulls latest changes on their system
3. User imports JSON into Node-RED UI
4. User configures environment-specific settings (brokers, credentials)
5. User deploys in Node-RED

## Development Environment

The primary development environment is located at `/root/AIpro`. The `~/.bashrc` is configured to automatically navigate to this directory on shell startup.

Home Assistant instance is accessible via:
- Web interface for configuration
- SSH for command-line access
- Node-RED UI for flow editing
