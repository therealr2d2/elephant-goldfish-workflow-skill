# Methodology: Elephants, Goldfish, and Intent-Based Engineering

Derived from Dave Rensin's "Elephants, Goldfish and the New Golden Age of Software Engineering" (2026).

## The Core Problem
AI writes code faster than humans can read it. Relying on "vibe-based" coding or long-context chat history leads to "AI drift," where the agent makes assumptions the human hasn't verified.

## The Solution: Intent as Source Code
In the new era, the **Design Document** is the true source code. The actual implementation (Python, JS, etc.) is a transient artifact that should be perfectly reproducible from a high-quality spec.

## Roles

### 1. The Elephant (The High-Context Session)
- **Nature:** Long-running, broad memory.
- **Responsibility:** Deep thinking, brainstorming, and trade-off analysis.
- **Output:** A "Goldfish-proof" specification.

### 2. The Goldfish (The Zero-Context Session)
- **Nature:** Stateless, zero memory.
- **Responsibility:** Execution and Validation.
- **The Test:** If a clean agent cannot implement the task perfectly from the spec alone, the spec is the failure point, not the agent.

### 3. The Mean Reviewer (The Adversarial Agent)
- **Responsibility:** Red-teaming. Finding holes in the plan and flaws in the execution to ensure the human remains the ultimate judge of quality.

## The Golden Rule
**"Documentation is the only truth."** If it isn't in the spec, it doesn't exist in the project.
