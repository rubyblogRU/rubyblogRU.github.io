---
layout: post
title:  "Как сделать ЧПУ (человеко-понятные-урлы) с конвертацией русских в латиницу"
date:   2020-01-21 06:00:00 +0300
categories: others
---

**Задача:** сделать ЧПУ.

**Для решение задачи задействуем 2 гема:**

- [https://github.com/norman/friendly_id](https://github.com/norman/friendly_id)
- [https://github.com/norman/babosa](https://github.com/norman/babosa)

Для этого, пропишем в Gemfile:

```ruby
gem 'friendly_id', '~> 5.2.4'
gem 'babosa'
```

Сделаем бандл инсталл:

```bash
bundle install
```

Для демонстрации, создадим ресурс User:

```bash
rails g model User name:string email:string

rake db:migrate

rails g controller Users
```

Пропишем в config/routes.rb:

```ruby
resources :users
```

Создадим миграцию:

```ruby
rails g migration AddSlugToUsers slug:uniq
```

Сгенерируем friendly_id:

```bash
rails generate friendly_id
```

Накатим миграци:
```bash
rails db:migrate
```

app/models/user.rb:

```ruby
class User < ApplicationRecord
  extend FriendlyId
  friendly_id :name, use: :slugged

  def normalize_friendly_id(text)
    text.to_slug.transliterate(:russian).normalize.to_s
  end
end
```

app/controllers/users_controller.rb

```ruby
class UserController < ApplicationController
  def show
    @user = User.friendly.find(params[:id])
  end
end
```

Создадим вьюху app/views/users/show.html.erb:

```ruby
<h1><%= @user.name %></h1>
<p><%= @user.email %></p>
<p><%= @user.slug %></p>
```

Откроем Rails-консоль и создадим юзера:

```bash
rails c

User.create! name: "Анфиса Николаевна Добронравова"
```

Запустим приложение:
```bash
rails s
```

Откроем в браузере: [http://localhost:3000/users/anfisa-nikolaevna-dobronravova](http://localhost:3000/users/anfisa-nikolaevna-dobronravova)

Готово!



