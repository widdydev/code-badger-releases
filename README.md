<p align="center">
  <img src="assets/codebadger-logo.png" width="160" alt="CodeBadger logo: a badger wearing pixel sunglasses" />
</p>

<h1 align="center">CodeBadger</h1>

<p align="center">
  <strong>Big pull requests. Clear picture.</strong><br />
  Your desktop workspace for code reviews on GitHub — with Codex or Claude Code, evidence from the
  code, and you in charge.
</p>

<p align="center">
  <a href="https://github.com/widdydev/code-badger-releases/releases/latest"><strong>Download the latest release</strong></a>
  ·
  <a href="#install">Install</a>
  ·
  <a href="#features">Features</a>
  ·
  <a href="#faq">FAQ</a>
</p>

---

CodeBadger tells you which pull requests wait for you, explains each change step by step, checks
it against its ticket and its tests, and turns your findings into a native GitHub review. Your
review data stays on your computer. The AI provider you already use does the reading.

- **Know whose turn it is** — a dashboard and an inbox for the reviews you owe and your own pull
  requests.
- **Understand any change** — a guided walkthrough, even for pull requests with hundreds of files.
- **Check it properly** — findings with evidence, acceptance criteria from Linear, tests that
  really prove something, and a clear approve recommendation.
- **Publish to GitHub** — one native review with line comments, only when you confirm it.

## Contents

- [Install](#install)
- [First steps](#first-steps)
- [Features](#features)
  - [Dashboard: your to-dos](#dashboard-your-to-dos)
  - [Inbox: reviews you owe and your own pull requests](#inbox-reviews-you-owe-and-your-own-pull-requests)
  - [Walkthrough](#walkthrough)
  - [Findings](#findings)
  - [Large pull requests in chapters](#large-pull-requests-in-chapters)
  - [Acceptance criteria from Linear](#acceptance-criteria-from-linear)
  - [Tests and CI, without tautological tests](#tests-and-ci-without-tautological-tests)
  - [Approve recommendation](#approve-recommendation)
  - [Impact, risk and rollback](#impact-risk-and-rollback)
  - [Follow-up chat](#follow-up-chat)
  - [New commits and follow-up reviews](#new-commits-and-follow-up-reviews)
  - [Publish a GitHub review](#publish-a-github-review)
  - [Every file type](#every-file-type)
- [Keyboard shortcuts](#keyboard-shortcuts)
- [Settings](#settings)
- [Privacy and security](#privacy-and-security)
- [Updates](#updates)
- [FAQ](#faq)

## Install

| System                                      | File                                          |
| ------------------------------------------- | --------------------------------------------- |
| macOS 13 or later, Apple silicon            | `code-badger-<version>.dmg`                   |
| Windows 10 (1809) or later, x64 — _preview_ | `code-badger-<version>-windows-x64-setup.exe` |

1. Open the [latest release](https://github.com/widdydev/code-badger-releases/releases/latest) and
   download the file for your system.
2. **macOS:** open the `.dmg` and drag CodeBadger into Applications. The app is signed and
   notarized by Apple.
3. **Windows:** run the setup `.exe`. The Windows build is not signed yet, so SmartScreen may warn
   you. Choose **More info › Run anyway**.

Other files in a release (`.zip`, `.blockmap`, `latest*.yml`) are for automatic updates. You do
not need to download them.

### What CodeBadger needs

CodeBadger uses the command-line tools you already have. It runs no server of its own.

| Tool                                                                                                                     | Why                                    | Required    |
| ------------------------------------------------------------------------------------------------------------------------ | -------------------------------------- | ----------- |
| [Git](https://git-scm.com/)                                                                                              | Fetches the pull request's code        | Yes         |
| [GitHub CLI](https://cli.github.com/), signed in                                                                         | Reads pull requests, publishes reviews | Yes         |
| [Codex CLI](https://github.com/openai/codex) or [Claude Code](https://docs.anthropic.com/en/docs/claude-code), signed in | Runs the analysis                      | One of them |
| Linear personal API key                                                                                                  | Checks acceptance criteria of tickets  | Optional    |

Sign in once in a terminal:

```bash
gh auth login
codex login        # or: claude   (then /login)
```

**Settings › Environment** shows what CodeBadger found and what is still missing. On Windows,
Claude Code also needs Git for Windows (Git Bash); the Environment card explains the setup.

## First steps

1. **Open CodeBadger.** It starts on the **Dashboard**.
2. **Check your tools** in **Settings › Environment** (`⌘,` / `Ctrl+,`).
3. **Pick a pull request.** Open the **Inbox** and choose one that waits for you — or paste any
   GitHub pull request link (`⇧⌘V` / `Shift+Ctrl+V` imports the link from your clipboard).
4. **Start the analysis.** Choose Codex or Claude Code, the model and the effort. Importing costs
   nothing; only the analysis uses your provider.
5. **Review.** Read the walkthrough, check the findings, ask follow-up questions.
6. **Finish the review** (`⇧⌘E` / `Shift+Ctrl+E`): choose Approve, Request changes or Comment and
   publish it to GitHub.

## Features

### Dashboard: your to-dos

The Dashboard answers one question: what should I do now?

- **Your turn** — the pull requests waiting for you, the most urgent first.
- **Next steps** — up to five things to do, each with one button: resume an open review, review a
  new revision after new commits, retry a failed analysis, start an analysis, or publish a finished
  review.
- **This week** — reviews waiting for you, active reviews, analyses run and reviews published.
- When nothing waits, it says so: _You're all caught up._

### Inbox: reviews you owe and your own pull requests

The Inbox reads GitHub and sorts by whose turn it is. Its count in the sidebar shows only the pull
requests where you have to act.

**To review**

- Every pull request where GitHub currently requests your review, also through one of your teams.
- Oldest request first. With a **review deadline** (Settings › Preferences, 1–20 working days),
  rows show _Due Wed_, _Due today_ or _Overdue · 2 days_, and overdue ones move to the top.
  Working days are Monday to Friday in your time zone.
- When you submit a review, the pull request moves to **Waiting on others** until someone requests
  your review again. New commits after your approval show as _Changed since your approval_.

**My pull requests**

- **Changes requested** — a reviewer asked for changes and you have not re-requested their review.
- **Ready to merge** — approved, with no open requests.
- **No reviewer** — nobody was asked to review it yet.
- Everything else waits on reviewers, with their names. Drafts and bot reviews never count.

### Walkthrough

CodeBadger arranges the change into connected steps in a sensible reading order, not in file
order. Each step has a short explanation, the matching code in a diff viewer beside it, and —
where it helps — a diagram, pseudocode or a small tree. Each step also says how carefully to read
it (read carefully, read normally, skim) and why. Mark steps as read and files as viewed; your
progress is saved.

### Findings

- Each finding cites the code. CodeBadger checks the quoted excerpt against the exact Git revision
  and fixes line numbers only when the match is unique.
- Every finding has a severity from one fixed scale: **blocker** (harm on a path that is used:
  wrong results, data loss, any security defect, breaking an external contract), **warning** (a
  real defect with limited reach), **suggestion** (a defect with negligible effect), **question**
  (asks the author whether a change is meant that way) and **nit** (a typo or misleading comment).
  A defect that fits a blocker is a blocker, even when it only happens in an edge case.
- Filter by severity and status. Nits and defects that already existed before the pull request
  start hidden.
- Each finding lists other places with the same pattern, so the whole class gets fixed at once.
- **Optional independent check:** three verifiers in separate sessions — one of them with the other
  provider when available — look for counter-evidence and vote _supported_, _contradicted_ or
  _unverified_. The check is advisory and never changes a finding.
- When you dismiss a finding, you say why: incorrect, intentional, or not important.
- **Fix list:** copy all open findings as a Markdown task list, sorted by severity, to hand to the
  author, Codex or Claude Code.

### Large pull requests in chapters

Big changes are reviewed completely, not sampled.

- Above **50 files, 5,000 diff lines or 5 MiB**, CodeBadger cuts the change into named chapters.
- Each chapter runs as a full review, **four at a time**. A closing session merges them into one
  walkthrough and one set of findings, without duplicates.
- Pick one chapter or see all of them. Findings can be grouped by chapter.
- If the app quits, the computer sleeps or a chapter fails, **Continue** runs only the missing
  chapters. The Mac stays awake while a review runs.
- Generated files, lockfiles, large data files and your own patterns (Settings › Review filter)
  stay out of the analysis and do not count toward the size. They stay in the file list, marked
  _not checked_.
- Limits: more than 30 chapters, a single file above 50,000 diff lines, or more than 30 MiB of diff
  stop before any provider starts.

### Acceptance criteria from Linear

Add a Linear personal API key in **Settings › Linear**. Every analysis then reads the Linear
tickets the pull request names and marks each acceptance criterion as **met**, **partly met** or
**not met**, with the code as evidence. Criteria the ticket does not list explicitly are marked as
_derived from the ticket text_.

When you request changes or comment, the unmet and partly met criteria are added to your GitHub
review as their own section. You can leave out single criteria.

### Tests and CI, without tautological tests

The **Tests & CI** tab keeps three kinds of evidence apart:

- **Test mapping** — which changed code has a related test. _No test mapping found_ never claims
  the code is untested.
- **Test execution** — CI checks that ran on exactly the analyzed commit.
- **Stated claims** — the test plan in the pull request description. Claims are never counted as
  proof.

**Tautological tests** are flagged: tests that only mirror the implementation, such as asserting
what a mock returns or which internal calls happen in which order. They are marked _Tautological_
with a reason and **do not count as proof**. A new tautological test gets its own finding.
Interactions at real boundaries — network, database, file system, IPC, clock — still count.

A fixed rule, not the model, rates the proof as strong, adequate, weak or missing.

### Approve recommendation

Every review shows one of four recommendations: **Ready to approve**, **Approve with comments**,
**Changes needed** or **Can't assess**. A fixed rule decides from open findings, acceptance
criteria, proof, and rollback safety, and names up to three reasons. Your GitHub review draft starts
with the matching decision: Approve, Request changes, or Comment when open questions to the author
are the only reason. You still decide.

The **?** beside the recommendation shows its rules in short. **How CodeBadger decides** — in the
sidebar, the **Help** menu and the command palette — explains them in full: which findings count,
what each severity means, and how acceptance criteria, tests and rollback safety come in.

### Impact, risk and rollback

- A **risk level** (low, medium, high) from the change itself — sensitive paths like auth,
  migrations, public APIs, CI, or widely imported files — computed without the model.
- **Blast radius** per file: how many other files import it.
- A **Safety and rollback** card: what must not change, how bad a failure would be, whether the
  change can be reverted or is behind a flag. Destructive migrations and data deletion are
  pointed out.

### Follow-up chat

Ask about a step, a finding or a selected piece of code. Each review has its own chat window that
you can place anywhere. Answers link back into the code, and the chat is saved. When you ask it to,
the chat can change a finding's status.

### New commits and follow-up reviews

**Check for updates** looks for new commits without starting an analysis. A changed pull request
becomes a new revision; earlier revisions, analyses and chats stay available.

A **follow-up analysis** reviews only what changed since your last review and labels findings as
_new_, _still open_ or _not detected_. _Not detected_ does not mean _fixed_ — you decide. Steps you
already read and marks you set carry over where they still match.

### Publish a GitHub review

**Finish review** turns your findings into a native GitHub review:

- Edit the findings in the draft, add your own, or leave some out.
- Findings on changed lines become line comments, others go into file or body comments.
- Check the Markdown preview, then choose **Approve**, **Request changes** or **Comment**.
- Before publishing, CodeBadger checks that the pull request has not changed. Nothing is published
  without your confirmation, and a review is never sent twice.

### Every file type

- **Markdown** files open rendered, including the images they reference.
- **Images** show before and after side by side, with a pixel diff.
- **Binary** files show a short notice instead of broken text.

## Keyboard shortcuts

macOS shown; on Windows use `Ctrl` for `⌘`.

| Shortcut              | Action                                                     |
| --------------------- | ---------------------------------------------------------- |
| `⌘K`                  | Command palette                                            |
| `⌘N`                  | New review                                                 |
| `⇧⌘V`                 | Import pull request from clipboard                         |
| `⇧⌘H` / `⇧⌘I` / `⇧⌘R` | Dashboard / Inbox / Reviews                                |
| `⌘1` … `⌘6`           | Overview, Walkthrough, Files, Findings, Impact, Tests & CI |
| `J` / `K`             | Next / previous step                                       |
| `R`                   | Mark step read and continue                                |
| `N` / `P`             | Next / previous open finding                               |
| `⌥←` / `⌥→`           | Previous / next evidence                                   |
| `⌘Enter`              | Send chat question                                         |
| `⇧⌘E`                 | Finish review                                              |
| `⌘,`                  | Settings                                                   |
| `?`                   | All keyboard shortcuts                                     |

## Settings

- **Environment** — which tools CodeBadger found, storage use and cleanup.
- **Preferences** — review deadline in working days, notifications when an analysis finishes.
- **Review filter** — your own path patterns to leave out of every analysis.
- **Linear** — personal API key for the acceptance criteria check.
- **Appearance** — light, dark or follow the system.
- **Your review flow** — local statistics on how you decided the findings.

## Privacy and security

- **Local-first.** Code snapshots, analyses, chats and review drafts are stored on your computer.
  CodeBadger has no server of its own.
- **Your provider reads the code.** For an analysis, Codex or Claude Code receives repository
  content under its own terms and your account settings. Local-first does not mean offline.
- **Limited agent access.** Codex runs in a read-only sandbox. Claude Code has its own file, shell
  and web tools switched off and reads only checked snapshot data through a restricted broker.
- **Pinned revisions.** Every explanation and finding refers to an exact Git revision, so older
  reviews stay traceable.
- **Nothing is published by itself.** GitHub reviews go out only after your confirmation.
- Crash reports are sent to Sentry so we can fix errors.

## Updates

CodeBadger checks this repository for new releases, downloads them in the background and installs
them when you quit. You can also restart right away when an update is ready; CodeBadger asks first
if work would be lost. Release notes are on the
[releases page](https://github.com/widdydev/code-badger-releases/releases).

## FAQ

<details>
<summary><strong>What does it cost?</strong></summary>

Importing and local preparation start no AI provider. Analyses,
follow-up questions and the optional independent check use the provider you chose and may cost
money or quota under your plan. You pick the model and effort before every analysis. Large
pull requests in chapters take longer and cost more.

</details>

<details>
<summary><strong>Can I review private repositories?</strong></summary>

Yes. You can import any pull request on github.com that your signed-in GitHub CLI account can
access — open, merged or closed.

</details>

<details>
<summary><strong>Does CodeBadger replace my own review?</strong></summary>

No. The AI helps you understand the change and find possible problems. You check the evidence and
make the decision. The recommendation and the independent check are hints, never automatic
actions.

</details>

<details>
<summary><strong>When does CodeBadger recommend changes?</strong></summary>

The recommendation is computed from the analysis, not by another AI call. The first level that
matches decides:

- **Can't assess:** the analysis did not finish, or chapters of a large pull request are missing.
- **Changes needed:** an open blocker, an acceptance criterion not met, or code that does not match
  the description.
- **Approve with comments:** an open warning or question to the author, a criterion or the
  description only partly met, no tests for the changed behavior, or an irreversible change without
  a feature flag.
- **Ready to approve:** none of the above.

A finding counts while it is open, unless it existed before the pull request or the verifiers
contradicted it. Suggestions, nits, the risk level and CI never count. CodeBadger only preselects
the decision in your GitHub review; nothing is sent until you publish. All rules are in **How
CodeBadger decides** in the app.

</details>

<details>
<summary><strong>Why does a test count as tautological?</strong></summary>

Because it would pass for any implementation that makes the same calls, or because it only checks
what it set up itself. Such a test breaks when the code is refactored, but not when the behavior is
wrong. CodeBadger names the reason next to each flagged test.

</details>

<details>
<summary><strong>Which languages does CodeBadger speak?</strong></summary>

English. The app, walkthroughs, findings, chat answers and published GitHub reviews are all in
English. The code under review can be in any programming language.

</details>

<details>
<summary><strong>Windows says the app is not safe.</strong></summary>

The Windows build is a preview and not code-signed yet. SmartScreen therefore warns on first
start. Choose **More info › Run anyway**.

</details>
