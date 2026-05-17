# Project Lore: The Birth of the Elephant-Goldfish Skill

## Context
This project was born from a deep dive into Dave Rensin's 2026 methodology, "Elephants, Goldfish and the New Golden Age of Software Engineering." The goal was to build a native Gemini CLI skill that eliminates "AI Drift" by separating high-context planning from zero-context execution.

## The Journey
Building this skill was a recursive exercise in the methodology itself. We used the Elephant-Goldfish workflow to *build* the Elephant-Goldfish workflow.

### The Adversary: Froggy 🐸
The development was defined by five rounds of brutal adversarial auditing. We personified the Mean Reviewer as "Froggy," a bug-eating architect who refused to settle for anything less than production-grade perfection. Froggy identified and forced us to solve:
- **Recursive Agent Blocks:** Sub-agents cannot spawn their own sub-agents.
- **State Amnesia:** Ensuring retry counters and failure logs are persisted in the SPEC.md YAML.
- **Merge Hazards:** Moving from simple copies to obocopy /MIR to handle deletions.

### The Great Path Leak 🐟
During Phase 3, we discovered that "Goldfish" (stateless sub-agents) often lose track of the project root and default to the user's home directory. This led to the creation of the **Absolute Path Protocol** (v1.5.0), a critical guardrail that ensures AI agents always work within their intended sandbox.

## How to Resume
If you are a future AI agent reading this:
1. Read SPEC.md for the technical blueprint.
2. Read SKILL.md for the implemented agent instructions.
3. Respect Froggy's legacy: Never trust an agent that doesn't prove its work with an absolute path and a shell verification.
