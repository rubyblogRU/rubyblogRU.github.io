---
layout: post
title:  "Пошаговая инструкция, как создать сущность в Rails"
date:   2019-08-05 06:00:00 +0300
categories: ruby
---

**Create model:**
```bash
rails g model Project title:string body:text
```

```bash
rake db:migrate
```

**Create controller:**
```bash
rails g controller Projects
```

**Write route in flie /config/routes.rb:**
```ruby
resources :projects
```

**Create files for views in /app/views/projects:**
```bash
touch index.html.erb show.html.erb new.html.erb edit.html.erb _form.html.erb _project.html.erb
```

**Create actions in controller /app/controllers/projects_controller.rb:**

```ruby
class ProjectsController < ApplicationController
  before_action :set_project, only: [:show, :edit, :update, :destroy]

  def index
    @projects = Project.all
  end

  def show
  end

  def new
    @project = Project.new
  end

  def edit
  end

  def create
    @project = Project.new(project_params)
    if @project.valid?
      @project.save
      redirect_to @project
    else
      render :new
    end
  end

  def update
    if @project.update(project_params)
      redirect_to @project
    else
      render :edit
    end
  end

  def destroy
    @project.destroy

    redirect_to projects_path
  end

  private

  def set_project
    @project = Project.find(params[:id])
  end

  def project_params
    params.require(:project).permit(:title, :body)
  end
end
```

**Edit files with views in /app/views/projects:**

**index.html.erb**

```ruby
<%= link_to 'Создать новый проект', new_project_path %>
<hr>

<h1>Проекты</h1>

<%= 'В текущей ленте нет проектов' if @projects.empty? %>

<%= render @projects %>
```

**_project.html.erb**

```ruby
<h3><%= link_to project.title, project_path(project) %></h3>
```

Файл _project.html.erb вместо кода в index.html.erb, заменяет цикл:

```ruby
<% @projects.each do |project| %>
  <h3><%= link_to project.title, project_path(project) %></h3>
<% end %>
```

**show.html.erb**

```ruby
<%= link_to 'Все проекты', projects_path %>
|
<%= link_to 'Создать новый проект', new_project_path %>
|
<%= link_to 'Редактировать текущий проект', edit_project_path(@project) %>
|
<%= link_to 'Удалить текущий проект', project_path(@project), method: :delete, data: { confirm: 'Действительно удалить?'} %>
<hr>

<h1><%= @project.title %></h1>

<p><%= @project.body %></p>
```


**new.html.erb**

```ruby
<h1>Создать проект:</h1>

<%= render 'form' %>
```


**edit.html.erb**

```ruby
<h1>Редактировать проект:</h1>

<%= render 'form' %>
```
**_form.html.erb**

```ruby
<% if @project.errors.any? %>
  <%= t('common.errors') %>
  <ul>
    <% @project.errors.full_messages.each do |message| %>
      <li><%= message %></li>
    <% end %>
  </ul>
<% end %>

<%= form_for @project do |f| %>
  <p>
    <%= f.label :title, 'Заголовок страницы:' %><br />
    <%= f.text_field :title %>
  </p>

  <p>
    <%= f.label :body, 'Текст страницы:' %><br />
    <%= f.text_area :body %>
  </p>

  <p><%= f.submit 'Сохранить' %> | <%= link_to 'Отмена', projects_path %></p>
<% end %>
```

**Добавим валидацию в Модель app/models/project.rb:**

```ruby
class Project < ApplicationRecord
  validates :title, :body, presence: true
end
```

Добавим в стили:

```css
.field_with_errors label {
  color: #d01612;
}

.field_with_errors input, .field_with_errors textarea {
  border: 2px solid #d01612;
}
```