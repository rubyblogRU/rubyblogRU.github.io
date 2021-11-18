---
layout: post
title:  "Include vs Extend"
date:   2019-11-20 06:00:00 +0300
categories: others
---

```ruby
module Foo
  def say
    "Fooo!"
  end
end

module Bar
  def say
    "Bar!!!"
  end
end

class Person
  include Foo
  extend Bar
end

dog = Person.new
p dog.say

p Person.say

# => "Fooo!"
# => "Bar!!!"
```