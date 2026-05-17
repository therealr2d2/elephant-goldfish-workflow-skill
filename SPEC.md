---
RetryCount: 4
Version: 1.5.0
LastReviewer: Froggy (Satisfied)
---

# Specification: Elephant-Goldfish Workflow Skill

## Overview
This skill implements the "Elephant and Goldfish" methodology as defined by Dave Rensin (2026). It ensures that software engineering tasks are driven by explicit, written intent (The Elephant) and verified by a zero-context executioner (The Goldfish) and an adversarial reviewer (The Mean Reviewer).

## References
- **Source Methodology:** "Elephants, Goldfish and the New Golden Age of Software Engineering" by Dave Rensin (2026).
- **URL:** https://drensin.medium.com/elephants-goldfish-and-the-new-golden-age-of-software-engineering-c33641a48874
- **Local Reference:** `references/methodology.md`

## 1. Triggering Mechanisms
- Keywords: `run-goldfish-workflow`, `start-elephant-planning`, `goldfish-method-execute`.
- Natural Language: "Start the goldfish method", "Let's do the elephant/goldfish thing", "Run the rensin workflow".

## 2. Workflow Phases (Instruction Logic)

### Phase 1: The Elephant (Persistent Planning)
- **Goal:** Draft a "Goldfish-proof" `SPEC.md`.
- **Action:** Document requirements, constraints, edge cases, and testing strategies. 
- **Absolute Path Discovery (NEW):** The Elephant MUST determine the absolute path of the project root (e.g., using `pwd`) and include it in the Context Bundle instructions.
- **Context Bundle:** Gather file list + contents. **Mandatory:** All file contents MUST be Base64 encoded. Format: `<file path="relative/path/to/file" encoding="base64">BASE64_CONTENT</file>`. Use ONLY relative paths.
- **Personalization:** Ask user if they want to name their Mean Reviewer (e.g., "Froggy").
- **Review Preference:** Ask user if they want to skip Phase 2 (Adversarial Spec Review).
- **State Check:** Ensure YAML frontmatter is present at the top of the file.

### Phase 2: Adversarial Pre-Review (Spec Attack) - OPTIONAL
- **Action:** Spawn the Mean Reviewer (e.g., "Froggy").
- **Persona:** A ruthless architect eager to find bugs.
- **Prompt:** "You are [ReviewerName]. Ruthlessly attack this SPEC.md. Find holes and ambiguities."
- **Exit Condition:** Spec is revised until [ReviewerName] finds no flaws. 
- **Stall Protection:** **HARD LIMIT of 3 iterations.** If no agreement is reached, the user acts as **Human Arbiter** to break the tie or proceed.

### Phase 3: The Goldfish (Stateless Execution)
- **Action:** Spawn "Goldfish" sub-agent (`invoke_agent`).
- **Input:** Feed the `SPEC.md`, the Base64 Context Bundle, any **Failure Logs**, and the **Absolute Project Root Path**.
- **Workspace Setup:** The Goldfish MUST use the **Absolute Project Root Path** to create the sandbox:
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
- **Tooling Mandate:** The Goldfish MUST use its available file-writing tools (e.g., `write_file`). Simply outputting code in text is a failure.
- **Strict Path Mandate:** The Goldfish MUST write all files EXCLUSIVELY to the absolute `$target` directory. 
- **Verification Rule:** Before reporting success, the Goldfish MUST run `ls $target` to provide IRREFUTABLE proof that the files exist inside the sandbox.
- **Decoding Rule:** The Goldfish must decode file contents using `[System.Convert]::FromBase64String($content)` before writing.
- **Task:** Implement strictly inside the absolute `$target` using relative paths only.

### Phase 4: Adversarial Post-Review (Code Attack)
- **Action:** Spawn [ReviewerName] again.
- **Input:** Provide [ReviewerName] with the `SPEC.md` and the full contents of the `./tmp-goldfish-run/` directory.
- **Prompt:** "Compare code in `./tmp-goldfish-run/` against `SPEC.md`. Find bugs, hallucinations, or missing requirements."
- **Decision Matrix:**
  - **Pass:** If NO bugs are found, proceed IMMEDIATELY to Phase 5.
  - **Spec Failure:** If the spec was unclear, return to Phase 1.
  - **Execution Failure:** Increment `RetryCount` in YAML. If `RetryCount` <= 2, retry Phase 3 and **attach the Reviewer's critique as a Failure Log**. If > 2, return to Phase 1.

### Phase 5: Synthesis & Cleanup
- **Safety Check:** Check `git status`. If "modified", prompt user: "Manual Merge Required (Concurrent Changes Detected)".
- **Backup:** Create `./backups/pre-goldfish-$(Get-Date -Format "yyyyMMdd-HHmmss")/`.
- **Merge:** Present Synthesis Plan. Upon approval, use `robocopy ./tmp-goldfish-run ./ /MIR` (or equivalent) to mirror the temp state to the main project.
- **Cleanup:** Delete `./tmp-goldfish-run`.

## 3. Tool Constraints
- **Sub-Agent Flatness:** Elephant must provide all data; Goldfish/Reviewer cannot invoke further sub-agents.
- **Environment:** Must use PowerShell-compatible commands for cross-platform reliability on Windows.

## 4. User Experience (UX)
- Status: "🐘 Elephant at work...", "👺 [ReviewerName] is hunting for bugs in the Spec...", "🐟 Goldfish executing...", "👺 [ReviewerName] is attacking the code..."
