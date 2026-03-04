# Bicep Cheatsheet

![Bicep Logo](https://learn.microsoft.com/en-us/azure/templates/media/template-deployment-overview/bicep-overview.svg)

---

## Table of Contents
1. [What is Bicep?](#1-what-is-bicep)
2. [Installation & Setup](#2-installation--setup)
3. [Bicep Syntax Basics](#3-bicep-syntax-basics)
4. [Parameters](#4-parameters)
5. [Variables](#5-variables)
6. [Resources](#6-resources)
7. [Outputs](#7-outputs)
8. [Functions](#8-functions)
9. [Modules](#9-modules)
10. [Deployment](#10-deployment)
11. [Best Practices](#11-best-practices)
12. [Troubleshooting](#12-troubleshooting)

---

## 1. What is Bicep?

Bicep is a domain-specific language (DSL) for declaring Azure resources. It provides a simpler, cleaner syntax compared to Azure Resource Manager (ARM) templates while maintaining full compatibility with ARM templates.

**Key Features:**
- Simpler syntax than JSON ARM templates
- Type safety and validation
- Code reusability through modules
- Seamless ARM template integration
- Cleaner variable and parameter management
- Built-in functions
- Symbolic references
- No nested JSON syntax

---

## 2. Installation & Setup

### Install Bicep CLI

```bash
# macOS with Homebrew
brew install bicep

# Ubuntu/Debian
curl -Lo bicep https://github.com/Azure/bicep/releases/latest/download/bicep-linux-x64
chmod +x ./bicep
sudo mv ./bicep /usr/local/bin/bicep

# Windows (PowerShell)
# Using Chocolatey
choco install bicep

# Using WinGet
winget install Microsoft.Bicep

# Using direct download
curl -Lo bicep.exe https://github.com/Azure/bicep/releases/latest/download/bicep-windows-x64.exe

# Verify installation
bicep --version
```

### Install Azure CLI

```bash
# macOS
brew install azure-cli

# Ubuntu/Debian
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash

# Windows
winget install Microsoft.AzureCLI

# Verify
az --version

# Login to Azure
az login

# Set default subscription
az account set --subscription "subscription-name"
```

### Project Structure

```
bicep-project/
├── main.bicep                  # Main template file
├── main.bicepparam             # Parameter file
├── parameters.json             # Alternative parameters
├── modules/
│   ├── storage.bicep           # Storage module
│   ├── network.bicep           # Network module
│   ├── compute.bicep           # Compute module
│   └── database.bicep          # Database module
├── variables.bicepvars         # Shared variables
├── README.md
└── .gitignore
```

---

## 3. Bicep Syntax Basics

### Basic Structure

```bicep
// Parameters - input values
param location string = 'eastus'
param environment string
param vmCount int = 3

// Variables - computed values
var uniqueName = 'app${uniqueString(resourceGroup().id)}'
var resourceTags = {
  environment: environment
  createdDate: utcNow('u')
}

// Resources - Azure resources
resource storageAccount 'Microsoft.Storage/storageAccounts@2021-06-01' = {
  name: uniqueName
  location: location
  kind: 'StorageV2'
  sku: {
    name: 'Standard_LRS'
  }
  properties: {
    accessTier: 'Hot'
  }
  tags: resourceTags
}

// Outputs - return values
output storageId string = storageAccount.id
output storageName string = storageAccount.name
```

### Comments

```bicep
// Single-line comment

/*
Multi-line comment
Can span multiple lines
*/

// Use comments to document parameters
param environment string = 'dev' // Environment name: dev, test, prod
```

### Data Types

```bicep
// String
param location string = 'eastus'
param name string

// Integer
param vmCount int = 2
param replicas int

// Boolean
param enableDiagnostics bool = true
param publicAccess bool

// Array
param subnets array = []
param tags array = ['env', 'app']

// Object
param configuration object = {
  cpu: 2
  memory: 4
}

// Array of objects
param environments array = [
  {
    name: 'dev'
    vmSize: 'Standard_B2s'
  }
  {
    name: 'prod'
    vmSize: 'Standard_D2s_v3'
  }
]
```

---

## 4. Parameters

### Parameter Declaration

```bicep
// Simple parameter
param location string

// Parameter with default value
param environment string = 'dev'

// Parameter with metadata
param vmSize string = 'Standard_B2s' = {
  minLength: 1
  maxLength: 50
  metadata: {
    description: 'Virtual machine size'
  }
}

// Required parameter (no default)
@minLength(1)
@maxLength(50)
param resourceName string

// Allowed values
@allowed([
  'eastus'
  'westus'
  'eastus2'
])
param location string

// Numeric constraints
@minValue(1)
@maxValue(10)
param instanceCount int

// String constraints
@minLength(3)
@maxLength(24)
param storageAccountName string
```

### Parameter Decorators

```bicep
// Description decorator
@description('Environment name: dev, test, or prod')
param environment string

// Min/Max value decorators
@minValue(1)
@maxValue(100)
param capacity int = 10

// Allowed values
@allowed([
  'dev'
  'test'
  'prod'
])
param environment string

// Min/Max length for strings
@minLength(3)
@maxLength(24)
param name string

// Secure parameter (for passwords/secrets)
@secure()
param password string

// Export parameter metadata
@metadata({
  description: 'Location for resources'
  example: 'eastus'
})
param location string
```

### Parameter Files (.bicepparam)

```bicep
// main.bicepparam
using './main.bicep'

param location = 'eastus'
param environment = 'prod'
param vmCount = 5
param vmSize = 'Standard_D2s_v3'
param resourceTags = {
  environment: 'production'
  costCenter: '12345'
}
```

---

## 5. Variables

### Variable Declaration

```bicep
// Simple variable
var location = 'eastus'

// String interpolation
var uniqueName = 'app${uniqueString(resourceGroup().id)}'

// Computed variable
var diskSizeGb = environment == 'prod' ? 128 : 32

// Object variable
var resourceTags = {
  environment: environment
  app: appName
  createdBy: 'bicep'
  createdDate: utcNow('u')
}

// Array variable
var subnetConfigs = [
  {
    name: 'frontend'
    addressPrefix: '10.0.1.0/24'
  }
  {
    name: 'backend'
    addressPrefix: '10.0.2.0/24'
  }
]

// Conditional variable
var vmSize = environment == 'prod' ? 'Standard_D2s_v3' : 'Standard_B2s'

// Variable using function
var resourceId = resourceGroup().id
var currentDate = utcNow('u')
var storageName = 'st${uniqueString(resourceGroup().id)}'
```

### Using Variables

```bicep
param environment string = 'dev'
param location string = 'eastus'
param appName string = 'myapp'

var uniqueName = '${appName}${uniqueString(resourceGroup().id)}'
var resourceTags = {
  environment: environment
  app: appName
}

resource storageAccount 'Microsoft.Storage/storageAccounts@2021-06-01' = {
  name: uniqueName
  location: location
  kind: 'StorageV2'
  sku: {
    name: 'Standard_LRS'
  }
  tags: resourceTags
}
```

---

## 6. Resources

### Basic Resource Syntax

```bicep
// Resource declaration
resource <logicalName> '<type>@<apiVersion>' = {
  name: '<resourceName>'
  location: '<location>'
  kind: '<kind>'
  sku: {
    name: '<skuName>'
  }
  properties: {
    // Resource-specific properties
  }
  tags: {
    // Tags
  }
}

// Reference another resource
resource vnet 'Microsoft.Network/virtualNetworks@2021-02-01' = {
  name: 'myVnet'
  location: location
  properties: {
    addressSpace: {
      addressPrefixes: [
        '10.0.0.0/16'
      ]
    }
  }
}

resource subnet 'Microsoft.Network/virtualNetworks/subnets@2021-02-01' = {
  parent: vnet  // Reference parent resource
  name: 'mySubnet'
  properties: {
    addressPrefix: '10.0.0.0/24'
  }
}
```

### Common Resource Examples

```bicep
// Storage Account
resource storageAccount 'Microsoft.Storage/storageAccounts@2021-06-01' = {
  name: storageName
  location: location
  kind: 'StorageV2'
  sku: {
    name: 'Standard_LRS'
  }
  properties: {
    accessTier: 'Hot'
    minimumTlsVersion: 'TLS1_2'
  }
  tags: tags
}

// Virtual Network
resource vnet 'Microsoft.Network/virtualNetworks@2021-02-01' = {
  name: vnetName
  location: location
  properties: {
    addressSpace: {
      addressPrefixes: [
        '10.0.0.0/16'
      ]
    }
  }
  tags: tags
}

// Subnet
resource subnet 'Microsoft.Network/virtualNetworks/subnets@2021-02-01' = {
  parent: vnet
  name: 'default'
  properties: {
    addressPrefix: '10.0.0.0/24'
  }
}

// Network Interface
resource nic 'Microsoft.Network/networkInterfaces@2021-02-01' = {
  name: nicName
  location: location
  properties: {
    ipConfigurations: [
      {
        name: 'ipconfig1'
        properties: {
          subnet: {
            id: subnet.id
          }
          privateIPAllocationMethod: 'Dynamic'
        }
      }
    ]
  }
  tags: tags
}

// Virtual Machine
resource vm 'Microsoft.Compute/virtualMachines@2020-12-01' = {
  name: vmName
  location: location
  properties: {
    hardwareProfile: {
      vmSize: vmSize
    }
    osProfile: {
      computerName: vmName
      adminUsername: adminUsername
      adminPassword: adminPassword
    }
    storageProfile: {
      imageReference: {
        publisher: 'MicrosoftWindowsServer'
        offer: 'WindowsServer'
        sku: '2019-Datacenter'
        version: 'latest'
      }
    }
    networkProfile: {
      networkInterfaces: [
        {
          id: nic.id
        }
      ]
    }
  }
  tags: tags
}

// Key Vault
resource keyVault 'Microsoft.KeyVault/vaults@2021-06-01-preview' = {
  name: kvName
  location: location
  properties: {
    tenantId: subscription().tenantId
    sku: {
      family: 'A'
      name: 'standard'
    }
    accessPolicies: []
    enableSoftDelete: true
  }
  tags: tags
}

// App Service
resource appServicePlan 'Microsoft.Web/serverfarms@2021-02-01' = {
  name: planName
  location: location
  sku: {
    name: 'B1'
    capacity: 1
  }
  kind: 'linux'
  properties: {
    reserved: true
  }
}

resource webApp 'Microsoft.Web/sites@2021-02-01' = {
  name: appName
  location: location
  kind: 'app,linux'
  properties: {
    serverFarmId: appServicePlan.id
  }
  tags: tags
}
```

### Symbolic References

```bicep
// Reference resource properties directly
resource storageAccount 'Microsoft.Storage/storageAccounts@2021-06-01' = {
  name: storageName
  location: location
  // ...
}

// Access properties
output storageName string = storageAccount.name
output storageId string = storageAccount.id
output primaryEndpoint string = storageAccount.properties.primaryEndpoints.blob

// Use in other resources
resource blobContainer 'Microsoft.Storage/storageAccounts/blobServices/containers@2021-06-01' = {
  parent: storageAccount  // Reference by logical name
  name: 'mycontainer'
  properties: {}
}
```

---

## 7. Outputs

### Output Declaration

```bicep
// Simple output
output location string = location

// Output resource property
output storageId string = storageAccount.id
output storageName string = storageAccount.name

// Output with computed value
output connectionString string = 'DefaultEndpointProtocol=https;AccountName=${storageAccount.name};AccountKey=${listKeys(storageAccount.id, storageAccount.apiVersion).keys[0].value};EndpointSuffix=core.windows.net'

// Output object
output resourceInfo object = {
  name: storageAccount.name
  id: storageAccount.id
  location: location
  endpoint: storageAccount.properties.primaryEndpoints.blob
}

// Output array
output resourceIds array = [
  storageAccount.id
  vnet.id
  subnet.id
]

// Secure output (for sensitive data)
@secure()
output connectionString string = connectionStringValue

// Output with metadata
output storageEndpoint string = storageAccount.properties.primaryEndpoints.blob
```

### Output Examples

```bicep
// Complete example
param location string = 'eastus'
param appName string

var uniqueName = '${appName}${uniqueString(resourceGroup().id)}'

resource storageAccount 'Microsoft.Storage/storageAccounts@2021-06-01' = {
  name: uniqueName
  location: location
  kind: 'StorageV2'
  sku: {
    name: 'Standard_LRS'
  }
  properties: {
    accessTier: 'Hot'
  }
}

resource vnet 'Microsoft.Network/virtualNetworks@2021-02-01' = {
  name: '${appName}vnet'
  location: location
  properties: {
    addressSpace: {
      addressPrefixes: [
        '10.0.0.0/16'
      ]
    }
  }
}

// Multiple outputs
output storageAccountId string = storageAccount.id
output storageAccountName string = storageAccount.name
output vnetId string = vnet.id
output deploymentInfo object = {
  appName: appName
  location: location
  storageAccount: storageAccount.name
  vnet: vnet.name
  timestamp: utcNow('u')
}
```

---

## 8. Functions

### Built-in Functions

```bicep
// String functions
output lowercase string = toLower('HELLO')
output uppercase string = toUpper('hello')
output substring string = substring('hello', 0, 3)
output indexOf int = indexOf('hello', 'l')
output replace string = replace('hello world', 'world', 'bicep')
output split array = split('a,b,c', ',')
output join string = join(['a', 'b', 'c'], ',')
output trim string = trim('  hello  ')

// Numeric functions
output max int = max(1, 5, 3)
output min int = min(1, 5, 3)

// Array functions
output length int = length(['a', 'b', 'c'])
output contains bool = contains(['a', 'b', 'c'], 'b')
output intersection array = intersection(['a', 'b'], ['b', 'c'])
output union array = union(['a', 'b'], ['c', 'd'])

// Object functions
output keys array = keys({
  name: 'John'
  age: 30
})
output values array = values({
  name: 'John'
  age: 30
})

// Unique string based on resource group
var uniqueName = '${appName}${uniqueString(resourceGroup().id)}'

// Current resource group info
var rgId = resourceGroup().id
var rgName = resourceGroup().name
var rgLocation = resourceGroup().location

// Subscription info
var subscriptionId = subscription().subscriptionId
var tenantId = subscription().tenantId

// Deployment info
var deploymentTime = utcNow('u')
var deploymentDate = utcNow('d')

// Condition function
var vmSize = environment == 'prod' ? 'Standard_D2s_v3' : 'Standard_B2s'

// Concat
var fullName = concat(firstName, ' ', lastName)

// String interpolation
var message = 'Hello, ${name}!'

// Base64 encoding
var encoded = base64('hello')
var decoded = base64ToJson('...')

// URI functions
output blobUri string = '${storageAccount.properties.primaryEndpoints.blob}container/blob.txt'

// List keys
output primaryKey string = listKeys(storageAccount.id, storageAccount.apiVersion).keys[0].value
```

### Function Examples

```bicep
param appName string
param environment string
param location string = 'eastus'

// String functions
var resourcePrefix = toLower('${appName}-${environment}')
var uniqueName = '${resourcePrefix}${uniqueString(resourceGroup().id)}'

// Conditional
var vmSize = environment == 'prod' ? 'Standard_D2s_v3' : 'Standard_B2s'
var replicaCount = environment == 'prod' ? 3 : 1

// Array and object functions
var locations = [location]
var tags = {
  environment: environment
  createdDate: utcNow('u')
}

// Use functions in resources
resource storageAccount 'Microsoft.Storage/storageAccounts@2021-06-01' = {
  name: uniqueName
  location: location
  kind: 'StorageV2'
  sku: {
    name: 'Standard_LRS'
  }
  properties: {
    accessTier: environment == 'prod' ? 'Hot' : 'Cool'
  }
  tags: tags
}

// Output functions
output connectionString string = 'DefaultEndpointProtocol=https;AccountName=${storageAccount.name};AccountKey=${listKeys(storageAccount.id, storageAccount.apiVersion).keys[0].value}'
output formattedName string = toUpper(storageAccount.name)
```

---

## 9. Modules

### Creating Modules

```bicep
// modules/storage.bicep
param location string
param storageAccountName string
param environment string
param tags object = {}

resource storageAccount 'Microsoft.Storage/storageAccounts@2021-06-01' = {
  name: storageAccountName
  location: location
  kind: 'StorageV2'
  sku: {
    name: 'Standard_LRS'
  }
  properties: {
    accessTier: environment == 'prod' ? 'Hot' : 'Cool'
  }
  tags: tags
}

output storageId string = storageAccount.id
output storageName string = storageAccount.name
output primaryEndpoint string = storageAccount.properties.primaryEndpoints.blob
```

### Using Modules

```bicep
// main.bicep
param location string = 'eastus'
param environment string = 'dev'
param appName string

var uniqueName = '${appName}${uniqueString(resourceGroup().id)}'
var tags = {
  environment: environment
  app: appName
}

// Call storage module
module storageModule 'modules/storage.bicep' = {
  name: 'storageDeployment'
  params: {
    location: location
    storageAccountName: uniqueName
    environment: environment
    tags: tags
  }
}

// Call network module
module networkModule 'modules/network.bicep' = {
  name: 'networkDeployment'
  params: {
    location: location
    vnetName: '${appName}vnet'
    environment: environment
    tags: tags
  }
}

// Use module outputs
output storageId string = storageModule.outputs.storageId
output storageName string = storageModule.outputs.storageName
output vnetId string = networkModule.outputs.vnetId
```

### Module with Conditions

```bicep
// modules/database.bicep
param location string
param dbName string
param createDatabase bool = true

resource database 'Microsoft.DBforPostgreSQL/servers@2017-12-01' = if (createDatabase) {
  name: dbName
  location: location
  sku: {
    name: 'B_Gen5_1'
    tier: 'Basic'
    capacity: 1
    family: 'Gen5'
  }
  properties: {
    createMode: 'Default'
    version: '10'
  }
}

output databaseId string = createDatabase ? database.id : ''
```

---

## 10. Deployment

### Deploy from Command Line

```bash
# Deploy bicep file
az deployment group create \
  --name "myDeployment" \
  --resource-group "myResourceGroup" \
  --template-file "main.bicep"

# Deploy with parameters
az deployment group create \
  --name "myDeployment" \
  --resource-group "myResourceGroup" \
  --template-file "main.bicep" \
  --parameters location=westus environment=prod

# Deploy with parameter file
az deployment group create \
  --name "myDeployment" \
  --resource-group "myResourceGroup" \
  --template-file "main.bicep" \
  --parameters "main.bicepparam"

# Deploy with JSON parameters
az deployment group create \
  --name "myDeployment" \
  --resource-group "myResourceGroup" \
  --template-file "main.bicep" \
  --parameters @parameters.json

# What-if (preview changes)
az deployment group what-if \
  --resource-group "myResourceGroup" \
  --template-file "main.bicep" \
  --parameters location=eastus

# Validate template
az deployment group validate \
  --resource-group "myResourceGroup" \
  --template-file "main.bicep"
```

### Subscription-Level Deployment

```bash
# Deploy at subscription level
az deployment sub create \
  --name "myDeployment" \
  --location "eastus" \
  --template-file "main.bicep"
```

### Convert and Build

```bash
# Convert bicep to ARM template
bicep build main.bicep
# Output: main.json

# Build and output to specific file
bicep build main.bicep --outfile main-arm.json

# Decompile ARM template to bicep
bicep decompile main.json
# Output: main.bicep

# Install bicep (via CLI)
az bicep install
az bicep upgrade
```

---

## 11. Best Practices

### Naming Conventions

```bicep
// Use consistent naming
param location string = 'eastus'
param environment string = 'dev'
param appName string

var resourcePrefix = '${appName}-${environment}'
var storageAccountName = '${toLower(appName)}${toLower(environment)}${uniqueString(resourceGroup().id)}'
var vnetName = '${resourcePrefix}-vnet'
var subnetName = '${resourcePrefix}-subnet'
var nsgName = '${resourcePrefix}-nsg'
var nicName = '${resourcePrefix}-nic'
var vmName = '${resourcePrefix}-vm'
```

### Parameter Organization

```bicep
// Organize parameters logically
@description('Location for all resources')
param location string = 'eastus'

@description('Environment: dev, test, or prod')
@allowed([
  'dev'
  'test'
  'prod'
])
param environment string = 'dev'

@description('Application name')
@minLength(3)
@maxLength(24)
param appName string

@description('VM size')
@allowed([
  'Standard_B2s'
  'Standard_D2s_v3'
  'Standard_D4s_v3'
])
param vmSize string = 'Standard_B2s'

@description('Number of instances')
@minValue(1)
@maxValue(10)
param instanceCount int = 1
```

### Tag Strategy

```bicep
param environment string
param appName string
param costCenter string
param owner string

var defaultTags = {
  environment: environment
  application: appName
  costCenter: costCenter
  owner: owner
  createdDate: utcNow('u')
  managedBy: 'bicep'
}

// Apply tags to all resources
resource storageAccount 'Microsoft.Storage/storageAccounts@2021-06-01' = {
  name: storageName
  location: location
  kind: 'StorageV2'
  sku: {
    name: 'Standard_LRS'
  }
  tags: defaultTags
}
```

### Conditional Resources

```bicep
param environment string
param createProdResources bool = environment == 'prod'

// Only create in production
resource premiumStorage 'Microsoft.Storage/storageAccounts@2021-06-01' = if (createProdResources) {
  name: premiumStorageName
  location: location
  kind: 'StorageV2'
  sku: {
    name: 'Standard_GRS'  // Geo-redundant for prod
  }
  properties: {
    accessTier: 'Hot'
  }
}

output premiumStorageId string = createProdResources ? premiumStorage.id : ''
```

### Modular Structure

```bicep
// main.bicep - orchestrates modules
param location string = 'eastus'
param environment string = 'dev'
param appName string

var tags = {
  environment: environment
  app: appName
}

// Network module
module network 'modules/network.bicep' = {
  name: 'networkModule'
  params: {
    location: location
    appName: appName
    environment: environment
    tags: tags
  }
}

// Storage module
module storage 'modules/storage.bicep' = {
  name: 'storageModule'
  params: {
    location: location
    appName: appName
    environment: environment
    tags: tags
  }
}

// Compute module
module compute 'modules/compute.bicep' = {
  name: 'computeModule'
  params: {
    location: location
    appName: appName
    environment: environment
    vnetId: network.outputs.vnetId
    subnetId: network.outputs.subnetId
    tags: tags
  }
}

output deploymentInfo object = {
  networkId: network.outputs.vnetId
  storageId: storage.outputs.storageId
  computeId: compute.outputs.vmId
}
```

---

## 12. Troubleshooting

### Validate and Debug

```bash
# Validate syntax
bicep build main.bicep --outfile /dev/null

# Get verbose output
az deployment group create \
  --resource-group "myResourceGroup" \
  --template-file "main.bicep" \
  --debug

# Convert to ARM template for inspection
bicep build main.bicep
cat main.json

# Validate with ARM template schema
az deployment group validate \
  --resource-group "myResourceGroup" \
  --template-file "main.bicep"

# Check what will be deployed
az deployment group what-if \
  --resource-group "myResourceGroup" \
  --template-file "main.bicep" \
  --parameters environment=prod
```

### Common Issues

```bicep
// Issue: Missing quotes on string values
// Wrong:
sku: Standard_LRS
// Right:
sku: 'Standard_LRS'

// Issue: Incorrect API version
// Check available versions:
// az provider show --namespace Microsoft.Storage --query "resourceTypes[?resourceType=='storageAccounts'].apiVersions"

// Issue: Type mismatch
// Wrong:
param count int = 'five'
// Right:
param count int = 5

// Issue: Missing resource reference
// Wrong:
id: storageAccount
// Right:
id: storageAccount.id

// Issue: Circular module dependencies
// Avoid having modules reference each other

// Issue: Undefined variable
// Make sure variable is defined before use
var uniqueName = 'app${uniqueString(resourceGroup().id)}'
// Use it correctly:
name: uniqueName
```

### Deployment Troubleshooting

```bash
# View deployment history
az deployment group list --resource-group "myResourceGroup" --output table

# View failed deployment details
az deployment group show \
  --resource-group "myResourceGroup" \
  --name "myDeployment" \
  --query "properties.error"

# View operation details
az deployment operation group list \
  --resource-group "myResourceGroup" \
  --name "myDeployment"

# Delete failed deployment
az deployment group delete \
  --resource-group "myResourceGroup" \
  --name "myDeployment"

# Clean up resources
az group delete --name "myResourceGroup" --yes
```

---

## Quick Reference

```bash
# Build and Deploy
bicep build main.bicep                              # Compile to ARM template
az deployment group create \
  --resource-group myRG \
  --template-file main.bicep                        # Deploy bicep
az deployment group what-if \
  --resource-group myRG \
  --template-file main.bicep                        # Preview changes

# Parameters
az deployment group create \
  --resource-group myRG \
  --template-file main.bicep \
  --parameters location=westus environment=prod     # Inline parameters
az deployment group create \
  --resource-group myRG \
  --template-file main.bicep \
  --parameters main.bicepparam                      # Parameter file

# Validation
bicep build main.bicep                              # Check syntax
az deployment group validate \
  --resource-group myRG \
  --template-file main.bicep                        # Validate deployment

# Info
az deployment operation group list \
  --resource-group myRG \
  --name myDeployment                               # View deployment info
az deployment group show \
  --resource-group myRG \
  --name myDeployment \
  --query properties.outputs                        # View outputs
```

---

## Essential Bicep File Example

```bicep
// main.bicep - Complete example
@description('Location for resources')
param location string = 'eastus'

@description('Environment type')
@allowed([
  'dev'
  'test'
  'prod'
])
param environment string = 'dev'

@description('Application name')
param appName string

var resourceTags = {
  environment: environment
  application: appName
  createdDate: utcNow('u')
}

var uniqueName = '${toLower(appName)}${uniqueString(resourceGroup().id)}'

// Virtual Network
resource vnet 'Microsoft.Network/virtualNetworks@2021-02-01' = {
  name: '${appName}-vnet'
  location: location
  properties: {
    addressSpace: {
      addressPrefixes: [
        '10.0.0.0/16'
      ]
    }
  }
  tags: resourceTags
}

// Storage Account
resource storageAccount 'Microsoft.Storage/storageAccounts@2021-06-01' = {
  name: uniqueName
  location: location
  kind: 'StorageV2'
  sku: {
    name: environment == 'prod' ? 'Standard_GRS' : 'Standard_LRS'
  }
  properties: {
    accessTier: 'Hot'
  }
  tags: resourceTags
}

// Outputs
output vnetId string = vnet.id
output storageName string = storageAccount.name
output storageId string = storageAccount.id
output deploymentTime string = utcNow('u')
```

---

## Resources

| Resource | URL |
|----------|-----|
| **Official Documentation** | https://learn.microsoft.com/en-us/azure/azure-resource-manager/bicep/ |
| **Bicep GitHub** | https://github.com/Azure/bicep |
| **Bicep Playground** | https://aka.ms/bicep |
| **Resource Reference** | https://learn.microsoft.com/en-us/azure/templates/ |
| **Best Practices** | https://learn.microsoft.com/en-us/azure/azure-resource-manager/bicep/best-practices |
| **Azure CLI** | https://learn.microsoft.com/en-us/cli/azure/ |

---

**Last Updated:** 2026
**Bicep Version:** 0.20+
**Azure CLI Version:** 2.36+

For latest information, visit [Bicep Official Documentation](https://learn.microsoft.com/en-us/azure/azure-resource-manager/bicep/)