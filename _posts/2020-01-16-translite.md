---
layout: post
title:  "Тест-драйв гемов для транслитерации кириллицы. Выясняем, какой гем лучше отработает задачу."
date:   2020-01-15 06:00:00 +0300
categories: others
---

Проведём тест-драйв гемов для транслитерации кириллицы и выясним, какой гем лучше отработает задачу. Проведём транслитерацию с русского в латиницу (английский).

## Варианты гемов для транслитерации кириллицы (участники тест-драйва)

1. gem russian
2. gem translit
3. gem cyrillizer

## Тестируем gem russian

```bash
gem install russian
```

```ruby
require 'russian'
Russian.translit('Поставка комплектующих для двигателей внутреннего сгорания')

# => "Postavka komplektuyuschih dlya dvigateley vnutrennego sgoraniya"
```

## Тестируем gem translit

```bash
gem install translit
```

```ruby
require 'translit'

str = 'Поставка комплектующих для двигателей внутреннего сгорания'
Translit.convert(str, :english)

# => "Postawka komplektuüschih dlq dwigatelej wnutrennego sgoraniq"
```

## Тестируем gem cyrillizer

```bash
gem install cyrillizer
```

```ruby
require 'cyrillizer'

puts 'Поставка комплектующих для двигателей внутреннего сгорания'.to_lat

# => Postavka komplektuющih dlя dvigateleй vnutrennego sgoraniя
```

## Результат тест-драйва

Как видите, побеждает gem russian с отработкой без ошибок:

```ruby
# => "Postavka komplektuyuschih dlya dvigateley vnutrennego sgoraniya"
```
Оставшиеся два гема отработали с ошибками:

```ruby
# => "Postawka komplektuüschih dlq dwigatelej wnutrennego sgoraniq"
# => Postavka komplektuющih dlя dvigateleй vnutrennego sgoraniя
```