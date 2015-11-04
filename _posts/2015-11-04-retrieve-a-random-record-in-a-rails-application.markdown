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
      records = offset(rand(count - the_count)).limit(the_count)
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
irb(main):051:0> Book.random
   (0.2ms)  SELECT COUNT(*) FROM "books"
  Book Load (0.2ms)  SELECT  "books".* FROM "books"  ORDER BY "books"."id" ASC LIMIT 1 OFFSET 88
=> #<Book id: 89, title: "Title 89", created_at: "2015-11-04 09:28:32", updated_at: "2015-11-04 09:28:32">
irb(main):052:0> Book.random(2)
   (0.1ms)  SELECT COUNT(*) FROM "books"
  Book Load (0.1ms)  SELECT  "books".* FROM "books" LIMIT 2 OFFSET 16
=> #<ActiveRecord::Relation [#<Book id: 17, title: "Title 17", created_at: "2015-11-04 09:28:32", updated_at: "2015-11-04 09:28:32">, #<Book id: 18, title: "Title 18", created_at: "2015-11-04 09:28:32", updated_at: "2015-11-04 09:28:32">]>
```

Enjoy querying randomly!
