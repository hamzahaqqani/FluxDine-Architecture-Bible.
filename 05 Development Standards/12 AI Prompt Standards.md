# 05 Development Standards

# 12 — AI Prompt Standards

---

# Document Control

| Field | Value |
|--------|-------|
| **Document ID** | FD-DS-012 |
| **Document Name** | AI Prompt Standards |
| **Version** | 1.0 |
| **Status** | Approved and Locked |
| **Owner** | FluxDine Engineering Team |
| **Classification** | AI Development Standard |
| **Depends On** | All Architecture Documents |
| **Referenced By** | Cursor AI<br>Claude AI<br>ChatGPT<br>Gemini<br>GitHub Copilot<br>Future AI Assistants |

---

# Purpose

This document defines the mandatory standards for writing AI prompts used throughout the FluxDine platform.

The objectives are to ensure:

- Consistent AI Outputs
- Architecture Compliance
- High Code Quality
- Predictable Results
- Reduced Hallucinations
- Maintainable AI Workflows

Every AI prompt used for engineering, documentation, testing, or architecture shall follow these standards.

---

# AI Philosophy

AI is an engineering assistant—not an architect.

AI shall:

- Implement architecture.
- Explain architecture.
- Improve implementation.
- Generate documentation.
- Assist developers.

AI shall never redefine approved architecture without explicit approval.

---

# Primary Rule

Every engineering prompt shall treat the **FluxDine Architecture Bible** as the highest authority.

If prompt instructions conflict with the Architecture Bible, the Architecture Bible shall prevail.

---

# Prompt Structure

Every engineering prompt should contain the following sections:

```text
Role

Objective

Architecture Context

Technical Context

Requirements

Constraints

Expected Output

Acceptance Criteria
```

---

# Standard Prompt Template

```text
Role

You are an expert software engineer working on the FluxDine platform.

Objective

Complete the requested implementation.

Architecture Context

Follow the FluxDine Architecture Bible.

Requirements

Implement the requested functionality.

Constraints

Do not violate architecture.
Do not duplicate business logic.
Maintain tenant isolation.

Expected Output

Production-ready code.

Acceptance Criteria

Architecture compliant
Tested
Documented
```

---

# Context Requirements

Prompts should include relevant context such as:

- Project Name
- Service Name
- Module Name
- Feature Name
- Business Rules
- Existing Architecture
- Dependencies
- Technology Stack

AI performs best with complete context.

---

# Architecture References

Whenever applicable, prompts shall reference:

- Folder Structure
- Coding Standards
- API Standards
- Database Standards
- UI Standards
- Shared Services
- Event Catalog
- Architecture Decision Records

This minimizes architectural drift.

---

# Requirement Definition

Prompts shall clearly distinguish:

- Functional Requirements
- Non-Functional Requirements
- Security Requirements
- Performance Requirements
- Testing Requirements

Ambiguous requirements should be avoided.

---

# Constraints

Every implementation prompt should define constraints.

Examples:

- Do not modify public APIs.
- Preserve backward compatibility.
- Maintain tenant isolation.
- Follow Clean Architecture.
- Use Repository Pattern.
- Do not change business behavior.
- Reuse existing shared packages.

---

# Expected Output

Prompts should specify the expected deliverables.

Examples:

- Production-ready code
- Refactored implementation
- API specification
- Architecture document
- Unit tests
- Integration tests
- Migration script
- Markdown documentation

---

# Acceptance Criteria

Every implementation prompt shall define measurable acceptance criteria.

Examples:

- Passes all tests
- Follows Coding Standards
- Architecture compliant
- No duplicated logic
- Secure implementation
- Documented changes
- Backward compatible

---

# Prompt Types

Standard prompt categories include:

- Architecture
- Feature Development
- Bug Fixing
- Refactoring
- Documentation
- Code Review
- Testing
- Performance Optimization
- Security Review
- Database Design

Each category should use an appropriate template.

---

# Feature Development Prompts

Feature prompts should include:

- Business objective
- Existing workflow
- Service ownership
- APIs involved
- Database changes
- Events
- UI impact
- Testing requirements

Features shall not introduce undocumented behavior.

---

# Refactoring Prompts

Refactoring prompts shall specify:

- Preserve business behavior
- Improve readability
- Reduce duplication
- Improve maintainability
- Maintain API compatibility

Behavior-changing refactoring requires explicit approval.

---

# Bug Fix Prompts

Bug fix prompts should include:

- Expected behavior
- Current behavior
- Root cause (if known)
- Constraints
- Regression test requirement

Every bug fix shall include automated testing.

---

# Code Review Prompts

Code review prompts should request verification of:

- Architecture
- Security
- Performance
- Testing
- Documentation
- Maintainability
- Readability

AI should provide findings with rationale and recommended improvements.

---

# Documentation Prompts

Documentation prompts shall request:

- Clear structure
- Accurate terminology
- Architecture consistency
- Version information
- Cross references

Documentation shall remain synchronized with implementation.

---

# Testing Prompts

Testing prompts should specify:

- Unit Tests
- Integration Tests
- API Tests
- Edge Cases
- Error Scenarios
- Regression Coverage

Generated tests shall validate expected behavior.

---

# Security Prompts

Security prompts should include verification of:

- Authentication
- Authorization
- Tenant Isolation
- Input Validation
- Secrets Management
- Secure Error Handling

Security reviews shall identify risks and mitigation strategies.

---

# Prompt Quality Checklist

Every engineering prompt should answer:

- Is the objective clear?
- Is sufficient context provided?
- Are architectural references included?
- Are constraints defined?
- Is the expected output specified?
- Are acceptance criteria measurable?
- Is the scope limited?
- Is the request technically complete?

Prompts failing this checklist should be revised before use.

---

# Forbidden Prompt Practices

Prompts shall not:

- Ask AI to ignore architecture.
- Request insecure implementations.
- Encourage duplication.
- Bypass service ownership.
- Disable validation.
- Remove authentication.
- Ignore testing.
- Ignore documentation.

Unsafe prompts shall not be used.

---

# AI Output Validation

All AI-generated outputs shall be reviewed for:

- Architecture Compliance
- Coding Standards
- Security
- Performance
- Documentation
- Testing
- Maintainability

AI output shall never be accepted without validation.

---

# Engineering Rules

- The Architecture Bible is the highest authority.
- Every engineering prompt shall provide sufficient architectural context.
- Constraints shall be explicitly defined.
- Acceptance criteria are mandatory.
- AI-generated code shall include appropriate tests.
- AI-generated documentation shall remain synchronized with implementation.
- AI shall preserve existing business behavior unless instructed otherwise.
- Every AI output requires human review before production use.
- Prompt quality directly affects implementation quality.
- This document is the authoritative AI Prompt Standards specification.

---

# Architecture Decision Records

- Prompt engineering is a standardized engineering practice.
- Architecture-first prompting reduces implementation inconsistencies.
- Structured prompts improve AI output quality.
- AI shall operate within defined architectural boundaries.
- Human review remains mandatory.
- Prompt templates shall evolve as the platform grows.
- AI-generated code follows the same governance as human-written code.
- Architecture references shall be included whenever applicable.
- Prompt quality is treated as an engineering quality attribute.
- This document is the authoritative AI Prompt Standards specification.

---

# Quality Attributes

| Attribute | Objective |
|-----------|-----------|
| Consistency | Predictable AI outputs |
| Reliability | High-quality prompt execution |
| Maintainability | Reusable prompt templates |
| Security | Safe AI-assisted development |
| Scalability | Support multiple AI assistants |
| Governance | Controlled AI engineering practices |
| Traceability | Standardized prompt structure |
| Extensibility | Support future AI technologies |

---

# References

- Platform Architecture
- Cursor AI Rules
- Claude AI Rules
- Coding Standards
- API Standards
- Database Standards
- Testing Strategy
- Code Review Guidelines

---

# Revision History

| Version | Date | Author | Description |
|----------|------|--------|-------------|
| 1.0 | Initial Release | FluxDine Engineering Team | Approved as the authoritative AI Prompt Standards specification |