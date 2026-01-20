# SAP Skills Power for Kiro

[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)
[![GitHub release](https://img.shields.io/github/v/release/mfigueir/sap-skills-power)](https://github.com/mfigueir/sap-skills-power/releases)
[![Kiro Power](https://img.shields.io/badge/Kiro-Power-purple)](https://kiro.dev)
[![SAP](https://img.shields.io/badge/SAP-Ecosystem-0FAAFF)](https://www.sap.com)

Production-ready SAP development knowledge and best practices for building modern SAP applications in Kiro IDE. This power provides comprehensive guidance across the entire SAP ecosystem including BTP, CAP, Fiori, ABAP, HANA, and Analytics Cloud.

## 🚀 Features

### 35+ Specialized SAP Skills

- **SAP BTP Platform** (14 skills): Cloud services, connectivity, integration, and platform operations
- **UI Development** (4 skills): SAPUI5, Fiori Tools, and modern UI patterns
- **Data & Analytics** (5 skills): Datasphere, Analytics Cloud, and data intelligence
- **Core Technologies** (6 skills): ABAP, CAP, HANA, and AI/ML development
- **Tooling & Development** (4 skills): CLI tools, linters, and quality assurance

### Automatic Context Activation

The power automatically activates when working with:
- `.cds` files (CAP models)
- `manifest.json` (UI5 apps)
- `.abap` files (ABAP code)
- `mta.yaml` (BTP deployments)
- `xs-security.json` (Security configs)
- `.hdbcds`, `.hdbview`, `.hdbprocedure` (HANA artifacts)
- `ui5.yaml` (UI5 projects)

## 📦 Installation

### Option 1: Install from Kiro Powers Panel

1. Open Kiro IDE
2. Open Command Palette (`Cmd/Ctrl + Shift + P`)
3. Search for "Kiro: Configure Powers"
4. Search for "SAP Skills"
5. Click "Install"

### Option 2: Manual Installation

1. Clone or download this repository
2. Copy the `sap-skills` folder to your Kiro powers directory:
   - **macOS/Linux**: `~/.kiro/powers/`
   - **Windows**: `%USERPROFILE%\.kiro\powers\`
3. Restart Kiro or reload the window

### Option 3: Install from GitHub

```bash
cd ~/.kiro/powers/
git clone https://github.com/mfigueir/sap-skills-power.git sap-skills
```

## 🎯 Quick Start

### 1. Automatic Activation

Simply open any SAP project file, and the power activates automatically:

```bash
# Open a CAP project
code my-cap-app/db/schema.cds

# Open a UI5 project
code my-ui5-app/webapp/manifest.json

# Open an ABAP file
code my-abap-project/src/zcl_example.clas.abap
```

### 2. Ask Kiro for Help

```
"Create a CAP service for managing products with draft support"
"Build a Fiori List Report for my OData service"
"Refactor this ABAP code following Clean ABAP principles"
"Deploy my CAP app to BTP Cloud Foundry"
```

### 3. Use Steering Guides

The power includes comprehensive guides:
- `getting-started.md` - Quick start and common workflows
- `best-practices.md` - SAP development standards
- `cap-development.md` - CAP-specific patterns
- `ui5-development.md` - UI5 and Fiori guidance

## 💡 Usage Examples

### Building a CAP Application

```javascript
// Ask Kiro:
"Create a CAP bookshop service with:
- Books and Authors entities
- Draft support for editing
- Input validation
- OData V4 service"

// Kiro uses sap-cap-capire skill to provide:
// ✅ CDS model with best practices
// ✅ Service implementation patterns
// ✅ Authentication setup
// ✅ Deployment configuration
```

### Creating a Fiori UI

```javascript
// Ask Kiro:
"Create a Fiori List Report for my Books service with:
- Responsive design
- Custom filters
- Export to Excel
- Accessibility compliance"

// Kiro uses sapui5 and sap-fiori-tools skills
```

### Modernizing ABAP Code

```abap
" Ask Kiro:
"Analyze this ABAP code and modernize it using:
- Clean ABAP principles
- CDS views instead of SELECT statements
- Modern ABAP syntax"

" Kiro uses sap-abap and sap-abap-cds skills
```

### BTP Service Integration

```javascript
// Ask Kiro:
"Configure connectivity to my on-premise SAP ECC system:
- Cloud Connector setup
- Destination configuration
- Authentication handling"

// Kiro uses sap-btp-connectivity skill
```

## 🛠️ Skill Categories

### Tooling & Development
- `skill-review` - 14-phase quality audit process
- `sap-api-style` - API documentation standards
- `sap-hana-cli` - HANA CLI operations
- `sapui5-linter` - UI5 code analysis

### SAP BTP Platform (14 skills)
- `sap-btp-developer-guide` - Comprehensive BTP patterns
- `sap-btp-connectivity` - Cloud Connector and destinations
- `sap-btp-integration-suite` - Integration flows and APIs
- `sap-btp-cloud-logging` - Centralized logging
- `sap-btp-job-scheduling` - Background job management
- `sap-btp-service-manager` - Service instance management
- And 8 more BTP skills...

### UI Development
- `sapui5` - Complete UI5 framework
- `sap-fiori-tools` - Fiori development
- `sapui5-cli` - UI5 CLI tools
- `sapui5-linter` - UI5 linting

### Data & Analytics
- `sap-datasphere` - Data modeling
- `sap-sac-scripting` - Analytics Cloud scripting
- `sap-sac-planning` - Planning applications
- `sap-sac-custom-widget` - Custom widget development
- `sap-hana-cloud-data-intelligence` - Data pipelines

### Core Technologies
- `sap-cap-capire` - Cloud Application Programming Model
- `sap-abap` - ABAP development
- `sap-abap-cds` - CDS views
- `sap-sqlscript` - HANA SQLScript
- `sap-ai-core` - AI/ML deployment
- `sap-hana-ml` - HANA ML library

## 🔗 Integration with MCP Servers

This power works seamlessly with official SAP MCP servers:

### SAP CAP MCP Server
```bash
npm install -g @cap-js/mcp-server
```

### SAP UI5 MCP Server
```bash
npm install -g @ui5/mcp-server
```

### ABAP Analyzer MCP
Custom ABAP code analysis and modernization tools.

Together, these provide a complete SAP development environment in Kiro.

## 📚 Documentation

### Steering Guides

- **[Getting Started](steering/getting-started.md)** - Quick start guide and common workflows
- **[Best Practices](steering/best-practices.md)** - SAP development standards
- **[CAP Development](steering/cap-development.md)** - CAP-specific patterns and examples
- **[UI5 Development](steering/ui5-development.md)** - UI5 and Fiori guidance

### Project Structure

```
sap-skills/
├── POWER.md                    # Power configuration and overview
├── README.md                   # This file
└── steering/
    ├── getting-started.md      # Quick start guide
    ├── best-practices.md       # SAP standards
    ├── cap-development.md      # CAP patterns
    └── ui5-development.md      # UI5 guidance
```

## 🎓 Learning Resources

### Official SAP Documentation
- [SAP CAP Documentation](https://cap.cloud.sap/docs/)
- [SAPUI5 Documentation](https://ui5.sap.com/)
- [SAP BTP Documentation](https://help.sap.com/docs/btp)
- [Clean ABAP Guide](https://github.com/SAP/styleguides)

### Community Resources
- [SAP Community](https://community.sap.com/)
- [SAP Samples on GitHub](https://github.com/SAP-samples)
- [CAP Community](https://cap.cloud.sap/docs/community/)

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

### How to Contribute

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

### Guidelines

- Follow SAP development best practices
- Include documentation for new skills
- Test with real SAP projects
- Update steering guides as needed

## 📋 Requirements

- **Kiro IDE** version 1.0.0 or higher
- **Node.js** 18+ (for CAP projects)
- **SAP BTP Account** (for cloud deployments)
- **SAP UI5 CLI** (for UI5 projects)
- **@sap/cds-dk** (for CAP development)

## 🐛 Troubleshooting

### Power Not Activating

**Check:**
1. File extensions match SAP project types
2. Project has SAP dependencies in `package.json`
3. Use SAP-specific keywords in prompts

### Skills Not Loading

**Solution:**
1. Restart Kiro IDE
2. Check power installation path
3. Verify `POWER.md` frontmatter is valid

### Need More Context

**Provide:**
- Project type (CAP, UI5, ABAP)
- Target platform (BTP, on-premise)
- SAP products in use
- Specific requirements

## 📄 License

This project is licensed under the GNU General Public License v3.0 - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

Based on the [SAP Skills for Claude Code](https://github.com/secondsky/sap-skills) repository by [secondsky](https://github.com/secondsky).

Special thanks to:
- SAP Community for comprehensive documentation
- Kiro team for the Powers framework
- All contributors to SAP open-source projects

## 📞 Support

- **Issues**: [GitHub Issues](https://github.com/mfigueir/sap-skills-power/issues)
- **Discussions**: [GitHub Discussions](https://github.com/mfigueir/sap-skills-power/discussions)
- **SAP Community**: [community.sap.com](https://community.sap.com/)

## 🗺️ Roadmap

- [ ] Add more ABAP modernization patterns
- [ ] Expand BTP service integration examples
- [ ] Add SAP Build Apps guidance
- [ ] Include SAP Integration Suite patterns
- [ ] Add SAP Analytics Cloud advanced examples
- [ ] Create video tutorials
- [ ] Add interactive examples

## 📊 Version History

- **1.0.0** (2026-01-14)
  - Initial release
  - 35+ SAP skills
  - 4 comprehensive steering guides
  - Automatic context activation
  - Integration with SAP MCP servers

---

**Made with ❤️ for the SAP Developer Community**

*Empowering SAP developers with AI-assisted development in Kiro IDE*
