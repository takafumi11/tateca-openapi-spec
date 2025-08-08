# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Tateca OpenAPI specification repository that defines the API for a financial management system. The system allows users to manage groups, transactions, and settlements with support for multi-currency operations.

## Repository Structure

The repository now follows modular OpenAPI best practices with separated files for maintainability:

```
tateca-openapi-spec/
├── .redocly.yaml                    # Redocly configuration
├── .github/workflows/               # CI/CD pipelines
│   └── api-validation.yml
├── openapi/                         # OpenAPI specification files
│   ├── openapi.yaml                 # Main OpenAPI file (references only)
│   ├── components/                  # Reusable components
│   │   ├── info.yaml               # API info section
│   │   ├── security.yaml           # Security schemes
│   │   ├── parameters/             # Parameter definitions
│   │   │   └── index.yaml
│   │   ├── responses/              # Response definitions
│   │   │   └── index.yaml
│   │   └── schemas/                # Data models
│   │       ├── index.yaml          # Schema references
│   │       └── *.yaml              # Individual schema files
│   ├── paths/                      # API endpoint definitions
│   │   └── *.yaml                  # Individual path files
│   └── examples/                   # Request/response examples
├── docs/                           # Generated documentation
├── dist/                           # Built/bundled files
├── scripts/                        # Utility scripts
│   └── split-spec.js               # Script to split monolithic specs
├── package.json                    # Node.js dependencies and scripts
└── tateca-api.yaml                 # Original monolithic spec (legacy)
```

## API Architecture

The Tateca API is organized into several main functional areas:

### Authentication
- Supports dual authentication: Firebase JWT (iOS clients) and API Key (Lambda systems)
- User management with UID-based identification

### Core Entities
- **Users**: Account management with currency preferences
- **Groups**: Multi-user financial groups with invitation system
- **Transactions**: Expense tracking with participant splitting
- **Settlements**: Automatic debt calculation and resolution
- **Exchange Rates**: Multi-currency support with date-based rates

### Key Features
- Group-based expense sharing
- Automatic settlement calculations
- Multi-currency transaction support
- Transaction history and participant tracking

## Development Commands

The repository now includes npm scripts for comprehensive OpenAPI workflow management:

### Core Commands
- `npm run lint` - Validate OpenAPI specification with Redocly
- `npm run bundle` - Bundle modular files into single specification
- `npm run build-docs` - Generate HTML documentation
- `npm run preview` - Preview documentation locally
- `npm run validate` - Validate with JSON output for CI/CD
- `npm run dev` - Full development workflow (bundle + docs + serve)
- `npm run test` - Run all validation and bundling

### Utility Commands  
- `npm run stats` - Get specification statistics
- `npm run serve` - Serve documentation locally on port 8080
- `node scripts/split-spec.js <file>` - Split monolithic spec into modular structure

### CI/CD Integration
The repository includes GitHub Actions workflow (`.github/workflows/api-validation.yml`) that automatically:
- Validates OpenAPI specifications on PR/push
- Bundles specifications for distribution
- Generates and deploys documentation to GitHub Pages
- Provides artifact downloads for bundled specs

## Working with the API Specification

### Modular File Structure
- **Main spec**: `openapi/openapi.yaml` contains references to modular files
- **Schemas**: Add new data models in `openapi/components/schemas/`
- **Paths**: Add new endpoints in `openapi/paths/`
- **Parameters**: Add reusable parameters in `openapi/components/parameters/`
- **Examples**: Add request/response examples in `openapi/examples/`

### Development Workflow
1. Make changes to modular files in `openapi/` directory
2. Run `npm run lint` to validate changes
3. Run `npm run bundle` to create distribution file
4. Run `npm run build-docs` to update documentation
5. Commit both source files and generated artifacts

### Standards and Guidelines
- Use OpenAPI 3.0 standards when making modifications
- Maintain consistency with existing schema patterns and response structures
- Ensure all endpoints include proper error responses (400, 404, 500)
- Follow the established naming conventions for operations and schemas
- Use `$ref` for reusable components to maintain DRY principles

## APIOps Implementation (GitOps for APIs)

This repository implements APIOps methodology, applying GitOps principles to API management:

### Core APIOps Principles
1. **Git as Source of Truth**: All API definitions, configurations, and policies are stored in Git
2. **Declarative Configuration**: API infrastructure defined through YAML/JSON files
3. **Automated Workflows**: CI/CD pipelines automate validation, testing, and deployment
4. **Audit Trail**: Complete version control history of all API changes

### APIOps Workflow
```mermaid
graph LR
    A[Developer] --> B[Clone Repo]
    B --> C[Create Branch]
    C --> D[Modify OpenAPI]
    D --> E[Create PR]
    E --> F[Automated Validation]
    F --> G[Review & Merge]
    G --> H[Auto Deploy]
    H --> I[Update Documentation]
```

### Implementation Features
- **Change Detection**: Automated detection of API changes through Git diffs
- **Quality Gates**: Lint validation and breaking change detection in CI/CD
- **Environment Consistency**: Same specifications deployed across dev/staging/production
- **Rollback Capability**: Quick reversion to previous stable API versions
- **Security Integration**: Automated security scanning and policy enforcement

### Benefits in Practice
- **Automated Change Tracking**: Complete history of API evolution
- **Quality Assurance**: Enforced standards through continuous validation
- **Faster Deployments**: Streamlined updates via automated pipelines  
- **Risk Reduction**: Safe rollback to previous stable versions
- **Developer Self-Service**: Teams can manage API lifecycle independently

### APIOps Tools Integration
- **Redocly CLI**: Specification validation and bundling
- **GitHub Actions**: Automated workflows and deployments
- **GitHub Pages**: Automated documentation publishing
- **Git Hooks**: Pre-commit validation and formatting