# 👨‍💻 Gintas Indriliunas — Projects

**Software Engineer · DevSecOps · Full Stack Developer**

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=flat&logo=linkedin&logoColor=white)](https://linkedin.com/in/gintas-indriliunas-75778b51)
[![GitHub](https://img.shields.io/badge/GitHub-181717?style=flat&logo=github&logoColor=white)](https://github.com/gindriliunas)

---

## 📋 Projects Overview

| Project | Stack | Type | Live |
|---|---|---|---|
| [🏥 Health & Fitness Booking Platform](#-health--fitness-booking-platform) | Next.js · AWS ECS · RDS · Terraform | Full Stack + DevSecOps | [Demo 1](https://loom.com/share/deb83c6d095744ad9b8379a9081086de) · [Demo 2](https://loom.com/share/5eadbfc7a18d4201b) |
| [🍽️ Serverless Food Ordering Platform](#-serverless-food-ordering-platform) | Vue 3 · Lambda · DynamoDB · SNS/SQS | Serverless + DevSecOps | [Demo](https://loom.com/share/d52573db16ca434ab8f77a9aaece8466) |
| [📦 Order Management System](#-order-management-system) | Node.js · Express · Vue 3 | Full Stack REST API | [Live](https://ordering-theta-neon.vercel.app) |
| [💬 WhatsApp Clone](#-whatsapp-clone) | Next.js · Firebase · Realtime | Real-time Messaging | [Live](https://whatsapp-clone-three-teal.vercel.app) |

---

## 🏥 Health & Fitness Booking Platform

> A production-grade multi-tenant booking platform for health and fitness professionals — built on AWS with a full DevSecOps CI/CD pipeline integrated at every stage.

📹 **[Demo 1 — Provider Dashboard](https://loom.com/share/deb83c6d095744ad9b8379a9081086de)** · **[Demo 2 — Client Portal](https://loom.com/share/5eadbfc7a18d4201b)**
🔗 **[github.com/gindriliunas/booking-platform](https://github.com/gindriliunas/booking-platform)**

### Architecture

```mermaid
graph TD
    A[👤 User] -->|HTTPS :443| B[Application Load Balancer]
    B -->|TCP :3000| C[ECS Fargate\nNext.js App]
    C -->|PostgreSQL :5432| D[(RDS PostgreSQL\nPrivate Subnet)]
    C -->|JWT| E[AWS Cognito\nAuthentication]
    C -->|Fetch| F[AWS Secrets Manager]
    G[AWS GuardDuty] -.->|Threat Detection| C
    G -.->|Threat Detection| D

    style A fill:#4F46E5,color:#fff
    style B fill:#0EA5E9,color:#fff
    style C fill:#10B981,color:#fff
    style D fill:#F59E0B,color:#fff
    style E fill:#8B5CF6,color:#fff
    style F fill:#EF4444,color:#fff
    style G fill:#6B7280,color:#fff
```

### DevSecOps Pipeline

```mermaid
flowchart LR
    A[📝 Code Push] --> B[Gitleaks\nSecret Scan]
    B --> C[Snyk\nDependency CVEs]
    C --> D[CodeQL\nStatic Analysis]
    D --> E[Trivy\nContainer Scan]
    E --> F[Checkov + tfsec\nTerraform IaC]
    F --> G[OWASP ZAP\nDAST]
    G --> H[✅ Deploy\nto ECS]

    style A fill:#374151,color:#fff
    style B fill:#EF4444,color:#fff
    style C fill:#EF4444,color:#fff
    style D fill:#EF4444,color:#fff
    style E fill:#EF4444,color:#fff
    style F fill:#EF4444,color:#fff
    style G fill:#EF4444,color:#fff
    style H fill:#10B981,color:#fff
```

### Tech Stack

```
Next.js 14 · TypeScript · Tailwind CSS
AWS ECS Fargate · RDS PostgreSQL · Cognito · Secrets Manager · GuardDuty
Drizzle ORM · Terraform · GitHub Actions
Snyk · CodeQL · Trivy · Checkov · tfsec · OWASP ZAP · Gitleaks
```

---

## 🍽️ Serverless Food Ordering Platform

> A fully serverless food ordering system on AWS — using event-driven architecture with SNS/SQS for async order processing, Cognito auth, and a Terraform-provisioned infrastructure.

📹 **[Demo — Order Flow & Real-Time Confirmation](https://loom.com/share/d52573db16ca434ab8f77a9aaece8466)**
🔗 **[github.com/gindriliunas/food-ordering](https://github.com/gindriliunas/food-ordering)**

### Architecture

```mermaid
graph TD
    A[👤 User] -->|HTTPS| B[CloudFront + S3\nVue.js Frontend]
    B -->|JWT| C[API Gateway\nHTTP API]
    C -->|Validate JWT| D[AWS Cognito]
    C --> E[Lambda\nOrder Handler]
    E -->|Read/Write| F[(DynamoDB\nOrders Table)]
    E -->|Publish| G[SNS Topic\nOrderPlaced]
    G -->|Subscribe| H[SQS Queue]
    H -->|Trigger| I[Lambda\nWorker]
    I -->|Update Status| F
    H -->|On Failure| J[Dead Letter Queue]

    style A fill:#4F46E5,color:#fff
    style B fill:#0EA5E9,color:#fff
    style C fill:#8B5CF6,color:#fff
    style D fill:#8B5CF6,color:#fff
    style E fill:#10B981,color:#fff
    style F fill:#F59E0B,color:#fff
    style G fill:#EF4444,color:#fff
    style H fill:#EF4444,color:#fff
    style I fill:#10B981,color:#fff
    style J fill:#6B7280,color:#fff
```

### Order Flow

```mermaid
sequenceDiagram
    participant U as User
    participant API as API Gateway
    participant L as Lambda
    participant DB as DynamoDB
    participant SNS as SNS
    participant SQS as SQS
    participant W as Worker Lambda

    U->>API: POST /orders (JWT)
    API->>L: Invoke handler
    L->>DB: Save order (PENDING)
    L->>SNS: Publish OrderPlaced
    L->>U: 201 Created
    SNS->>SQS: Fan-out message
    SQS->>W: Trigger worker
    W->>DB: Update status (CONFIRMED)
```

### Tech Stack

```
Vue 3 · Vite · TypeScript (Frontend)
Node.js 20 · Lambda · API Gateway · DynamoDB · SNS · SQS
AWS Cognito · CloudFront · S3 · Terraform
GitHub Actions · tfsec · npm audit · Jest
```

---

## 📦 Order Management System

> A full-stack order management system with a Node.js/Express REST API and Vue 3 frontend — featuring comprehensive server-side validation, order history, and product catalogue integration.

🔗 **[Live Demo](https://ordering-theta-neon.vercel.app)** · **[github.com/gindriliunas/Ordering](https://github.com/gindriliunas/Ordering)**

### Application Flow

```mermaid
flowchart TD
    A[👤 User] --> B{Action}
    B -->|Submit Order| C[Vue 3 Frontend\nValidation]
    B -->|View History| G[Order History\nwith Filters]
    C -->|POST /api/orders| D[Express API\nServer Validation]
    D -->|Valid| E[✅ Store Order\nPENDING → CONFIRMED]
    D -->|Invalid| F[❌ 400 Error\nValidation Details]
    E --> G
    F --> A

    style A fill:#4F46E5,color:#fff
    style C fill:#10B981,color:#fff
    style D fill:#0EA5E9,color:#fff
    style E fill:#10B981,color:#fff
    style F fill:#EF4444,color:#fff
    style G fill:#F59E0B,color:#fff
```

### Validation Rules

```mermaid
graph LR
    A[Order Submitted] --> B{Customer\nName?}
    B -->|Empty| Z[❌ Rejected]
    B -->|Valid| C{Delivery\nDate?}
    C -->|Past / Invalid| Z
    C -->|Future| D{Items\nPresent?}
    D -->|None| Z
    D -->|Yes| E{SKU exists\nin catalogue?}
    E -->|No| Z
    E -->|Yes| F{Name\nmatches SKU?}
    F -->|No| Z
    F -->|Yes| G{Quantity\n> 0?}
    G -->|No| Z
    G -->|Yes| H{Order ID\nunique?}
    H -->|Duplicate| Z
    H -->|Unique| Y[✅ Accepted]

    style Y fill:#10B981,color:#fff
    style Z fill:#EF4444,color:#fff
```

### Tech Stack

```
Node.js · Express · TypeScript (Backend)
Vue 3 · Vite · TypeScript (Frontend)
In-memory storage · Vercel deployment
```

---

## 💬 WhatsApp Clone

> A real-time messaging web app built with Next.js and Firebase — featuring Google authentication, live Firestore listeners, and a responsive UI that matches WhatsApp Web.

🔗 **[Live Demo](https://whatsapp-clone-three-teal.vercel.app)** · **[github.com/gindriliunas/whatsapp-clone](https://github.com/gindriliunas/whatsapp-clone)**

### Real-time Architecture

```mermaid
graph TD
    A[👤 User A] -->|Google OAuth| B[Firebase Auth]
    C[👤 User B] -->|Google OAuth| B
    B -->|JWT Token| D[Next.js App\nVercel]
    D -->|Read/Write| E[(Firestore\nDatabase)]
    E -->|Real-time\nListener| D
    D -->|Live Updates| A
    D -->|Live Updates| C

    style A fill:#4F46E5,color:#fff
    style B fill:#F59E0B,color:#fff
    style C fill:#8B5CF6,color:#fff
    style D fill:#10B981,color:#fff
    style E fill:#EF4444,color:#fff
```

### Message Flow

```mermaid
sequenceDiagram
    participant U as User A
    participant App as Next.js
    participant FS as Firestore
    participant B as User B

    U->>App: Type and send message
    App->>FS: Write to messages collection
    FS-->>App: onSnapshot listener fires
    App-->>B: Message appears instantly
    Note over FS,App: No polling — real-time push
```

### Tech Stack

```
Next.js 14 · React · TypeScript · Tailwind CSS
Firebase Firestore · Firebase Auth (Google OAuth)
Vercel · Cursor IDE
```

---

## 🔐 Security Approach

All projects follow a shift-left security philosophy — security checks integrated into the development workflow, not bolted on after:

```mermaid
graph LR
    A[💻 Write Code] -->|Gitleaks| B[Commit]
    B -->|Dependency Review\nSnyk| C[Pull Request]
    C -->|CodeQL\nESLint| D[Build]
    D -->|Trivy| E[Container]
    E -->|Checkov\ntfsec| F[Infrastructure]
    F -->|OWASP ZAP| G[Staging]
    G -->|GuardDuty| H[🚀 Production]

    style A fill:#374151,color:#fff
    style H fill:#10B981,color:#fff
```

---

## 📊 Skills Demonstrated

| Skill | Projects |
|---|---|
| AWS Cloud Architecture | Booking Platform, Food Ordering |
| Serverless / Lambda | Food Ordering |
| DevSecOps Pipelines | Booking Platform, Food Ordering |
| Infrastructure as Code (Terraform) | Booking Platform, Food Ordering |
| Real-time Systems | WhatsApp Clone |
| REST API Design | Ordering, Food Ordering |
| Multi-tenant SaaS | Booking Platform |
| Event-driven Architecture | Food Ordering |
| TypeScript | All Projects |
| AI-assisted Development | WhatsApp Clone |

