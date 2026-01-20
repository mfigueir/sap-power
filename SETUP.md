# Setup Guide for SAP Skills Power

This guide walks you through setting up the SAP Skills Power in Kiro IDE and preparing your environment for SAP development.

## Prerequisites

### Required Software

1. **Kiro IDE** (version 1.0.0 or higher)
   - Download from [kiro.dev](https://kiro.dev)

2. **Node.js** (version 18 or higher)
   ```bash
   node --version  # Should be v18.0.0 or higher
   ```

3. **Git**
   ```bash
   git --version
   ```

### Optional but Recommended

4. **SAP Cloud Foundry CLI** (for BTP deployments)
   ```bash
   cf --version
   ```

5. **@sap/cds-dk** (for CAP development)
   ```bash
   npm install -g @sap/cds-dk
   cds --version
   ```

6. **UI5 CLI** (for UI5 development)
   ```bash
   npm install -g @ui5/cli
   ui5 --version
   ```

## Installation Methods

### Method 1: Install from Kiro Powers Panel (Recommended)

1. Open Kiro IDE
2. Press `Cmd/Ctrl + Shift + P` to open Command Palette
3. Type "Kiro: Configure Powers"
4. Search for "SAP Skills"
5. Click "Install"
6. Restart Kiro or reload window

### Method 2: Manual Installation

#### macOS/Linux

```bash
# Create powers directory if it doesn't exist
mkdir -p ~/.kiro/powers

# Clone the repository
cd ~/.kiro/powers
git clone https://github.com/mfigueir/sap-skills-power.git sap-skills

# Restart Kiro
```

#### Windows

```powershell
# Create powers directory if it doesn't exist
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.kiro\powers"

# Clone the repository
cd "$env:USERPROFILE\.kiro\powers"
git clone https://github.com/YOUR_USERNAME/sap-skills-power.git sap-skills

# Restart Kiro
```

### Method 3: Download ZIP

1. Download the latest release from GitHub
2. Extract the ZIP file
3. Copy the `sap-skills` folder to:
   - **macOS/Linux**: `~/.kiro/powers/`
   - **Windows**: `%USERPROFILE%\.kiro\powers\`
4. Restart Kiro

## Verification

### Check Installation

1. Open Kiro IDE
2. Open Command Palette (`Cmd/Ctrl + Shift + P`)
3. Type "Kiro: List Powers"
4. Verify "SAP Skills" appears in the list

### Test Activation

1. Create a test file:
   ```bash
   mkdir test-sap
   cd test-sap
   touch schema.cds
   ```

2. Open `schema.cds` in Kiro

3. Ask Kiro:
   ```
   "Create a simple CAP data model for a bookshop"
   ```

4. Verify Kiro responds with CAP-specific guidance

## SAP MCP Server Integration

### Install SAP CAP MCP Server

```bash
npm install -g @cap-js/mcp-server
```

### Configure in Kiro

1. Open Command Palette
2. Type "Kiro: Configure MCP"
3. Add to your MCP configuration:

```json
{
  "mcpServers": {
    "sap-cap": {
      "command": "npx",
      "args": ["@cap-js/mcp-server"],
      "disabled": false
    }
  }
}
```

### Install SAP UI5 MCP Server

```bash
npm install -g @ui5/mcp-server
```

Add to MCP configuration:

```json
{
  "mcpServers": {
    "sap-ui5": {
      "command": "npx",
      "args": ["@ui5/mcp-server"],
      "disabled": false
    }
  }
}
```

## Project Setup

### CAP Project

```bash
# Create new CAP project
cds init my-cap-app
cd my-cap-app

# Open in Kiro
code .

# Ask Kiro for help
# "Create a data model for managing products"
```

### UI5 Project

```bash
# Create new UI5 project
mkdir my-ui5-app
cd my-ui5-app
npm init -y
npm install @ui5/cli --save-dev

# Open in Kiro
code .

# Ask Kiro for help
# "Create a Fiori List Report application"
```

### Existing SAP Project

```bash
# Clone your project
git clone <your-sap-project-url>
cd <project-name>

# Install dependencies
npm install

# Open in Kiro
code .

# The power will automatically activate
```

## Configuration

### Customize File Patterns

Edit `~/.kiro/powers/sap-skills/POWER.md` to add custom file patterns:

```yaml
filePatterns:
  - "**/*.cds"
  - "**/manifest.json"
  - "**/*.abap"
  - "**/custom-pattern.ext"  # Add your pattern
```

### Adjust Keywords

Modify keywords to control when the power activates:

```yaml
keywords: ["sap", "btp", "cap", "fiori", "custom-keyword"]
```

## Troubleshooting

### Power Not Loading

**Check installation path:**
```bash
# macOS/Linux
ls -la ~/.kiro/powers/sap-skills

# Windows
dir %USERPROFILE%\.kiro\powers\sap-skills
```

**Verify POWER.md exists:**
```bash
cat ~/.kiro/powers/sap-skills/POWER.md
```

### Skills Not Activating

**Check file extension:**
- Ensure you're working with SAP file types (.cds, .abap, manifest.json)

**Use explicit keywords:**
- Include SAP-specific terms in your prompts
- Example: "Using SAP CAP best practices..."

**Restart Kiro:**
```bash
# Reload window
Cmd/Ctrl + Shift + P → "Developer: Reload Window"
```

### MCP Server Issues

**Check MCP server status:**
```bash
# Open Command Palette
# Type "Kiro: Show MCP Servers"
```

**Restart MCP servers:**
```bash
# Open Command Palette
# Type "Kiro: Restart MCP Servers"
```

**Check logs:**
```bash
# Open Command Palette
# Type "Kiro: Show MCP Logs"
```

## Environment Variables

### SAP BTP

```bash
# Set BTP credentials (optional)
export CF_API=https://api.cf.us10.hana.ondemand.com
export CF_ORG=your-org
export CF_SPACE=your-space
```

### HANA Cloud

```bash
# Set HANA credentials (optional)
export HANA_HOST=your-hana-host
export HANA_PORT=443
```

## Next Steps

1. **Read the guides:**
   - [Getting Started](steering/getting-started.md)
   - [Best Practices](steering/best-practices.md)
   - [CAP Development](steering/cap-development.md)
   - [UI5 Development](steering/ui5-development.md)

2. **Try examples:**
   - Create a CAP service
   - Build a Fiori app
   - Deploy to BTP

3. **Join the community:**
   - [SAP Community](https://community.sap.com/)
   - [GitHub Discussions](https://github.com/YOUR_USERNAME/sap-skills-power/discussions)

## Support

- **Documentation**: [README.md](README.md)
- **Issues**: [GitHub Issues](https://github.com/mfigueir/sap-skills-power/issues)
- **Discussions**: [GitHub Discussions](https://github.com/mfigueir/sap-skills-power/discussions)

## Updates

### Check for Updates

```bash
cd ~/.kiro/powers/sap-skills
git pull origin main
```

### Enable Auto-Updates

Add to your Kiro settings:

```json
{
  "kiro.powers.autoUpdate": true
}
```

---

**Ready to start developing with SAP Skills Power!** 🚀
