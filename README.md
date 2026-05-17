# Elephant-Goldfish Workflow Skill (v1.5.0)

A native Gemini CLI skill implementing the **Elephant and Goldfish** methodology for intent-driven software engineering, as defined by Dave Rensin (2026).

## 🐘 The Methodology

This project eliminates "AI Drift" by separating high-context planning from zero-context execution.

- **The Elephant (Planning):** A long-running, high-context session that drafts an airtight SPEC.md.
- **The Goldfish (Execution):** A stateless, zero-context sub-agent that implements the spec in a clean sandbox.
- **The Mean Reviewer (Adversarial):** A ruthless auditor (personified here as **Froggy 🐸**) who attacks the spec and the code to find holes and bugs.

## 🛠️ Key Features

- **Absolute Path Protocol:** Ensures stateless sub-agents never lose track of the project root.
- **Recursive Agent Protection:** Orchestration logic that bypasses sub-agent spawning limitations.
- **Stateful YAML Frontmatter:** Persists retry counts and failure logs across stateless execution attempts.
- **Sandbox Execution:** All Goldfish work is performed in a 	mp-goldfish-run directory and verified before merging.

## 📁 Project Structure

- SKILL.md: The formal instruction set for the Gemini CLI.
- SPEC.md: The technical blueprint and state tracker.
- references/: Documentation on the Rensin methodology.

## 🚀 How to Use

Trigger the workflow using one of the following commands:
- 
un-goldfish-workflow
- start-elephant-planning
- goldfish-method-execute

## 📜 History

This project was built using the methodology it implements. It survived five rounds of brutal adversarial auditing by **Froggy** before being declared production-ready.

---
*"Documentation is the only truth."*
