[[ساختار استفاده از گیت در این پروژه ]]
```bash
git status
git branch --show-current
git branch -a
git remote -v
git remote show origin
git remote
git log --oneline --graph --decorate -n 20
git branch -vv
git config --list --show-origin
git config --list | findstr branch
git config --list | findstr remote
git config --list | grep branch
git config --list | grep remote

```
---
tags:
```bash 
git show-ref --tags
git tag -n
```
---
See branches merged into main
Run:

bash
git branch --merged main
This shows local branches already merged into main.
See branches not yet merged

git branch --no-merged main

---
(.venv) sara@MacBook-Pro evck160c- decentralized % git remote -v

github  git@github-work:farhad3113teslapower/evck160c.git (fetch)
github  git@github-work:farhad3113teslapower/evck160c.git (push)
origin  /Volumes/public/Sara/Local_git_ocpp/evck160c-decentralized.git (fetch)
origin  /Volumes/public/Sara/Local_git_ocpp/evck160c-decentralized.git (push)
(.venv) sara@MacBook-Pro evck160c- decentralized % 
---

---
[[بهترین روش گیت درون سازمانی]]
---

# Important concept: “upstream” can mean two different things

This causes lots of confusion.

## 1) Remote
A **remote** is a named repository location, such as:

- `origin` → GitHub
- `localserver` → your Windows server Git repo

Example:
```bash
git remote add origin https://github.com/user/repo.git
git remote add localserver ssh://server/path/repo.git
```

## 2) Upstream branch
An **upstream branch** means the branch your current branch tracks.

Example:
- local branch `main`
- tracks `origin/main`

That means:
```bash
git pull
git push
```
know where to go by default.

So when you say:
> I set git's upstream to my local server instead of github

That could mean either:

- you changed the **remote URL**
- or your branch is **tracking the local server branch**
- or both

The commands above will reveal which one happened.

---

# Recommended clean setup for a large project

In most professional setups, I recommend this structure:

## Remotes
- `origin` = your main canonical remote, usually **GitHub**
- `local` or `backup` = your local Windows server remote

Example:
- `origin` → GitHub
- `backup` → local server

This is easier than making local server the main `origin`.

---

# Recommended branch strategy

If you want something simple and robust:

## Option A: Simple and clean
- `main` = production/stable
- `develop` = integration branch
- feature branches:
  - `feature/login-fix`
  - `feature/report-export`

Use this if:
- you work alone or with a small team
- you want order without too much complexity

## Option B: Very simple solo workflow
- `main`
- short-lived feature branches

Use this if:
- you are mostly solo
- you want minimum overhead

For a “massive” project, I usually recommend **Option A**.

---

# Recommended versioning system

Use **Semantic Versioning**:

```text
MAJOR.MINOR.PATCH
```

Example:
- `1.0.0`
- `1.1.0`
- `1.1.1`
- `2.0.0`

## Meaning
- **MAJOR**: breaking changes
- **MINOR**: backward-compatible new features
- **PATCH**: backward-compatible bug fixes

## In Git
Use **tags** for releases:
```bash
git tag -a v1.0.0 -m "Release v1.0.0"
git push origin v1.0.0
```

You can also push tags to your local backup remote.

---

# Recommended commit message style

Use a clear format, for example:

```text
feat: add report export
fix: resolve login timeout issue
refactor: simplify billing service
docs: update deployment instructions
chore: update dependencies
```

This will help a lot later in a large repo.

---

# Likely future setup we may apply

Once I see your current state, I may guide you to something like this:

## If GitHub should be the main remote
```bash
git remote rename origin localserver
git remote add origin https://github.com/yourname/yourrepo.git
```

Then set branch tracking:
```bash
git branch --set-upstream-to=origin/main main
```

And keep local server as backup:
```bash
git push localserver main
git push origin main
```

Or push all branches to both when needed.

---

# If you want both remotes permanently

That is completely possible.

Example:
```bash
git remote -v
```

might become:

```text
origin      https://github.com/yourname/project.git (fetch)
origin      https://github.com/yourname/project.git (push)
localserver ssh://yourserver/path/project.git (fetch)
localserver ssh://yourserver/path/project.git (push)
```

Then you can do:

```bash
git push origin main
git push localserver main
```

If you want, we can even automate “push to both” later.

---

# Extra safety step before any remote changes

Because this is a massive project, before changing remote configuration I also recommend:

## Check if you have uncommitted work
```bash
git status
```

## Optional backup branch
```bash
git branch backup-before-remote-fix
```

## Optional full bundle backup
This is very safe:
```bash
git bundle create full-backup.bundle --all
```

That creates a portable backup of the whole repo history.

I strongly recommend this before major cleanup.

---
چطوری بین دو نسخه فایل جابجا بشم:
git log -- /path/to/file
find the commit Hash
git restore --source=<commit hash> -- opt/evck/common/nfc_reader.py
اگر می‌خوای تفاوتش با نسخه‌ی الان رو ببینی:
git diff -- opt/evck/common/nfc_reader.py
برگشتن به نسخه فعلی:
git restore --source=HEAD -- opt/evck/common/nfc_reader.py
