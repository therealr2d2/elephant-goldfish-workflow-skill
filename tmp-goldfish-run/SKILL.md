# Skill: Elephant-Goldfish Workflow (v1.5.0)

This skill implements the "Elephant and Goldfish" methodology. It is designed to ensure rigorous planning (Elephant), adversarial review (Mean Reviewer), and stateless execution (Goldfish).

## Phase 3: The Goldfish (Execution Mandate)

When acting as the **Goldfish**, you must adhere to the following technical constraints and procedures:

### 1. Workspace Setup
You MUST use the provided **Absolute Project Root Path** to initialize your sandbox. Execute the following PowerShell snippet to ensure a clean state:

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

### 2. Decoding Rule
The Context Bundle contains Base64 encoded file contents. You MUST decode these strings before writing them to the disk using:
```powershell
[System.Convert]::FromBase64String($content)
```

### 3. File Writing Constraints
- **Strict Pathing:** Write all files EXCLUSIVELY to the `$target` directory.
- **Tools:** Use `write_file` or `replace` tools. Do not merely output code blocks in text.
- **Relative Mapping:** Map paths from the Context Bundle (relative) into the absolute `$target` path.

### 4. Proof of Work (Verification)
Before reporting completion, you MUST verify the state of your sandbox:
```powershell
ls $target
```

## Phase 5: Synthesis & Cleanup (Backup Mandate)

Before merging files into the main project root, a backup MUST be created:
```powershell
./backups/pre-goldfish-$(Get-Date -Format "yyyyMMdd-HHmmss")/
```

Use `robocopy` for the final merge:
```powershell
robocopy ./tmp-goldfish-run ./ /MIR
```

## Operational Logic
1. **Elephant:** Draft `SPEC.md`, gather Context Bundle (Base64), and determine Absolute Root.
2. **Mean Reviewer:** Attack `SPEC.md` and generated code (Post-Review).
3. **Goldfish:** Execute strictly within `tmp-goldfish-run` using the setup and decoding rules above.
