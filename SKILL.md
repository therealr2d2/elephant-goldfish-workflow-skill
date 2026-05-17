---
name: elephant-goldfish-workflow
description: Implements the Rensin "Elephant and Goldfish" methodology for intent-driven software engineering via persistent planning and zero-context execution.
---

# Skill: Elephant-Goldfish Workflow

This skill implements the "Elephant and Goldfish" methodology (Rensin 2026). It ensures that software engineering tasks are driven by explicit written intent and verified by a zero-context executioner and an adversarial reviewer.

<instructions>
## Workflow Overview
Always follow these five phases to ensure high-quality, reproducible code changes.

### Phase 1: The Elephant (Persistent Planning)
1. **Goal:** Draft a "Goldfish-proof" `SPEC.md`.
2. **Action:** Document all requirements, constraints, edge cases, and testing strategies.
3. **Personalization:** Ask the user: "What would you like to name your Mean Reviewer? (Default: Froggy)".
4. **Preference:** Ask the user if they wish to skip Phase 2 (Adversarial Spec Review).
5. **Absolute Path Discovery:** Determine the absolute path of the project root (e.g., via `pwd`) and prepare to include it in the Context Bundle instructions.
6. **Context Bundle:** Gather necessary files. **All contents MUST be Base64 encoded.** Format: `<file path="relative/path" encoding="base64">BASE64_CONTENT</file>`.
7. **Frontmatter:** Ensure `SPEC.md` has YAML frontmatter:
   ```yaml
   RetryCount: 0
   Version: 1.x.x
   LastReviewer: [ReviewerName]
   ```
8. **UX:** Display "🐘 Elephant at work...".

### Phase 2: Adversarial Pre-Review (Spec Attack) - OPTIONAL
1. **Action:** Spawn the Mean Reviewer (e.g., "[ReviewerName]").
2. **Persona:** A ruthless architect eager to find bugs and ambiguities.
3. **Stall Protection:** **HARD LIMIT of 3 iterations.** If no agreement is reached, the user acts as **Human Arbiter** to break the tie.
4. **UX:** Display "👺 [ReviewerName] is hunting for bugs in the Spec...".

### Phase 3: The Goldfish (Stateless Execution)
1. **Action:** Spawn a "Goldfish" sub-agent (`invoke_agent`) with NO previous chat history.
2. **Input:** Feed ONLY the `SPEC.md`, the Base64 Context Bundle, any **Failure Logs**, and the **Absolute Project Root Path**.
3. **Workspace Setup:** The Goldfish MUST use the **Absolute Project Root Path** to create the sandbox using this PowerShell logic:
   ```powershell
   $absRoot = "[ProvidedAbsolutePath]";
   $target = Join-Path $absRoot "tmp-goldfish-run";
   for ($i=0; $i -lt 5; $i++) {
     if (Test-Path $target) {
       try { Remove-Item -Recurse -Force $target -ErrorAction Stop; break }
       catch { Write-Host "Retrying cleanup..."; Start-Sleep -s 2 }
     } else { break }
   };
   New-Item -ItemType Directory $target -Force
   ```
4. **Tooling Mandate:** The Goldfish MUST use `write_file` or `replace` tools.
5. **Strict Path Mandate:** Write ALL files EXCLUSIVELY to the absolute `$target` directory.
6. **Decoding Rule:** Use `[System.Convert]::FromBase64String($content)` to decode contents.
7. **Verification Rule:** Before reporting success, the Goldfish MUST run `ls $target` to prove existence.
8. **UX:** Display "🐟 Goldfish executing...".

### Phase 4: Adversarial Post-Review (Code Attack)
1. **Action:** Spawn [ReviewerName] again.
2. **Input:** Provide [ReviewerName] with the `SPEC.md` and the full contents of the absolute `tmp-goldfish-run` directory.
3. **Decision Matrix:**
   - **Pass:** If NO bugs are found, proceed IMMEDIATELY to Phase 5.
   - **Spec Failure:** If the spec was unclear, return to Phase 1.
   - **Execution Failure:** Increment `RetryCount` in YAML. If <= 2, retry Phase 3 and **attach the Reviewer's critique as a Failure Log**. If > 2, return to Phase 1.
4. **UX:** Display "👺 [ReviewerName] is attacking the code...".

### Phase 5: Synthesis & Cleanup
1. **Safety Check:** Check `git status`. If "modified", prompt: "Manual Merge Required (Concurrent Changes Detected)".
2. **Backup:** Create a timestamped backup: `./backups/pre-goldfish-$(Get-Date -Format "yyyyMMdd-HHmmss")/`.
3. **Merge:** Present Synthesis Plan. Upon approval, use `robocopy [AbsTarget] [AbsRoot] /MIR`.
4. **Cleanup:** Delete the absolute `tmp-goldfish-run` directory.
</instructions>

<available_resources>
- **Methodology Reference:** `references/methodology.md`
- **Source Article:** https://drensin.medium.com/elephants-goldfish-and-the-new-golden-age-of-software-engineering-c33641a48874
</available_resources>
