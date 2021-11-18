---
layout: post
title:  "Установка node, npm, yarn"
date:   2020-05-02 06:00:00 +0300
categories: others
---

- [https://nodejs.org/en/](https://nodejs.org/en/)
- [https://github.com/nodesource/distributions#deb](https://github.com/nodesource/distributions#deb)
- [https://github.com/yarnpkg/yarn](https://github.com/yarnpkg/yarn)
- [https://yarnpkg.com/getting-started/install](https://yarnpkg.com/getting-started/install)


**Bundler:**

```
gem install bundler:2.1.4
```

**Установка Node.js v14.x:**

```bash
# Using Ubuntu
curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash -
sudo apt-get install -y nodejs

node -v
```

**Установка Node.js v13.x:**

```bash
# Using Ubuntu
curl -sL https://deb.nodesource.com/setup_13.x | sudo -E bash -
sudo apt-get install -y nodejs

node -v
```

**Установка npm:**

```bash
sudo apt install npm
npm -v
```

**Установка Yarn:**
```bash
curl -sL https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
sudo apt-get update && sudo apt-get install yarn

yarn -v
```
