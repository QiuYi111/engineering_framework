<div align="center">
  <h1>Framework</h1>
  <p><em>"Pragmatic > Dogmatic. Automation > Manual. Consensus > Command."</em></p>
</div>

---

This framework is distilled from high-performance engineering teams to solve the problem of "Chaos in Growth." It enforces process strictly by leveraging Environment as Code, Contract-First design, Strict Domain-Driven Design (DDD), and seamless AI Collaboration.

## 🌟 The Core Philosophy

- **Environment as Code:** "It works on my machine" is an invalid defense. If it requires more than `make init` and `make up` to start developing, it is broken. Docker Compose and Dev Containers are not optional.
- **Contract First (Schema-Driven):** We do not write code to "see if it works." We agree on the interface, then we fulfill the contract. No backend code is written until the API definition (Protobuf/OpenAPI) is reviewed and merged.
- **Strict Isolation (Pragmatic DDD):** Physical design must prevent "Spaghetti Dependencies". The `domain` logic is pure and depends on **NOTHING**. The `infrastructure` (Database, HTTP/gRPC) depends on the `domain`, never the reverse.
- **AI-First Collaboration:** Workflows involve AI agents governed by customized prompts (`CLAUDE.md`). AI generates boilerplates, assists in Testing, and executes routines, while humans define architecture and verify functionality.

## 🏗 Standard Architecture

We employ a Three-Layer Standard to maintain maintainable and testable software without confusing terminology:

```text
Project/
├── api/                  # [Contract Layer] Protobufs / OpenAPI Specs
├── cmd/                  # [Boot Layer] Main applications (Wires everything)
├── internal/             # [Private Code]
│   ├── domain/           # [Core Domain] Pure Business Logic & Interfaces
│   └── infrastructure/   # [Infrastructure & Adapter] DB/HTTP Implementations
├── docs/                 # [Documentation] PRDs, Plans, Wikis, Reports
├── tests/                # [Verification] E2E / BDD Tests
├── scripts/              # [Automation] Helper scripts
├── 3rdParty/             # [Dependencies] Git submodules for external repos
└── Makefile              # [Interface] The Development Command Center
```

## 🚀 Getting Started

Follow these steps to apply the **Neural-Grid Framework** to your project:

1. **The AI Collaboration Foundation**: Check `templates/CLAUDE.md`. Copy it to your root and customize `<DOMAIN_EXPERT>` and `<TECH_STACK>` to establish operational contexts for AI agents.
2. **The Law**: Adopt `templates/CONTRIBUTING.md`. It sets the team's expectations immediately. Replace language/tools as needed but preserve the structural principles.
3. **The Interface**: Adopt `templates/Makefile`. It's the sole surface area everyone interacts with. Setup `make init`, `make test`, and `make lint` specific to your tech stack.
4. **The Gatekeeper**: Start using `templates/.pre-commit-config.yaml`. This ensures bad formatting or garbage code never touches the main branch.
5. **The Architecture**: Adopt Pragmatic DDD inside your `internal/` package as shown above to segregate domain logic from third-party or DB logic.

## 🔄 The "Golden Path" Workflow

Creativity belongs in the solution, not the process. The process should be boring and automatic. For any new feature ticket, proceed with:

1. **Define/Contract:** Change the API definition first (`api/proto` or equivalent).
2. **Generate:** Run `make proto` or similar to stamp out types and schemas.
3. **Test:** Write the exact failure case via BDD (Integration Tests) and TDD (Unit Tests).
4. **Implement:** Fill the `domain` implementation until tests turn Green. Refactor as needed.
5. **Verify:** Run the local gatekeeper (`make verify` or CI checks).

## 📄 Included Templates

This repository includes several ready-to-use boilerplate templates under `templates/`:

- `ARCHITECTURE.md`: Simple guide on isolating `domain` and `infrastructure`.
- `CLAUDE.md`: Highly opinionated prompt framework for strict Agent roles (TDD-RED, Implementer, Refactorer, Review).
- `CONTRIBUTING.md`: Development rules, Git workflow, and Makefile references.
- `PRD_TEMPLATE.md`: Structured, AI-readable Product Requirements Document standard.
- `.pre-commit-config.yaml`: Pre-configured pre-commit gating mechanism.
- `Makefile`: Foundation template for build environments.

---

**Observability & Gatekeeping**

> Logs are for machines, not humans (Banned: `print`, `console.log`). The CI/CD pipeline is the ultimate authority. If CI is red, the branch does not exist.
