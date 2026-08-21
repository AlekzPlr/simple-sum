---
title: Product Brief: simple-sum
status: draft
created: 2026-08-21
updated: 2026-08-21
---

# Product Brief: simple-sum

## Executive Summary

`simple-sum` is a minimal interactive command-line application that asks a user for two numbers, computes their sum, and displays the result. It accepts integers and decimals, handles invalid input gracefully, and runs inside a Docker container. It is deliberately tiny.

The product exists for two reasons, and it is important to keep them separate. As a *product*, `simple-sum` is a small, well-specified utility: prompt for two numbers, show the sum. As a *process vehicle*, it is the subject used to exercise the complete BMAD + Cursor multi-agent + GitHub + Docker lifecycle defined by the project playbook. This brief defines the product — the *what* and the *why* — and is intentionally silent on implementation architecture, tooling, and workflow, which belong to downstream artifacts.

Nothing here should be expanded to make the product "more interesting." The product is the spec, and the spec is small.

## Product Definition vs. Development Process

These two concerns are kept separate and must not be conflated:

- **Product requirements** — what the application does and how it behaves for its user. These are fully defined below (Solution, Scope, Success Criteria).
- **Development-process requirements** — how the application is built and delivered (BMAD lifecycle, multi-agent workflow, GitHub, Docker build). These are the *reason this project exists* but are not product scope. They are addressed by the project playbook and the downstream PRD, not by this brief.

If a requirement cannot be stated as user-visible behavior or a product boundary, it does not belong in this brief.

## The Problem

There are two problems this product addresses, at different levels, and it is honest to name both.

At the process level: the team needs a canonical, unambiguous product definition to drive the BMAD + Cursor + GitHub + Docker lifecycle end-to-end. A real product with real complexity would obscure the lifecycle; a well-specified minimal product makes it observable and repeatable.

At the functional level: a user needs to sum two numbers from a terminal without fuss — integers or decimals, with clear feedback when input is wrong.

The cost of not defining this precisely is scope creep: the temptation to add features to justify the exercise. The product's smallness is a requirement, not a placeholder.

## The Solution

`simple-sum` is a Python command-line program, executed inside Docker, that:

1. Prompts the user for two numbers.
2. Reads and validates each input.
3. If an input is not a valid number, prints an error message and re-prompts for that same number until valid input is received.
4. Computes the sum of the two valid numbers.
5. Displays the result.

The experience is a short, linear interaction: enter a number, enter another, read the sum. There is no session, no state beyond the two inputs, and no way to misuse it beyond entering non-numeric text — which is handled.

## What Makes This Different

Honestly: nothing technically. `simple-sum` is not differentiated by an unfair advantage, and it should not pretend to be. Its differentiators are process qualities, not product features:

- **Disciplined minimalism** — the product is complete precisely because it does less. Scope is the feature.
- **A precise, testable specification** — every behavior is defined (validation, error text, re-prompting, result display), so the definition can pass a gate and seed a PRD without ambiguity.

Its real advantage is that a crisp minimal spec makes the full lifecycle demonstrable and reusable for future, more complex products.

## Who This Serves

- **Primary: the development team.** The team is the real audience. Success is a product definition that is unambiguous enough to drive the lifecycle end-to-end and pass a gate without re-litigating scope.
- **Secondary: the terminal user.** Someone who types two numbers and reads a sum. Success is a correct result with clear, predictable feedback.

## Success Criteria

How we know `simple-sum` is working:

1. Given two integers, it displays their correct sum.
2. Given two decimal numbers, it displays their correct sum.
3. Invalid numeric input prints exactly the error message `Error: please enter a valid number.` and re-prompts, rather than crashing.
4. No invalid input causes an unhandled traceback.
5. The application runs inside a Docker container.
6. The implementation consists of `main.py` and a `Dockerfile` (plus `.dockerignore` if useful), with no external Python libraries.
7. No product functionality exists beyond entering two numbers and displaying their sum.

These are product outcomes; how they are verified (tests, CI, Docker build) is development-process concern and out of scope here.

## Scope

**In (first version):**

- Prompt for two numbers and display their sum.
- Accept integers and decimals.
- Display decimal results as-is, without rounding or formatting (floating-point artifacts are acceptable).
- Validate input and re-prompt on invalid input.
- Run inside a Docker container.
- All user-facing text in English.

**Out (explicitly):**

- Graphical or web interface.
- Any database, API, or external service.
- Third-party Python packages (none required).
- Any product feature beyond two-number summation.
- Anything that exists only to make the project more complex.

**Deferred to PRD:** the precise Docker run mode (interactive TTY vs. piped stdin) and the exact prompt wording. These affect behavior but not the product definition, and will be pinned down during PRD without changing this brief's scope.

## Vision

There is no growth path, and that is intentional. If this succeeds, it stays exactly as small as it is — and in doing so validates the lifecycle so that future products can reuse the same discipline. The vision is not a bigger `simple-sum`; it is a reliable, repeatable way to take any idea from brief to working, containerized software.
