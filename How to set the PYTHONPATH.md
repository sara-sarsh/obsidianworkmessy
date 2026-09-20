The error:

```
export: PYTHONPATH: inconsistent type for assignment
```

happens in **zsh** (the default shell on macOS) when `PYTHONPATH` was previously defined as a **different parameter type** (often an array or special parameter) and you're trying to assign a **string**.

Your `echo PYTHONPATH` command also missed the `$`, so it just printed the literal text instead of the variable value.

### First check the current type
Run:

```bash
typeset -p PYTHONPATH
```

If it's an array you'll see something like:

```
typeset -a PYTHONPATH
```

### Fix 1 (most common)
Unset it and set it again:

```bash
unset PYTHONPATH
export PYTHONPATH="$(pwd)/opt/evck"
```

### Fix 2 (zsh array style, sometimes cleaner)
In zsh, `PYTHONPATH` can be tied to the array `path` style variables:

```bash
PYTHONPATH="$(pwd)/opt/evck"
export PYTHONPATH
```

### Verify it worked

```bash
echo $PYTHONPATH
```

You should see something like:

```
/Users/sara/.../evck160c-decentralized/opt/evck
```

### Small tip
Your command should be **one line**, not two separate ones unless you intend that:

```bash
export PYTHONPATH="$(pwd)/opt/evck"
```

---

If you want, I can also show a **more robust way to append to PYTHONPATH** (so you don’t overwrite existing entries), which is usually safer for Python projects.