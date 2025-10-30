# ERD Repository (bigER + VS Code)

This repository is for students to submit Entity‑Relationship Diagrams (ERDs) using the bigER VS Code extension.

## Prerequisites
- VS Code installed
- Git installed and a GitHub account
- Java 11+ runtime (JDK recommended)
- VS Code extension: BIGER Modeling Tool (`BIGModelingTools.erdiagram`)

### Install Java quickly
- macOS (Homebrew): `brew install --cask temurin17`
- Windows (winget): `winget install --id EclipseAdoptium.Temurin.17.JDK -e`
- Verify: `java -version` (shows 11+)

## Workflow (Fork → Clone → Model → PR)
1. Fork this repo: open https://github.com/BADM554/ERD and click “Fork”.
2. Clone your fork in VS Code:
   - Command Palette → “Git: Clone” → paste your fork URL `https://github.com/<your-username>/ERD.git` → Open folder.
3. Create your ERD file:
   - Ensure the bigER extension is installed and Java works (`java -version`).
   - Add a new file at the repo root, e.g., `Firstname_Lastname.erd`.
   - Command Palette → “New Sample ER Model” to start, or write your own.
   - To view the diagram: use the “Open Diagram” button in the editor toolbar or press Ctrl/⌘+O.
4. Commit and push to your fork:
   - Source Control view → stage the `.erd` file → commit with a clear message → push/sync.
5. Open a Pull Request to the course repo:
   - VS Code: “Create Pull Request”, or on GitHub: “Compare & pull request”.
   - Base: `BADM554/ERD` (main). Head: your fork/branch.

## Git one‑time setup
Run once in a VS Code terminal:
```
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

## Updating your fork (optional)
If the course repo updates, sync before new work:
```
git remote add upstream https://github.com/BADM554/ERD.git
git fetch upstream
git checkout main
# choose merge or rebase
git merge upstream/main   # or: git rebase upstream/main
```

## Troubleshooting
- bigER inactive or no diagram: ensure file ends with `.erd` and the extension is enabled.
- Java not found: install JDK 11+ (see above) and restart VS Code.
- Cannot push: confirm you cloned your fork (URL has your username), not the course repo.

## Notes
- Place your `.erd` file(s) at the repository root unless told otherwise.
- Keep commits small and messages descriptive.
