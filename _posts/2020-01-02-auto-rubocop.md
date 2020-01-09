---
layout: post
title:  "Авто Rubocop"
date:   2020-01-02 06:00:00 +0300
categories: others
---

Terminal:

```bash
git ls-files -m | xargs ls -1 2>/dev/null | grep .rb | xargs rubocop -a --force-exclusion
```
