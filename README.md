<p align="center">
  <img src="assets/backplanes-logo.png" alt="Backplanes" width="420">
</p>

<h1 align="center">Backplanes CLI</h1>

Turn long agent sessions into clear, local HTML reports.

Backplanes CLI reads your agent transcript, asks an analyzer to summarize what
happened, and renders a report you can inspect, share, or use during review. It
is designed for engineers who want a fast answer to:

- What did this agent session do?
- Were there security-relevant findings?
- Which files, tools, and workflows mattered?
- What should I review before trusting the result?

## Table of Contents

- [Install](#install)
- [Quick Start](#quick-start)
- [What You Get](#what-you-get)
- [Common Workflows](#common-workflows)
- [Command Reference](#command-reference)
- [Tips](#tips)

## Install

Install the latest release:

```sh
curl -fsSL https://backplanes.com/spotlight/install.sh | sh
```

Verify the install:

```sh
backplanes --version
backplanes --help
```

Update later:

```sh
backplanes update
```

Check for an update without installing it:

```sh
backplanes update --check
```

## Quick Start

From a project where you have Claude Code sessions:

```sh
backplanes report
```

Open the report as soon as it is written:

```sh
backplanes report --open
```

Preview what Backplanes would analyze without invoking the analyzer:

```sh
backplanes report --dry-run
```

## What You Get

Each report is a local HTML file with:

- A top-level verdict for the session.
- Security findings, when the session contains meaningful concerns.
- Informational findings that are not treated as errors.
- Evidence and commands tied back to transcript activity.
- Engineering analytics for review, delivery, and operational context.
- A one-line title so prior reports are easy to browse later.

Backplanes is intentionally local-first: reports are written on your machine and
can be opened in your browser.

## Common Workflows

### Report the current or latest session

```sh
backplanes report
backplanes report --open
```

Use this when you are already in the project directory and want the most likely
current session.

### Report a specific session

```sh
backplanes report --session <session-id>
backplanes report --session <session-id> --open
```

### Browse sessions before reporting

```sh
backplanes sessions list
backplanes sessions list --project-only --days 7
backplanes sessions report
```

`sessions report` opens an interactive picker. Use the arrow keys to select a
session, then press Enter to generate the report.

### Batch report recent sessions

```sh
backplanes sessions report --all --project-only --days 7
```

Use `--dry-run` first if you want to see the scope before spending analyzer
tokens:

```sh
backplanes sessions report --all --project-only --days 7 --dry-run
```

### Open an existing report

```sh
backplanes report list
backplanes report open
backplanes report open <session-id>
```

`report list` opens an interactive picker in a terminal. In scripts or pipes, it
prints report rows instead:

```sh
backplanes report list --limit 5 | cat
```

### Use a specific analyzer or model

```sh
backplanes report --analyzer claude --model claude-opus-4-1
backplanes report --analyzer codex --model gpt-5.2
```

### Use a custom analyzer command

```sh
backplanes report --classifier "claude -p --no-session-persistence"
```

Use this when you want full control over the command that produces findings
JSON.

## Command Reference

### `backplanes report`

Create, list, and open reports.

```sh
backplanes report
backplanes report latest
backplanes report from-transcript /path/to/session.jsonl
backplanes report list
backplanes report open
```

Useful options:

| Option | Description |
| --- | --- |
| `--session <id>` | Report a specific session. |
| `--open` | Open the report after writing it. |
| `--dry-run` | Show what would run without invoking the analyzer. |
| `--analyzer <auto|claude|codex>` | Choose the analyzer CLI. |
| `--model <name>` | Pass a model to the analyzer invocation. |
| `--classifier <command>` | Use a fully custom analyzer command. |

### `backplanes sessions`

Find sessions you can report on.

```sh
backplanes sessions list
backplanes sessions report
backplanes sessions report --all
```

Useful options:

| Option | Description |
| --- | --- |
| `--project-only` | Limit results to the current project. |
| `--project <dir>` | Limit results to an explicit project directory. |
| `--days <n>` | Only include sessions modified in the last `n` days. |
| `--limit <n>` | Limit printed session rows. |
| `--all` | Generate reports for every discovered session. |

### `backplanes update`

Keep the CLI current.

```sh
backplanes update
backplanes update --check
```

## Tips

- Start with `backplanes report --dry-run` if you are unsure which transcript
  will be analyzed.
- Use `backplanes sessions list --project-only --days 7` to narrow a busy
  session history.
- Use `backplanes report list` to reopen previous reports by title.
- Pass `--model` when you want to pin a specific analyzer model for consistency.
- Run `backplanes <command> --help` for the complete options for any command.
