# Mermaid Diagrams Code

## Hướng dẫn sử dụng:

1. Copy code Mermaid của từng diagram
2. Vào trang https://mermaid.live/ 
3. Paste code vào editor
4. Export ảnh PNG (recommend: Scale 2x, transparent background)
5. Save ảnh với tên file đúng như path bên dưới
6. Copy ảnh vào folder `static/images/5-Workshop/5.1-Workshop-overview/`

---

## 1. Architecture Diagram

**File path:** `static/images/5-Workshop/5.1-Workshop-overview/architecture-diagram.png`

**Mermaid code:**

```mermaid
graph TB
    subgraph "Client Layer"
        A[Web Browser<br/>React SPA]
    end

    subgraph "CDN & Edge"
        B[CloudFront Distribution<br/>dutf3c70nnjzl]
    end

    subgraph "Static Hosting"
        C[S3 Frontend Bucket<br/>smartdocai-frontend-...]
    end

    subgraph "API Layer"
        D[API Gateway REST<br/>d60866voq5]
    end

    subgraph "Compute Layer"
        E[Lambda Function<br/>smartdocai<br/>Docker Container]
    end

    subgraph "Scheduled Tasks"
        F[EventBridge Rule<br/>rate 5 minutes<br/>cleanup-unconfirmed]
    end

    subgraph "Authentication"
        G[Cognito User Pool<br/>us-east-1_3oq5wIiuu<br/>Email + Google OAuth]
    end

    subgraph "Storage Layer"
        H[S3 Storage Bucket<br/>smartdocai-storage-...<br/>Documents + Avatars]
        I[DynamoDB Table<br/>smartdocai-user-profiles<br/>User metadata]
    end

    subgraph "AI Services"
        J[Amazon Bedrock<br/>Mixtral 8x7B LLM<br/>Titan Embeddings V2]
    end

    subgraph "Container Registry"
        K[ECR Repository<br/>smartdocai<br/>Docker Images]
    end

    subgraph "CI/CD Pipeline"
        L[CodePipeline<br/>smartdocai-be-pipeline]
        M[CodeBuild<br/>smartdocai-be-build<br/>pytest + Docker build]
        N[GitHub Repository<br/>TakunKenjo/SmartdocAI-AWS]
    end

    subgraph "Monitoring"
        O[CloudWatch Logs<br/>/aws/lambda/smartdocai]
    end

    %% User flow
    A -->|HTTPS| B
    B -->|Cache Miss| C
    B -->|API Requests| D
    D -->|Invoke| E
    
    %% Scheduled tasks
    F -->|Trigger every 5min| E
    
    %% Backend dependencies
    E -->|JWT Validation| G
    E -->|Store/Retrieve Files| H
    E -->|Query/Update Profile| I
    E -->|RAG Processing| J
    E -->|Write Logs| O
    
    %% CI/CD flow
    N -->|Push to main| L
    L -->|Trigger Build| M
    M -->|pytest + Docker build| K
    K -->|Deploy Container| E

    style A fill:#e1f5ff
    style E fill:#ff9999
    style G fill:#ffffcc
    style J fill:#ccffcc
    style F fill:#ffccff
```

---

## 2. Authentication Flow (Register → Confirm → Login)

**File path:** `static/images/5-Workshop/5.1-Workshop-overview/auth-flow.png`

**Mermaid code:**

```mermaid
sequenceDiagram
    participant U as User Browser
    participant CF as CloudFront
    participant AG as API Gateway
    participant L as Lambda
    participant C as Cognito
    participant DB as DynamoDB
    
    Note over U,DB: 1. REGISTRATION
    U->>CF: POST /auth/register<br/>(email, password, fullname, phone, DOB)
    CF->>AG: Forward request
    AG->>L: Invoke Lambda
    L->>L: Validate inputs<br/>(phone +84, DOB 1900-2026, XSS check)
    L->>C: sign_up(email, password)
    C-->>L: User created (UNCONFIRMED)
    C->>U: Send verification code via email
    L-->>U: 200 OK (User created)
    
    Note over U,DB: 2. EMAIL CONFIRMATION
    U->>AG: POST /auth/confirm-signup<br/>(email, code)
    AG->>L: Invoke Lambda
    L->>C: confirm_sign_up(code)
    C-->>L: Status: CONFIRMED
    L->>DB: Create profile record
    DB-->>L: Profile created
    L-->>U: 200 OK (Confirmed)
    
    Note over U,DB: 3. LOGIN
    U->>AG: POST /auth/login<br/>(email, password)
    AG->>L: Invoke Lambda
    L->>C: initiate_auth()
    C-->>L: JWT tokens (ID + Access)
    L-->>U: 200 OK + JWT tokens
    U->>U: Store tokens in localStorage
```

---

## 3. Document Upload & RAG Query Flow

**File path:** `static/images/5-Workshop/5.1-Workshop-overview/rag-flow.png`

**Mermaid code:**

```mermaid
sequenceDiagram
    participant U as User Browser
    participant L as Lambda
    participant S3 as S3 Storage
    participant F as FAISS Vector DB
    participant B as Bedrock<br/>(Titan + Mixtral)
    
    Note over U,B: 1. DOCUMENT UPLOAD
    U->>L: GET /documents/upload-url
    L->>S3: Generate presigned URL (1h expiry)
    S3-->>L: Presigned URL
    L-->>U: Return presigned URL
    U->>S3: PUT file directly to S3<br/>(bypass Lambda)
    S3-->>U: Upload complete
    
    Note over U,B: 2. DOCUMENT INDEXING
    U->>L: POST /documents<br/>(notify about new file)
    L->>S3: Download document
    S3-->>L: PDF/DOCX file
    L->>L: Parse content (PDF→text)
    L->>L: Chunk text<br/>(500 tokens, 50 overlap)
    L->>B: Generate embeddings (Titan V2)
    B-->>L: Vector embeddings (1024-dim)
    L->>F: Store vectors in FAISS<br/>(in-memory, per-user)
    F-->>L: Index updated
    L-->>U: 200 OK (Document indexed)
    
    Note over U,B: 3. RAG QUERY
    U->>L: POST /chat<br/>(query: "What is X?")
    L->>F: Semantic search (retrieve top-k chunks)
    F-->>L: Relevant chunks
    L->>B: Prompt: Context + Query<br/>(Mixtral 8x7B)
    B-->>L: Generated answer
    L-->>U: 200 OK + answer + sources
```

---

## 4. EventBridge Auto-Cleanup Flow

**File path:** `static/images/5-Workshop/5.1-Workshop-overview/eventbridge-cleanup.png`

**Mermaid code:**

```mermaid
sequenceDiagram
    participant EB as EventBridge Rule<br/>(rate: 5 minutes)
    participant L as Lambda
    participant C as Cognito User Pool
    participant CW as CloudWatch Logs
    
    Note over EB,CW: SCHEDULED CLEANUP (Every 5 minutes)
    EB->>L: Invoke Lambda<br/>{"source": "aws.events"}
    L->>L: Route to cleanup_unconfirmed_users()<br/>(max_age: 5 minutes)
    L->>C: list_users(Filter: UNCONFIRMED)
    C-->>L: Return unconfirmed users list
    
    loop For each user
        L->>L: Check UserCreateDate<br/>If > 5 minutes ago
        alt User expired
            L->>C: admin_delete_user(username)
            C-->>L: User deleted
            L->>CW: Log: "Deleted user {username}"
        else User still valid
            L->>CW: Log: "Skipped user {username}"
        end
    end
    
    L->>CW: Log: "Cleanup complete. Deleted X users"
    L-->>EB: Execution complete
```

---

## Export Settings (recommend):

- **Format:** PNG
- **Background:** Transparent (hoặc white nếu cần)
- **Scale:** 2x (cho chất lượng cao)
- **Theme:** Default

## Checklist sau khi export:

- [ ] architecture-diagram.png (14 AWS services)
- [ ] auth-flow.png (Registration → Confirmation → Login)
- [ ] rag-flow.png (Upload → Indexing → Query)
- [ ] eventbridge-cleanup.png (Scheduled cleanup)
- [ ] lambda-modules.png (Lambda code structure tree)
- [ ] cicd-pipeline.png (GitHub → CodePipeline → CodeBuild → ECR → Lambda)
- [ ] three-tier-architecture.png (3-tier layering model)
- [ ] storage-structure.png (S3 + DynamoDB data model)
- [ ] rag-pipeline-simple.png (6-step RAG process flowchart)
- [ ] cleanup-resources-flow.png (3-phase simplified cleanup flow)
- [ ] Copy tất cả vào: `static/images/5-Workshop/5.1-Workshop-overview/`

---

## 10. AWS Resources Cleanup Flow (Simplified)

**File path:** `static/images/5-Workshop/5.6-Conclusion/cleanup-resources-flow.png`

**Mermaid code:**

```mermaid
flowchart LR
    Start([Start Cleanup]) --> Phase1
    
    subgraph Phase1["Phase 1: Stop Traffic<br/>(3 min)"]
        E1[EventBridge Rule]
        E2[CloudFront Distribution<br/>⏱️ 30min wait]
        E3[API Gateway]
        E1 --> E2 --> E3
    end
    
    Phase1 --> Phase2
    
    subgraph Phase2["Phase 2: Remove Compute<br/>(5 min)"]
        C1[CodePipeline + CodeBuild]
        C2[Lambda Function]
        C3[ECR Repository]
        C1 --> C2 --> C3
    end
    
    Phase2 --> Phase3
    
    subgraph Phase3["Phase 3: Delete Data<br/>(5 min)"]
        D1[S3 Buckets x2<br/>Frontend + Storage]
        D2[DynamoDB Table]
        D3[Cognito User Pool]
        D4[IAM Roles]
        D1 --> D2 --> D3 --> D4
    end
    
    Phase3 --> Verify{Verify<br/>All Deleted?}
    Verify -->|Yes| Done([✅ Complete])
    Verify -->|No| Error[Check Dependencies]
    Error --> Phase1
    
    style Start fill:#e1f5ff
    style Phase1 fill:#ffccff,stroke:#ff66cc,stroke-width:3px
    style Phase2 fill:#ffffcc,stroke:#ffcc00,stroke-width:3px
    style Phase3 fill:#ccffcc,stroke:#66cc66,stroke-width:3px
    style E2 fill:#ff9999
    style Done fill:#66ff66
    style Error fill:#ffcccc
```

---

## 5. Lambda Modules Structure

**File path:** `static/images/5-Workshop/5.1-Workshop-overview/lambda-modules.png`

**Mermaid code:**

```mermaid
graph TB
    subgraph "app_api.py - Main Lambda Handler"
        A[FastAPI Application]
        A --> B["/auth"]
        A --> C["/documents"]
        A --> D["/chat"]
        A --> E["/profile"]
        A --> F["/cleanup"]
    end
    
    subgraph "modules/ - Business Logic"
        G["auth_service.py<br/>Cognito integration + validators"]
        H["profile_service.py<br/>DynamoDB CRUD"]
        I["document_processor.py<br/>PDF/DOCX parsing"]
        J["vector_store.py<br/>FAISS vector DB"]
        K["rag_chain.py<br/>LangChain RAG pipeline"]
        L["self_rag.py<br/>Self-correcting RAG"]
    end
    
    B -.-> G
    C -.-> I
    C -.-> J
    D -.-> K
    D -.-> L
    E -.-> H
    F -.-> G
    
    style A fill:#ff9999
    style B fill:#e1f5ff
    style C fill:#e1f5ff
    style D fill:#e1f5ff
    style E fill:#e1f5ff
    style F fill:#e1f5ff
```

---

## 6. CI/CD Pipeline Flow

**File path:** `static/images/5-Workshop/5.1-Workshop-overview/cicd-pipeline.png`

**Mermaid code:**

```mermaid
graph LR
    A["GitHub Repository<br/>TakunKenjo/SmartdocAI-AWS<br/>(main branch)"] -->|Push trigger| B["CodePipeline<br/>smartdocai-be-pipeline<br/>(Orchestrator)"]
    
    B -->|Source stage| C["CodeBuild<br/>smartdocai-be-build"]
    
    C -->|1. Install deps| C1["pip install -r requirements.txt"]
    C -->|2. Lint code| C2["flake8 (exit-zero)"]
    C -->|3. Run tests| C3["pytest (~60 tests)<br/>HARD FAIL if tests fail"]
    C -->|4. Build Docker| C4["docker build -t smartdocai"]
    
    C4 -->|Push image| D["ECR Repository<br/>smartdocai<br/>(Container Registry)"]
    
    D -->|Deploy stage| E["Lambda Function<br/>smartdocai<br/>(Update with new image)"]
    
    style A fill:#e1f5ff
    style B fill:#ffffcc
    style C fill:#ffcccc
    style D fill:#ccffcc
    style E fill:#ff9999
```

---

## 7. Three-Tier Architecture

**File path:** `static/images/5-Workshop/5.1-Workshop-overview/three-tier-architecture.png`

**Mermaid code:**

```mermaid
graph TB
    subgraph "Presentation Layer - Tầng giao diện"
        P1[CloudFront CDN]
        P2[S3 Static Hosting]
        P1 --> P2
    end
    
    subgraph "Application Layer - Tầng xử lý"
        A1[API Gateway]
        A2[Lambda Function<br/>FastAPI + RAG]
        A3[EventBridge<br/>Scheduled Tasks]
        A1 --> A2
        A3 --> A2
    end
    
    subgraph "Data Layer - Tầng dữ liệu"
        D1[Cognito<br/>User Pool]
        D2[DynamoDB<br/>Profiles]
        D3[S3 Storage<br/>Documents]
        D4[Bedrock AI<br/>LLM + Embeddings]
    end
    
    P1 -.->|API Requests| A1
    A2 --> D1
    A2 --> D2
    A2 --> D3
    A2 --> D4
    
    style P1 fill:#e1f5ff
    style P2 fill:#e1f5ff
    style A1 fill:#ffffcc
    style A2 fill:#ff9999
    style A3 fill:#ffccff
    style D1 fill:#ccffcc
    style D2 fill:#ccffcc
    style D3 fill:#ccffcc
    style D4 fill:#ccffcc
```

---

## 8. Storage Structure (S3 + DynamoDB Data Model)

**File path:** `static/images/5-Workshop/5.1-Workshop-overview/storage-structure.png`

**Mermaid code:**

```mermaid
graph TB
    subgraph "S3 Bucket: smartdocai-storage-623035187993"
        S3["/"]
        S3 --> U1["/users/{user_id}/"]
        U1 --> D1["/documents/"]
        U1 --> A1["/avatar/"]
        D1 --> F1["document1.pdf<br/>document2.docx<br/>..."]
        A1 --> F2["profile.jpg"]
    end
    
    subgraph "DynamoDB Table: smartdocai-user-profiles"
        DB[Partition Key: user_id]
        DB --> ATTR["Attributes:<br/>• email<br/>• fullname<br/>• phone (+84...)<br/>• dob (YYYY-MM-DD)<br/>• avatar_url<br/>• created_at<br/>• updated_at"]
    end
    
    subgraph "Cognito User Pool: us-east-1_3oq5wIiuu"
        COG["User Attributes:<br/>• sub (UUID)<br/>• email<br/>• email_verified<br/>• custom:fullname<br/>• custom:phone<br/>• custom:dob"]
    end
    
    COG -.->|user_id = sub| DB
    DB -.->|avatar_url references| A1
    
    style S3 fill:#ccffcc
    style U1 fill:#e1f5ff
    style D1 fill:#ffffcc
    style A1 fill:#ffffcc
    style DB fill:#ff9999
    style COG fill:#ffccff
```

---

## 9. RAG Pipeline - Simple Flowchart

**File path:** `static/images/5-Workshop/5.1-Workshop-overview/rag-pipeline-simple.png`

**Mermaid code:**

```mermaid
flowchart TD
    Start([User uploads document]) --> Step1[1. Parse Document<br/>PDF/DOCX → Text]
    Step1 --> Step2[2. Text Chunking<br/>500 tokens per chunk<br/>50 tokens overlap]
    Step2 --> Step3[3. Generate Embeddings<br/>Titan V2<br/>1024 dimensions]
    Step3 --> Step4[4. Store Vectors<br/>FAISS in-memory<br/>per-user isolation]
    Step4 --> Ready([Document indexed])
    
    Query([User query]) --> Step5[5. Semantic Search<br/>Retrieve top-k chunks<br/>from FAISS]
    Step5 --> Step6[6. LLM Generation<br/>Context + Query<br/>Mixtral 8x7B]
    Step6 --> Response([Answer + Sources])
    
    style Start fill:#e1f5ff
    style Step1 fill:#ffffcc
    style Step2 fill:#ffffcc
    style Step3 fill:#ffcccc
    style Step4 fill:#ccffcc
    style Ready fill:#e1f5ff
    style Query fill:#e1f5ff
    style Step5 fill:#ccffcc
    style Step6 fill:#ffcccc
    style Response fill:#e1f5ff
```
