# Getting Started with SAP Skills Power

This guide helps you start using the SAP Skills Power effectively in your SAP development projects.

## Quick Start

The SAP Skills Power automatically activates based on your project context. When you work on SAP-related files, Kiro will intelligently load relevant skills.

### Automatic Activation

Skills activate when you work with:

- **CAP Projects**: `.cds` files, `package.json` with `@sap/cds`
- **UI5 Projects**: `manifest.json`, UI5 controllers/views
- **ABAP Files**: `.abap`, `.cds` (ABAP CDS)
- **HANA Projects**: `.hdbcds`, `.hdbview`, `.hdbprocedure`
- **BTP Configurations**: `mta.yaml`, `xs-security.json`

### Manual Activation

You can also explicitly reference skills in your prompts:

```
"Using SAP CAP best practices, create a service..."
"Following SAPUI5 patterns, implement a list report..."
"Apply ABAP clean code principles to refactor..."
```

## Common Workflows

### 1. Building a New CAP Application

**Skills Used**: `sap-cap-capire`, `sap-btp-developer-guide`

```bash
# Kiro will guide you through:
# 1. Project initialization
# 2. CDS model creation
# 3. Service implementation
# 4. Authentication setup
# 5. Deployment to BTP
```

**What to Ask**:
- "Create a CAP service for managing products"
- "Add authentication to my CAP service"
- "Deploy this CAP app to BTP Cloud Foundry"

### 2. Developing a Fiori Application

**Skills Used**: `sapui5`, `sap-fiori-tools`, `sapui5-linter`

```bash
# Kiro will help with:
# 1. Fiori Elements template selection
# 2. UI5 component structure
# 3. OData model binding
# 4. Responsive design
# 5. Accessibility compliance
```

**What to Ask**:
- "Create a Fiori List Report for my OData service"
- "Add a custom action to my Fiori Elements app"
- "Make this UI5 view responsive"

### 3. Modernizing ABAP Code

**Skills Used**: `sap-abap`, `sap-abap-cds`, ABAP Analyzer MCP

```bash
# Kiro will assist with:
# 1. Analyzing legacy ABAP code
# 2. Creating CDS views
# 3. Implementing RAP (RESTful ABAP Programming)
# 4. Applying Clean ABAP principles
# 5. Generating modern equivalents
```

**What to Ask**:
- "Analyze this ABAP code and suggest improvements"
- "Create a CDS view for this database table"
- "Convert this ABAP function to a RAP service"

### 4. Setting Up BTP Services

**Skills Used**: `sap-btp-connectivity`, `sap-btp-service-manager`, `sap-btp-integration-suite`

```bash
# Kiro will guide:
# 1. Service instance creation
# 2. Destination configuration
# 3. Cloud Connector setup
# 4. Integration flow design
# 5. Security configuration
```

**What to Ask**:
- "Configure a destination to my on-premise system"
- "Set up Cloud Connector for SAP ECC"
- "Create an integration flow for order processing"

### 5. Data Modeling in HANA

**Skills Used**: `sap-sqlscript`, `sap-hana-cli`, `sap-datasphere`

```bash
# Kiro will help with:
# 1. CDS view creation
# 2. Calculation views
# 3. SQLScript procedures
# 4. Performance optimization
# 5. Data federation
```

**What to Ask**:
- "Create a calculation view for sales analytics"
- "Optimize this SQLScript procedure"
- "Design a data model in Datasphere"

## Project Types

### CAP Node.js Project

**Typical Structure**:
```
my-cap-app/
├── db/
│   └── schema.cds          # Data models
├── srv/
│   ├── service.cds         # Service definitions
│   └── service.js          # Service implementation
├── app/
│   └── fiori-app/          # UI5 applications
├── package.json
└── mta.yaml                # BTP deployment
```

**Key Skills**: `sap-cap-capire`, `sap-btp-developer-guide`

### UI5 Application

**Typical Structure**:
```
my-ui5-app/
├── webapp/
│   ├── manifest.json       # App descriptor
│   ├── Component.js        # Component controller
│   ├── view/               # XML views
│   ├── controller/         # Controllers
│   └── model/              # Models
├── ui5.yaml
└── package.json
```

**Key Skills**: `sapui5`, `sap-fiori-tools`, `sapui5-linter`

### ABAP Project

**Typical Structure**:
```
abap-project/
├── src/
│   ├── zcl_*.clas.abap     # Classes
│   ├── z*_cds.ddls.asddls  # CDS views
│   └── z*.fugr.xml         # Function groups
└── package.devc.xml
```

**Key Skills**: `sap-abap`, `sap-abap-cds`

## Best Practices

### 1. Let Context Drive Skills

Don't explicitly request skills unless needed. Kiro automatically loads relevant skills based on:
- File types you're editing
- Project structure
- Dependencies in package.json
- Keywords in your prompts

### 2. Be Specific in Requests

**Good**:
- "Create a CAP service with draft support for managing orders"
- "Implement a Fiori List Report with custom filters"
- "Refactor this ABAP code following Clean ABAP principles"

**Less Effective**:
- "Create a service"
- "Make a UI"
- "Fix this code"

### 3. Leverage Multiple Skills

Complex tasks often benefit from multiple skills:

```
"Create a full-stack SAP application:
- CAP backend with OData V4
- Fiori Elements frontend
- Deploy to BTP Cloud Foundry
- Add authentication and authorization"
```

This will activate: `sap-cap-capire`, `sapui5`, `sap-fiori-tools`, `sap-btp-developer-guide`

### 4. Use Quality Tools

Always run quality checks:

```
"Run UI5 linter on my application"
"Review this code for SAP best practices"
"Check API documentation style"
```

### 5. Follow SAP Standards

The power enforces SAP standards automatically:
- API naming conventions
- Code structure patterns
- Security best practices
- Performance guidelines
- Accessibility requirements

## Troubleshooting

### Skills Not Activating

**Check**:
1. File extensions match SAP project types
2. Project has SAP dependencies
3. Use SAP-specific keywords in prompts

### Conflicting Recommendations

**Solution**:
- Specify which skill to prioritize
- Example: "Using Fiori Elements patterns, not custom UI5..."

### Need More Context

**Provide**:
- Project type (CAP, UI5, ABAP)
- Target platform (BTP, on-premise)
- SAP products in use
- Specific requirements

## Next Steps

1. **Explore Specific Skills**: Check other steering files for detailed guidance
2. **Review Examples**: See `examples.md` for common patterns
3. **Check Best Practices**: Read `best-practices.md` for SAP standards
4. **Integration Guide**: See `integration.md` for combining with MCP servers

## Resources

- [SAP Skills Repository](https://github.com/secondsky/sap-skills)
- [SAP CAP Documentation](https://cap.cloud.sap/docs/)
- [SAPUI5 Documentation](https://ui5.sap.com/)
- [SAP BTP Documentation](https://help.sap.com/docs/btp)
- [Clean ABAP Guide](https://github.com/SAP/styleguides)
