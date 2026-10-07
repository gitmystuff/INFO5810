# Setting Up Git

Your **GitHub account** is where your code lives online. **Git** is the tool installed on your own computer that talks to GitHub. You need both — having a GitHub account doesn't mean Git is installed locally.

## 1. Install Git

**Check if you already have it:**

```bash
git --version
```

If you see a version number, skip to Step 2.

**If not installed:**

- **Windows:** download and run the installer from **https://git-scm.com/download/win**. Accept the defaults during install — the default options (including "Git Bash" and using VS Code as the default editor if you select it) are fine for this course.
- **Mac:** run `git --version` in Terminal — if Git isn't installed, macOS will prompt you to install the Xcode Command Line Tools, which include it. Accept that prompt.
- **Linux:** `sudo apt install git` (Debian/Ubuntu) or `sudo dnf install git` (Fedora).

## 2. Tell Git Who You Are

Git stamps every commit with a name and email. Set this once, globally:

```bash
git config --global user.name "Your Name"
git config --global user.email "your_email@example.com"
```

Use the **same email as your GitHub account** so your commits get linked to your profile.

**Verify:**

```bash
git config --global --list
```

## 3. Authenticate with GitHub

When you push code, GitHub needs to confirm it's really you. As of now, GitHub requires either a **Personal Access Token (PAT)** or the **GitHub CLI** — plain passwords no longer work over HTTPS.

**Easiest path — GitHub CLI:**

1. Install it: **https://cli.github.com** (or `winget install GitHub.cli` / `brew install gh` / `sudo apt install gh`)
2. Authenticate:
   ```bash
   gh auth login
   ```
3. Follow the prompts: choose **GitHub.com**, **HTTPS**, and **Login with a web browser**. It'll give you a one-time code and open your browser to confirm.

Once done, Git operations (`clone`, `push`, `pull`) against your own repos will just work — no more username/password prompts.

## 4. Clone the RAG Lab Repo

Navigate to wherever you keep your projects, then:

```bash
git clone https://github.com/gitmystuff/deep-learning-rag-agent.git
cd deep-learning-rag-agent
```

**Verify it worked:**

```bash
git status
```

You should see `On branch main, nothing to commit, working tree clean`.

## 5. The Core Workflow You'll Actually Use

You don't need to master Git — you need these four commands, in this order, used repeatedly:

```bash
git status          # what changed?
git add .            # stage everything that changed
git commit -m "message describing what you did"
git push             # send it to GitHub
```

Run `git status` liberally — it's harmless and tells you exactly what state you're in.

## 6. Publishing Your Own Copy (for the RAG lab)

The RAG lab asks each team member to publish their finished work to their **own** GitHub account. From VS Code:

1. Open the **Source Control** panel in the sidebar (`Ctrl/Cmd+Shift+G`)
2. Click **Publish to GitHub**
3. Choose **Public**
4. Give it a name — this becomes part of your portfolio, so name it something real

## Troubleshooting

| Problem | Fix |
| :--- | :--- |
| `git: command not found` | Git isn't installed — revisit Step 1 |
| Push asks for a password and rejects it | GitHub no longer accepts plain passwords — run `gh auth login` (Step 3) |
| `fatal: not a git repository` | You're not inside a cloned/initialized project folder — `cd` into it first |
| Wrong name/email on commits | Re-run the `git config --global` commands in Step 2, then make a new commit |
