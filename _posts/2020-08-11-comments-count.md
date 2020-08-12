---
layout: post
title:  "Добавление кешированного счётчика комментариев к статье"
date:   2020-05-04 06:00:00 +0300
categories: others
---

Предположим, что у нас уже есть модели `Article` и `Comment`.

**Задача:** выводить счётчик комментариев к статье.

### Сделаем следующее:

1. Создадим миграцию:

```ruby
class AddCommentsCountToArticles < ActiveRecord::Migration[6.0]
  def change
    add_column :articles, :comments_count, :integer, default: 0
  end
end
```

2. Прогоним миграцию `rails db:migrate`

2. Создадим `rake task`:

Создайте файл `lib/tasks/update_comments_count.rake`

```ruby
namespace :db do
  desc 'Update comments count'
  task update_comments_count: :environment do
    Article.reset_column_information
    Article.all.each do |article|
      Article.update_counters article.id, comments_count: article.comments.length
    end
  end
end
```

3. Добавим `counter_cache` для связи в модели `Comment`:

```ruby
class Comment < ApplicationRecord
  belongs_to :article, counter_cache: true
end
```

4. Запустим таск:

```bash
rails db:update_comments_count
```

5. Добавим во view `app/views/articles/show.html.erb`

```ruby
<h2>Comments (<%= @article.comments_count %>) :</h2>
```

Запустим приложение `rails s`, добавим комментарий, и счётчик обновится автоматически.

---
Yeah! Удачи. Автор статьи: [@krdprog](https://github.com/krdprog/)
