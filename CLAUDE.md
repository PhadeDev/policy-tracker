# Policy Tracker

Power Apps canvas app. Consult `/var/home/Phaderon/PowerApps/` (the "Power Apps Bible") for general Power Apps rules, known-bad patterns, and control reference — check `reference/database.html` there first.

**This is the only local clone of this repo.** A second, abandoned clone used to exist at `~/PowerApps-Apps/policy-tracker/` (stuck a week+ stale, from the brief paste-as-new-screen phase) and caused a real wrong-answer incident when a session read from it instead of this one — it was deleted 2026-07-14. If you ever see a `policy-tracker` folder anywhere other than here, treat it as suspicious, not as an alternate source of truth.

## Before writing any "full replacement" formula for OnVisible, a button OnSelect, or anything else already built

**The files in `yaml/` on disk go stale fast.** The user builds live in Studio and only exports back to a GitHub issue when asked — local files can lag behind by hours or days, and the app keeps growing features (e.g. a whole Thursday date-picker block got added to `ViewItem.OnVisible` between two sessions with no warning). Pasting a "full replacement" built from a stale file will silently delete whatever was added since, and it will look like data loss to the user, not like a stale-cache bug.

**Before generating a full-property replacement for anything non-trivial:**
1. Check `gh issue list --repo PhadeDev/policy-tracker --state all --limit 15` for the newest export-bearing issue (look for the freshest "ALL YAMLS" or numbered screen export).
2. Pull that attachment and diff it against `yaml/*.yaml` before trusting either as current.
3. If there's any doubt, ask the user for a fresh export rather than guess — a targeted question costs one message; a wrong full-property paste can break the live app with no undo once it's saved.

For small, well-isolated edits (a single line, a clearly-scoped block), prefer giving anchored "find this exact text, change to this" instructions over a full-property paste — smaller surface area for staleness to matter.

## Write proposed changes into the local files immediately, don't just say them in chat

The user applies changes by hand in Studio and often doesn't re-export for a while. A local file that's current-but-unconfirmed is far more useful than one that's clean-but-stale. So: whenever you propose a formula/property change for any screen, `Formulas.txt`, or `OnStart.txt`, write it directly into the actual file under `yaml/` right away, in the same turn.

**Editing the file and committing it are two separate steps — do not commit or push automatically.** Just edit the file and say what you changed and why. The user is new to git and specifically wants to stay the one who decides when something becomes a real, shared commit — don't take that decision for them, even for a small change. Wait for something like "commit that" / "push it" / "save that" before running any `git commit` or `git push`.

**Exception: guide HTML under `docs/guides/`.** Those only take effect once pushed (GitHub Pages serves them, and Studio paste-pages need to be live URLs) — for those specifically, ask "want me to commit and push this guide?" rather than defaulting to silent-edit-only, since an unpushed guide is just as useless to the user as no guide at all. The `yaml/` mirror files have no such requirement — they're pure local reference, no reason to rush them to GitHub.

**When a change does get committed**, mark it as proposed vs. confirmed-applied in the commit message (Power Fx doesn't reliably preserve inline comments through a Studio round-trip, so don't rely on those) — prefix with `PROPOSED:`, e.g. `PROPOSED: add ShowToast() to Formulas.txt`, then a later `CONFIRMED APPLIED:` commit once the user says they've actually pasted it into Studio. Never rewrite history to "fix" an earlier message — always add a new commit.

Before trusting any local file as the app's real current state: check whether it even has uncommitted changes first (`git status`/`git diff` — a proposed edit might be sitting there un-committed), then check whether its latest relevant commit says `PROPOSED` or `CONFIRMED APPLIED`. Either way it's still a guess relative to what's actually live in Studio, and still needs the same "diff against a fresh export before a full-property paste" discipline above.

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
