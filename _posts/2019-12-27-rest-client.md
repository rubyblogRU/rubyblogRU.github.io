---
layout: post
title:  "Insomnia: программа для тестирования REST JSON API"
date:   2019-11-27 06:00:00 +0300
categories: others
---

Insomnia &mdash; это программа для тестирования REST API.

[https://insomnia.rest/](https://insomnia.rest/)

Установка Insomnia на Ubuntu/Debian:

```bash
# Add to sources
echo "deb https://dl.bintray.com/getinsomnia/Insomnia /" \
    | sudo tee -a /etc/apt/sources.list.d/insomnia.list

# Add public key used to verify code signature
wget --quiet -O - https://insomnia.rest/keys/debian-public.key.asc \
    | sudo apt-key add -

# Refresh repository sources and install Insomnia
sudo apt-get update
sudo apt-get install insomnia
```
Другие ОС: [https://support.insomnia.rest/article/23-installation](https://support.insomnia.rest/article/23-installation)