# Zellis Group Overview

Zellis Group is a leading provider of payroll, HR, and employee benefits solutions in the UK, Ireland, and globally. This document summarizes its products and recent acquisition of Hastee.

## Products and Acquisition Diagram

The following diagram illustrates Zellis Group’s business units (Zellis, Moorepay, Benefex) and the Hastee acquisition, highlighting their services and customer segments.

```mermaid
graph TD
A[Zellis Group] --> B[Zellis]
A --> C[Moorepay]
A --> D[Benefex]
A --> E[Hastee<br/><i>Acquired June 2025</i>]
A --> J[Elementsuite<br/><i>Acquired January 2025</i>]

    B -->|HR, WFM, Payroll Software<br/>Large Enterprises| F[Customers<br/>~1/3 FTSE 100]
    C -->|HR, Payroll Services<br/>SMEs 50-1000| G[Customers<br/>Thousands of SMEs]
    D -->|Employee Benefits Platform<br/>Global, 2M+ Employees| H[Customers<br/>900+ Companies]
    E -->|Financial Wellbeing<br/>Integrated into HR Offerings| B
    J -->|AI-enabled HR, WorkForceManagement Software<br/>Retail, Hospitality, Manufacturing| K[Customers<br/>McDonald's, Travelodge, Five Guys]

    %% Note about Hastee
    E --> I[Note: Hastee Acquisition<br/>- Enhances financial wellbeing<br/>- Expands HR capabilities]

    %% Note about elementsuite
    J --> L[Note: elementsuite Acquisition<br/>- Enhances AI-powered HR suite<br/>- Expands enterprise offerings]

    %% Styling
    classDef parent fill:#f9f,stroke:#333,stroke-width:2px;
    class A parent;
    classDef business fill:#dfd,stroke:#333,stroke-width:2px;
    class B,C,D,E,J business;
    classDef customer fill:#bbf,stroke:#333,stroke-width:2px;
    class F,G,H,K customer;
    classDef note fill:#fff,stroke:#333,stroke-width:1px;
    class I,L note;
```

## Elementsuite Function Summary
```mermaid
graph TD
    A[elementsuite<br/><i>AI-enabled HR & WFM Software</i>] --> B[Core Functions]
    
    B --> C[Recruitment & Onboarding<br/><i>Streamlined hiring, seamless onboarding</i>]
    B --> D[Workforce Management<br/><i>Labour forecasting, scheduling, shift management</i>]
    B --> E[HR Management<br/><i>Employee lifecycle, self-service, analytics</i>]
    B --> F[Mobile-Friendly Platform<br/><i>Accessible anywhere, anytime</i>]
    B --> G[AI-Powered Features<br/><i>Automation, insights, efficiency</i>]

    C --> H[Benefits<br/>- Reduces recruitment costs<br/>- Enhances candidate experience]
    D --> I[Benefits<br/>- Optimizes labour costs<br/>- Improves team performance]
    E --> J[Benefits<br/>- Centralizes HR processes<br/>- Empowers employees]
    F --> K[Benefits<br/>- Increases accessibility<br/>- Boosts productivity]
    G --> L[Benefits<br/>- Streamlines operations<br/>- Enhances decision-making]

    %% Integration with Zellis
    A --> M[Integration with Zellis<br/><i>Combines with payroll & HR platform</i>]
    M --> N[Enhanced Offering<br/>- End-to-end HR & payroll<br/>- Serves enterprises like McDonald's, Travelodge]

    %% Styling
    classDef parent fill:#f9f,stroke:#333,stroke-width:2px;
    class A parent;
    classDef function fill:#dfd,stroke:#333,stroke-width:2px;
    class B,C,D,E,F,G function;
    classDef benefit fill:#bbf,stroke:#333,stroke-width:2px;
    class H,I,J,K,L benefit;
    classDef integration fill:#ffb,stroke:#333,stroke-width:2px;
    class M,N integration;
```


## ElementSuite Questions

Will I get chance to work on / learn RUST ?

### Architecture 

- Is there an integration exercise incoming / ongoing?
- What are the databases, APIS in/out, messages in/out ?
- Where is it deployed to / run on ?
- What are the volumes ?


### Data Model ???

```mermaid
erDiagram
EMPLOYEE ||--o{ JOB_APPLICATION : submits
EMPLOYEE ||--o{ SCHEDULE : assigned
EMPLOYEE ||--o{ PERFORMANCE_RECORD : has
EMPLOYEE ||--o{ TRAINING : completes
JOB_APPLICATION ||--o{ POSITION : applies_for
SCHEDULE ||--o{ SHIFT : contains
POSITION ||--o{ DEPARTMENT : belongs_to
PERFORMANCE_RECORD ||--o{ KPI : tracks
TRAINING ||--o{ COURSE : includes

    EMPLOYEE {
        int employee_id PK
        string first_name
        string last_name
        string email
        date hire_date
        string role
    }
    JOB_APPLICATION {
        int application_id PK
        int employee_id FK
        int position_id FK
        date application_date
        string status
    }
    POSITION {
        int position_id PK
        string title
        int department_id FK
        string description
    }
    DEPARTMENT {
        int department_id PK
        string name
        string manager_id FK
    }
    SCHEDULE {
        int schedule_id PK
        int employee_id FK
        date start_date
        date end_date
    }
    SHIFT {
        int shift_id PK
        int schedule_id FK
        datetime start_time
        datetime end_time
        string location
    }
    PERFORMANCE_RECORD {
        int record_id PK
        int employee_id FK
        date review_date
        string feedback
    }
    KPI {
        int kpi_id PK
        int record_id FK
        string metric_name
        float value
    }
    TRAINING {
        int training_id PK
        int employee_id FK
        int course_id FK
        date completion_date
    }
    COURSE {
        int course_id PK
        string title
        string type
        int duration_hours
    }
```

### Look Through Job Spec 

