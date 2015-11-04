---
layout: post
title: "Retrieve a random record in a Rails application"
description: "A simple concern to add to some of the models. Retrieve one or several random records"
category: blog
---

Sometimes, you don't want to fetch the `.last` or `.first` record of a given model,
but something at random. In ActiveRecord, there is nothing built in. Here is a simple
concern you can add to your rails application to add this feature.

```ruby
# app/models/concerns/randomable.rb
module Randomable
  extend ActiveSupport::Concern

  class_methods do
    def random(the_count = 1)
      records = offset([0, rand(count) - the_count].max).limit(the_count)
      the_count == 1 ? records.first : records
    end
  end
end

```

Then it's very easy to use, just include the module in your model (can be used
in several models at will!).

```ruby
# app/models/book.rb
class Book < ActiveRecord::Base
  include Randomable
end
```

As you can see, you can call `.random` without any arguments to fetch **one**
record, or `.random(count)` with an integer argument to fetch an `ActiveRecord::Relation`
of records.

```bash
$ bin/rails c
irb(main):001:0> Book.random
  Book Load (3.6ms)  SELECT  "books".* FROM "books"  ORDER BY RANDOM() LIMIT 1
=> #<Book id: 22, title: "Madame Bovary", created_at: "2015-11-04 09:28:32", updated_at: "2015-11-04 09:28:32">
irb(main):002:0> Book.random(2)
  Book Load (0.4ms)  SELECT  "books".* FROM "books"  ORDER BY RANDOM() LIMIT 2
=> #<ActiveRecord::Relation [#<Book id: 64, title: "Le Rouge et le Noir", created_at: "2015-11-04 09:28:32", updated_at: "2015-11-04 09:28:32">, #<Book id: 88, title: "L'Assomoir", created_at: "2015-11-04 09:28:32", updated_at: "2015-11-04 09:28:32">]>
```

Enjoy querying randomly!
