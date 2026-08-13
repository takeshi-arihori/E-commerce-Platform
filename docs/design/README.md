# Design

This directory contains confirmed design policies and links to architecture decisions.

Design is treated as a **trade-off decision process**: compare viable options under project constraints, select one, and record both the selected and rejected alternatives when the decision is significant.

The following six layers are used as a **design checklist**, not as a strict waterfall sequence.

## 1. Upstream Design

- Business process design
- Service design
- UX design
- Information architecture
- Domain design

## 2. Architecture Design

- System architecture
- Infrastructure
- Network
- Security
- Availability
- Reliability
- Performance
- Scalability
- Cost
- Operations / Monitoring
- Migration

## 3. External Design

- UI
- API
- Conceptual / Logical data model
- External integrations
- Authentication
- Authorization / Permissions
- Reports
- Error messages

## 4. Internal Design

- Modules / Packages
- Types / Classes
- Physical table design
- State transitions
- Transactions
- Concurrency / Consistency
- Logging
- Caching
- Naming conventions

## 5. Cross-cutting Design

- Test design
- CI/CD
- Deployment
- Repository workflow
- Development workflow
- Team / Responsibility boundaries

## 6. AI-assisted Development Design

- Context design
- Agent design
- Human-in-the-loop / Gate design

## Decision Records

Important choices must be documented in [`../adr/`](../adr/) with context, constraints, options, trade-offs, decision, rejected alternatives, and rationale.
