---
layout: post
title:  "Как сделать RESTful JSON API на Ruby on Rails"
date:   2019-12-15 06:00:00 +0300
categories: others
---

**Предположим, что у нас есть модель Post:**

```bash
rails g model Post title:string body:text
```

```bash
rake db:migrate
```

**Чтобы создать RESTful JSON API на Ruby on Rails, надо:**

1. Добавить в Gemfile:

```ruby
gem 'jbuilder', '~> 2.7'
```

2. Сделать bundle install

```bash
bundle install
```

3. Прописать в /config/routes.rb

```ruby
# /config/routes.rb
Rails.application.routes.draw do
  namespace 'api' do
    namespace 'v1' do
      resources :posts
    end
  end
end
```

4. Создать каталог /app/controllers/api/v1/ и файл /app/controllers/api/v1/posts_controller.rb

```ruby
# /app/controllers/api/v1/posts_controller.rb
module Api
  module V1
    class PostsController < ApplicationController
      before_action :set_post, only: [:show, :update, :destroy]

      skip_forgery_protection

      # GET /api/v1/posts
      def index
        @posts = Post.all
        if @posts.present?
          render json: @posts, status: :ok
        else
          render json: {status: 'ERROR', message: 'Posts not found'}, status: :not_found
        end
      end

      # GET /api/v1/posts/:id
      def show
        render json: @post, status: :ok
      end

      # POST /api/v1/posts
      def create
        @post = Post.new(post_params)

        if @post.save
          render json: {status: 'SUCCESS', message: 'Created post', data: @post}, status: :ok
        else
          render json: {status: 'ERROR', message: 'Post not created', data: @post.errors}, status: :unprocessable_entity
        end
      end

      # PATCH, PUT /api/v1/posts/:id
      def update
        if @post.update_attributes(post_params)
          render json: {status: 'SUCCESS', message: 'Updated post', data: @post}, status: :ok
        else
          render json: {status: 'ERROR', message: 'Post not updated', data: @post.errors}, status: :unprocessable_entity
        end

      end

      # DELETE /api/v1/posts/:id
      def destroy
        @post.destroy
        render json: {status: 'SUCCESS', message: 'Deleted post', data: @post}, status: :ok
      end

      private

      def set_post
        @post = Post.find(params[:id])
        rescue Exception
          render json: {status: 'ERROR', message: 'Post not found'}, status: :not_found
      end

      def post_params
        params.require(:post).permit(:title, :body)
      end
    end
  end
end
```
5. Проверять API мы будем программой Insomnia. [Тут был обзор программы](http://rubyblog.ru/others/rest-client/)

**Наш созданный REST JSON API:**

|  Action    |  Method    |  Path    |  Comment    |
|-------|-------|-------|-------|
| **List** | GET | /api/v1/posts | Просмотр списка постов |
| **Create** | POST | /api/v1/posts | Создать новый пост |
| **Read** | GET | /api/v1/posts/:id | Просмотр поста |
| **Update** | PATCH, PUT | /api/v1/posts/:id | Изменить пост |
| **Delete** | DELETE | /api/v1/posts/:id | Удалить пост |

**Чтобы создать новый пост в теле запроса POST по адресу /api/v1/posts
отправим:**

```ruby
{
  "title": "This is test",
  "body": "Hello API!"
}
```