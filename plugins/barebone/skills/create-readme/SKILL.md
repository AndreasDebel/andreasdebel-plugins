---
name: create-readme
description: Write (or update) a README.md for one folder or project that tells a student exactly how to run that part of the program and check by hand that it works — copy-paste CLI commands with the output to expect, every one actually run before it's written down. Implements barebone principle 8. Use this whenever the user types /create-readme, or asks to add/write/update a README for a folder, project, service or repo, asks "how do I run this", wants run instructions or manual test steps documented, or wants to be able to start and try out a part of their program from the command line.
user-invocable: true
license: MIT
---

# /create-readme — How to run this part, and how to see that it works

Write a `README.md` for one runnable part of a program: what to type to start it, and what
to type to check by hand that it does its job. This is barebone principle 8 — read it in
`${CLAUDE_PLUGIN_ROOT}/principles/BAREBONE-PRINCIPLES.md` — turned into a procedure.

The reader is a student who just cloned the repo and wants to see the thing work before
reading its code. Everything in the README serves that; anything that doesn't is cut.

## Step 1 — Pick the folder, and check it's runnable

Target is the folder the user named, else the current working directory. Then decide
whether it's a **runnable part**: something a student can start on its own — it has an
entry point, a project/build file, a `package.json` script, a `main`, a server to launch.

- **Runnable:** carry on.
- **Not runnable** (plain source files nothing starts directly): say so and stop, naming
  the nearest folder that *is* runnable. A README there would have nothing true to say.
- **Contains several runnable parts** (e.g. a repo root with an API and a client): ask
  whether to write one README per part, a root README that runs them together, or both.
  A root README is worth it when the parts only make sense running together — then it says
  the order to start them in and points to each part's own README for the details.

## Step 2 — Find the real commands

Read, don't guess: build/project files, `package.json` scripts, launch configs
(`launchSettings.json`, `.vscode/launch.json`), Dockerfiles/compose files, the entry point,
and any existing README or `CLAUDE.md`. From them, establish:

- **Prerequisites** — the runtime/SDK and version the project file actually targets, and
  anything else that must already be running (a database, another service in this repo).
- **Run command** — from which directory, and what the terminal shows when it's up
  (the port, the "listening on" line, the prompt).
- **Manual checks** — one command per thing this part does, exercising the real scenario:
  `curl` against the actual routes, the CLI invoked with a real argument, a request to the
  page. Take routes, ports and arguments from the code, not from convention.

Use bash syntax (`curl`, `/dev/null`, `\` line breaks). If the user is on Windows, note
once near the top that commands are for Git Bash, unless the project is clearly
PowerShell-only.

## Step 3 — Run every command before writing it down

Start the part, run each check, and copy the **actual** output into the README (trim long
output to the lines that show it worked). A README with a command that doesn't work is
worse than none: the student assumes they broke something.

If something can't be run here — it needs credentials, a service you can't start, hardware
— keep the command, mark it `(not verified)` in the README, and tell the user. Stop any
process you started when you're done.

If a command fails because of a bug, don't fix the code from this skill. Report it and ask.

## Step 4 — Write it

Use this shape. Every section is short; omit one only if it would be empty.

````markdown
# <Folder/part name>

<One or two sentences: what this part does, and what it talks to.>

## Prerequisites

- <runtime + version>
- <anything that must already be running, with a link to its README>

## Run

```bash
cd <path from repo root>
<run command>
```

You should see:

```
<the line(s) that show it started>
```

## Check that it works

1. <What this checks, in a few words>

   ```bash
   <command>
   ```

   Expected:

   ```
   <actual output>
   ```

2. ...

## Stop

<Ctrl+C, or the command to stop it.>
````

Rules:

- **Only what helps run and check it.** No architecture overview, badges, contribution
  guide, or licence section — the code teaches the architecture; this file gets it running.
- **Order the checks like the scenario.** The first check is the simplest proof it's alive;
  the last is the thing the exercise is actually about.
- **If a README already exists**, keep the user's own sections byte for byte and replace
  or add only the run/check sections. Say what you changed.

## Step 5 — Report

In two or three lines: the path written, which commands were verified and which weren't,
and any bug Step 3 turned up.
