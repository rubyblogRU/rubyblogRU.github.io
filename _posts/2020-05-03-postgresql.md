---
layout: post
title:  "Установка PostgreSQL"
date:   2020-05-03 06:00:00 +0300
categories: others
---

**Заметка в разработке**

- [https://www.postgresql.org/download/linux/ubuntu/](https://www.postgresql.org/download/linux/ubuntu/)


```bash
sudo apt-get install postgresql postgresql-contrib
sudo apt-get install libpq-dev

sudo su - postgres
createuser --pwprompt alex
```
