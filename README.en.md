# Vibe Coding MCP Design

[中文](README.md) | English

## Overview
Following the principle of "write documentation only, no code", this project designs a set of tools to support documentation workflow, generate structured document content, and store these contents as structured data in a database.

## Core Requirements
- Manage project requirements, testing, and issue tracking through fixed document formats (feature descriptions, UI styles, usage processes, layouts, test cases, bug records, release records).
- These documents need to be refined into structured data and stored in the database according to preset field requirements.
- Each document format needs supporting tools for:
  - Extraction: Extract key information from documents and convert it to structured data.
  - Reading: Read structured data from the database and generate documents in fixed formats.

### Specific Operation Process Includes:
1. Write requirement documents, generate initial structured data.
2. Write test cases, generate acceptance results.
3. Record bugs, regenerate related documents or data.

## Key Points Analysis

### Fixed Formats
7 document formats (feature descriptions, UI styles, usage processes, layouts, test cases, bug records, release records) require clear field names for each format to ensure data structure is unified and easy to extract and store.

### Tool Design
Each format needs two types of functionality:
- Extraction tools: Extract key information from user-submitted documents and convert it to structured data for database storage.
- Reading tools: Read structured data from the database and generate documents in fixed formats.

### Database Storage
Need to design a database schema that can accommodate fields for all formats. Fields need to be generic and flexible to adapt to different format requirements.

### Operation Loop
Requirements -> Test Cases -> Acceptance Results -> Bug Records -> Regeneration, forming a complete workflow that tools need to support.

### Field Naming
Field names need to be intuitive, generic, and easy to understand while supporting different document formats. For example:
- "Feature Description" may need fields like "Feature Name", "Feature Objective", "Inputs/Outputs", etc.
- "Bug Record" needs fields like "Bug Description", "Severity", "Reproduction Steps", etc.

### Tool Design Details
Tools need to support document parsing (e.g., extracting data from Markdown, Word, or plain text) and mapping parsed results to database fields. Reading tools need to render database data into user-friendly document formats.

### Extraction and Reading
#### Extraction
Requires a parser that can extract information from unstructured or semi-structured documents based on preset field rules. For example, using regular expressions or natural language processing (NLP) technology to identify titles, lists, tables, etc. in documents.

#### Reading
Requires a template engine to fill structured data from the database into predefined document templates to generate formatted documents.

### Database Schema Design
The database needs a flexible schema to support fields for different document types, potentially using a relational database (like MySQL) or NoSQL database (like MongoDB) to accommodate dynamic fields.

### Workflow Support
Tools need to support closed-loop operations from requirements to bug records, for example, tracking relationships between requirements, test cases, and bugs through version control or associated IDs.

## Field Name Design
Design clear field names for each document format to ensure fields are intuitive and cover core document information. Here are the suggested field names for each format:

### Feature Description
- FeatureID: Feature unique identifier
- FeatureName: Feature name
- Description: Feature description
- Objective: Feature objective
- Inputs: Input parameters
- Outputs: Output results
- Dependencies: Dependencies
- Priority: Priority (High/Medium/Low)
- Status: Status (e.g., Pending, In Progress, Completed)
- CreatedAt: Creation time
- UpdatedAt: Update time

### UI Style
- UIID: UI design unique identifier
- ComponentName: Component name
- StyleDescription: Style description
- ColorScheme: Color scheme
- Typography: Typography
- LayoutType: Layout type (e.g., responsive, fixed)
- Assets: Related resources (e.g., images, icons)
- Status: Status (e.g., Designing, Completed)
- CreatedAt: Creation time
- UpdatedAt: Update time

### Usage Process
- FlowID: Process unique identifier
- FlowName: Process name
- Steps: Operation steps (list or JSON format)
- Actors: Participating roles (e.g., user, admin)
- Preconditions: Preconditions
- Postconditions: Postconditions
- Exceptions: Exception handling
- Status: Status (e.g., Pending, Confirmed)
- CreatedAt: Creation time
- UpdatedAt: Update time

### Layout
- LayoutID: Layout unique identifier
- LayoutName: Layout name
- Structure: Layout structure (e.g., grid, flow)
- Components: Component list
- Constraints: Layout constraints (e.g., screen size)
- ResponsiveRules: Responsive rules
- Status: Status (e.g., Designing, Completed)
- CreatedAt: Creation time
- UpdatedAt: Update time

### Test Case
- TestCaseID: Test case unique identifier
- TestCaseName: Test case name
- FeatureID: Associated feature ID
- Description: Test description
- Preconditions: Test preconditions
- Steps: Test steps
- ExpectedResult: Expected result
- ActualResult: Actual result
- Status: Status (e.g., Passed, Failed, Pending)
- CreatedAt: Creation time
- UpdatedAt: Update time

### Bug Record
- BugID: Bug unique identifier
- FeatureID: Associated feature ID
- TestCaseID: Associated test case ID
- Description: Bug description
- Severity: Severity (Critical/High/Medium/Low)
- ReproduceSteps: Reproduction steps
- Environment: Running environment (e.g., OS, browser)
- Status: Status (e.g., Open, Fixing, Fixed)
- Assignee: Assignee
- CreatedAt: Creation time
- UpdatedAt: Update time

### Release Record
- ReleaseID: Release record unique identifier
- FeatureID: Associated feature ID
- Version: Version number
- ReleaseDate: Release date
- Changes: Change content
- Status: Status (e.g., Planned, Released)
- Notes: Notes
- CreatedAt: Creation time
- UpdatedAt: Update time

## Database Schema Design
Design a unified database schema to support the above tools, using a relational database (MySQL as an example):

```sql
-- Feature Description Table
CREATE TABLE Features (
    FeatureID VARCHAR(50) PRIMARY KEY,
    FeatureName VARCHAR(100),
    Description TEXT,
    Objective TEXT,
    Inputs TEXT,
    Outputs TEXT,
    Dependencies TEXT,
    Priority ENUM('High', 'Medium', 'Low'),
    Status ENUM('Pending', 'InProgress', 'Completed'),
    CreatedAt DATETIME,
    UpdatedAt DATETIME
);

-- UI Style Table
CREATE TABLE UIStyles (
    UIID VARCHAR(50) PRIMARY KEY,
    ComponentName VARCHAR(100),
    StyleDescription TEXT,
    ColorScheme TEXT,
    Typography TEXT,
    LayoutType VARCHAR(50),
    Assets TEXT,
    Status ENUM('Designing', 'Completed'),
    CreatedAt DATETIME,
    UpdatedAt DATETIME
);

-- Usage Process Table
CREATE TABLE Flows (
    FlowID VARCHAR(50) PRIMARY KEY,
    FlowName VARCHAR(100),
    Steps TEXT,
    Actors TEXT,
    Preconditions TEXT,
    Postconditions TEXT,
    Exceptions TEXT,
    Status ENUM('Pending', 'Confirmed'),
    CreatedAt DATETIME,
    UpdatedAt DATETIME
);

-- Layout Table
CREATE TABLE Layouts (
    LayoutID VARCHAR(50) PRIMARY KEY,
    LayoutName VARCHAR(100),
    Structure TEXT,
    Components TEXT,
    Constraints TEXT,
    ResponsiveRules TEXT,
    Status ENUM('Designing', 'Completed'),
    CreatedAt DATETIME,
    UpdatedAt DATETIME
);

-- Test Case Table
CREATE TABLE TestCases (
    TestCaseID VARCHAR(50) PRIMARY KEY,
    TestCaseName VARCHAR(100),
    FeatureID VARCHAR(50),
    Description TEXT,
    Preconditions TEXT,
    Steps TEXT,
    ExpectedResult TEXT,
    ActualResult TEXT,
    Status ENUM('Passed', 'Failed', 'Pending'),
    CreatedAt DATETIME,
    UpdatedAt DATETIME,
    FOREIGN KEY (FeatureID) REFERENCES Features(FeatureID)
);

-- Bug Record Table
CREATE TABLE Bugs (
    BugID VARCHAR(50) PRIMARY KEY,
    FeatureID VARCHAR(50),
    TestCaseID VARCHAR(50),
    Description TEXT,
    Severity ENUM('Critical', 'High', 'Medium', 'Low'),
    ReproduceSteps TEXT,
    Environment TEXT,
    Status ENUM('Open', 'Fixing', 'Fixed'),
    Assignee VARCHAR(100),
    CreatedAt DATETIME,
    UpdatedAt DATETIME,
    FOREIGN KEY (FeatureID) REFERENCES Features(FeatureID),
    FOREIGN KEY (TestCaseID) REFERENCES TestCases(TestCaseID)
);

-- Release Record Table
CREATE TABLE Releases (
    ReleaseID VARCHAR(50) PRIMARY KEY,
    FeatureID VARCHAR(50),
    Version VARCHAR(50),
    ReleaseDate DATE,
    Changes TEXT,
    Status ENUM('Planned', 'Released'),
    Notes TEXT,
    CreatedAt DATETIME,
    UpdatedAt DATETIME,
    FOREIGN KEY (FeatureID) REFERENCES Features(FeatureID)
);
```

## Workflow Support
The tools support the following workflows:

### Write Requirements -> Generate
1. User submits feature description document, uses "Feature Description Extraction Tool" to extract data and store in Features table.
2. Uses "Feature Description Reading Tool" to generate formatted requirement document.

### Write Test Cases -> Acceptance Results
1. User submits test case document, uses "Test Case Extraction Tool" to extract data and store in TestCases table, associating with FeatureID.
2. After test execution, updates ActualResult and uses "Test Case Reading Tool" to generate acceptance result document.

### Record Bugs -> Regenerate
1. User submits bug record document, uses "Bug Record Extraction Tool" to extract data and store in Bugs table, associating with FeatureID and TestCaseID.
2. Uses "Bug Record Reading Tool" to generate bug report, or regenerates related documents after fixes.

## Summary
By designing fields and supporting extraction/reading tools for each document format, combined with a unified database schema, we can achieve conversion from documents to structured data and support closed-loop workflow of requirements, testing, and bug tracking. The tool design emphasizes user-friendliness, automated document parsing and generation processes, intuitive and flexible field names, and database support for relational queries to track data relationships. 