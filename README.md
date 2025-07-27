# CodeAgent - Central Configuration Management Platform

## Project Overview

CodeAgent is a centralized web application built with ASP.NET Blazor for managing service configurations across multiple deployment stages. The application provides a unified interface for teams to efficiently and securely manage application settings, secrets, and configuration values that are distributed across Azure App Configuration and Azure Key Vault instances.

## Problem Statement

Managing configuration settings across multiple services and deployment stages presents several challenges:

- **Lack of Visibility**: With all service settings distributed across Azure App Configuration instances per stage, it's difficult to maintain an overview of configuration changes, versions, and differences between stages
- **Manual Management**: Direct manipulation of Azure resources is time-consuming and error-prone
- **No Change Tracking**: Limited ability to track configuration changes, rollbacks, and audit trails
- **Stage Comparison**: Difficult to compare configuration differences between development, QA, and production environments
- **Security Concerns**: Direct access to Azure resources increases security risks and requires broader permissions

## Solution Goals

### Primary Objectives
- **Centralized Management**: Single web interface for managing all service configurations across stages
- **Enhanced Visibility**: Clear overview of settings, versions, and stage differences
- **Audit Trail**: Complete change history and version tracking
- **Secure Operations**: Eliminate need for direct Azure resource access
- **Efficiency**: Streamline configuration deployment and management workflows

### Key Features
- Web-based configuration management interface
- Multi-stage environment support (dev, qa, prod-emea, prod-us)
- Integration with Azure App Configuration and Azure Key Vault
- Configuration versioning and change tracking
- Stage comparison and difference visualization
- Secure deployment workflows
- Role-based access control

## Architecture Overview

### Technology Stack
- **Frontend**: ASP.NET Blazor Server
- **Backend**: ASP.NET Core Web API
- **Database**: SQL Server (central source of truth)
- **Container Platform**: Azure Kubernetes Service (AKS)
- **Configuration Storage**: Azure App Configuration
- **Secret Management**: Azure Key Vault
- **Authentication**: Azure Active Directory integration

### Data Flow
1. **Central Database**: Acts as the single source of truth for all configuration data
2. **Web Interface**: Provides user-friendly management capabilities
3. **Azure Integration**: Synchronizes changes to target Azure App Configuration and Key Vault instances
4. **Service Consumption**: Containerized services consume settings via existing Azure App Configuration integration

### Current Integration Pattern
Services currently integrate with Azure App Configuration using the following pattern:

```csharp
public static void AddAzureAppConfiguration(this ConfigurationManager configurationManager, params string[] keyFilter)
{
    configurationManager.AddAzureAppConfiguration(options =>
    {
        options.Connect(configurationManager["PDM_CONFIG_CONNECTION"]); 
        foreach (string key in keyFilter)
        {
            options.Select(KeyFilter.Any, key);
        }
        options.ConfigureKeyVault(kv =>
        {
            kv.SetCredential(new DefaultAzureCredential());
        });
    });
}
```

## Deployment Stages

The application manages configurations across four distinct stages:

| Stage | Description | Environment |
|-------|-------------|-------------|
| **dev** | Development environment | Development team testing |
| **qa** | Quality assurance environment | Pre-production testing |
| **prod-emea** | Production Europe/Middle East/Africa | Production workloads (EMEA region) |
| **prod-us** | Production United States | Production workloads (US region) |

## Security Model

- **Azure Managed Identity**: Leverages `DefaultAzureCredential` for secure Azure resource access
- **Centralized Secrets**: All sensitive values stored in Azure Key Vault with references in App Configuration
- **No Direct Access**: Teams interact only with the web application, eliminating direct Azure resource manipulation
- **Audit Logging**: Complete audit trail of all configuration changes and deployments

## Development Status

🚧 **Project Status**: Planning and Architecture Phase

The project is currently in the initial planning phase. Detailed application specifications, database schema design, and implementation roadmap will be defined in subsequent development phases.

## Next Steps

1. **Detailed Requirements Gathering**: Define specific UI/UX requirements and workflows
2. **Database Schema Design**: Design central configuration database structure
3. **API Specification**: Define REST API endpoints for configuration management
4. **Security Implementation**: Design authentication and authorization mechanisms
5. **Azure Integration**: Implement Azure App Configuration and Key Vault synchronization
6. **Containerization**: Create Docker containers and Kubernetes manifests for AKS deployment

## Contributing

This project is currently in the planning phase. Contribution guidelines and development workflows will be established as the project progresses.

## License

[License information to be determined]

---

*Last Updated: July 2025*
