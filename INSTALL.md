# Installing Intent Cherry-Pick

This package contains a Codex skill named `intent-cherry-pick`.

The installable skill directory is:

```text
Intent Cherry-Pick/intent-cherry-pick/
```

Install that directory into your Codex skills directory.

## Windows PowerShell

From the repository root:

```powershell
$skillsDir = Join-Path $HOME ".codex\skills"
New-Item -ItemType Directory -Force -Path $skillsDir | Out-Null
Copy-Item -Recurse -Force "Intent Cherry-Pick\intent-cherry-pick" $skillsDir
```

Verify:

```powershell
Get-ChildItem "$HOME\.codex\skills\intent-cherry-pick"
```

## macOS / Linux

From the repository root:

```bash
mkdir -p "$HOME/.codex/skills"
cp -R "Intent Cherry-Pick/intent-cherry-pick" "$HOME/.codex/skills/"
```

Verify:

```bash
ls "$HOME/.codex/skills/intent-cherry-pick"
```

## Updating An Existing Install

If the skill is already installed, replace the existing directory.

Windows PowerShell:

```powershell
$target = Join-Path $HOME ".codex\skills\intent-cherry-pick"
if (Test-Path $target) {
    Remove-Item -Recurse -Force $target
}
Copy-Item -Recurse "Intent Cherry-Pick\intent-cherry-pick" (Join-Path $HOME ".codex\skills")
```

macOS / Linux:

```bash
rm -rf "$HOME/.codex/skills/intent-cherry-pick"
cp -R "Intent Cherry-Pick/intent-cherry-pick" "$HOME/.codex/skills/"
```

## Activation

Restart Codex or start a new thread so the skill list is refreshed.

Then invoke it explicitly:

```text
Use $intent-cherry-pick to port commit <commit> from <source-branch> to <target-branch>.
```

Example:

```text
Use $intent-cherry-pick to port commit f437cc373 from pc_dev_main to pc_dev_taptap.
```

## Expected Installed Layout

```text
~/.codex/skills/intent-cherry-pick/
  SKILL.md
  agents/
    openai.yaml
  references/
    record-template.md
    review-checklists.md
```

## Notes

- Do not install the outer `Intent Cherry-Pick/` folder itself as the skill.
- Install the inner `intent-cherry-pick/` folder.
- The skill does not push temporary branches to remotes.
- The skill does not run project builds or tests by default.
