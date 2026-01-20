---
name: sap-power
displayName: SAP Power
description: Production-ready SAP development knowledge covering BTP, CAP, Fiori, ABAP, HANA, and Analytics Cloud
icon: icon.png
keywords: ["sap", "btp", "cap", "fiori", "ui5", "sapui5", "abap", "hana", "cds", "odata", "cloud", "analytics", "datasphere", "integration", "sac"]
filePatterns:
  - "**/*.cds"
  - "**/manifest.json"
  - "**/*.abap"
  - "**/mta.yaml"
  - "**/xs-security.json"
  - "**/*.hdbcds"
  - "**/*.hdbview"
  - "**/*.hdbprocedure"
  - "**/ui5.yaml"
  - "**/package.json"
---

# SAP Power

Production-ready SAP development knowledge and best practices for building modern SAP applications. This power provides comprehensive guidance across the entire SAP ecosystem including BTP, CAP, Fiori, ABAP, HANA, and Analytics Cloud.

## Overview

The SAP Power brings 35+ specialized SAP development skills to Kiro, enabling context-aware AI assistance for:

- **SAP BTP Platform** (14 skills): Cloud services, connectivity, integration, and platform operations
- **UI Development** (4 skills): SAPUI5, Fiori Tools, and modern UI patterns
- **Data & Analytics** (5 skills): Datasphere, Analytics Cloud, and data intelligence
- **Core Technologies** (6 skills): ABAP, CAP, HANA, and AI/ML development
- **Tooling & Development** (4 skills): CLI tools, linters, and quality assurance

## Key Capabilities

### 🔧 Development Tools
- **sapui5-linter**: Static code analysis for UI5 applications
- **sap-hana-cli**: Database operations and HANA development
- **sap-api-style**: API documentation following SAP standards
- **skill-review**: 14-phase quality audit process

### ☁️ SAP BTP Platform
- **sap-btp-developer-guide**: Comprehensive BTP development patterns
- **sap-btp-connectivity**: Connect cloud apps to on-premise systems
- **sap-btp-integration-suite**: Build integration flows and APIs
- **sap-btp-cloud-logging**: Centralized logging and monitoring
- **sap-btp-job-scheduling**: Schedule and manage background jobs
- **sap-btp-service-manager**: Manage service instances and bindings

### 🎨 UI Development
- **sapui5**: Complete SAPUI5 framework development
- **sap-fiori-tools**: Fiori application development and deployment
- **sapui5-cli**: Command-line tools for UI5 projects
- Modern responsive design patterns

### 📊 Data & Analytics
- **sap-datasphere**: Data modeling and management
- **sap-sac-scripting**: Analytics Cloud scripting API
- **sap-sac-planning**: Planning applications and models
- **sap-sac-custom-widget**: Custom widget development
- **sap-hana-cloud-data-intelligence**: Data pipeline orchestration

### ⚙️ Core Technologies
- **sap-cap-capire**: Cloud Application Programming Model
- **sap-abap**: ABAP development patterns and best practices
- **sap-abap-cds**: Core Data Services views
- **sap-sqlscript**: HANA SQLScript development
- **sap-ai-core**: Machine learning model deployment
- **sap-hana-ml**: HANA ML library integration

## When to Use This Power

Use the SAP Power when:

- Building SAP BTP applications with CAP or Fiori
- Modernizing legacy ABAP applications
- Developing SAPUI5 or Fiori Elements applications
- Integrating SAP systems with cloud services
- Creating data models in Datasphere or HANA
- Building Analytics Cloud dashboards and widgets
- Implementing SAP best practices and patterns
- Setting up BTP services and connectivity

## Integration with Other Powers

This power complements:

- **SAP CAP MCP Server** (`@cap-js/mcp-server`): Official CAP tooling
- **SAP UI5 MCP Server** (`@ui5/mcp-server`): Official UI5 tooling
- **ABAP Analyzer**: Custom ABAP code analysis

Together, these provide a complete SAP development environment in Kiro.

## Usage Examples

### Example 1: Building a CAP Application

```typescript
// Kiro uses sap-cap-capire skill to guide CAP development
// Automatically provides:
// - CDS model best practices
// - Service implementation patterns
// - Authentication and authorization
// - OData V4 service patterns
```

### Example 2: Creating Fiori UI

```typescript
// Kiro uses sapui5 and sap-fiori-tools skills
// Provides:
// - Fiori Elements templates
// - UI5 control recommendations
// - Responsive design patterns
// - Accessibility compliance
```

### Example 3: ABAP Modernization

```abap
" Kiro uses sap-abap and sap-abap-cds skills
" Guides:
" - Modern ABAP syntax
" - CDS view creation
" - RESTful ABAP Programming (RAP)
" - Clean ABAP principles
```

### Example 4: BTP Service Integration

```javascript
// Kiro uses sap-btp-connectivity and sap-btp-integration-suite
// Helps with:
// - Cloud Connector setup
// - Destination configuration
// - API Management integration
// - Event-driven architectures
```

## Skill Categories

### Tooling & Development (4 skills)
- `skill-review`: Quality assurance audit
- `sap-api-style`: API documentation standards
- `sap-hana-cli`: HANA CLI operations
- `sapui5-linter`: UI5 code analysis

### SAP BTP Platform (14 skills)
- `sap-btp-best-practices`: Development patterns
- `sap-btp-build-work-zone-advanced`: Work Zone development
- `sap-btp-business-application-studio`: BAS development
- `sap-btp-cias`: Identity and access management
- `sap-btp-cloud-logging`: Logging service
- `sap-btp-cloud-platform`: Core platform services
- `sap-btp-cloud-transport-management`: Transport management
- `sap-btp-connectivity`: Connectivity service
- `sap-btp-developer-guide`: Comprehensive guide
- `sap-btp-integration-suite`: Integration development
- `sap-btp-intelligent-situation-automation`: Automation workflows
- `sap-btp-job-scheduling`: Job scheduling
- `sap-btp-master-data-integration`: Master data sync
- `sap-btp-service-manager`: Service management

### UI Development (4 skills)
- `sap-fiori-tools`: Fiori development
- `sapui5`: UI5 framework
- `sapui5-cli`: UI5 CLI tools
- `sapui5-linter`: UI5 linting

### Data & Analytics (5 skills)
- `sap-datasphere`: Data modeling
- `sap-hana-cloud-data-intelligence`: Data pipelines
- `sap-sac-custom-widget`: Custom widgets
- `sap-sac-planning`: Planning apps
- `sap-sac-scripting`: Scripting API

### Core Technologies (6 skills)
- `sap-abap`: ABAP development
- `sap-abap-cds`: CDS views
- `sap-ai-core`: AI/ML development
- `sap-cap-capire`: CAP framework
- `sap-cloud-sdk-ai`: Cloud SDK for AI
- `sap-hana-ml`: HANA ML library
- `sap-sqlscript`: SQLScript development

## Best Practices

The SAP Power enforces:

1. **SAP Standard Compliance**: Follow official SAP guidelines and patterns
2. **Clean Code**: Modern, maintainable code practices
3. **Security First**: Authentication, authorization, and data protection
4. **Performance**: Optimized queries and efficient data handling
5. **Accessibility**: WCAG-compliant UI development
6. **Documentation**: Comprehensive API and code documentation

## Quality Standards

All skills in this power maintain:

- ✅ Production-ready code examples
- ✅ SAP-validated patterns
- ✅ Up-to-date with latest releases
- ✅ Comprehensive error handling
- ✅ Security best practices
- ✅ Performance optimization

## Source

Based on the [SAP Skills for Claude Code](https://github.com/secondsky/sap-skills) repository by secondsky.

Licensed under GNU General Public License v3.0.

## Version

Power Version: 1.0.0
Based on: sap-skills v2.1.0 (2025-12-27)
Last Updated: 2026-01-14
