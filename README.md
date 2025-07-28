## CardDemo -- Mainframe CardDemo Application

- [CardDemo -- Mainframe CardDemo Application](#carddemo----mainframe-card-demo-application)
- [Description](#description)
  - [Target Audiences](#target-audiences)
  - [Core Capabilities](#core-capabilities)
  - [Modernization Learning Objectives](#modernization-learning-objectives)
- [Technologies Used](#technologies-used)
  - [Core Mainframe Technologies](#core-mainframe-technologies)
  - [Integration Architecture](#integration-architecture)
- [Application Architecture](#application-architecture)
  - [System Overview](#system-overview)
  - [Transaction Processing Flow](#transaction-processing-flow)
  - [Communication Architecture](#communication-architecture)
  - [Data Architecture](#data-architecture)
- [Installation on the Mainframe](#installation-on-the-mainframe)
- [Application Details](#application-details)
  - [User Types and Access Control](#user-types-and-access-control)
  - [User Functions](#user-functions)
  - [Admin Functions](#admin-functions)
  - [System Components](#system-components)
    - [Online Transaction Processing (CICS)](#online-transaction-processing-cics)
    - [Batch Processing (JCL)](#batch-processing-jcl)
  - [User Interface Architecture](#user-interface-architecture)
    - [BMS Design Patterns](#bms-basic-mapping-support-design-patterns)
    - [Screen Flow Architecture](#screen-flow-architecture)
    - [Screen Development Patterns](#screen-development-patterns)
- [Running Full Batch](#running-full-batch)
- [Modernization Use Cases](#modernization-use-cases)
  - [Legacy Code Patterns for Analysis](#legacy-code-patterns-for-analysis)
  - [Transformation Opportunities](#transformation-opportunities)
  - [Modernization Strategies Demonstrated](#modernization-strategies-demonstrated)
- [Development Patterns](#development-patterns)
  - [COBOL Programming Patterns](#cobol-programming-patterns)
  - [CICS Programming Model](#cics-programming-model)
  - [Data Management Patterns](#data-management-patterns)
- [Support](#support)
- [Roadmap](#roadmap)
  - [Planned Enhancements](#planned-enhancements)
- [Contributing](#contributing)
- [License](#license)
- [Project Status](#project-status)

<br/>

## Description

CardDemo is a comprehensive mainframe credit card management application designed to demonstrate real-world mainframe modernization use cases. Built using traditional mainframe technologies including COBOL, CICS, VSAM, and JCL, this application serves as a practical learning resource for organizations looking to discover, migrate, modernize, or integrate legacy mainframe systems with modern cloud infrastructure.

### Target Audiences

**Developers and Architects**: Learn mainframe programming patterns, CICS transaction processing, VSAM data management, and batch processing workflows that are common in enterprise mainframe environments.

**Modernization Teams**: Analyze legacy code patterns, understand transformation challenges, and explore modernization strategies including service extraction, API enablement, and cloud migration approaches.

**Business Users**: Understand mainframe application functionality through a familiar credit card management domain with features like account management, transaction processing, and statement generation.

### Core Capabilities

The CardDemo application demonstrates a complete mainframe ecosystem with:

- **Online Transaction Processing**: Real-time user interactions through CICS for account management, card operations, and transaction processing
- **Batch Processing**: Background jobs for daily transaction posting, interest calculations, statement generation, and data maintenance
- **Data Management**: Comprehensive VSAM file structures for customers, accounts, cards, transactions, and cross-reference data
- **User Security**: Role-based access control with separate user and administrator functions
- **Reporting**: Statement generation in both plain text and HTML formats

### Modernization Learning Objectives

The application intentionally includes diverse coding styles and patterns to exercise analysis and transformation tooling. Key modernization scenarios demonstrated include:

- Legacy COBOL program structures and data handling patterns
- CICS transaction processing and program communication
- VSAM file organization and data access patterns
- JCL batch processing workflows and job dependencies
- Mainframe-specific programming constructs and control flow
- Integration points suitable for service extraction and API development

Note that the coding style varies across the application to provide realistic scenarios for modernization tooling analysis and transformation exercises.

<br/>

## Technologies Used

### Core Mainframe Technologies

**1. COBOL (Common Business-Oriented Language)**
- Primary programming language for all business logic
- Online programs handle CICS transaction processing and user interactions
- Batch programs manage data processing, calculations, and reporting
- Demonstrates traditional mainframe programming patterns and data structures

**2. CICS (Customer Information Control System)**
- Transaction processing monitor managing online user interactions
- Handles screen management through BMS (Basic Mapping Support)
- Provides program-to-program communication via COMMAREA
- Manages concurrent user sessions and transaction integrity

**3. VSAM (Virtual Storage Access Method)**
- Primary data storage using Key Sequenced Data Sets (KSDS)
- Indexed file organization for efficient data access
- Cross-reference files linking customers, accounts, and cards
- Supports both online transaction access and batch processing

**4. JCL (Job Control Language)**
- Batch job execution and workflow management
- Data loading, transformation, and maintenance operations
- File backup and recovery procedures
- Integration between online and batch processing components

**5. RACF (Resource Access Control Facility)**
- Security framework for user authentication and authorization
- Role-based access control (Admin vs. Regular users)
- Resource protection and audit capabilities

### Integration Architecture

The technologies work together to create a complete mainframe application ecosystem:
- **COBOL + CICS**: Online transaction processing with real-time user interactions
- **COBOL + JCL**: Batch processing for background operations and data maintenance
- **VSAM**: Shared data layer accessible by both online and batch components
- **BMS**: Screen definition and management for user interface
- **RACF**: Security layer protecting all system resources

<br/>

## Installation on the Mainframe 

To install this repository on the mainframe please follow the following steps

1. Clone this repository to your local development environment

2. Create datasets on the mainframe
   * It is recommended to group them under a High Level Qualifier (HLQ)for all your datasets. 
   * Upload the following application source folders from the main branch of git repository on to your mainframe
      using $INDFILE or your preferred upload tool.
      
3. Use data for testing using either of the below approaches

   **Use the supplied sample data**
   
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


4. **Compile the Programs**
   
   Use the compilation process standard for your mainframe environment. Sample compilation JCLs are provided in the `samples/` directory to help you create appropriate build procedures for your system.
   
   **Compilation Dependencies**:
   - COBOL compiler (Enterprise COBOL recommended)
   - CICS preprocessor for online programs
   - BMS assembler for mapset compilation
   - Appropriate load library definitions   

5. **Create CICS Resources**
   
   Configure the CARDDEMO resource group in your CICS region. Update High Level Qualifiers (HLQs) in all definitions to match your environment.
   
   **Option 1 (Recommended): Use DFHCSDUP**
   
   Execute the DFHCSDUP JCL to install all required resources from the provided CSD definitions:
   
   **Resource Groups**:
   - **CARDDEMO Group**: Main application resource group
   - **Mapsets**: BMS mapset definitions for all screens
   - **Transactions**: Transaction definitions with program associations
   - **Programs**: COBOL program definitions and load library references
   - **Files**: VSAM file definitions and access permissions
      
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
6. **Access the Application**

   **Online Functions**:
   - Start CardDemo using transaction `CC00` (Signon Screen)
   - **Administrator Access**: 
     - User ID: `ADMIN001`
     - Password: `PASSWORD`
     - Functions: User management, system administration
   - **Regular User Access**:
     - User ID: `USER0001` 
     - Password: `PASSWORD`
     - Functions: Account management, transactions, bill payment
   
   **Batch Processing**: 
   - See [Running Full Batch](#running-full-batch) section for complete batch execution procedures
   - Execute jobs in the specified sequence for proper data integrity

## Running Full Batch 
   
  * Execute the following JCLs in order

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

## Application Architecture

### System Overview

CardDemo implements a traditional mainframe three-tier architecture with presentation, business logic, and data layers clearly separated:

**Presentation Layer**: BMS (Basic Mapping Support) maps define screen layouts and user interaction patterns. Each screen is defined by a mapset containing field definitions, positioning, and attributes.

**Business Logic Layer**: COBOL programs handle all business processing, divided into online transaction programs (CICS) and batch processing programs (JCL). Programs communicate through standardized COMMAREA structures.

**Data Layer**: VSAM files provide persistent storage with indexed access. Cross-reference files maintain relationships between entities, enabling efficient data retrieval and integrity.

### Transaction Processing Flow

1. **User Authentication**: Login screen (COSGN00C) validates credentials against USRSEC file
2. **Menu Navigation**: Main menu (COMEN01C) or Admin menu (COADM01C) based on user type
3. **Function Execution**: Specific transaction programs handle business operations
4. **Data Access**: Programs read/write VSAM files through standardized I/O operations
5. **Screen Updates**: BMS maps format and display results to users

### Communication Architecture

Programs communicate through the CARDDEMO-COMMAREA structure containing:
- Navigation context (from/to program and transaction IDs)
- User session information (user ID, type, authentication status)
- Business data (customer, account, card information)
- Program state management (entry/reentry context)

### Data Architecture

**Master Files**:
- CUSTDATA: Customer personal and contact information
- ACCTDATA: Account details, balances, and status
- CARDDATA: Credit card information and limits

**Transaction Files**:
- TRANSACT: Posted transaction records
- DALYTRAN: Daily transactions pending posting

**Reference Files**:
- CARDXREF: Links cards to accounts and customers
- USRSEC: User security and authentication data
- DISCGRP, TRANCATG, TRANTYPE: Reference data for transaction processing

## Application Details 

CardDemo is a comprehensive credit card management application built using traditional mainframe technologies. The application provides complete lifecycle management for credit card operations including customer onboarding, account management, transaction processing, and statement generation.

### User Types and Access Control

**Regular Users**: 
- View account information and balances
- Manage credit card details and limits
- View transaction history and details
- Process bill payments
- Generate and view statements

**Administrative Users**:
- Manage user accounts (create, update, delete users)
- Access administrative functions and reports
- Perform system maintenance operations
- Monitor application usage and performance

Access control is enforced through the USRSEC file with role-based permissions validated at login and maintained throughout the user session.

<br/>

### User Functions

![Alt text](./diagrams/Application-Flow-User.png?raw=true "User Flow")

<br/>

### Admin Functions

![Alt text](./diagrams/Application-Flow-Admin.png?raw=true "Admin Flow")

<br/>

### System Components

#### **Online Transaction Processing (CICS)**

**Authentication and Navigation**:
| Transaction | BMS Map | Program  | Function | Description |
| :---------- | :------ | :------- | :------- | :---------- |
| CC00        | COSGN00 | COSGN00C | User Authentication | Validates user credentials against USRSEC file, determines user type, and routes to appropriate menu |
| CM00        | COMEN01 | COMEN01C | Main Menu | Primary navigation for regular users with dynamic menu generation based on user permissions |
| CA00        | COADM01 | COADM01C | Admin Menu | Administrative functions menu with elevated privilege operations |

**Account Management**:
| Transaction | BMS Map | Program  | Function | Description |
| :---------- | :------ | :------- | :------- | :---------- |
| CAVW        | COACTVW | COACTVWC | Account View | Display account details, balances, and status information with real-time data access |
| CAUP        | COACTUP | COACTUPC | Account Update | Modify account information with validation and audit trail capabilities |

**Credit Card Operations**:
| Transaction | BMS Map | Program  | Function | Description |
| :---------- | :------ | :------- | :------- | :---------- |
| CCLI        | COCRDLI | COCRDLIC | Card List | Display all cards associated with customer accounts with filtering options |
| CCDL        | COCRDSL | COCRDSLC | Card Details | View detailed card information including limits, status, and transaction summary |
| CCUP        | COCRDUP | COCRDUPC | Card Update | Modify card details, limits, and status with business rule validation |

**Transaction Processing**:
| Transaction | BMS Map | Program  | Function | Description |
| :---------- | :------ | :------- | :------- | :---------- |
| CT00        | COTRN00 | COTRN00C | Transaction List | Browse transaction history with search and filter capabilities |
| CT01        | COTRN01 | COTRN01C | Transaction View | Display detailed transaction information including merchant data and processing status |
| CT02        | COTRN02 | COTRN02C | Transaction Add | Create new transactions with validation against account limits and card status |

**Reporting and Payments**:
| Transaction | BMS Map | Program  | Function | Description |
| :---------- | :------ | :------- | :------- | :---------- |
| CR00        | CORPT00 | CORPT00C | Reports | Generate various transaction and account reports with flexible date ranges |
| CB00        | COBIL00 | COBIL00C | Bill Payment | Process payments against account balances with immediate posting |

**User Administration**:
| Transaction | BMS Map | Program  | Function | Description |
| :---------- | :------ | :------- | :------- | :---------- |
| CU00        | COUSR00 | COUSR00C | List Users | Display all system users with role and status information |
| CU01        | COUSR01 | COUSR01C | Add User | Create new user accounts with role assignment and security setup |
| CU02        | COUSR02 | COUSR02C | Update User | Modify user information, roles, and security settings |
| CU03        | COUSR03 | COUSR03C | Delete User | Remove user accounts with proper security validation |

#### **Batch Processing (JCL)**

**Data Management and Setup**:
| Job      | Program  | Function | Description |
| :------- | :------- | :------- | :---------- |
| DUSRSECJ | IEBGENER | User Security Setup | Initialize USRSEC file with default admin and user accounts |
| DEFGDGB  | IDCAMS   | GDG Base Definition | Setup Generation Data Groups for backup and historical data management |
| CLOSEFIL | IEFBR14  | File Closure | Close VSAM files in CICS region before batch processing |
| OPENFIL  | IEFBR14  | File Opening | Make VSAM files available to CICS after batch processing |

**Master Data Loading**:
| Job      | Program  | Function | Description |
| :------- | :------- | :------- | :---------- |
| ACCTFILE | IDCAMS   | Account Master Load | Load account data from flat files to VSAM with validation and error handling |
| CARDFILE | IDCAMS   | Card Master Load | Load credit card data with account linkage validation |
| CUSTFILE | IDCAMS   | Customer Master Load | Load customer demographic and contact information |
| XREFFILE | IDCAMS   | Cross-Reference Load | Load card-to-account-to-customer relationships for efficient data access |

**Reference Data Management**:
| Job      | Program  | Function | Description |
| :------- | :------- | :------- | :---------- |
| DISCGRP  | IDCAMS   | Disclosure Groups | Load regulatory disclosure group definitions |
| TRANCATG | IDCAMS   | Transaction Categories | Load transaction category types for classification |
| TRANTYPE | IDCAMS   | Transaction Types | Load transaction type definitions and processing rules |
| TCATBALF | IDCAMS   | Category Balances | Load transaction category balance tracking data |

**Transaction Processing Workflow**:
| Job      | Program  | Function | Description |
| :------- | :------- | :------- | :---------- |
| TRANFILE | IDCAMS   | Transaction Master Load | Initialize transaction file structure |
| TRANBKP  | IDCAMS   | Transaction Backup | Create backup copies of transaction data before processing |
| POSTTRAN | CBTRN02C | Daily Transaction Posting | Process daily transactions, validate against accounts, update balances |
| COMBTRAN | SORT     | Transaction Consolidation | Combine and sort transaction files for reporting |
| INTCALC  | CBACT04C | Interest Calculation | Calculate and post interest charges to accounts |

**Reporting and Indexing**:
| Job      | Program  | Function | Description |
| :------- | :------- | :------- | :---------- |
| CREASTMT | CBSTM03A | Statement Generation | Generate customer statements in text and HTML formats |
| TRANIDX  | IDCAMS   | Transaction Indexing | Define alternate indexes for efficient transaction file access |

**Batch Processing Dependencies**:

The batch jobs follow a specific execution sequence to maintain data integrity:

1. **Setup Phase**: 
   ```
   CLOSEFIL → ACCTFILE → CARDFILE → CUSTFILE → XREFFILE → OPENFIL
   ```

2. **Daily Processing Cycle**: 
   ```
   POSTTRAN → INTCALC → COMBTRAN → CREASTMT
   ```

3. **Maintenance Operations**: 
   ```
   TRANBKP → TRANIDX → Performance optimization
   ```

**Critical Dependencies**:
- Files must be closed in CICS before batch processing
- Master data loading must complete before transaction processing
- Transaction posting must precede interest calculations
- Statement generation requires completed transaction processing

<br/>

### User Interface Architecture

#### **BMS (Basic Mapping Support) Design Patterns**

The application uses BMS mapsets to define screen layouts and user interactions. Each screen follows consistent design patterns:

**Screen Header**: Standard header with transaction ID, program name, current date/time, and system identification
**Navigation Area**: Function key assignments and menu options
**Data Entry Fields**: Input fields with validation attributes and field-level help
**Message Area**: Error messages, confirmations, and user guidance
**Status Information**: User context, session information, and processing status

#### **Screen Flow Architecture**

**Authentication Flow**:
- **Signon Screen (COSGN00)**: User authentication with credential validation
- **Role-based Routing**: Automatic navigation to appropriate menu based on user type
- **Session Management**: Maintains user context throughout the session

**Main Application Screens**:

**Signon Screen (CC00 - COSGN00)**
- User ID and password entry with field-level validation
- Security integration with USRSEC file for authentication
- Automatic routing to Main Menu (CM00) or Admin Menu (CA00) based on user role
- Error handling for invalid credentials and account lockout

![Alt text](./diagrams/Signon-Screen.png?raw=true "Signon Screen")

**Main Menu (CM00 - COMEN01)**
- Dynamic menu generation based on user permissions
- Numbered option selection with validation
- Context-sensitive help and navigation
- Integration with all user-accessible functions

![Alt text](./diagrams/Main-Menu.png?raw=true "Main Menu")

**Admin Menu (CA00 - COADM01)**
- Administrative function access with elevated privileges
- User management operations (create, update, delete users)
- System administration and maintenance functions
- Audit trail and security logging

![Alt text](./diagrams/Admin-Menu.png?raw=true "Admin Menu")

#### **Screen Development Patterns**

**BMS Map Structure**: Each screen consists of:
- **Mapset Definition** (.bms file): Field positions, attributes, colors, and layout
- **COBOL Program** (.cbl file): Business logic, validation, and data processing
- **Copybook Integration**: Shared data structures and communication areas

**Field Attributes**: Standardized DFHMDF attributes:
- **ASKIP**: Protected display-only fields
- **UNPROT**: User input fields with validation
- **FSET**: Fields that retain data across screen refreshes
- **BRT/NORM**: Highlighting for emphasis and error indication

**Error Handling Patterns**:
- Standardized error message area at bottom of screen
- Field-level cursor positioning for validation errors
- Color coding (RED for errors, GREEN for success, YELLOW for warnings)
- Consistent message formatting and user guidance

**Navigation Standards**:
- **ENTER**: Process current screen input
- **F3**: Exit/Return to previous screen or main menu
- **Clear**: Reset screen to initial state
- **PA Keys**: Context-sensitive help and shortcuts

**Data Validation**:
- Client-side validation through BMS field attributes
- Server-side validation in COBOL programs
- Cross-field validation and business rule enforcement
- Real-time feedback for user input errors

<br/>

## Support

If you have questions or requests for improvement please raise an issue in the repository.

<br/>

## Modernization Use Cases

### Legacy Code Patterns for Analysis

CardDemo demonstrates common mainframe programming patterns that modernization tools need to analyze and transform:

**1. COBOL Programming Constructs**
- Traditional COBOL data structures with COMP and COMP-3 variables
- PERFORM loops and conditional logic patterns
- File I/O operations with VSAM datasets
- COPY statement usage for shared data structures

**2. CICS Transaction Processing Patterns**
- Program-to-program communication using XCTL and COMMAREA
- BMS map handling for screen I/O operations
- CICS command usage for file operations and system services
- Transaction context management and error handling

**3. Data Access Patterns**
- VSAM file organization and access methods
- Cross-reference file relationships and data integrity
- Batch vs. online data access patterns
- Record locking and concurrency control

**4. Batch Processing Workflows**
- JCL job dependencies and execution sequences
- Data transformation and validation processes
- File backup and recovery procedures
- Report generation and output management

### Transformation Opportunities

**Service Extraction**: Individual COBOL programs can be analyzed for business logic extraction and conversion to microservices.

**API Development**: CICS transactions provide natural boundaries for REST API development with clear input/output interfaces.

**Data Modernization**: VSAM file structures can be mapped to relational or NoSQL databases while preserving data relationships.

**User Interface Modernization**: BMS maps provide screen layout and field definitions that can be transformed to modern web interfaces.

**Workflow Automation**: JCL batch processing sequences can be converted to cloud-native workflow orchestration.

### Modernization Strategies Demonstrated

**1. Strangler Fig Pattern**: Gradual replacement of mainframe components while maintaining system functionality
**2. Database Modernization**: Migration from VSAM to modern database systems
**3. API-First Approach**: Exposing mainframe business logic through modern APIs
**4. Cloud Migration**: Moving batch processing to cloud-native solutions
**5. User Experience Modernization**: Replacing terminal-based interfaces with web applications

## Development Patterns

### COBOL Programming Patterns

**Program Structure**: Standard IDENTIFICATION, ENVIRONMENT, DATA, and PROCEDURE divisions with consistent naming conventions.

**Data Definition**: Use of copybooks for shared data structures, COMP variables for performance, and level-88 condition names for readability.

**Error Handling**: Consistent RESP and RESP2 code checking for CICS operations with standardized error message handling.

**Modular Design**: Separation of business logic, data access, and presentation layers with clear program interfaces.

### CICS Programming Model

**Transaction Design**: Each transaction handles a specific business function with clear entry and exit points.

**Communication Areas**: Standardized COMMAREA structure for passing data between programs and maintaining session context.

**Screen Handling**: BMS mapset usage for consistent screen layouts and user interaction patterns.

**Resource Management**: Proper file opening/closing and resource cleanup in batch and online environments.

### Data Management Patterns

**VSAM File Design**: Key Sequenced Data Sets (KSDS) for efficient indexed access with alternate indexes for multiple access paths.

**Data Relationships**: Cross-reference files maintaining referential integrity between customers, accounts, and cards.

**Backup and Recovery**: Generation Data Groups (GDG) for maintaining historical data and backup copies.

**Concurrency Control**: Record-level locking in CICS for data integrity in multi-user environments.

## Roadmap

### Planned Enhancements

**1. Database Integration**
- **Relational Database Support**: Integration with Db2 for modern SQL access patterns
- **Hierarchical Database**: IMS database calls for legacy data access scenarios
- **NoSQL Integration**: Document database support for flexible data models

**2. Modern Integration Patterns**
- **File Transfer Protocols**: FTP/SFTP integration for external data exchange
- **Message Queue Integration**: MQ support for asynchronous processing and event-driven architecture
- **API Gateway**: REST API exposure of mainframe transactions for distributed application integration
- **Event Streaming**: Real-time data streaming for modern analytics and monitoring

**3. Cloud-Native Features**
- **Containerization**: Docker support for portable deployment
- **Microservices Architecture**: Service decomposition examples and patterns
- **Observability**: Logging, monitoring, and tracing integration
- **DevOps Integration**: CI/CD pipeline examples for mainframe applications

**4. Modernization Tooling**
- **Code Analysis**: Enhanced static analysis capabilities for transformation planning
- **Migration Utilities**: Automated conversion tools for common modernization scenarios
- **Testing Framework**: Comprehensive test harness for validation during transformation
- **Documentation Generation**: Automated documentation from legacy code analysis

<br/>

## Contributing

We are looking forward to receiving contributions and enhancements to this initial codebase from the mainframe code base

Feel free to raise issues, create code and raise merge requests for enhancements so that we can build out this application as a resource for programmers wanting to understand and modernize their mainframes.

<br/>

## License

This is intended to be a community resource and it is released under the Apache 2.0 license.

<br/>

## Project Status

**Current Version**: v1.0 - Production Ready

The CardDemo application is actively maintained and serves as a comprehensive reference implementation for mainframe modernization scenarios. The codebase demonstrates production-quality mainframe development patterns and is suitable for:

- **Educational Use**: Learning mainframe technologies and programming patterns
- **Modernization Planning**: Analyzing legacy code structures and transformation opportunities  
- **Tool Development**: Testing and validating modernization tools and frameworks
- **Proof of Concepts**: Demonstrating modernization approaches and integration patterns

**Upcoming Enhancements**: 
- Enhanced database integration (Db2, IMS)
- Modern integration patterns (APIs, messaging)
- Cloud-native deployment options
- Advanced modernization tooling examples

**Community Contributions**: We welcome contributions that enhance the application's value as a modernization learning resource. See the [Contributing](#contributing) section for guidelines.

<br/>


