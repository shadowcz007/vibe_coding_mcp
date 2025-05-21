# vibe coding mcp 设计

[English](README.md) | 中文

## 概述
围绕"只写文档，不写代码"的原则，设计一套工具来支持文档化工作流程，生成结构化的文档内容，并将这些内容提炼为结构化数据存储到数据库中。

## 核心需求
- 希望通过固定的文档格式（功能描述、UI风格、使用流程、布局、测试用例、bug记录、上线记录）来管理项目的需求、测试和问题跟踪。
- 这些文档需要被提炼为结构化数据，按照预设的字段要求存储到数据库中。
- 每种文档格式需要有配套的工具，用于：
  - 提炼：从文档中提取关键信息，转换为结构化数据。
  - 读取：从数据库中读取结构化数据，生成符合固定格式的文档。

### 具体操作流程包括：
1. 编写需求文档，生成初始结构化数据。
2. 编写测试用例，生成验收结果。
3. 记录bug，重新生成相关文档或数据。

## 关键点分析

### 固定格式
7种文档格式（功能描述、UI风格、使用流程、布局、测试用例、bug记录、上线记录），需要为每种格式定义清晰的字段名称，确保数据结构统一且易于提取和存储。

### 工具设计
每种格式需要两类功能：
- 提炼工具：从用户提交的文档中提取关键信息，转换为结构化数据存储到数据库。
- 读取工具：从数据库中读取结构化数据，生成符合固定格式的文档。

### 数据库存储
需要设计一个数据库 schema，能够容纳所有格式的字段，字段需要通用且灵活，适应不同格式的需求。

### 操作闭环
需求 -> 测试用例 -> 验收结果 -> bug记录 -> 再次生成，形成一个完整的工作流，工具需要支持这一闭环。


### 字段命名
字段名称需要直观、通用、易于理解，同时支持不同格式的文档。例如：
- "功能描述"可能需要字段如"功能名称""功能目标""输入输出"等
- "bug记录"需要字段如"bug描述""严重性""复现步骤"等

### 工具设计
工具需要支持文档解析（例如从Markdown、Word或纯文本中提取数据），并将解析结果映射到数据库字段。读取工具则需要将数据库数据渲染为用户友好的文档格式。

### 提炼与读取
#### 提炼
需要一个解析器，能够根据预设字段规则，从非结构化或半结构化的文档中提取信息。例如，使用正则表达式或自然语言处理（NLP）技术识别文档中的标题、列表、表格等。

#### 读取
需要一个模板引擎，将数据库中的结构化数据填充到预定义的文档模板中，生成格式化的文档。

### 数据库设计
数据库需要一个灵活的 schema，支持不同文档类型的字段，可能使用关系型数据库（如MySQL）或NoSQL数据库（如MongoDB）以适应动态字段。

### 工作流支持
工具需要支持从需求到bug记录的闭环操作，例如通过版本控制或关联ID跟踪需求、测试用例和bug之间的关系。

## 字段名称设计
为每种文档格式设计清晰的字段名称，确保字段直观且能覆盖文档的核心信息。以下是每种格式的字段名称建议：

### 功能描述
- FeatureID：功能唯一标识
- FeatureName：功能名称
- Description：功能描述
- Objective：功能目标
- Inputs：输入参数
- Outputs：输出结果
- Dependencies：依赖项
- Priority：优先级（高/中/低）
- Status：状态（如待开发、开发中、已完成）
- CreatedAt：创建时间
- UpdatedAt：更新时间

### UI风格
- UIID：UI设计唯一标识
- ComponentName：组件名称
- StyleDescription：风格描述
- ColorScheme：配色方案
- Typography：字体样式
- LayoutType：布局类型（如响应式、固定）
- Assets：相关资源（如图片、图标）
- Status：状态（如设计中、已完成）
- CreatedAt：创建时间
- UpdatedAt：更新时间

### 使用流程
- FlowID：流程唯一标识
- FlowName：流程名称
- Steps：操作步骤（列表或JSON格式）
- Actors：参与角色（如用户、管理员）
- Preconditions：前置条件
- Postconditions：后置条件
- Exceptions：异常处理
- Status：状态（如待确认、已确认）
- CreatedAt：创建时间
- UpdatedAt：更新时间

### 布局
- LayoutID：布局唯一标识
- LayoutName：布局名称
- Structure：布局结构（如网格、流式）
- Components：包含的组件列表
- Constraints：布局约束（如屏幕尺寸）
- ResponsiveRules：响应式规则
- Status：状态（如设计中、已完成）
- CreatedAt：创建时间
- UpdatedAt：更新时间

### 测试用例
- TestCaseID：测试用例唯一标识
- TestCaseName：测试用例名称
- FeatureID：关联功能ID
- Description：测试描述
- Preconditions：测试前置条件
- Steps：测试步骤
- ExpectedResult：预期结果
- ActualResult：实际结果
- Status：状态（如通过、失败、待执行）
- CreatedAt：创建时间
- UpdatedAt：更新时间

### bug记录
- BugID：bug唯一标识
- FeatureID：关联功能ID
- TestCaseID：关联测试用例ID
- Description：bug描述
- Severity：严重性（紧急/高/中/低）
- ReproduceSteps：复现步骤
- Environment：运行环境（如OS、浏览器）
- Status：状态（如开放、修复中、已修复）
- Assignee：负责人
- CreatedAt：创建时间
- UpdatedAt：更新时间

### 上线记录
- ReleaseID：上线记录唯一标识
- FeatureID：关联功能ID
- Version：版本号
- ReleaseDate：上线日期
- Changes：变更内容
- Status：状态（如计划中、已上线）
- Notes：备注
- CreatedAt：创建时间
- UpdatedAt：更新时间

## 工具设计
为每种文档格式设计的工具，分为提炼工具和读取工具，所有工具遵循"只写文档，不写代码"的原则，重点描述功能、流程和数据处理方式。

### 1. 功能描述工具

#### 提炼工具
**功能**：从功能描述文档中提取关键信息，生成结构化数据。

**输入**：用户提交的文档（Markdown、Word或纯文本），包含功能名称、描述、目标、输入输出等。

**处理流程**：
1. 解析文档，识别标题（如"功能名称"）、段落（如描述）、列表（如输入输出）。
2. 按预设字段（FeatureID, FeatureName, Description等）提取内容。
3. 验证数据完整性（如功能名称是否为空）。
4. 将提取的数据存储到数据库的功能表中，生成唯一的FeatureID。

**输出**：结构化数据，存储到数据库。

#### 读取工具
**功能**：从数据库读取功能描述数据，生成格式化的功能描述文档。

**输入**：用户指定FeatureID或查询条件（如状态、优先级）。

**处理流程**：
1. 从数据库的功能表中查询指定数据。
2. 使用预定义的文档模板（例如Markdown模板），将数据填充到模板中。
3. 生成格式化的文档，包含标题、描述、输入输出等。

**输出**：格式化的功能描述文档（Markdown或PDF格式）。

### 2. UI风格工具

#### 提炼工具
**功能**：从UI风格文档中提取配色、字体、布局等信息。

**输入**：UI设计文档，包含风格描述、配色方案、字体样式等。

**处理流程**：
1. 解析文档，识别关键字段（如"配色方案：蓝色主题"）。
2. 提取字段内容，映射到UIID, ComponentName, ColorScheme等。
3. 存储到数据库的UI风格表中，生成唯一的UIID。

**输出**：结构化数据，存储到数据库。

#### 读取工具
**功能**：从数据库读取UI风格数据，生成UI设计文档。

**输入**：用户指定UIID或查询条件。

**处理流程**：
1. 查询数据库的UI风格表。
2. 使用模板将数据渲染为文档，包含配色、字体、布局等信息。

**输出**：格式化的UI风格文档。

### 3. 使用流程工具

#### 提炼工具
**功能**：从使用流程文档中提取步骤、角色、前置条件等。

**输入**：流程文档，包含步骤列表、角色描述等。

**处理流程**：
1. 解析文档，识别有序列表（步骤）、角色、前置条件等。
2. 提取字段，存储到数据库的流程表中，生成FlowID。

**输出**：结构化数据。

#### 读取工具
**功能**：生成使用流程文档。

**输入**：FlowID或查询条件。

**处理流程**：
1. 查询数据库的流程表。
2. 使用模板生成流程文档，包含步骤、角色等。

**输出**：格式化的流程文档。

### 4. 布局工具

#### 提炼工具
**功能**：从布局文档中提取结构、组件、约束等。

**输入**：布局文档，包含网格结构、组件列表等。

**处理流程**：
1. 解析文档，提取布局结构、组件等信息。
2. 存储到数据库的布局表中，生成LayoutID。

**输出**：结构化数据。

#### 读取工具
**功能**：生成布局文档。

**输入**：LayoutID或查询条件。

**处理流程**：
1. 查询数据库的布局表。
2. 使用模板生成布局文档。

**输出**：格式化的布局文档。

### 5. 测试用例工具

#### 提炼工具
**功能**：从测试用例文档中提取测试步骤、预期结果等。

**输入**：测试用例文档，包含描述、步骤、预期结果。

**处理流程**：
1. 解析文档，提取字段如TestCaseName, Steps, ExpectedResult。
2. 存储到数据库的测试用例表，生成TestCaseID，关联FeatureID。

**输出**：结构化数据。

#### 读取工具
**功能**：生成测试用例文档或验收结果。

**输入**：TestCaseID或查询条件。

**处理流程**：
1. 查询数据库，获取测试用例数据。
2. 生成测试用例文档或验收结果报告（包含ActualResult）。

**输出**：格式化的测试用例文档或验收报告。

### 6. bug记录工具

#### 提炼工具
**功能**：从bug记录文档中提取bug描述、严重性等。

**输入**：bug记录文档，包含描述、复现步骤等。

**处理流程**：
1. 解析文档，提取字段如Description, Severity, ReproduceSteps。
2. 存储到数据库的bug表，生成BugID，关联FeatureID和TestCaseID。

**输出**：结构化数据。

#### 读取工具
**功能**：生成bug记录文档。

**输入**：BugID或查询条件。

**处理流程**：
1. 查询数据库的bug表。
2. 使用模板生成bug记录文档。

**输出**：格式化的bug文档。

### 7. 上线记录工具

#### 提炼工具
**功能**：从上线记录文档中提取版本号、变更内容等。

**输入**：上线记录文档，包含版本号、变更描述等。

**处理流程**：
1. 解析文档，提取字段如Version, Changes。
2. 存储到数据库的上线记录表，生成ReleaseID。

**输出**：结构化数据。

#### 读取工具
**功能**：生成上线记录文档。

**输入**：ReleaseID或查询条件。

**处理流程**：
1. 查询数据库的上线记录表。
2. 使用模板生成上线记录文档。

**输出**：格式化的上线记录文档。

## 数据库 Schema 设计
为支持上述工具，设计一个统一的数据库 schema，使用关系型数据库（以MySQL为例）：

```sql
-- 功能描述表
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

-- UI风格表
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

-- 使用流程表
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

-- 布局表
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

-- 测试用例表
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

-- bug记录表
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

-- 上线记录表
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

## 工作流支持
工具支持以下工作流：

### 写需求 -> 生成
1. 用户提交功能描述文档，使用"功能描述提炼工具"提取数据存储到Features表。
2. 使用"功能描述读取工具"生成格式化的需求文档。

### 写测试用例 -> 验收结果
1. 用户提交测试用例文档，使用"测试用例提炼工具"提取数据存储到TestCases表，关联FeatureID。
2. 执行测试后更新ActualResult，使用"测试用例读取工具"生成验收结果文档。

### 记录bug -> 再次生成
1. 用户提交bug记录文档，使用"bug记录提炼工具"提取数据存储到Bugs表，关联FeatureID和TestCaseID。
2. 使用"bug记录读取工具"生成bug报告，或在修复后重新生成相关文档。

## 总结
通过为每种文档格式设计字段和配套的提炼/读取工具，结合统一的数据库 schema，可以实现从文档到结构化数据的转换，并支持需求、测试、bug跟踪的闭环工作流。工具设计注重用户友好性，文档解析和生成过程自动化，字段名称直观且灵活，数据库支持关联查询以跟踪数据关系。
