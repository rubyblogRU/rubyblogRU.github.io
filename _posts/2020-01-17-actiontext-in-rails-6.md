---
layout: post
title:  "Инструкция: как настроить форму с RichText полем в Rails 6 (как включить ActionText)"
date:   2020-01-17 06:00:00 +0300
categories: others
---
Создадим контроллер:

```bash
rails g controller Posts
```

Пропишем в routes.rb:
```ruby
resources :posts
```

Создадим модель:
```bash
rails g model Post title:string
```

Накатим миграцию:
```ruby
rake db:migrate
```

Активируем ActionText:
```bash
rails action_text:install
```

Накатим миграцию:
```bash
rake db:migrate
```

Добавим в app/models/post.rb:

```ruby
has_rich_text :text
```

Пропишем в контроллере posts_controller.rb:
```ruby
class PostsController < ApplicationController
  def new
  end

  def create
    @post = Post.new(post_params)
    @post.save
    redirect_to @post
  end

  private

  def post_params
    params.require(:post).permit(:title, :text)
  end
end
```

Пропишем во вьюхе app/views/posts/new.rb:

```ruby
<%= form_with scope: :post, url: posts_path, local: true do |form| %>
  <p>
    <%= form.label :title %><br>
    <%= form.text_field :title %>
  </p>

  <p>
    <%= form.label :text %><br>
    <%= form.rich_text_area :text %>
  </p>

  <p>
    <%= form.submit %>
  </p>
<% end %>
```

**Обратите внимание, на значение поля: form.rich_text_area**

Пропишем в контроллере:
```ruby
def show
  @post = Post.find(params[:id])
end
```

Пропишем во вьюхе app/views/posts/show.html.erb

```ruby
<p>Post Title: <strong><%= @post.title %></strong></p>
<p>Post Text: <strong><%= @post.text %></strong></p>
<p><%= link_to 'Create new Post', new_post_path %></p>
```

Запустим:
```bash
rails s
```

Откроем:
```txt
http://localhost:3000/posts/new
```