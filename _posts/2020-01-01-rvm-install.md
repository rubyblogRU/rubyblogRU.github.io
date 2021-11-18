---
layout: post
title:  "Как установить RVM"
date:   2020-01-01 06:00:00 +0300
categories: others
---

Если надо сделать так, чтобы на компьютере было несколько версий Ruby и Ruby on Rails, или что-то не работает при базовой установке, вам поможет с этим RVM (Ruby Version Manager).

Сначала надо удалить уже установленные Ruby и Ruby on Rails и поставить всё через rvm - [https://rvm.io](https://rvm.io)

**Команда удаления ruby:**

```bash
sudo apt-get purge ruby
```

**Инструкция по установке RVM (команды в терминале GNU/Linux):**

```bash
sudo apt-get install libgdbm-dev libncurses5-dev automake libtool bison libffi-dev

gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB

curl -sSL https://get.rvm.io | bash -s stable

source ~/.rvm/scripts/rvm

rvm install 2.6.5

rvm use 2.6.5 --default

ruby -v
```

Команды RVM можно посмотреть тут: [http://cheat.errtheblog.com/s/rvm](http://cheat.errtheblog.com/s/rvm)

**Альтернатива:** [RBENV - https://github.com/rbenv/rbenv](https://github.com/rbenv/rbenv)