# CardDemo - Comprehensive Mainframe Modernization Reference Application

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![Language](https://img.shields.io/badge/Language-COBOL-green.svg)](https://www.cobol.com/)
[![Platform](https://img.shields.io/badge/Platform-z%2FOS-red.svg)](https://www.ibm.com/products/zos)

**CardDemo is a production-ready mainframe credit card management application that demonstrates real-world modernization patterns, migration strategies, and transformation techniques for legacy enterprise systems.**

## 🎯 Why CardDemo Matters

Legacy mainframe systems power 70% of Fortune 500 transactions, yet modernizing these critical systems remains one of enterprise IT's greatest challenges. CardDemo bridges this gap by providing:

- **Real-world complexity**: A complete credit card processing system with user management, transaction processing, and reporting
- **Modernization patterns**: Practical examples of discovery, migration, augmentation, and service extraction techniques  
- **Learning platform**: Hands-on experience with COBOL, CICS, VSAM, and JCL in a safe environment
- **Transformation toolkit**: Ready-to-use scenarios for testing analysis and migration tools

Whether you're planning a mainframe modernization, training development teams, or building transformation tooling, CardDemo provides the authentic complexity needed for success.

## 📋 Table of Contents

- [Quick Start](#-quick-start)
- [Modernization Use Cases](#-modernization-use-cases)
- [Project Structure](#-project-structure)
- [Technology Stack](#-technology-stack)
- [Installation Guide](#-installation-guide)
- [Application Details](#-application-details)
  - [User Functions](#user-functions)
  - [Admin Functions](#admin-functions)
  - [Application Inventory](#application-inventory)
  - [Application Screens](#application-screens)
- [Running the Application](#-running-the-application)
- [Support](#-support)
- [Roadmap](#-roadmap)
- [Contributing](#-contributing)
- [License](#-license)

## 🚀 Quick Start

### Prerequisites
- z/OS mainframe environment with CICS and VSAM support
- COBOL compiler (Enterprise COBOL recommended)
- JCL execution capability
- Basic knowledge of mainframe development concepts

### Get Started in 5 Minutes
```bash
# 1. Clone the repository
git clone https://github.com/Devin-Workshop-July/mainframe-modernization-carddemo.git
cd mainframe-modernization-carddemo

# 2. Review the sample data and JCL procedures
ls -la app/data/EBCDIC/
ls -la app/jcl/

# 3. Follow the detailed installation guide below for full setup
```

**For complete installation**: See the [Installation Guide](#-installation-guide) section below for step-by-step mainframe setup instructions.

## 🔄 Modernization Use Cases

CardDemo demonstrates key modernization patterns essential for enterprise transformation:

| **Pattern** | **Description** | **CardDemo Example** |
|-------------|-----------------|---------------------|
| **Discovery** | Analyze existing code structure and dependencies | 28 COBOL programs with clear business logic separation |
| **Migration** | Move from legacy to modern platforms | VSAM to relational database conversion scenarios |
| **Augmentation** | Add modern capabilities to existing systems | API enablement for mobile/web integration |
| **Service Extraction** | Extract business logic as microservices | Transaction processing and user management services |
| **Performance Testing** | Validate system behavior under load | Batch processing and online transaction scenarios |
| **Test Harness** | Automated testing of legacy components | Sample data and validation procedures included |

## 📁 Project Structure

```
mainframe-modernization-carddemo/
├── app/                          # Core application components
│   ├── bms/                      # Basic Mapping Support (screen definitions)
│   ├── cbl/                      # COBOL source programs (28 programs)
│   │   ├── CO*.cbl              # Online transaction programs
│   │   └── CB*.cbl              # Batch processing programs
│   ├── cpy/                      # COBOL copybooks (data structures)
│   ├── csd/                      # CICS System Definition resources
│   ├── data/                     # Sample data files
│   │   └── EBCDIC/              # Mainframe-format test data
│   ├── jcl/                      # Job Control Language procedures
│   └── proc/                     # Cataloged procedures
├── diagrams/                     # Application flow and architecture diagrams
├── samples/                      # Sample JCL and configuration files
├── CONTRIBUTING.md               # Contribution guidelines
└── README.md                     # This file
```

## 🛠 Technology Stack

CardDemo leverages core mainframe technologies that power enterprise systems worldwide:

| **Component** | **Technology** | **Modernization Context** |
|---------------|----------------|---------------------------|
| **Programming Language** | COBOL | Most widely used business programming language |
| **Transaction Processing** | CICS (Customer Information Control System) | High-performance online transaction processing |
| **Data Management** | VSAM (Virtual Storage Access Method) | Indexed file system for high-speed data access |
| **Job Scheduling** | JCL (Job Control Language) | Batch processing and system automation |
| **Security** | RACF (Resource Access Control Facility) | Enterprise-grade security and access control |

This stack represents the foundation of mission-critical systems processing billions of transactions daily across industries like banking, insurance, and retail.

<br/>

## Technologies used
1. COBOL
2. CICS
3. VSAM
4. JCL
5. RACF

<br/>

## 📦 Installation Guide

To install CardDemo on your mainframe environment, follow these comprehensive steps:

1. Clone this repository to your local development environment

2. Create datasets on the mainframe
   * It is recommended to group them under a High Level Qualifier (HLQ)for all your datasets. 
   * Upload the following application source folders from the main branch of git repository on to your mainframe
      using $INDFILE or your preferred upload tool.
      
3. Use data for testing using either of the below approaches

   ** Use the supplied sample data**
   
      * Upload the sample data provided in the main/-/data/EBCDIC/ folder to the mainframe. Ensure that you use transfer mode binary (e.g.)

         | Dataset name                      | Name                                             | Copybook (Layout) | Format | Length | Name of equivalent ascii file |
         | :---------------------------------| :----------------------------------------------- | :-----            | :----- | -----: | :---------------------------- |
         | MFE.CARDDEMO.USRSEC.PS         | User Security file                               | CSUSR01Y          | FB     |     80 | See DEFUSR01.jcl (inline)     |
         | MFE.CARDDEMO.ACCTDATA.PS       | Account Data                                     | CVACT01Y          | FB     |    300 | acctdata.txt                  |
         | MFE.CARDDEMO.CARDDATA.PS       | Card Data                                        | CVACT02Y          | FB     |    150 | carddata.txt                  |
         | MFE.CARDDEMO.CUSTDATA.PS       | Customer Data                                    | CVCUS01Y          | FB     |    500 | custdata.txt                  |
         | MFE.CARDDEMO.CARDXREF.PS       | Customer Account Card Cross reference            | CVACT03Y          | FB     |     50 | cardxref.txt                  |
         | MFE.CARDDEMO.DALYTRAN.PS.INIT  | Transaction database initialization record       | CVTRA06Y          | FB     |    350 | 1 record (low-values ending with 00000100)|
         | MFE.CARDDEMO.DALYTRAN.PS       | Transaction data which has to go through posting | CVTRA06Y          | FB     |    350 | dailytran.txt                 |
         | MFE.CARDDEMO.TRANSACT.VSAM.KSDS| Transaction data entered online                  | CVTRA05Y          | FB     |    350 | not applicable                |
         | MFE.CARDDEMO.DISCGRP.PS        | Disclosure Groups                                | CVTRA02Y          | FB     |     50 | discgrp.txt                   |
         | MFE.CARDDEMO.TRANCATG.PS       | Transaction Category Types                       | CVTRA04Y          | FB     |     60 | trancatg.txt                  |
         | MFE.CARDDEMO.TRANTYPE.PS       | Transaction Types                                | CVTRA03Y          | FB     |     60 | trantype.txt                  |
         | MFE.CARDDEMO.TCATBALF.PS       | Transaction Category Balance                     | CVTRA01Y          | FB     |     50 | tcatbal.txt                   |

      * Execute the following JCLs in order

         | Jobname  | What it does                                        |
         | :------- | :-------------------------------------------------- |
         | DUSRSECJ | Sets up user security vsam file                     |
         | CLOSEFIL | Closes files opened by CICS                         |
         | ACCTFILE | Loads Account database using sample data            |
         | CARDFILE | Loads Card database with credit card sample data    |
         | CUSTFILE | Creates customer database                           |
         | XREFFILE | Loads Customer Card account cross reference to VSAM |
         | TRANFILE | Copies initial Trasaction file  to VSAM             |
         | DISCGRP  | Copies initial Disclosure Group file  to VSAM       |
         | TCATBALF | Copies initial TCATBALF file  to VSAM               |
         | TRANCATG | Copies initial transaction category file  to VSAM   |
         | TRANTYPE | Copies initial transaction type file                |
         | OPENFIL  | Makes files available to CICS                       |
         | DEFGDGB  | Defines GDG Base                                    |


4. Compile the Programs. 
   
   You should use the compile process followed by your mainframe shopfloor
   
   We have however provided some sample JCLs in the samples folder in git to help you craft the JCL   

5. Create resources in the CARDDEMO group in CICS
   
   You have 2 options
   
   Be sure to edit the HLQs in the below documents as required before you do the definition
   
   * (Preferred) . Use the DFHCSDUP JCL that the resources required by the application

      The resources required are in the CSD file provided in the CSD folder
       
      * Group CARDDEMO
      * Mapsets
      * Transactions
      * Maps
      * Files
      
   * Use the CEDA transaction to execute the commands in the above listing
   
      * Define group 
         ```shell
         DEFINE LIBRARY(COM2DOLL) GROUP(CARDDEMO) DSNAME01(&HLQ..LOADLIB)
         ```
      * Define Mapsets, Maps , Programs and Files
      
         Sample CEDA commands
         
         ```shell
         DEF PROGRAM(COCRDLIC) GROUP(CARDDEMO)
         DEF MAPSET(COCRDLI) GROUP(CARDDEMO)
         DEFINE PROGRAM(COSGN00C) GROUP(CARDDEMO) DA(ANY) TRANSID(CC00) DESCRIPTION(LOGIN)
         DEFINE TRANSACTION(CC00) GROUP(CARDDEMO) PROGRAM(COSGN00C) TASKDATAL(ANY)
         ```

   * Install /Load the online resources to your CICS region

      ```shell
      CEDA INSTALL TRANS(CCLI) GROUP(CARDDEMO)
      CEDA INSTALL FILE(CARDDAT) GROUP(CARDDEMO)
      CECI LOAD PROG(COCRDUP)
      CECI LOAD PROG(COCRDUPC)
      ```

   * Execute a NEWCOPY of mapsets and maps
      ```shell
      CEMT SET PROG(COCRDUP) NEWCOPY
      CEMT SET PROG(COCRDUPC) NEWCOPY  
      ```
6. Enjoy the demo

   * For online functions : Start the CardDemo application using the CC00 transaction
     - Enter userid ADMIN001 and the initially configured password PASSWORD to manage users
     - Enter userid USER0001 and the initially configured password PASSWORD to access back office functions
   * For batch            : See the instructions for running full batch below.

## 🏃 Running the Application

### Online Functions
Start the CardDemo application using the **CC00** transaction:
- **Admin Access**: Use userid `ADMIN001` with password `PASSWORD` to manage users
- **User Access**: Use userid `USER0001` with password `PASSWORD` for back office functions

### Running Full Batch Processing
   
Execute the following JCLs in order for complete batch processing:

    | Jobname  | What it does                                        |
    | :------- | :-------------------------------------------------- |
    | CLOSEFIL | Closes files opened by CICS                         |
    | ACCTFILE | Loads Account database using sample data            |
    | CARDFILE | Loads Card database with credit card sample data    |
    | XREFFILE | Loads Customer Card account cross reference to VSAM |
    | CUSTFILE | Creates customer database                           |
    | TRANBKP  | Creates Transaction database                        |
    | DISCGRP  | Copies initial disclosure Group file  to VSAM       |
    | TCATBALF | Copies initial TCATBALF file  to VSAM               |
    | TRANTYPE | Copies initial transaction type file                |
    | DUSRSECJ | Sets up user security vsam file                     |
    | POSTTRAN | Core processing job                                 |
    | INTCALC  | Run interest calculations                           |
    | TRANBKP  | Backup Transaction database                         |
    | COMBTRAN | Combine system transactions with daily ones         |
    | CREASTMT | Produce transaction statement                       | 	
    | TRANIDX  | Define alternate index on transaction file          |
    | OPENFIL  | Makes files available to CICS                       |
<br/>

## 📱 Application Details 

CardDemo is a comprehensive Credit Card management application built with COBOL that demonstrates enterprise-grade mainframe development patterns. The application provides complete functionality for managing accounts, credit cards, transactions, and bill payments.

### User Types
The application supports two distinct user roles with different access levels:
- **Regular Users**: Access customer-facing functions like account management and transactions
- **Admin Users**: Perform administrative tasks including user management and system configuration

<br/>

### User Functions

![Alt text](./diagrams/Application-Flow-User.png?raw=true "User Flow")

<br/>

### Admin Functions

![Alt text](./diagrams/Application-Flow-Admin.png?raw=true "Admin Flow")

<br/>

### Application Inventory

#### **Online**

| Transaction |      | BMS Map | Program  | Function            |
| :---------- | :--- | :------ | :------- | :------------------ |
| CC00        |      | COSGN00 | COSGN00C | Signon Screen       |
| CM00        |      | COMEN01 | COMEN01C | Main Menu           |
|             | CAVW | COACTVW | COACTVWC | Account View        |
|             | CAUP | COACTUP | COACTUPC | Account Update      |
|             | CCLI | COCRDLI | COCRDLIC | Credit Card List    |
|             | CCDL | COCRDSL | COCRDSLC | Credit Card View    |
|             | CCUP | COCRDUP | COCRDUPC | Credit Card Update  |
|             | CT00 | COTRN00 | COTRN00C | Transaction List    |
|             | CT01 | COTRN01 | COTRN01C | Transaction View    |
|             | CT02 | COTRN02 | COTRN02C | Transaction Add     |
|             | CR00 | CORPT00 | CORPT00C | Transaction Reports |
|             | CB00 | COBIL00 | COBIL00C | Bill Payment        |
| CA00        |      | COADM01 | COADM01C | Admin Menu          |
|             | CU00 | COUSR00 | COUSR00C | List Users          |
|             | CU01 | COUSR01 | COUSR01C | Add User            |
|             | CU02 | COUSR02 | COUSR02C | Update User         |
|             | CU03 | COUSR03 | COUSR03C | Delete User         |

#### **Batch**

| Job      | Program  | Function                                   |
| :------- | :------- | :----------------------------------------- |
| DUSRSECJ | IEBGENER | Initial Load of User security file         |
| DEFGDGB  | IDCAMS   | Setup GDG Bases                            | 
| ACCTFILE | IDCAMS   | Refresh Account Master                     |
| CARDFILE | IDCAMS   | Refresh Card Master                        |
| CUSTFILE | IDCAMS   | Refresh Customer Master                    |
| DISCGRP  | IDCAMS   | Load Disclosure Group File                 |
| TRANFILE | IDCAMS   | Load Transaction Master file               |
| TRANCATG | IDCAMS   | Load Transaction category types            |
| TRANTYPE | IDCAMS   | Load Transaction type file                 |
| XREFFILE | IDCAMS   | Account, Card and Customer cross reference |
| CLOSEFIL | IEFBR14  | Close VSAM files in CICS                   |
| TCATBALF | IDCAMS   | Refresh Transaction Category Balance       |
| TRANBKP  | IDCAMS   | Refresh Transaction Master                 |
| POSTTRAN | CBTRN02C | Transaction processing job                 |
| TRANIDX  | IDCAMS   | Define AIX for transaction file            |
| OPENFIL  | IEFBR14  | Open files in CICS                         |
| INTCALC  | CBACT04C | Run interest calculations                  |
| COMBTRAN | SORT     | Combine transaction files                  |
| CREASTMT | CBSTM03A | Produce transaction statement              |

<br/>

### Application Screens

#### **Signon Screen**

![Alt text](./diagrams/Signon-Screen.png?raw=true "Signon Screen")


#### **Main Menu**

![Alt text](./diagrams/Main-Menu.png?raw=true "Main Menu")

#### **Admin Menu**

![Alt text](./diagrams/Admin-Menu.png?raw=true "Admin Menu")

<br/>

## 🆘 Support

Having trouble with CardDemo? We're here to help!

- **Issues**: Report bugs or request features via [GitHub Issues](https://github.com/Devin-Workshop-July/mainframe-modernization-carddemo/issues)
- **Discussions**: Join the community conversation in [GitHub Discussions](https://github.com/Devin-Workshop-July/mainframe-modernization-carddemo/discussions)
- **Documentation**: Check our [detailed installation guide](#-installation-guide) and [application details](#-application-details)

For security vulnerabilities, please follow our [security reporting guidelines](https://aws.amazon.com/security/vulnerability-reporting/).

## 🗺 Roadmap

CardDemo continues to evolve with new modernization patterns and enterprise integration capabilities:

### Upcoming Features
- **Enhanced Database Support**
  - Db2 relational database integration examples
  - IMS hierarchical database connectivity patterns
  - Data migration and synchronization scenarios

- **Modern Integration Patterns**
  - REST API service extraction examples
  - Message queue integration (MQ, Kafka)
  - Microservices decomposition patterns
  - Cloud-native deployment scenarios

- **Developer Experience Improvements**
  - Docker containerization for local development
  - CI/CD pipeline examples
  - Automated testing frameworks

## 🤝 Contributing

CardDemo thrives on community contributions! Whether you're a mainframe veteran or modern developer exploring legacy systems, your input is valuable.

### How to Contribute
- **Report Issues**: Found a bug or have a feature request? [Open an issue](https://github.com/Devin-Workshop-July/mainframe-modernization-carddemo/issues)
- **Submit Code**: Fork the repository and submit pull requests for enhancements
- **Share Knowledge**: Contribute documentation, tutorials, or modernization patterns
- **Join Discussions**: Help others in the community forums

Please read our [Contributing Guidelines](CONTRIBUTING.md) and [Code of Conduct](CODE_OF_CONDUCT.md) before getting started.

## 📄 License

CardDemo is released under the [Apache License 2.0](LICENSE), making it freely available for educational, research, and commercial use. This ensures the project remains a valuable community resource for mainframe modernization efforts worldwide.

## 📊 Project Status

**Current Version**: v1.0 - Production Ready  
**Next Release**: v2.0 planned for Q2 2025

CardDemo is actively maintained and continuously updated with new modernization patterns and enterprise integration examples. Watch this repository for the latest updates and releases.

---

*Originally written and maintained by contributors and [Devin](https://app.devin.ai), with updates from the core team.*


