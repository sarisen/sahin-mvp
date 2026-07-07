---
theme: default
title: MVP Backend Architecture
info: |
  Modern, open-source focused MVP backend architecture.
class: text-center
highlighter: shiki
lineNumbers: false
drawings:
  persist: false
transition: slide-left
mdc: true
---

# MVP Backend Architecture

Modern, open-source focused, production-ready MVP stack

<div class="mt-10 text-lg opacity-70">
Hono • PostgreSQL • Redis • BullMQ • MinIO • GlitchTip • Uptime Kuma
</div>

---
layout: center
---

# Goal

Build a backend that is simple enough for MVP, but clean enough to scale.

<div class="chips justify-center mt-10 text-xl">
  <span class="pill" style="--c:#f97316">Fast development</span>
  <span class="pill" style="--c:#22c55e">Low infrastructure cost</span>
  <span class="pill" style="--c:#3b82f6">Open-source first</span>
  <span class="pill" style="--c:#38bdf8">Easy deployment</span>
  <span class="pill" style="--c:#a855f7">Type-safe backend</span>
  <span class="pill" style="--c:#14b8a6">Observable from day one</span>
</div>

---
layout: center
---

# Architecture Overview

```mermaid {scale: 0.5}
flowchart TD
    A["📱 Client Apps<br/>React · SwiftUI · Kotlin"] --> B["🔥 Hono API<br/>Better Auth · Zod · Pino · REST"]

    B --> C["⚙️ Business Layer<br/>User · Payment · Notification · AI"]

    C --> D["🐘 PostgreSQL<br/>Drizzle ORM"]
    C --> E["⚡ Redis<br/>Cache · Rate Limit · BullMQ"]
    C --> F["📦 MinIO<br/>S3-compatible Storage"]

    E --> G["👷 Background Workers<br/>Email · Push · AI · PDF · Cron"]

    B -.-> H["📊 Observability<br/>GlitchTip · Uptime Kuma"]
    G -.-> H

    classDef client fill:#3b82f6,stroke:#1d4ed8,color:#fff,stroke-width:2px;
    classDef api fill:#f97316,stroke:#c2410c,color:#fff,stroke-width:2px;
    classDef logic fill:#8b5cf6,stroke:#6d28d9,color:#fff,stroke-width:2px;
    classDef data fill:#10b981,stroke:#047857,color:#fff,stroke-width:2px;
    classDef worker fill:#ec4899,stroke:#be185d,color:#fff,stroke-width:2px;
    classDef obs fill:#64748b,stroke:#334155,color:#fff,stroke-width:2px;

    class A client;
    class B api;
    class C logic;
    class D,E,F data;
    class G worker;
    class H obs;
```

---
layout: two-cols
---

# Core Stack

<div class="stack">
  <div class="row"><span class="layer">API</span><span class="tool" style="--c:#f97316">Hono</span></div>
  <div class="row"><span class="layer">Validation</span><span class="tool" style="--c:#3b82f6">Zod</span></div>
  <div class="row"><span class="layer">Auth</span><span class="tool" style="--c:#ef4444">Better Auth</span></div>
  <div class="row"><span class="layer">ORM</span><span class="tool" style="--c:#22c55e">Drizzle</span></div>
  <div class="row"><span class="layer">Database</span><span class="tool" style="--c:#38bdf8">PostgreSQL</span></div>
  <div class="row"><span class="layer">Cache / Queue</span><span class="tool" style="--c:#dc2626">Redis</span><span class="tool" style="--c:#f43f5e">BullMQ</span></div>
  <div class="row"><span class="layer">Storage</span><span class="tool" style="--c:#e11d48">MinIO</span></div>
  <div class="row"><span class="layer">Logging</span><span class="tool" style="--c:#a855f7">Pino</span></div>
  <div class="row"><span class="layer">Monitoring</span><span class="tool" style="--c:#14b8a6">GlitchTip</span><span class="tool" style="--c:#eab308">Uptime Kuma</span></div>
</div>

<style>
.stack { display: flex; flex-direction: column; gap: 0.4rem; margin-top: 0.5rem; font-size: 0.92rem; }
.stack .row { display: flex; align-items: center; gap: 0.6rem; }
.stack .layer { width: 118px; opacity: 0.6; flex: none; }
.stack .tool {
  padding: 0.1rem 0.65rem; border-radius: 0.5rem; font-weight: 600;
  color: var(--c);
  background: color-mix(in srgb, var(--c) 16%, transparent);
  border: 1px solid color-mix(in srgb, var(--c) 45%, transparent);
}
</style>

::right::

# Why this stack?

<div class="chips flex-col mt-6 text-lg">
  <span class="pill" style="--c:#f97316">minimal moving parts</span>
  <span class="pill" style="--c:#22c55e">mostly open-source</span>
  <span class="pill" style="--c:#3b82f6">fast to build</span>
  <span class="pill" style="--c:#38bdf8">easy to self-host</span>
  <span class="pill" style="--c:#a855f7">no premature microservices</span>
  <span class="pill" style="--c:#14b8a6">can scale later without rewriting</span>
</div>

---
layout: center
---

# Request Flow

```mermaid {scale: 0.8}
sequenceDiagram
    participant Client as 📱 Client
    participant API as 🔥 Hono API
    participant Auth as 🔐 Better Auth
    participant Zod as ✅ Zod
    participant Service as ⚙️ Service
    participant DB as 🐘 PostgreSQL

    Client->>API: HTTP Request
    API->>Auth: Authenticate user
    API->>Zod: Validate payload
    API->>Service: Execute business logic
    Service->>DB: Read / Write data
    DB-->>Service: Result
    Service-->>API: Response
    API-->>Client: JSON Response
```

---
layout: center
---

# Background Jobs

Long-running tasks should not block API requests.

```mermaid {scale: 0.7}
flowchart LR
    A["🔥 API Request"] --> B["⚡ Redis / BullMQ Queue"]
    B --> C["👷 Worker"]
    C --> D["✉️ Email"]
    C --> E["🔔 Push"]
    C --> F["🤖 AI Processing"]
    C --> G["📄 PDF / Export"]
    C --> H["⏰ Scheduled Jobs"]

    classDef api fill:#f97316,stroke:#c2410c,color:#fff,stroke-width:2px;
    classDef queue fill:#ef4444,stroke:#b91c1c,color:#fff,stroke-width:2px;
    classDef worker fill:#ec4899,stroke:#be185d,color:#fff,stroke-width:2px;
    classDef task fill:#10b981,stroke:#047857,color:#fff,stroke-width:2px;

    class A api;
    class B queue;
    class C worker;
    class D,E,F,G,H task;
```

<div class="mt-8 text-xl opacity-75">
API returns fast. Workers process heavy tasks asynchronously.
</div>

---
layout: two-cols
---

# Redis Usage

<div class="chips flex-col mt-6 text-lg">
  <span class="pill" style="--c:#dc2626">Cache</span>
  <span class="pill" style="--c:#dc2626">Rate limiting</span>
  <span class="pill" style="--c:#dc2626">BullMQ queue backend</span>
  <span class="pill" style="--c:#dc2626">Temporary data</span>
  <span class="pill" style="--c:#dc2626">Future WebSocket scaling</span>
</div>

::right::

# MinIO Usage

<div class="chips flex-col mt-6 text-lg">
  <span class="pill" style="--c:#e11d48">Images</span>
  <span class="pill" style="--c:#e11d48">Documents</span>
  <span class="pill" style="--c:#e11d48">Exports</span>
  <span class="pill" style="--c:#e11d48">Attachments</span>
  <span class="pill" style="--c:#e11d48">S3-compatible object storage</span>
</div>

---
layout: center
---

# Observability

```mermaid {scale: 0.75}
flowchart TD
    A["🔥 Hono API"] --> B["📝 Pino Logs"]
    A --> C["🐞 GlitchTip<br/>Error Tracking"]
    A --> D["📈 Uptime Kuma<br/>Availability + Response Time"]
    E["👷 Workers"] --> C
    E --> B

    classDef api fill:#f97316,stroke:#c2410c,color:#fff,stroke-width:2px;
    classDef worker fill:#ec4899,stroke:#be185d,color:#fff,stroke-width:2px;
    classDef obs fill:#64748b,stroke:#334155,color:#fff,stroke-width:2px;

    class A api;
    class E worker;
    class B,C,D obs;
```

<div class="mt-8 text-xl opacity-75">
For MVP, we keep monitoring simple: errors + uptime + structured logs.
</div>

---
layout: center
---

# What We Intentionally Exclude From MVP

<div class="chips justify-center mt-10 text-xl">
  <span class="pill muted" style="--c:#ef4444">Microservices</span>
  <span class="pill muted" style="--c:#ef4444">Kubernetes</span>
  <span class="pill muted" style="--c:#ef4444">Event bus</span>
  <span class="pill muted" style="--c:#ef4444">OpenTelemetry</span>
  <span class="pill muted" style="--c:#ef4444">Prometheus / Grafana / Loki</span>
  <span class="pill muted" style="--c:#ef4444">Distributed tracing</span>
</div>

<div class="mt-10 text-2xl">
Less infrastructure. Faster MVP.
</div>

---
layout: center
---

# Deployment

```mermaid {scale: 0.6}
flowchart TD
    A["🔧 Git Repository"] --> B["🐳 Docker Build"]
    B --> C["🚀 Coolify"]
    C --> D["🔥 Hono API"]
    C --> E["👷 Worker"]
    C --> F["🐘 PostgreSQL"]
    C --> G["⚡ Redis"]
    C --> H["📦 MinIO"]
    C --> I["🐞 GlitchTip"]
    C --> J["📈 Uptime Kuma"]

    classDef src fill:#3b82f6,stroke:#1d4ed8,color:#fff,stroke-width:2px;
    classDef build fill:#0ea5e9,stroke:#0369a1,color:#fff,stroke-width:2px;
    classDef deploy fill:#8b5cf6,stroke:#6d28d9,color:#fff,stroke-width:2px;
    classDef svc fill:#10b981,stroke:#047857,color:#fff,stroke-width:2px;

    class A src;
    class B build;
    class C deploy;
    class D,E,F,G,H,I,J svc;
```

---
layout: center
class: text-center
---

# Final Decision

<div class="text-3xl leading-12 mt-8">
Start simple. Stay open-source. Keep the architecture ready to scale.
</div>

<div class="mt-10 text-xl opacity-70">
Hono + PostgreSQL + Redis + BullMQ + MinIO + GlitchTip + Uptime Kuma
</div>
