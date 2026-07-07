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
Hono • PostgreSQL • Redis • BullMQ • MinIO • GlitchTip • Uptime Kuma
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
Aligned with the UnicAthlete concept, the plan offers rational, cost-effective
solutions in critical areas — <b>data integrity</b>, <b>gamification</b>, and
<b>AI integration</b>. Discredited technologies like blockchain are avoided; instead
we adopt modern, sustainable approaches such as <b>Immutable Ledgers</b> and
<b>Verifiable Credentials</b>. Due to cost sensitivity, video analysis is positioned
as the <b>final layer</b> of the verification process.
</div>

<details class="fulltext">
<summary>Full text</summary>
<div class="ft-body">
<p>This document details the proposed technical stack and architecture for the UnicAthlete MVP (Minimum Viable Product). Our goal is to build a robust backend infrastructure that meets the objectives of rapid development, low infrastructure cost, an open-source focus, and scalability.</p>
<p>The plan, aligned with the current UnicAthlete concept, offers rational and cost-effective solutions in critical areas such as data integrity, gamification, and artificial intelligence integration. Notably, discredited technologies like blockchain have been avoided; instead, modern and sustainable approaches such as Immutable Ledgers and Verifiable Credentials have been adopted. Due to cost sensitivity, video analysis is positioned as the final layer of the verification process.</p>
</div>
</details>

---
layout: default
---

# 1. Introduction

<div class="lead mt-4">
The UnicAthlete MVP is a platform where <b>athletes</b> showcase their talents,
<b>scouts</b> discover potential, and <b>coaches</b> track athlete development.
</div>

<div class="cards mt-8" style="grid-template-columns: repeat(3, 1fr);">
  <div class="card" style="--c:#3b82f6">
    <div class="card-title">Athletes</div>
    <div class="card-body">Showcase talent and build a verifiable performance profile.</div>
  </div>
  <div class="card" style="--c:#a855f7">
    <div class="card-title">Scouts</div>
    <div class="card-body">Discover and evaluate emerging talent with trusted data.</div>
  </div>
  <div class="card" style="--c:#22c55e">
    <div class="card-title">Coaches</div>
    <div class="card-body">Track development over time and guide progression.</div>
  </div>
</div>

<div class="note mt-8">
This plan explains the backend architecture, the chosen technologies, and the
rationale behind each choice. Primary objective: deliver value quickly on a clean,
scalable structure that supports future growth and evolution.
</div>

<details class="fulltext">
<summary>Full text</summary>
<div class="ft-body">
<p>The UnicAthlete MVP is designed as a platform where athletes can showcase their talents, scouts can discover potential talents, and coaches can track athlete development. This technical implementation plan explains the backend architecture of the MVP, the chosen technologies, and the rationale behind these choices. Our primary objectives are to deliver value quickly while establishing a clean and scalable structure that allows for future growth and evolution.</p>
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

<details class="fulltext">
<summary>Full text</summary>
<div class="ft-body">
<p>The backend architecture of the UnicAthlete MVP is designed with a modular and layered approach. This structure ensures that each component has a specific responsibility and can be developed and scaled independently. The overall architecture consists of the following main layers: Client Apps, API Layer (Hono API), Business Layer, Data Layer, Background Workers, and Observability.</p>
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
    <div class="card-body">Core processes: user management, payments, notifications, and AI integrations.</div>
  </div>
  <div class="card" style="--c:#10b981">
    <div class="card-title">Data Layer</div>
    <div class="card-body">PostgreSQL (relational), Redis (cache & queue), MinIO (object storage).</div>
  </div>
  <div class="card" style="--c:#ec4899">
    <div class="card-title">Background Workers</div>
    <div class="card-body">Async work: email, push, AI processing, PDF/report exports, scheduled tasks.</div>
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
<p><b>Business Layer:</b> Houses core business processes such as user management, payment processing, notifications, and AI integrations.</p>
<p><b>Data Layer:</b> Includes components like PostgreSQL (relational database), Redis (caching and queue management), and MinIO (object storage).</p>
<p><b>Background Workers:</b> Asynchronously executes long-running or intensive operations such as email delivery, push notifications, AI processing, PDF/report exports, and scheduled tasks.</p>
<p><b>Observability:</b> GlitchTip (error tracking) and Uptime Kuma (service status monitoring) are used to monitor system health and performance.</p>
</div>
</details>

---
layout: default
---

# 3. Core Technology Stack & Rationale

<div class="lead mt-3">The stack is guided by six core principles:</div>

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
    <div class="card-body">Bring the MVP to market quickly.</div>
  </div>
  <div class="card" style="--c:#38bdf8">
    <div class="card-title">Easy self-hosting</div>
    <div class="card-body">Keep infrastructure costs under control.</div>
  </div>
  <div class="card" style="--c:#a855f7">
    <div class="card-title">No premature microservices</div>
    <div class="card-body">Start monolithic; scale only when needed.</div>
  </div>
  <div class="card" style="--c:#14b8a6">
    <div class="card-title">Scale without rewrites</div>
    <div class="card-body">Chosen tech natively supports future growth.</div>
  </div>
</div>

<details class="fulltext">
<summary>Full text</summary>
<div class="ft-body">
<p>The technology stack selected for the UnicAthlete MVP is based on the following core principles:</p>
<p><b>Minimal Moving Parts:</b> To reduce complexity and increase development speed.</p>
<p><b>Mostly Open-Source:</b> To lower costs and benefit from community support.</p>
<p><b>Rapid Development:</b> To bring the MVP to market quickly.</p>
<p><b>Easy Self-Hosting:</b> To keep infrastructure costs under control.</p>
<p><b>Avoiding Premature Microservices:</b> Starting with a monolithic structure to avoid unnecessary complexity, scaling only when needed.</p>
<p><b>Scalability Without Rewrites:</b> Ensuring that initially selected technologies can natively support future growth.</p>
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
  <span class="pill" style="--c:#14b8a6">can scale later without rewriting</span>
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
    <div class="card-body">Secure, scalable authentication. Manages user sessions and authorization processes.</div>
  </div>
</div>

<details class="fulltext">
<summary>Full text</summary>
<div class="ft-body">
<p><b>Hono (API Framework):</b> A lightweight, fast, and web-standard-compliant API framework. It offers high performance even in edge environments. It aligns perfectly with our goals of rapid development and low resource consumption.</p>
<p><b>Zod (Data Validation):</b> A schema-based data validation library fully compatible with TypeScript. It ensures the reliability and type safety of incoming API requests and data structures, catching errors at an early stage.</p>
<p><b>Better Auth (Authentication):</b> Provides secure and scalable authentication solutions. It manages user sessions and authorization processes.</p>
</div>
</details>

---
layout: default
---

# 3.2 Data Management

<div class="cards mt-3" style="grid-template-columns: 1fr 1fr;">
  <div class="card" style="--c:#38bdf8">
    <div class="card-title">PostgreSQL — Database</div>
    <div class="card-body">Robust, reliable, open-source relational database. A secure long-term choice with strong scalability and community support.</div>
  </div>
  <div class="card" style="--c:#22c55e">
    <div class="card-title">Drizzle — ORM</div>
    <div class="card-body">Modern ORM with end-to-end TypeScript type safety. Simplifies database interactions and boosts efficiency.</div>
  </div>
  <div class="card" style="--c:#dc2626">
    <div class="card-title">Redis — Cache & Queue</div>
    <div class="card-body">High-performance key-value store: caching, rate limiting, BullMQ backend, ephemeral data, future WebSocket scaling.</div>
  </div>
  <div class="card" style="--c:#e11d48">
    <div class="card-title">MinIO — Object Storage</div>
    <div class="card-body">S3-compatible, open-source storage for images, documents, and exports. Cloud-provider independence and cost efficiency.</div>
  </div>
</div>

<details class="fulltext">
<summary>Full text</summary>
<div class="ft-body">
<p><b>PostgreSQL (Database):</b> A robust, reliable, and open-source database for relational data. Thanks to its extensive feature set, scalability, and robust community support, it represents a secure long-term solution.</p>
<p><b>Drizzle ORM (Object-Relational Mapper):</b> A modern ORM that provides end-to-end type safety with TypeScript. It simplifies database interactions and increases development efficiency.</p>
<p><b>Redis (Cache and Queue):</b> A high-performance key-value store. It will be utilized for Caching (reduces database load and speeds up response times), Rate Limiting (controls request rates to prevent API abuse), BullMQ Queue Backend (manages background jobs), Ephemeral Data Storage (fast storage for short-lived data), and Future WebSocket Scaling (foundation for real-time communication).</p>
<p><b>MinIO (Object Storage):</b> An S3-compatible, open-source object storage solution. It is used for storing large files such as images, documents, exported reports, and other attachments. It provides cloud provider independence and cost efficiency.</p>
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
<p><b>BullMQ (Message Queue):</b> A robust, Redis-based queue system for managing background jobs. It increases the system's responsiveness by decoupling long-running operations (email delivery, AI processing, PDF generation) from API requests.</p>
<p><b>Pino (Logging):</b> A fast and low-overhead Node.js logging library. It provides highly performant logging for production environments.</p>
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
  <span class="pill" style="--c:#a855f7">Temporary data</span>
  <span class="pill" style="--c:#14b8a6">Future WebSocket scaling</span>
</div>

::right::

# MinIO Usage

<div class="chips flex-col items-start mt-6 text-lg">
  <span class="pill" style="--c:#3b82f6">Images</span>
  <span class="pill" style="--c:#38bdf8">Documents</span>
  <span class="pill" style="--c:#22c55e">Exports</span>
  <span class="pill" style="--c:#14b8a6">Attachments</span>
  <span class="pill" style="--c:#a855f7">S3-compatible object storage</span>
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

# Trust & Verification

<div class="lead mt-3">
We chose modern, sustainable approaches over hype — no blockchain.
</div>

<div class="cards mt-5" style="grid-template-columns: 1fr 1fr;">
  <div class="card" style="--c:#94a3b8">
    <div class="card-title" style="text-decoration: line-through;">Blockchain</div>
    <div class="card-body">Discredited and unnecessarily costly for this use case — deliberately avoided.</div>
  </div>
  <div class="card" style="--c:#38bdf8">
    <div class="card-title">Immutable Ledgers</div>
    <div class="card-body">Tamper-evident records that protect data integrity without blockchain overhead.</div>
  </div>
  <div class="card" style="--c:#a855f7">
    <div class="card-title">Verifiable Credentials</div>
    <div class="card-body">Cryptographically verifiable claims for trusted athlete achievements.</div>
  </div>
  <div class="card" style="--c:#ec4899">
    <div class="card-title">Video Analysis — final layer</div>
    <div class="card-body">Due to cost sensitivity, positioned as the last step of the verification process.</div>
  </div>
</div>

<details class="fulltext">
<summary>Full text</summary>
<div class="ft-body">
<p>Notably, discredited technologies like blockchain have been avoided; instead, modern and sustainable approaches such as Immutable Ledgers and Verifiable Credentials have been adopted. These protect data integrity without blockchain overhead and provide cryptographically verifiable claims for trusted athlete achievements. Due to cost sensitivity, video analysis is positioned as the final layer of the verification process.</p>
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
layout: default
---

# 4. Conclusion

<div class="lead mt-4">
This plan builds the UnicAthlete MVP on a <b>robust, scalable, and cost-effective</b>
backend. The selected technologies and architectural approaches support rapid
development and future growth, while remaining critical to achieving
<b>data integrity</b> and <b>optimal user interaction</b>.
</div>

<div class="lead mt-6">
This blueprint positions UnicAthlete as an innovative and structurally sound platform
in the field of <b>sports talent discovery and development</b>.
</div>

<div class="chips mt-8 text-base">
  <span class="pill" style="--c:#22c55e">Robust</span>
  <span class="pill" style="--c:#3b82f6">Scalable</span>
  <span class="pill" style="--c:#f97316">Cost-effective</span>
  <span class="pill" style="--c:#a855f7">Data integrity</span>
</div>

<details class="fulltext">
<summary>Full text</summary>
<div class="ft-body">
<p>This technical implementation plan ensures that the UnicAthlete MVP is built upon a robust, scalable, and cost-effective backend infrastructure. The selected technologies and architectural approaches support rapid development and future growth while being critical to achieving data integrity and optimal user interaction goals. This blueprint will help position UnicAthlete as an innovative and structurally sound platform in the field of sports talent discovery and development.</p>
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
Hono + PostgreSQL + Redis + BullMQ + MinIO + GlitchTip + Uptime Kuma
</div>
