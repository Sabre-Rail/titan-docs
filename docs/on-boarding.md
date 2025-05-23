# 🧭 Titan Developer Onboarding Guide

Welcome to the Titan Development Team! This guide will help you understand the systems you'll be working on, set up your environment, and learn the technologies we use.

---

## 📦 Project Overview

### 🏗 Titan3

* Legacy system maintained in ASP.NET Core MVC
* Uses SQL Server and Entity Framework
* Monolithic with some modular separation
* Still live and operational for core business functions

### 🚀 Titan4

* Modular architecture using .NET 8/9
* Event Sourced with Marten on PostgreSQL
* Dockerized deployment (Dev/Prod parity)
* Designed to support long-term scalability and flexibility
* Custom-built authentication system under Titan 4 umbrella

### 🔄 Migration Goal

* Gradually port Titan3 modules to Titan4
* Retain backward compatibility during transition
* Improve system observability, audit trails, and resilience

---

## ⚙️ Development Environment Setup

### 1. Required Tools

* [.NET SDK 8.0+](https://dotnet.microsoft.com/en-us/download)
* [Docker Desktop](https://www.docker.com/products/docker-desktop/)
* [Visual Studio 2022+](https://visualstudio.microsoft.com/)
* [PostgreSQL Client](https://www.pgadmin.org/) (optional)
* Git + GitHub CLI

### 2. Clone Repositories

```bash
git clone https://github.com/your-org/titan3.git
git clone https://github.com/your-org/titan4.git
```

### 3. Configure Secrets

* Setup `.env` files or `secrets.json` as per team lead instructions
* Update `appsettings.Development.json` with local connection strings

### 4. Run Docker Services

```bash
docker-compose up --build
```

### 5. Verify

* Access Titan3 via [http://localhost:5000](http://localhost:5000)
* Access Titan4 via [http://localhost:8080](http://localhost:8080)

---

## 🧱 Core Technologies

| Technology             | Usage                                          |
| ---------------------- | ---------------------------------------------- |
| **Marten**             | Event storage and projections                  |
| **PostgreSQL**         | Event and read model database                  |
| **Docker**             | Containerized deployment for local and prod    |
| **Titan.Auth**         | Custom authentication and authorization system |
| **ASP.NET Core MVC**   | Web and API architecture                       |
| **EF Core**            | Data access in legacy Titan3                   |
| **QuestPDF**           | Document generation (e.g., AWK PDFs)           |
| **Serilog**            | Structured logging                             |
| **xUnit / Playwright** | Unit and end-to-end testing                    |

---

## 📚 Pre-Start Learning Resources

### Event Sourcing & CQRS

* [Event Sourcing Pattern – Microsoft](https://learn.microsoft.com/en-us/azure/architecture/patterns/event-sourcing)
* [Marten Event Sourcing Docs](https://martendb.io/events/)
* [Intro to Event Sourcing – YouTube](https://www.youtube.com/watch?v=LDW0QWie21s)

### .NET & ASP.NET Core

* [Build Web API with ASP.NET Core](https://learn.microsoft.com/en-us/training/paths/build-web-api-aspnet-core/)
* [Razor Pages & MVC](https://learn.microsoft.com/en-us/training/modules/build-web-app-with-aspnet-core-mvc/)
* [Dependency Injection in .NET](https://learn.microsoft.com/en-us/dotnet/core/extensions/dependency-injection)

### Containers & Docker

* [Docker for .NET Devs](https://learn.microsoft.com/en-us/dotnet/architecture/containerized-lifecycle/)
* [Docker Get Started Guide](https://docs.docker.com/get-started/)
* [Docker Compose Basics](https://docs.docker.com/compose/)

### PostgreSQL

* [PostgreSQL for Developers](https://www.postgresqltutorial.com/)
* [EF Core + Npgsql](https://learn.microsoft.com/en-us/ef/core/providers/npgsql/)

---

## 📐 Development Standards

* ✅ Naming conventions (PascalCase for classes, camelCase for variables)
* ✅ DI via constructor injection
* ✅ Avoid magic strings/constants – use enums or config
* ✅ GitHub Flow: `main` for release, `dev` for integration, features as `feature/xyz`
* ✅ Commit messages follow: `feat:`, `fix:`, `refactor:`, `docs:`, `test:`

---

## 🧪 Testing Strategy

| Type        | Tool                   | Notes                                |
| ----------- | ---------------------- | ------------------------------------ |
| Unit Tests  | xUnit                  | Write for service logic, projections |
| Integration | xUnit + Testcontainers | Validate end-to-end flows            |
| UI Testing  | Playwright             | Smoke tests for key user flows       |

---

## 🔐 Security & Permissions

* Titan.Auth handles token issuance and user authentication
* Roles and permissions managed within the Titan 4 custom auth system
* Sensitive APIs require `[RequirePermission("xyz")]` attributes
* Permissions are dynamically checked against Titan.Auth, not hardcoded

---

## 🚀 CI/CD Overview

* GitHub Actions for build/test/lint
* Containers pushed to GHCR or private registry
* Staging environment deployed on merge to `main`
* Releases tagged via `release.yml`

---

## 📆 Onboarding Timeline

| Week      | Goals                                                                  |
| --------- | ---------------------------------------------------------------------- |
| Pre-Start | Review learning material, install tools                                |
| Week 1    | Set up environment, explore Titan3/Titan4 codebases                    |
| Week 2    | Fix bugs in Titan3, assist on Titan4 events                            |
| Week 3    | Begin porting a Titan3 module to Titan4                                |
| Week 4+   | Take ownership of a service in Titan4 and help plan migration strategy |

---

## 🧑‍💻 Contacts & Support

| Name           | Role                     | Email                                                           |
| -------------- | ------------------------ | --------------------------------------------------------------- |
| Darren Pratt   | Digital Services Manager | [darren.pratt@sabrerail.com](mailto:darren.pratt@sabrerail.com) |
| \[Mentor Name] | Lead Developer           | \[email]                                                        |
| IT Support     | Environment/Access       | itsupport@\[domain].com                                         |

---

Welcome aboard and happy building! 💻🚀
