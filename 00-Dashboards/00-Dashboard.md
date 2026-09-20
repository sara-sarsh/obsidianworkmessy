---
type: dashboard
update: 2026-03-05
---
# **📌 Task Dashboard**

> Centralized task view for the entire vault.  
> Auto‑updates using Dataview.  
> Tag your actionable lines with **#todo** or add `- [ ]` tasks anywhere.

---

# **1. 🔍 All TODO‑Tagged Tasks (Unfinished)**

```dataview
task
from ""
where !completed and contains(tags, "todo")
sort file.mtime desc
```

---

# **2. 📁 Tasks Grouped by File**

```dataview
task
from ""
where contains(tags, "todo") and !completed
group by file.link
```

---

# **3. 🗂 Tasks from a Specific Folder (Docs/)**

```dataview
task
from "Docs"
where !completed and contains(tags, "todo")
sort file.mtime desc
```

---

# **4. ⭐ Priority Tasks**

Use one of these formats inside tasks:

```
- [ ] Rewrite authentication section  #todo #priority/high
- [ ] Add performance benchmarks  #todo #priority/med
- [ ] Add performance benchmarks  #todo #priority/low
```

Then the dashboard:

```dataview
task
from ""
where !completed and contains(tags, "todo")
sort (contains(tags, "priority/high") ? 0 :
      contains(tags, "priority/med") ? 1 :
	  contains(tags, "priority/low") ? 2 :3),  
     file.mtime desc
```

Priority order: high → medium → the rest.

---

# **5. 🕒 Recently Added/Edited Tasks**

```dataview
task
from ""
where !completed and contains(tags, "todo")
sort created desc
limit 20
```

---

# **6. 🧹 Orphaned TODO tags (not tasks)**  
> Useful if sometimes you write `#todo` but forget to make it a task.

```dataview
list
from ""
where contains(tags, "todo") and !any(file.tasks.where(t => contains(t.tags, "todo")))
```

---

# **7. 📝 Completed Tasks (Last 7 Days)**

```dataview
task
from ""
where completed and completion >= date(today) - dur(7 days)
sort completion desc
```

---

# **8. ⚡ Quick Refresh**

Use command palette:

`Dataview: Force refresh`

Add a hotkey (recommended).

---

# **9. (Optional) Tasks Plugin Queries**
If you use the **Tasks** community plugin, here are ready‑to‑run sections.

### 🔖 Tasks Plugin: All #todo

```tasks
tag includes todo
not done
sort by filename
```

### 🔖 Priority (Tasks plugin)

```tasks
tag includes priority/high
not done
```

---

# **10. How to Tag Tasks in Your Notes**

Use either style:

### Checkbox + tag:
```
- [ ] Write unit test for API  #todo
```

### Line tagged but no checkbox:
```
- refactor task scheduling engine #todo
```

Both are collected.

---

If you want, I can also generate:

- a slimmer minimalist dashboard  
- a super‑advanced dashboard (counts + progress bars + heatmaps)  
- an implementation that works **without** Dataview or Tasks (core plugins only)