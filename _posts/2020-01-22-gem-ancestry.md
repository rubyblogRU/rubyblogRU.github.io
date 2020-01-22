---
layout: post
title:  "Создание каталога с вложенными категориями и подкатегориями. Подключаем гем ancestry"
date:   2020-01-22 06:00:00 +0300
categories: others
---

Чтобы сделать структуру каталога с категориями и подкатегориями, воспользуемся гемом [https://github.com/stefankroes/ancestry](https://github.com/stefankroes/ancestry)

Итак, поехали.

### Создадим ресурс категории:

```bash
rails g model Category name:string description:text
rails g controller Categories
rake db:migrate
```

```ruby
# config/routes.rb
Rails.application.routes.draw do
  resources :categories
end
```

### Добавим в Gemfile и создадим миграцию:
```ruby
gem 'ancestry'
```

```bash
bundle install
```

```bash
rails g migration add_ancestry_to_categories ancestry:string:index
rake db:migrate
```

### Продолжим работу с ресурсом категории:

**Добавим в модель категории:**

```ruby
class Category < ApplicationRecord
  has_ancestry
end
```

**Добавим в контроллер категории:**

```ruby
class CategoriesController < ApplicationController
  before_action :set_category, only: [:show, :edit, :update, :destroy]

  def index
    @categories = Category.all
  end

  def show
  end

  def new
    @category = Category.new
  end

  def create
    @category = Category.new(category_params)
    if @category.valid?
      @category.save
      redirect_to @category
    else
      render :new
    end
  end

  def update
    if @category.update(category_params)
      redirect_to @category
    else
      render :edit
    end
  end

  def destroy
    @category.destroy
    redirect_to categories_path
  end

  private

  def set_category
    @category = Category.find(params[:id])
  end

  def category_params
    params.require(:category).permit(:name, :description, :parent_id)
  end
end
```

**Поработаем с вьюхами и паршилами:**

```ruby
# app/views/categories/index.html.erb:

<h1>Категории</h1>

<%= render partial: "category", locals: { collection: Category.all } %>

<%= link_to 'Новая категория', new_category_path %>
```

```ruby
# app/views/categories/_category.html.erb:
<ul class="categories">
  <% collection.arrange.each do |category, sub_item| %>
    <li>
      <%= link_to category.name, category_path(category) %>

      <% if category.has_children? %>
        <%= render partial: "category", locals: { collection: category.children } %>
      <% end %>
    </li>
  <% end %>
</ul>
```

```ruby
# app/views/categories/new.html.erb:
<h1>Создать категорию</h1>

<%= render 'form' %>
```

```ruby
# app/views/categories/edit.html.erb:
<h1>Редактировать категорию</h1>

<%= render 'form' %>
```

```ruby
# app/views/categories/_form.html.erb:
<%= form_for @category do |f| %>
  <p>
    <%= f.label :name %>
    <%= f.text_field :name %>
  </p>

  <p>
    <%= f.label :description %>
    <%= f.text_area :description %>
  </p>

  <p>
    <%= f.label :parent_id %>
    <%= f.collection_select :parent_id, Category.order(:name), :id, :name, include_blank: true %>
  </p>

  <p>
    <%= f.submit 'Сохранить' %> |
    <%= link_to 'Отмена', categories_path %>
  </p>
<% end %>
```

```ruby
# app/views/categories/show.html.erb:
<h1><%= @category.name %></h1>

<p><%= @category.description %></p>

<p>
  <%= link_to 'Редактировать категорию', edit_category_path(@category) %> |
  <%= link_to 'Удалить категорию', category_path(@category), method: :delete, data: { confirm: 'Точно удалить?' } %> |
  <%= link_to 'Назад в категории', categories_path %>
</p>

<% if @category.has_children? %>
  <h3>Подкатегории у категории: <%= @category.name %></h3>
  <div class="submenu">
    <%= render 'submenu_categories', categories: @category.root.descendants.arrange %>
  </div>
<% end %>
```

```ruby
# app/views/categories/_submenu_categories.html.erb:
<ul>
  <% categories.each do |category, children| %>
    <li>
      <%= link_to_unless_current category.name, category %>
      <%= render 'submenu_categories', categories: children if children.present? %>
    </li>
  <% end %>
</ul>
```

Готово. Теперь можно вкладывать подкатегории в категории. Плюс у нас готовы 2 типа меню, отображающие всю структуру категорий-подкатегорий (в index.html.erb) и меню отображающее только подкатегории данной категории (в show.html.erb)

Плюс Вы можете добавить меню с корневыми категориями (добавим в layout):

```ruby
# app/views/layouts/application.html.erb
<ul id="menu">
  <% Category.roots.each do |category| %>
    <li><%= link_to category.name, category %></li>
  <% end %>
</ul>

<hr>
```

Можно ещё почитать: [https://hackernoon.com/nested-categories-with-rails-gem-ancestry-112ca8bbbf98](https://hackernoon.com/nested-categories-with-rails-gem-ancestry-112ca8bbbf98)

Более подробно про гем и его настройки, читайте тут: [https://github.com/stefankroes/ancestry](https://github.com/stefankroes/ancestry)