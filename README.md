# Agentic Workflows

Personal Codex marketplace containing reusable GitHub planning and issue-refinement skills.

## Included skills

- `$pathfinder`: Turn an ambiguous initiative into dependency-aware, agent-ready GitHub issues.
- `$grill-me`: Interrogate an issue for missing decisions before implementation begins.

## Install

Register the GitHub-backed marketplace on each device:

```powershell
codex plugin marketplace add connorwally/Agentic-Development --ref main
codex plugin add agentic-workflows@connor-personal
```

Start a new Codex thread after installation, then invoke a skill explicitly:

```text
$pathfinder
$grill-me 123
```

Authenticate the GitHub connector separately on each device. Credentials are not bundled with the plugin.

## Update

After changes are pushed to `main`, refresh and reinstall:

```powershell
codex plugin marketplace upgrade connor-personal
codex plugin add agentic-workflows@connor-personal
```

Start a new thread to pick up the updated skills.
