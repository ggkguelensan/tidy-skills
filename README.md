# Tidy Skills

Three Codex skills for making small, behavior-preserving code changes using
Kent Beck's empirical software-design method:

- `tidy-typescript` — TypeScript structure changes and typed API migrations.
- `tidy-react` — React component, hook, state, effect, and boundary changes.
- `tidy-effector` — Effector and effector-react model changes.

Each directory is a self-contained skill with its own `SKILL.md`, agent
metadata, and reference guide.

## Installation

Copy the skill directories into your Codex skills directory:

```powershell
Copy-Item -Recurse tidy-* "$env:USERPROFILE\.codex\skills\"
```

Restart Codex after installing or updating the skills.
