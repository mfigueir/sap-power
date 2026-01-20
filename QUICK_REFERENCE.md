# SAP Skills Power - Quick Reference

A quick reference guide for using the SAP Skills Power in Kiro IDE.

## 🚀 Quick Start

```bash
# Open any SAP file
code my-project/db/schema.cds

# Ask Kiro for help
"Create a CAP service for managing orders"
```

## 📋 Common Commands

### CAP Development

```
"Create a CAP data model for [entity]"
"Add draft support to my service"
"Implement custom handlers for [action]"
"Add authentication to my CAP service"
"Deploy this CAP app to BTP"
```

### UI5 Development

```
"Create a Fiori List Report for [service]"
"Add a custom action to my Fiori app"
"Make this view responsive"
"Implement value help for [field]"
"Add navigation to [target]"
```

### ABAP Development

```
"Analyze this ABAP code"
"Create a CDS view for [table]"
"Modernize this ABAP function"
"Apply Clean ABAP principles"
"Convert to RAP service"
```

### BTP Operations

```
"Configure destination to [system]"
"Set up Cloud Connector"
"Create integration flow for [process]"
"Deploy to Cloud Foundry"
"Configure XSUAA security"
```

## 🎯 File Type Triggers

| File Type | Extension | Auto-Activates |
|-----------|-----------|----------------|
| CDS Models | `.cds` | ✅ |
| UI5 Manifest | `manifest.json` | ✅ |
| ABAP Code | `.abap` | ✅ |
| MTA Descriptor | `mta.yaml` | ✅ |
| Security Config | `xs-security.json` | ✅ |
| HANA CDS | `.hdbcds` | ✅ |
| HANA View | `.hdbview` | ✅ |
| HANA Procedure | `.hdbprocedure` | ✅ |
| UI5 Config | `ui5.yaml` | ✅ |

## 🛠️ Skill Categories

### Tooling (4 skills)
- `skill-review` - Quality audit
- `sap-api-style` - API docs
- `sap-hana-cli` - HANA CLI
- `sapui5-linter` - UI5 linting

### BTP Platform (14 skills)
- `sap-btp-developer-guide`
- `sap-btp-connectivity`
- `sap-btp-integration-suite`
- `sap-btp-cloud-logging`
- `sap-btp-job-scheduling`
- `sap-btp-service-manager`
- And 8 more...

### UI Development (4 skills)
- `sapui5`
- `sap-fiori-tools`
- `sapui5-cli`
- `sapui5-linter`

### Data & Analytics (5 skills)
- `sap-datasphere`
- `sap-sac-scripting`
- `sap-sac-planning`
- `sap-sac-custom-widget`
- `sap-hana-cloud-data-intelligence`

### Core Technologies (6 skills)
- `sap-cap-capire`
- `sap-abap`
- `sap-abap-cds`
- `sap-sqlscript`
- `sap-ai-core`
- `sap-hana-ml`

## 💡 Best Practices

### Be Specific
❌ "Create a service"
✅ "Create a CAP OData V4 service with draft support"

### Provide Context
❌ "Fix this"
✅ "Refactor this ABAP code following Clean ABAP principles"

### Use Keywords
Include SAP-specific terms:
- CAP, CDS, OData
- Fiori, UI5, SAPUI5
- BTP, Cloud Foundry
- ABAP, RAP, CDS Views
- HANA, SQLScript

## 🔧 Troubleshooting

### Power Not Activating
```bash
# Check installation
ls ~/.kiro/powers/sap-skills

# Restart Kiro
Cmd/Ctrl + Shift + P → "Developer: Reload Window"
```

### Skills Not Loading
```
# Use explicit keywords
"Using SAP CAP best practices, create..."

# Check file type
# Ensure you're editing .cds, .abap, manifest.json, etc.
```

### MCP Server Issues
```bash
# Check MCP status
Cmd/Ctrl + Shift + P → "Kiro: Show MCP Servers"

# Restart MCP
Cmd/Ctrl + Shift + P → "Kiro: Restart MCP Servers"
```

## 📚 Documentation

| Guide | Purpose |
|-------|---------|
| [Getting Started](steering/getting-started.md) | Quick start & workflows |
| [Best Practices](steering/best-practices.md) | SAP standards |
| [CAP Development](steering/cap-development.md) | CAP patterns |
| [UI5 Development](steering/ui5-development.md) | UI5 guidance |

## 🔗 Quick Links

- [SAP CAP Docs](https://cap.cloud.sap/docs/)
- [SAPUI5 Docs](https://ui5.sap.com/)
- [SAP BTP Docs](https://help.sap.com/docs/btp)
- [Clean ABAP](https://github.com/SAP/styleguides)
- [SAP Community](https://community.sap.com/)

## ⌨️ Keyboard Shortcuts

| Action | Shortcut |
|--------|----------|
| Open Command Palette | `Cmd/Ctrl + Shift + P` |
| Ask Kiro | `Cmd/Ctrl + K` |
| Open File | `Cmd/Ctrl + P` |
| Search in Files | `Cmd/Ctrl + Shift + F` |

## 🎓 Example Workflows

### Create CAP App
```bash
1. cds init my-app
2. cd my-app && code .
3. Ask: "Create a data model for products"
4. Ask: "Add a service with CRUD operations"
5. Ask: "Add authentication"
```

### Create Fiori App
```bash
1. mkdir my-fiori-app && cd my-fiori-app
2. code .
3. Ask: "Create a Fiori List Report"
4. Ask: "Add custom filters"
5. Ask: "Make it responsive"
```

### Deploy to BTP
```bash
1. Ask: "Create mta.yaml for deployment"
2. Ask: "Configure XSUAA security"
3. Ask: "Add HANA database binding"
4. Run: mbt build && cf deploy
```

## 📊 Skill Matrix

| Task | Primary Skill | Secondary Skills |
|------|---------------|------------------|
| CAP Service | `sap-cap-capire` | `sap-btp-developer-guide` |
| Fiori App | `sapui5`, `sap-fiori-tools` | `sapui5-linter` |
| ABAP Modernization | `sap-abap` | `sap-abap-cds` |
| BTP Deployment | `sap-btp-developer-guide` | `sap-btp-service-manager` |
| HANA Development | `sap-sqlscript` | `sap-hana-cli` |
| Integration | `sap-btp-integration-suite` | `sap-btp-connectivity` |

## 🆘 Getting Help

1. **Check Documentation**: Start with [README.md](README.md)
2. **Search Issues**: [GitHub Issues](https://github.com/mfigueir/sap-skills-power/issues)
3. **Ask Community**: [SAP Community](https://community.sap.com/)
4. **Create Issue**: Report bugs or request features

---

**Print this page for quick reference!** 📄
