---
title: git clone procedure
UID: <% tp.date.now("YYYYMMDDHHmmss") %>
creation-date: <% tp.date.now("YYYY-MM-DD") %>
creation-date-jalali:
---

## the subject discription:
I needed to git clone the project to match the new project stlye.
it was facing some errors, like the file name mismatch with the NTFS style and rules, so here is the approach that I am taking for solving the problem,
- first I try to fix the repo from the github
-  rmdir /s /q evck160c
- git clone https://github.com/farhad3113teslapower/evck160c.git

- then if the problem presists:
-
- clone the repository structure without checking out the files:
   - git clone -n https://github.com/farhad3113teslapower/evck160c.git
   - cd evck160c
  - git sparse-checkout init --cone
   - git sparse-checkout set /* !doc/test_update: !opt/evck/gui_app/static/172.19.0.1:8082/
   -    git checkout


- enable the sparse checkout:
	- git sparse-checkout init --cone
## changing the remote repo after cloning
- git remote -v 
- git remote 