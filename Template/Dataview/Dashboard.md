---
type: dashboard
update: <% tp.date.now("YYYY-MM-DD") %>
---

# 🧠 Knowledge Dashboard

Your central overview for ideas, learning, projects, and daily activity.

---

# 📥 Inbox (Notes to Process)

Notes you captured but haven't processed into Zettels yet.

```dataview
LIST
FROM "01_Inbox"
SORT file.cday DESC
```

---

# 📚 Learning Topics

Topics you are currently studying and how connected they are.

```dataview
TABLE
length(file.inlinks) AS "Connections",
file.cday AS "Created"
FROM "Learning"
SORT length(file.inlinks) DESC
```

---

# 🧠 Latest Zettels

Newest permanent knowledge notes.

```dataview
TABLE
file.cday AS "Created",
file.tags AS "Tags"
FROM "02_Zettel"
SORT file.cday DESC
LIMIT 10
```

---

# 🔗 Most Connected Ideas

Highly linked ideas in your knowledge graph.

```dataview
TABLE
length(file.inlinks) AS "Connections"
FROM "02_Zettel"
SORT length(file.inlinks) DESC
LIMIT 10
```

---

# 🌱 Orphan Notes (Needs Linking)

Notes that are not connected to other notes yet.

```dataview
LIST
FROM "02_Zettel"
WHERE length(file.inlinks) = 0
```

---

# 🚧 Active Projects

Projects you are currently working on.

```dataview
TABLE
file.mtime AS "Last Edited"
FROM "03_Projects"
SORT file.mtime DESC
```

---

# 📅 Recent Daily Notes

Latest notes from your Persian daily notes folder.

```dataview
TABLE
file.cday AS "Created"
FROM "persian_dates/days"
SORT file.name DESC
LIMIT 7
```

---

# 📝 Recently Updated Notes

Most recently edited notes in the entire vault.

```dataview
TABLE
file.mtime AS "Last Edited"
FROM ""
SORT file.mtime DESC
LIMIT 10
```

---

# 🏷 Tag Overview

Shows which tags appear most often.

```dataview
TABLE
length(rows) AS "Count"
FROM ""
FLATTEN file.tags AS tag
GROUP BY tag
SORT length(rows) DESC
LIMIT 15
```

---
````
