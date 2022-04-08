# Migrations

## Migration methods

```ruby
change_column_null :books, :title, false  # Adds NOT NULL constraint
execute "UPADATE products SET locale = 'en'"
```

## Rename table

The following version of the migration will work even if the applicaiton no longer has an `OrderHistory` class:

```ruby
class CreateOrderHistories < ActiveRecord::Migration
  
  class Order < ApplicationRecord::Base; end
  class OrderHistory < ApplicationRecord::Base; end

  def change
    create_table :order_histories do |t|
      t.integer :order_id, null: false
      t.text :notes

      t.timestamps
    end

    order = Order.find :first
    OrderHistory.create(order: order_id, notes: 'test')
  end
end
```

## Add foreign key to users table

```ruby
def change
  add_reference :questions, :author, null: false, foreign_key: { to_table: :users }
end
```
This migration will: 

- create column `author_id BIGINT NOT NULL`
- foreign key `"fk_rails_d96516e73c" FOREIGN KEY (author_id) REFERENCES users(id)`
