# Policy Tracker

Power Apps canvas app. Consult `/var/home/Phaderon/PowerApps/` (the "Power Apps Bible") for general Power Apps rules, known-bad patterns, and control reference — check `reference/database.html` there first.

## Before writing any "full replacement" formula for OnVisible, a button OnSelect, or anything else already built

**The files in `yaml/` on disk go stale fast.** The user builds live in Studio and only exports back to a GitHub issue when asked — local files can lag behind by hours or days, and the app keeps growing features (e.g. a whole Thursday date-picker block got added to `ViewItem.OnVisible` between two sessions with no warning). Pasting a "full replacement" built from a stale file will silently delete whatever was added since, and it will look like data loss to the user, not like a stale-cache bug.

**Before generating a full-property replacement for anything non-trivial:**
1. Check `gh issue list --repo PhadeDev/policy-tracker --state all --limit 15` for the newest export-bearing issue (look for the freshest "ALL YAMLS" or numbered screen export).
2. Pull that attachment and diff it against `yaml/*.yaml` before trusting either as current.
3. If there's any doubt, ask the user for a fresh export rather than guess — a targeted question costs one message; a wrong full-property paste can break the live app with no undo once it's saved.

For small, well-isolated edits (a single line, a clearly-scoped block), prefer giving anchored "find this exact text, change to this" instructions over a full-property paste — smaller surface area for staleness to matter.

## Validation

Run `~/Projects/payaml-validate/validate.sh <file>` on any YAML before handing it over. Run `python3 ~/PowerApps/tools/audit-guide.py <path>` on any guide HTML before publishing — both are also wired as PostToolUse hooks, but check manually too.

## GitHub Pages

Rebuilds don't always auto-trigger reliably. After pushing a guide change, verify with:
```
gh api repos/PhadeDev/policy-tracker/pages/builds/latest --jq '.commit,.status'
```
against the commit you just pushed. If it's stuck on an old commit, kick it manually:
```
gh api repos/PhadeDev/policy-tracker/pages/builds -X POST
```
