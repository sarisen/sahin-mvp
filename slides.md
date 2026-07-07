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
routerMode: hash
---

# MVP Backend Architecture

<div class="text-2xl font-semibold mt-2" style="color:#38bdf8">unicathlete-mvp</div>

Technical stack & architecture for the UnicAthlete MVP

<div class="mt-10 text-lg opacity-70">
Hono • PostgreSQL • Redis • BullMQ • MinIO • Video Infra • GlitchTip • Uptime Kuma
</div>

---
layout: default
---

# Executive Summary

<div class="lead mt-4">
This document details the proposed technical stack and architecture for the
<b>UnicAthlete MVP</b>. The goal is a robust backend that meets four objectives:
rapid development, low infrastructure cost, an open-source focus, and scalability.
</div>

<div class="chips mt-6 text-base">
  <span class="pill" style="--c:#f97316">Rapid development</span>
  <span class="pill" style="--c:#22c55e">Low infrastructure cost</span>
  <span class="pill" style="--c:#3b82f6">Open-source focus</span>
  <span class="pill" style="--c:#38bdf8">Scalability</span>
</div>

<div class="lead mt-6">
Strictly aligned with the confirmed MVP scope, the plan offers rational, cost-effective
solutions for <b>data integrity</b>, <b>personal & organization workspaces</b>,
<b>pipeline management</b>, and <b>secure interactions</b>. We avoided discredited
technologies like blockchain; instead we adopt modern approaches for structuring
<b>verifiable data</b>. To mitigate infrastructure risks, complex media handling like
<b>video processing</b> is offloaded to dedicated streaming solutions.
</div>

<details class="fulltext">
<summary>Full text</summary>
<div class="ft-body">
<p>This document details the proposed technical stack and architecture for the UnicAthlete MVP (Minimum Viable Product). Our goal is to build a robust backend infrastructure that meets the objectives of rapid development, low infrastructure cost, an open-source focus, and scalability.</p>
<p>The plan is strictly aligned with the confirmed MVP scope, offering rational and cost-effective solutions for data integrity, personal and organization workspaces, pipeline management, and secure interactions. Notably, we avoided discredited technologies like blockchain; instead, we adopted modern approaches for structuring verifiable data. To mitigate infrastructure risks, complex media handling like video processing is offloaded to dedicated streaming solutions.</p>
</div>
</details>

---
layout: default
---

# 1. Introduction

<div class="lead mt-4">
A core platform where <b>athletes</b> showcase their talents, and
<b>scouts / recruiters</b> discover, manage, and track potential talents through
structured <b>workspaces</b>.
</div>

<div class="cards mt-8" style="grid-template-columns: 1fr 1fr;">
  <div class="card" style="--c:#3b82f6">
    <div class="card-title">Athletes</div>
    <div class="card-body">Showcase talent and build a verifiable performance profile.</div>
  </div>
  <div class="card" style="--c:#a855f7">
    <div class="card-title">Scouts / Recruiters</div>
    <div class="card-body">Discover, manage, and track potential talent through structured workspaces.</div>
  </div>
</div>

<div class="chips mt-6 text-base">
  <span class="pill" style="--c:#f97316">Search</span>
  <span class="pill" style="--c:#22c55e">Profiles</span>
  <span class="pill" style="--c:#3b82f6">Notes</span>
  <span class="pill" style="--c:#38bdf8">Messaging / Requests</span>
  <span class="pill" style="--c:#14b8a6">Basic verification</span>
</div>

<details class="fulltext">
<summary>Full text</summary>
<div class="ft-body">
<p>The UnicAthlete MVP is designed as a core platform where athletes can showcase their talents, and scouts/recruiters can discover, manage, and track potential talents through structured workspaces. This technical implementation plan explains the backend architecture of the MVP, the chosen technologies, and the rationale behind these choices. Our primary objective is to deliver immediate value by focusing strictly on the athlete and recruiter ecosystem (search, profiles, notes, messaging/requests, and basic verification) while establishing a clean and scalable structure for future phases.</p>
</div>
</details>

---
layout: center
class: text-center
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

<div class="note mt-10 mx-auto" style="max-width: 46rem;">
Deliver value quickly while establishing a clean, scalable structure that allows
for future growth and evolution.
</div>

---
layout: center
class: text-center
---

# 2. Architecture Overview

<div class="note mx-auto mb-4" style="max-width: 48rem;">
A modular, layered design where each component has a single responsibility and can
be developed and scaled independently.
</div>

```mermaid {scale: 0.5}
flowchart TD
    A["📱 Client Apps<br/>React · SwiftUI · Kotlin"] --> B["🔥 Hono API<br/>Better Auth · Zod · Pino · REST"]

    B --> C["⚙️ Business Layer<br/>User · Workspace · Pipeline · Messaging · Verification"]

    C --> D["🐘 PostgreSQL<br/>Drizzle ORM"]
    C --> E["⚡ Redis<br/>Cache · Rate Limit · BullMQ"]
    C --> F["📦 MinIO<br/>Docs · Images"]
    C --> V["🎬 Video Infra<br/>Mux · Cloudflare Stream"]

    E --> G["👷 Background Workers<br/>Email · Push · PDF · Cron"]

    B -.-> H["📊 Observability<br/>GlitchTip · Uptime Kuma"]
    G -.-> H

    classDef client fill:#3b82f6,stroke:#1d4ed8,color:#fff,stroke-width:2px;
    classDef api fill:#f97316,stroke:#c2410c,color:#fff,stroke-width:2px;
    classDef logic fill:#8b5cf6,stroke:#6d28d9,color:#fff,stroke-width:2px;
    classDef data fill:#10b981,stroke:#047857,color:#fff,stroke-width:2px;
    classDef video fill:#e11d48,stroke:#9f1239,color:#fff,stroke-width:2px;
    classDef worker fill:#ec4899,stroke:#be185d,color:#fff,stroke-width:2px;
    classDef obs fill:#64748b,stroke:#334155,color:#fff,stroke-width:2px;

    class A client;
    class B api;
    class C logic;
    class D,E,F data;
    class V video;
    class G worker;
    class H obs;
```

<details class="fulltext">
<summary>Full text</summary>
<div class="ft-body">
<p>The backend architecture of the UnicAthlete MVP is designed with a modular and layered approach. This structure ensures that each component has a specific responsibility and can be developed and scaled independently. The overall architecture consists of the following main layers: Client Apps, API Layer (Hono API), Business Layer, Data Layer (including a dedicated Video Infrastructure), Background Workers, and Observability.</p>
</div>
</details>

---
layout: default
---

# Architecture Layers

<div class="cards mt-2" style="grid-template-columns: 1fr 1fr;">
  <div class="card" style="--c:#3b82f6">
    <div class="card-title">Client Apps</div>
    <div class="card-body">User interfaces built with React (Web), SwiftUI (iOS), and Kotlin (Android).</div>
  </div>
  <div class="card" style="--c:#f97316">
    <div class="card-title">API Layer — Hono</div>
    <div class="card-body">A fast, lightweight framework that connects client apps to backend services.</div>
  </div>
  <div class="card" style="--c:#8b5cf6">
    <div class="card-title">Business Layer</div>
    <div class="card-body">User management, workspace & project pipelines, messaging, requests, and basic verification workflows.</div>
  </div>
  <div class="card" style="--c:#10b981">
    <div class="card-title">Data Layer</div>
    <div class="card-body">PostgreSQL (relational), Redis (cache & queue), MinIO (docs/images) + dedicated Video Infrastructure.</div>
  </div>
  <div class="card" style="--c:#ec4899">
    <div class="card-title">Background Workers</div>
    <div class="card-body">Async work: email, push, PDF/report exports, scheduled tasks.</div>
  </div>
  <div class="card" style="--c:#64748b">
    <div class="card-title">Observability</div>
    <div class="card-body">GlitchTip (error tracking) and Uptime Kuma (service status) watch system health.</div>
  </div>
</div>

<details class="fulltext">
<summary>Full text</summary>
<div class="ft-body">
<p><b>Client Apps:</b> User interfaces developed using React (Web), SwiftUI (iOS), and Kotlin (Android).</p>
<p><b>API Layer (Hono API):</b> A fast and lightweight API framework that facilitates communication between client applications and backend services.</p>
<p><b>Business Layer:</b> Houses core business processes such as user management, workspace and project pipelines, messaging, requests, and basic verification workflows.</p>
<p><b>Data Layer:</b> Includes components like PostgreSQL (relational database), Redis (caching and queue management), MinIO (for basic object storage like documents/images), and a dedicated Video Infrastructure for secure playback.</p>
<p><b>Background Workers:</b> Asynchronously executes long-running operations such as email delivery, push notifications, PDF/report exports, and scheduled tasks.</p>
<p><b>Observability:</b> GlitchTip (error tracking) and Uptime Kuma (service status monitoring) are used to monitor system health and performance.</p>
</div>
</details>

---
layout: default
---

# 3. Core Technology Stack & Rationale

<div class="lead mt-3">The stack is guided by five core principles:</div>

<div class="cards mt-5" style="grid-template-columns: 1fr 1fr;">
  <div class="card" style="--c:#f97316">
    <div class="card-title">Minimal moving parts</div>
    <div class="card-body">Reduce complexity, increase development speed.</div>
  </div>
  <div class="card" style="--c:#22c55e">
    <div class="card-title">Mostly open-source</div>
    <div class="card-body">Lower costs, benefit from community support.</div>
  </div>
  <div class="card" style="--c:#3b82f6">
    <div class="card-title">Rapid development</div>
    <div class="card-body">Bring the MVP to market quickly within the confirmed scope.</div>
  </div>
  <div class="card" style="--c:#38bdf8">
    <div class="card-title">Easy self-hosting</div>
    <div class="card-body">Keep infrastructure costs under control where applicable.</div>
  </div>
  <div class="card" style="--c:#a855f7">
    <div class="card-title">No premature microservices</div>
    <div class="card-body">Start monolithic; scale only when needed.</div>
  </div>
</div>

<details class="fulltext">
<summary>Full text</summary>
<div class="ft-body">
<p>The technology stack selected for the UnicAthlete MVP is based on the following core principles:</p>
<p><b>Minimal Moving Parts:</b> To reduce complexity and increase development speed.</p>
<p><b>Mostly Open-Source:</b> To lower costs and benefit from community support.</p>
<p><b>Rapid Development:</b> To bring the MVP to market quickly within the confirmed scope.</p>
<p><b>Easy Self-Hosting:</b> To keep infrastructure costs under control where applicable.</p>
<p><b>Avoiding Premature Microservices:</b> Starting with a monolithic structure to avoid unnecessary complexity, scaling only when needed.</p>
</div>
</details>

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

<div class="chips flex-col items-start mt-6 text-lg">
  <span class="pill" style="--c:#f97316">minimal moving parts</span>
  <span class="pill" style="--c:#22c55e">mostly open-source</span>
  <span class="pill" style="--c:#3b82f6">fast to build</span>
  <span class="pill" style="--c:#38bdf8">easy to self-host</span>
  <span class="pill" style="--c:#a855f7">no premature microservices</span>
  <span class="pill" style="--c:#14b8a6">strictly scoped for MVP</span>
</div>

---
layout: default
---

# 3.1 API & Data Validation

<div class="cards mt-4">
  <div class="card" style="--c:#f97316">
    <div class="card-title">Hono — API Framework</div>
    <div class="card-body">Lightweight, fast, web-standard-compliant. High performance even in edge environments — a perfect fit for rapid development and low resource consumption.</div>
  </div>
  <div class="card" style="--c:#3b82f6">
    <div class="card-title">Zod — Data Validation</div>
    <div class="card-body">Schema-based validation fully compatible with TypeScript. Guarantees the reliability and type safety of API requests, catching errors at an early stage.</div>
  </div>
  <div class="card" style="--c:#ef4444">
    <div class="card-title">Better Auth — Authentication</div>
    <div class="card-body">Secure, scalable authentication. Manages user sessions and workspace authorization processes.</div>
  </div>
</div>

<details class="fulltext">
<summary>Full text</summary>
<div class="ft-body">
<p><b>Hono (API Framework):</b> A lightweight, fast, and web-standard-compliant API framework. It offers high performance even in edge environments.</p>
<p><b>Zod (Data Validation):</b> A schema-based data validation library fully compatible with TypeScript. It ensures the reliability and type safety of incoming API requests and data structures.</p>
<p><b>Better Auth (Authentication):</b> Provides secure and scalable authentication solutions, managing user sessions and workspace authorization processes.</p>
</div>
</details>

---
layout: default
---

# 3.2 Data Management & Media Infrastructure

<div class="cards mt-3" style="grid-template-columns: 1fr 1fr;">
  <div class="card" style="--c:#38bdf8">
    <div class="card-title">PostgreSQL — Database</div>
    <div class="card-body">Relational data: profiles, workspaces, pipelines, notes. Robust, reliable, and open-source.</div>
  </div>
  <div class="card" style="--c:#22c55e">
    <div class="card-title">Drizzle — ORM</div>
    <div class="card-body">Modern ORM with end-to-end TypeScript type safety. Simplifies database interactions.</div>
  </div>
  <div class="card" style="--c:#dc2626">
    <div class="card-title">Redis — Cache & Queue</div>
    <div class="card-body">High-performance key-value store: caching, rate limiting, and background jobs (BullMQ).</div>
  </div>
  <div class="card" style="--c:#e11d48">
    <div class="card-title">MinIO — Basic Object Storage</div>
    <div class="card-body">S3-compatible, strictly for lightweight files: profile images, documents, exported reports.</div>
  </div>
  <div class="card" style="--c:#f43f5e; grid-column: 1 / -1;">
    <div class="card-title">Dedicated Video Infrastructure — Mux · AWS MediaConvert · Cloudflare Stream</div>
    <div class="card-body">Reliable uploads, processing, compression, streaming, and secure playback — without the operational risks of self-hosting video.</div>
  </div>
</div>

<details class="fulltext">
<summary>Full text</summary>
<div class="ft-body">
<p><b>PostgreSQL (Database):</b> A robust, reliable, and open-source database for relational data (profiles, workspaces, pipelines, notes).</p>
<p><b>Drizzle ORM (Object-Relational Mapper):</b> A modern ORM that provides end-to-end type safety with TypeScript, simplifying database interactions.</p>
<p><b>Redis (Cache and Queue):</b> A high-performance key-value store utilized for caching, rate limiting, and managing background jobs (BullMQ).</p>
<p><b>MinIO (Basic Object Storage):</b> An S3-compatible solution used strictly for storing lightweight files such as profile images, documents, and exported reports.</p>
<p><b>Dedicated Video Infrastructure (e.g., Mux, AWS MediaConvert, or Cloudflare Stream):</b> To ensure reliable uploads, processing, compression, streaming, and secure playback without the heavy operational risks of self-hosting video infrastructure.</p>
</div>
</details>

---
layout: center
class: text-center
---

# Request Flow

```mermaid {scale: 0.58}
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
class: text-center
---

# Background Jobs

Long-running tasks should not block API requests.

```mermaid {scale: 0.7}
flowchart LR
    A["🔥 API Request"] --> B["⚡ Redis / BullMQ Queue"]
    B --> C["👷 Worker"]
    C --> D["✉️ Email"]
    C --> E["🔔 Push"]
    C --> G["📄 PDF / Export"]
    C --> H["⏰ Scheduled Jobs"]

    classDef api fill:#f97316,stroke:#c2410c,color:#fff,stroke-width:2px;
    classDef queue fill:#ef4444,stroke:#b91c1c,color:#fff,stroke-width:2px;
    classDef worker fill:#ec4899,stroke:#be185d,color:#fff,stroke-width:2px;
    classDef task fill:#10b981,stroke:#047857,color:#fff,stroke-width:2px;

    class A api;
    class B queue;
    class C worker;
    class D,E,G,H task;
```

<div class="mt-6 text-xl opacity-75">
API returns fast. Workers process heavy tasks asynchronously.
</div>

<div class="note mx-auto mt-4" style="max-width: 50rem;">
<b>BullMQ</b> — a robust, Redis-based queue — decouples long-running operations from
API requests, while <b>Pino</b> provides fast, low-overhead structured logging.
</div>

<details class="fulltext">
<summary>Full text</summary>
<div class="ft-body">
<p><b>BullMQ (Message Queue):</b> A robust, Redis-based queue system for managing background jobs. It increases the system's responsiveness by decoupling operations (email delivery, PDF generation) from API requests.</p>
<p><b>Pino (Logging):</b> A fast and low-overhead Node.js logging library for production environments.</p>
<p><b>GlitchTip &amp; Uptime Kuma:</b> Open-source tools for real-time error tracking and service status monitoring.</p>
</div>
</details>

---
layout: two-cols
---

# Redis Usage

<div class="chips flex-col items-start mt-6 text-lg">
  <span class="pill" style="--c:#f97316">Cache</span>
  <span class="pill" style="--c:#eab308">Rate limiting</span>
  <span class="pill" style="--c:#ec4899">BullMQ queue backend</span>
</div>

::right::

# MinIO Usage

<div class="chips flex-col items-start mt-6 text-lg">
  <span class="pill" style="--c:#3b82f6">Profile images</span>
  <span class="pill" style="--c:#38bdf8">Documents</span>
  <span class="pill" style="--c:#22c55e">Exported reports</span>
  <span class="pill" style="--c:#a855f7">Lightweight files only</span>
</div>

<div class="note mt-6 text-base">
Heavy media (video) is handled by dedicated streaming infrastructure — not MinIO.
</div>

---
layout: center
class: text-center
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

<div class="mt-6 text-xl opacity-75">
For MVP, we keep monitoring simple: errors + uptime + structured logs.
</div>

<div class="note mx-auto mt-4" style="max-width: 52rem;">
<b>GlitchTip</b> — an open-source, Sentry-compatible platform — captures and reports
errors in real time, while <b>Uptime Kuma</b> tracks the availability and response
time of active services.
</div>

<details class="fulltext">
<summary>Full text</summary>
<div class="ft-body">
<p><b>GlitchTip (Error Tracking):</b> An open-source error tracking platform (Sentry compatible). It captures and reports errors in the application in real-time.</p>
<p><b>Uptime Kuma (Service Status Monitoring):</b> An open-source, self-hostable service status monitoring tool. It tracks the uptime and performance of active services.</p>
</div>
</details>

---
layout: default
---

# Trust & Media Strategy

<div class="lead mt-3">
Modern, pragmatic choices over hype — and offload risky infrastructure.
</div>

<div class="cards mt-5" style="grid-template-columns: 1fr 1fr;">
  <div class="card" style="--c:#94a3b8">
    <div class="card-title" style="text-decoration: line-through;">Blockchain</div>
    <div class="card-body">Discredited and unnecessarily costly for this use case — deliberately avoided.</div>
  </div>
  <div class="card" style="--c:#38bdf8">
    <div class="card-title">Verifiable Data</div>
    <div class="card-body">Modern approaches for structuring verifiable data, protecting integrity without blockchain overhead.</div>
  </div>
  <div class="card" style="--c:#a855f7">
    <div class="card-title">Secure Interactions</div>
    <div class="card-body">Workspace-scoped authorization for messaging, requests, and basic verification.</div>
  </div>
  <div class="card" style="--c:#e11d48">
    <div class="card-title">Offloaded Video</div>
    <div class="card-body">Complex media (video processing) offloaded to dedicated streaming solutions to mitigate infrastructure risk.</div>
  </div>
</div>

<details class="fulltext">
<summary>Full text</summary>
<div class="ft-body">
<p>Notably, we avoided discredited technologies like blockchain; instead, we adopted modern approaches for structuring verifiable data. To mitigate infrastructure risks, complex media handling like video processing is offloaded to dedicated streaming solutions (e.g., Mux, AWS MediaConvert, or Cloudflare Stream), ensuring reliable uploads, processing, compression, streaming, and secure playback.</p>
</div>
</details>

---
layout: center
class: text-center
---

# What We Intentionally Exclude From MVP

<div class="chips justify-center mt-10 text-xl">
  <span class="pill excluded">Microservices</span>
  <span class="pill excluded">Kubernetes</span>
  <span class="pill excluded">Event bus</span>
  <span class="pill excluded">OpenTelemetry</span>
  <span class="pill excluded">Prometheus / Grafana / Loki</span>
  <span class="pill excluded">Distributed tracing</span>
</div>

<div class="mt-10 text-2xl">
Less infrastructure. Faster MVP.
</div>

---
layout: center
class: text-center
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
    D -.-> K["🎬 Video Infra<br/>Mux · Cloudflare Stream<br/>(managed, external)"]

    classDef src fill:#3b82f6,stroke:#1d4ed8,color:#fff,stroke-width:2px;
    classDef build fill:#0ea5e9,stroke:#0369a1,color:#fff,stroke-width:2px;
    classDef deploy fill:#8b5cf6,stroke:#6d28d9,color:#fff,stroke-width:2px;
    classDef svc fill:#10b981,stroke:#047857,color:#fff,stroke-width:2px;
    classDef ext fill:#e11d48,stroke:#9f1239,color:#fff,stroke-width:2px;

    class A src;
    class B build;
    class C deploy;
    class D,E,F,G,H,I,J svc;
    class K ext;
```

---
layout: default
---

# 4. Conclusion

<div class="lead mt-4">
This plan builds the UnicAthlete MVP on a <b>robust, scalable, and cost-effective</b>
backend. By stripping away non-core functionalities and focusing purely on
<b>athletes</b>, <b>scouts / recruiters</b>, <b>workspaces</b>, and <b>secure media
handling</b>, this blueprint guarantees a rapid and stable go-to-market strategy.
</div>

<div class="lead mt-6">
The selected technologies support immediate MVP requirements while providing a solid
foundation for integrating <b>future roles and advanced features</b> when the platform
matures.
</div>

<div class="chips mt-8 text-base">
  <span class="pill" style="--c:#22c55e">Robust</span>
  <span class="pill" style="--c:#3b82f6">Scalable</span>
  <span class="pill" style="--c:#f97316">Cost-effective</span>
  <span class="pill" style="--c:#a855f7">Focused scope</span>
</div>

<details class="fulltext">
<summary>Full text</summary>
<div class="ft-body">
<p>This technical implementation plan ensures that the UnicAthlete MVP is built upon a robust, scalable, and cost-effective backend infrastructure. By stripping away non-core functionalities and focusing purely on athletes, scouts/recruiters, workspaces, and secure media handling, this blueprint guarantees a rapid and stable go-to-market strategy. The selected technologies support immediate MVP requirements while providing a solid foundation for integrating future roles and advanced features when the platform matures.</p>
</div>
</details>

---
layout: center
class: text-center
---

# Final Decision

<div class="text-3xl leading-12 mt-8">
Start simple. Stay open-source. Keep the architecture ready to scale.
</div>

<div class="mt-10 text-xl opacity-70">
Hono + PostgreSQL + Redis + BullMQ + MinIO + Video Infra + GlitchTip + Uptime Kuma
</div>
