`tree` is great for quickly understanding a project layout.

## Basic use

```bash
tree
```

Shows the current directory recursively as a tree.

Example:

```text
.
├── opt
│   └── evck
│       ├── common
│       ├── gui_app
│       └── ocpp_client
└── README.md
```

---

## Limit depth with `-L`

What you used:

```bash
tree -L 3
```

means:

- show directory tree
- only go **3 levels deep**

This is very useful because full trees can get huge.

Example:

```bash
tree -L 2
tree -L 3
tree -L 4
```

---

# Very useful `tree` commands

## 1. Show hidden files too

```bash
tree -a
```

This includes things like:

- `.venv`
- `.gitignore`
- `.env`

Combine with depth:

```bash
tree -a -L 2
```

---

## 2. Show only directories

```bash
tree -d
```

Very useful when you only want structure, not every file.

Example:

```bash
tree -d -L 3
```

Good for Python projects.

---

## 3. Show full path prefix

```bash
tree -f
```

This prints full relative paths.

Example:

```text
.
└── ./opt
    └── ./opt/evck
```

Useful when copying paths.

---

## 4. Ignore folders

Very important.

Example: ignore virtual env and git files:

```bash
tree -I ".venv|.git|__pycache__"
```

With depth:

```bash
tree -L 3 -I ".venv|.git|__pycache__"
```

`-I` means pattern ignore.

You can ignore many things with `|`.

Example:

```bash
tree -I ".venv|.git|node_modules|__pycache__"
```

---

## 5. Show only matching files with `-P`

Example: only Python files

```bash
tree -P "*.py"
```

With depth:

```bash
tree -L 3 -P "*.py"
```

If you want both matching and hidden/dirs, combine carefully.

---

## 6. Show file sizes

```bash
tree -h
```

Human-readable sizes.

Example:

```bash
tree -h -L 2
```

---

## 7. Show directory sizes

```bash
tree --du -h
```

Useful to see where disk space is going.

---

## 8. Save output to a file

```bash
tree -L 3 > tree.txt
```

Then inspect:

```bash
cat tree.txt
```

or open it in editor.

---

## 9. Run `tree` on a specific folder

```bash
tree opt/evck -L 2
```

Instead of current directory.

Very useful if repo root is noisy.

---

# Best commands for Python projects

These are the ones I use most:

## Project structure only

```bash
tree -d -L 3 -I ".venv|.git|__pycache__"
```

## Python files only

```bash
tree -L 3 -P "*.py" -I ".venv|.git|__pycache__"
```

## Show package layout

```bash
tree opt/evck -L 3 -I ".venv|.git|__pycache__"
```

## See where `requirements` files are

`tree` can help a little, but `find` is often better.

---

# Other commands you should know besides `tree`

## 1. `find`
Very powerful for locating files.

### Find all requirements files

```bash
find . -iname "requirements*.txt"
```

### Find all Python files

```bash
find . -iname "*.py"
```

### Find all `__init__.py`

```bash
find . -name "__init__.py"
```

This is especially useful for checking Python packages.

---

## 2. `ls`
Good for quick local inspection.

### Long format

```bash
ls -l
```

### Show hidden files

```bash
ls -la
```

### Show a specific folder

```bash
ls -la opt/evck
```

---

## 3. `pwd`
Print current directory:

```bash
pwd
```

Very important when debugging paths.

---

## 4. `du`
Disk usage.

### Size of current directory contents

```bash
du -sh *
```

### Size of hidden venv

```bash
du -sh .venv
```

---

## 5. `grep`
Search text in files.

### Find imports of `OPi.GPIO`

```bash
grep -R "OPi.GPIO" .
```

### Find where `nfc_reader` is imported

```bash
grep -R "nfc_reader" .
```

---

## 6. `which`
Find which executable is being used.

```bash
which python
which pip
```

Super useful for venv debugging.

---

## 7. `realpath` or `python` for path resolution
On some systems:

```bash
realpath .venv/bin/python
```

If unavailable, use Python:

```bash
python -c "from pathlib import Path; print(Path('.venv').resolve())"
```

---

# Best command combos for your current kind of work

## See project structure without noise

```bash
tree -d -L 4 -I ".venv|.git|__pycache__"
```

## See all Python package folders

```bash
find . -name "__init__.py"
```

## See all requirement files

```bash
find . -iname "*requirements*"
```

## See where a module is imported

```bash
grep -R "from common" opt/evck
grep -R "import OPi.GPIO" opt/evck
```

---

# A few practical examples

## Example 1: inspect repo structure
```bash
tree -L 3 -I ".venv|.git|__pycache__"
```

## Example 2: inspect only `opt/evck`
```bash
tree opt/evck -L 3 -I ".venv|.git|__pycache__"
```

## Example 3: inspect only directories
```bash
tree opt/evck -d -L 3
```

## Example 4: list only Python files
```bash
tree opt/evck -P "*.py" -L 3
```

---

# One important note

If `tree` is not installed, install it:

### macOS with Homebrew
```bash
brew install tree
```

### Debian/Ubuntu
```bash
sudo apt install tree
```

---

# Commands worth memorizing

For your workflow, I’d memorize these:

```bash
tree -L 3
tree -d -L 3
tree -a -L 2
tree -I ".venv|.git|__pycache__"
find . -name "__init__.py"
find . -iname "requirements*.txt"
grep -R "OPi.GPIO" .
which python
which pip
pwd
ls -la
```

---

If you want, I can next give you a **mini command-line cheat sheet for Python developers** with `tree`, `find`, `grep`, `xargs`, `du`, and `pip` together.