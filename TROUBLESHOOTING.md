# Troubleshooting

## Git and GitHub

**"git: command not found"**
: Git isn't installed, or the terminal was open before you installed it.
  Install Git, then close and reopen the terminal (or VS Code).

**"Repository not found"**
: Check the URL. Your fork is `https://github.com/<you>/all26`, and the
  team repo is `https://github.com/Team100/all26`.

**"Permission denied" / "Authentication failed" when pushing**
: You're probably pushing to `Team100/all26` instead of your fork. Run
  `git remote -v`, or in GitHub Desktop check that the repository is
  `<you>/all26`. Also make sure GitHub Desktop is signed in as **you**,
  not the last person who used the laptop.

**"Permission denied" writing files**
: You don't have permission for that folder. Clone into a folder in your
  user directory or on the desktop.

**The PR has lots of changes I didn't make**
: The base branch is wrong, or your fork is out of date. See
  [Fork workflow](FORK_WORKFLOW.md#common-problems).

## Building

**Build takes forever the first time**
: Normal. Gradle downloads a lot the first time. Wait a few minutes.

**Red squiggles everywhere, but the build works**
: VS Code's Java helper is confused. `Ctrl+Shift+P` → **Java: Clean Java
  Language Server Workspace**, and restart.

**Can't find `org.team100.lib...`**
: Your study doesn't include `lib` yet. See [Study](STUDY.md#include-the-lib-directory).

## Deploying and running

See [Deploy](DEPLOY.md#if-the-deploy-fails),
[SSH and checks](SSH_AND_CHECKS.md#common-problems), and
[Debugging](DEBUGGING.md).

Found a fix that isn't here? Add it!
