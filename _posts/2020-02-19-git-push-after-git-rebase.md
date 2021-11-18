---
layout: post
title:  "Отправка изменений на удалённую ветку после git rebase"
date:   2020-02-19 06:00:00 +0300
categories: others
---

- Переходим на ветку `new-feature-name`
- Делаем `git rebase master`
- Исправляем конфликты
- Делаем `git add` для файла
- Делаем `git rebase --continue`
- Отправляем изменения в ветку после git rebase: `git push origin new-feature-name --force-with-lease`
