# CardDemo - Comprehensive Mainframe Credit Card Management Application

**CardDemo is a complete mainframe credit card management system that demonstrates real-world mainframe modernization patterns using COBOL, CICS, VSAM, and JCL technologies.**

![User Flow](./diagrams/Application-Flow-User.png)

## Table of Contents

- [Overview](#overview)
- [Who Should Use This](#who-should-use-this)
- [Quick Start](#quick-start)
- [Project Structure](#project-structure)
- [Technologies Used](#technologies-used)
- [Installation on Mainframe](#installation-on-mainframe)
- [Application Details](#application-details)
  - [User Functions](#user-functions)
  - [Admin Functions](#admin-functions)
  - [Application Inventory](#application-inventory)
  - [Application Screens](#application-screens)
- [Running Full Batch](#running-full-batch)
- [Compatibility & Requirements](#compatibility--requirements)
- [Visual Architecture](#visual-architecture)
- [Support](#support)
- [Roadmap](#roadmap)
- [Contributing](#contributing)
- [License](#license)
- [Project Status](#project-status)

## Overview

CardDemo serves as a comprehensive example for organizations looking to **discover, migrate, modernize, or integrate legacy mainframe systems** with modern cloud infrastructure. This application provides practical, hands-on experience with mainframe technologies while demonstrating modernization use cases including performance testing, service enablement, test creation, and transformation tooling.

The application simulates a real-world credit card management system with complete functionality for account management, transaction processing, user administration, and batch operations. It's designed to provide mainframe coding scenarios that exercise analysis, transformation, and migration tooling in realistic business contexts.

**Key Capabilities:**
- Complete online transaction processing through CICS
- Comprehensive batch processing for daily operations
- User authentication and role-based access control
- Real-time account and card management
- Transaction reporting and bill payment processing
- Statement generation in multiple formats

## Who Should Use This

**Primary Audiences:**

- **Mainframe Modernization Teams**: Architects and developers working on legacy system transformation projects
- **Learning & Training**: Students and professionals learning mainframe technologies and modernization patterns
- **Tool Vendors**: Companies developing mainframe analysis, migration, or modernization tools
- **Enterprise Architects**: Teams evaluating mainframe modernization strategies and approaches

**User Types:**
- **Regular Users**: Can view accounts, manage credit cards, view transactions, and process bill payments
- **Administrators**: Can manage user accounts and perform system administration functions
- **Developers**: Can examine code patterns, data structures, and integration approaches

## Quick Start

### Prerequisites
- IBM mainframe environment with CICS and VSAM support
- COBOL compiler
- JCL execution capability
- RACF or equivalent security system

### Basic Setup
```shell
# 1. Clone the repository
git clone https://github.com/aws-samples/mainframe-modernization-carddemo.git
cd mainframe-modernization-carddemo

# 2. Upload source code to mainframe datasets
# Use your preferred upload tool (e.g., $INDFILE)

# 3. Execute initial setup JCLs in order:
# DUSRSECJ -> CLOSEFIL -> ACCTFILE -> CARDFILE -> CUSTFILE -> XREFFILE -> TRANFILE -> OPENFIL

# 4. Compile programs using your shop's procedures
# Sample JCLs available in samples/ directory

# 5. Define CICS resources using DFHCSDUP or CEDA
# Resources defined in CSD folder

# 6. Start the application
# Transaction: CC00
# Default Admin: ADMIN001/PASSWORD
# Default User: USER0001/PASSWORD
```

## Project Structure

```
mainframe-modernization-carddemo/
├── app/                          # Main application code
│   ├── cbl/                     # COBOL programs (business logic)
│   │   ├── CO*.cbl             # Online CICS programs
│   │   └── CB*.cbl             # Batch processing programs
│   ├── bms/                     # BMS maps (screen layouts)
│   ├── cpy/                     # COBOL copybooks (data structures)
│   ├── jcl/                     # JCL jobs (batch processing)
│   ├── data/                    # Sample data files
│   │   ├── ASCII/              # Human-readable sample data
│   │   └── EBCDIC/             # Mainframe-ready data files
│   └── catlg/                   # Dataset catalog information
├── samples/                      # Sample compilation procedures
│   ├── jcl/                     # Sample JCL for compilation
│   ├── proc/                    # JCL procedures for builds
│   └── m2/                      # Runtime samples
├── diagrams/                     # System documentation diagrams
├── CSD/                         # CICS resource definitions
├── CONTRIBUTING.md              # Contribution guidelines
└── README.md                    # This file
```

### Core Program Categories

**Online Programs (CO*.cbl):**
- Screen-based CICS transactions with BMS maps
- Handle user input validation and real-time processing
- Examples: `COSGN00C` (login), `COMEN01C` (main menu), `COACTUPC` (account update)

**Batch Programs (CB*.cbl):**
- Background processing and data transformation
- Daily operations like transaction posting and interest calculation
- Examples: `CBTRN02C` (transaction posting), `CBSTM03A` (statement generation)

**Utility Programs:**
- File I/O handlers and common utilities
- Support both online and batch operations

## Technologies Used

| Technology | Purpose | Version Notes |
|------------|---------|---------------|
| **COBOL** | Primary programming language | Enterprise COBOL for z/OS |
| **CICS** | Online transaction processing | CICS TS |
| **VSAM** | Data storage and access | KSDS (Key Sequenced Data Sets) |
| **JCL** | Job control and batch processing | z/OS JCL |
| **RACF** | Security and access control | z/OS RACF |
| **BMS** | Screen formatting and maps | Basic Mapping Support |

## Installation on Mainframe

### Detailed Setup Process

1. **Repository Setup**
   ```shell
   git clone https://github.com/aws-samples/mainframe-modernization-carddemo.git
   cd mainframe-modernization-carddemo
   ```

2. **Dataset Creation**
   - Create datasets under a High Level Qualifier (HLQ)
   - Upload source folders to mainframe using preferred tools
   - Ensure proper dataset characteristics (LRECL, BLKSIZE, etc.)

3. **Sample Data Loading**
   
   Upload the following datasets with binary transfer mode:

   | Dataset | Description | Copybook | Format | Length | ASCII Source |
   |---------|-------------|----------|--------|--------|--------------|
   | MFE.CARDDEMO.USRSEC.PS | User Security | CSUSR01Y | FB | 80 | DEFUSR01.jcl |
   | MFE.CARDDEMO.ACCTDATA.PS | Account Data | CVACT01Y | FB | 300 | acctdata.txt |
   | MFE.CARDDEMO.CARDDATA.PS | Card Data | CVACT02Y | FB | 150 | carddata.txt |
   | MFE.CARDDEMO.CUSTDATA.PS | Customer Data | CVCUS01Y | FB | 500 | custdata.txt |
   | MFE.CARDDEMO.CARDXREF.PS | Cross Reference | CVACT03Y | FB | 50 | cardxref.txt |

4. **JCL Execution Sequence**
   
   Execute these JCLs in the specified order:

   | Job | Function | Program |
   |-----|----------|---------|
   | DUSRSECJ | Setup user security VSAM file | IEBGENER |
   | CLOSEFIL | Close CICS files | IEFBR14 |
   | ACCTFILE | Load account database | IDCAMS |
   | CARDFILE | Load card database | IDCAMS |
   | CUSTFILE | Create customer database | IDCAMS |
   | XREFFILE | Load cross-reference data | IDCAMS |
   | TRANFILE | Initialize transaction file | IDCAMS |
   | OPENFIL | Open files for CICS | IEFBR14 |

5. **Program Compilation**
   - Use your shop's standard compilation procedures
   - Sample JCLs provided in `samples/` directory
   - Ensure all copybooks are accessible during compilation

6. **CICS Resource Definition**
   
   **Option A (Recommended):** Use DFHCSDUP with provided CSD file
   ```shell
   // Execute DFHCSDUP JCL with CSD definitions
   ```
   
   **Option B:** Manual CEDA commands
   ```shell
   DEFINE LIBRARY(COM2DOLL) GROUP(CARDDEMO) DSNAME01(&HLQ..LOADLIB)
   DEF PROGRAM(COCRDLIC) GROUP(CARDDEMO)
   DEF MAPSET(COCRDLI) GROUP(CARDDEMO)
   DEFINE TRANSACTION(CC00) GROUP(CARDDEMO) PROGRAM(COSGN00C)
   ```

7. **Application Startup**
   - Install resources: `CEDA INSTALL GROUP(CARDDEMO)`
   - Load programs: `CECI LOAD PROG(program-name)`
   - Execute NEWCOPY for updated programs

## Running Full Batch

For complete batch processing cycle, execute the following JCLs in sequence:

```shell
# File Management
CLOSEFIL    # Close CICS files
ACCTFILE    # Load account data
CARDFILE    # Load card data
CUSTFILE    # Load customer data
XREFFILE    # Load cross-reference data

# Security and Setup
DUSRSECJ    # Setup user security
DEFGDGB     # Define GDG bases

# Core Processing
POSTTRAN    # Process daily transactions
INTCALC     # Calculate interest
CREASTMT    # Generate statements

# File Operations
TRANBKP     # Backup transaction data
COMBTRAN    # Combine transaction files
OPENFIL     # Open files for CICS
```

## Application Details

CardDemo is a comprehensive credit card management system with distinct user roles and extensive functionality covering both online and batch processing scenarios.

### User Functions

![User Flow](./diagrams/Application-Flow-User.png)

Regular users can perform the following operations:
- **Account Management**: View account details, balances, and transaction history
- **Credit Card Operations**: List cards, view card details, and update card information
- **Transaction Processing**: View transaction history, add new transactions
- **Bill Payment**: Process payments and view payment history
- **Reporting**: Generate transaction reports and statements

### Admin Functions

![Admin Flow](./diagrams/Application-Flow-Admin.png)

Administrative users have access to:
- **User Management**: Create, update, and delete user accounts
- **System Administration**: Manage application settings and configurations
- **Security Operations**: Control user access and permissions
- **System Monitoring**: View system status and performance metrics

### Application Inventory

#### **Online Transactions**

| Transaction | Map | Program | Function |
|-------------|-----|---------|----------|
| CC00 | COSGN00 | COSGN00C | User Authentication |
| CM00 | COMEN01 | COMEN01C | Main Menu Navigation |
| CAVW | COACTVW | COACTVWC | Account View |
| CAUP | COACTUP | COACTUPC | Account Update |
| CCLI | COCRDLI | COCRDLIC | Credit Card List |
| CCDL | COCRDSL | COCRDSLC | Credit Card Details |
| CCUP | COCRDUP | COCRDUPC | Credit Card Update |
| CT00 | COTRN00 | COTRN00C | Transaction List |
| CT01 | COTRN01 | COTRN01C | Transaction View |
| CT02 | COTRN02 | COTRN02C | Add Transaction |
| CR00 | CORPT00 | CORPT00C | Transaction Reports |
| CB00 | COBIL00 | COBIL00C | Bill Payment |
| CA00 | COADM01 | COADM01C | Admin Menu |
| CU00 | COUSR00 | COUSR00C | List Users |
| CU01 | COUSR01 | COUSR01C | Add User |
| CU02 | COUSR02 | COUSR02C | Update User |
| CU03 | COUSR03 | COUSR03C | Delete User |

#### **Batch Processing**

| Job | Program | Function |
|-----|---------|----------|
| DUSRSECJ | IEBGENER | Initial user security file load |
| DEFGDGB | IDCAMS | Setup Generation Data Groups |
| ACCTFILE | IDCAMS | Refresh account master data |
| CARDFILE | IDCAMS | Refresh card master data |
| CUSTFILE | IDCAMS | Refresh customer master data |
| POSTTRAN | CBTRN02C | Daily transaction processing |
| INTCALC | CBACT04C | Interest calculation processing |
| CREASTMT | CBSTM03A | Customer statement generation |
| TRANBKP | IDCAMS | Transaction database backup |
| COMBTRAN | SORT | Combine transaction files |

### Application Screens

#### **Signon Screen**
![Signon Screen](./diagrams/Signon-Screen.png)

The application entry point where users authenticate using their credentials.

#### **Main Menu**
![Main Menu](./diagrams/Main-Menu.png)

Primary navigation interface for regular users to access all application functions.

#### **Admin Menu**
![Admin Menu](./diagrams/Admin-Menu.png)

Administrative interface providing access to user management and system administration functions.

## Compatibility & Requirements

### Mainframe Environment
- **z/OS**: Version 2.1 or higher
- **CICS TS**: Version 5.1 or higher
- **Enterprise COBOL**: Version 4.2 or higher
- **VSAM**: Standard z/OS VSAM support
- **RACF**: Or equivalent security system

### Development Tools
- **ISPF/PDF**: For source code editing
- **SDSF**: For job monitoring and output review
- **File Manager**: For data file management (optional)

### Storage Requirements
- **Source Libraries**: Approximately 5MB
- **Load Libraries**: Approximately 10MB
- **Data Files**: Approximately 50MB (with sample data)
- **Work Space**: Additional 100MB for processing

## Visual Architecture

The application includes comprehensive visual documentation:

- **User Flow Diagrams**: Complete user journey mapping
- **Admin Flow Diagrams**: Administrative process flows
- **Screen Mockups**: All application screens with sample data
- **Data Flow Diagrams**: Batch processing workflows

All diagrams are located in the `diagrams/` directory and referenced throughout this documentation.

## Support

For questions, issues, or improvement requests:

1. **GitHub Issues**: Use the repository issue tracker for bug reports and feature requests
2. **Discussions**: Join community discussions for general questions
3. **Documentation**: Refer to inline code comments and copybook documentation

When reporting issues, please include:
- Mainframe environment details (z/OS version, CICS version)
- Error messages and job outputs
- Steps to reproduce the issue
- Expected vs. actual behavior

## Roadmap

### Planned Enhancements

**Database Integration:**
- DB2 relational database integration
- IMS hierarchical database support
- Modern database connectivity options

**System Integration:**
- FTP/SFTP file transfer capabilities
- Message queue integration (MQ Series)
- REST API exposure for distributed applications
- Modern authentication mechanisms

**Modernization Features:**
- Cloud-native deployment options
- Containerization support
- DevOps pipeline integration
- Performance monitoring and analytics

**Development Tools:**
- Enhanced debugging capabilities
- Automated testing frameworks
- Code analysis and quality tools

## Contributing

We welcome contributions from the mainframe and modernization community! This project serves as a collaborative resource for learning and demonstrating mainframe modernization patterns.

**How to Contribute:**
1. Review our [Contributing Guidelines](CONTRIBUTING.md)
2. Check existing issues for contribution opportunities
3. Fork the repository and create feature branches
4. Submit pull requests with clear descriptions
5. Participate in code reviews and discussions

**Areas for Contribution:**
- Additional COBOL program examples
- Enhanced JCL procedures
- Documentation improvements
- Test data and scenarios
- Modernization use case examples

Please ensure all contributions maintain the educational and demonstrative nature of the project while following mainframe coding best practices.

## License

This project is licensed under the Apache License 2.0 - see the [LICENSE](LICENSE) file for complete details.

The Apache 2.0 license allows for both commercial and non-commercial use, modification, and distribution, making this project suitable for educational, research, and commercial modernization initiatives.

## Project Status

**Current Version**: 1.0 (Stable)
**Next Major Release**: v2.0 planned for Q1 2025

**Recent Updates:**
- Enhanced documentation and user guides
- Improved sample data sets
- Additional JCL examples and procedures
- Extended COBOL program examples

**Active Development Areas:**
- Database integration features
- Modern integration capabilities
- Enhanced testing frameworks
- Performance optimization examples

---

*Originally written and maintained by contributors and [Devin](https://app.devin.ai), with updates from the core team.*


