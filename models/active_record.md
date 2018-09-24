
## ActiveRecord

```ruby
User.create(name: 'Kenny', address: 'Some street, 123')

# This will return array of the corresponding model objects
User.create([
  {name: 'Kenny', address: 'Some street, 1'},
  {name: 'Jenny', address: 'Some street, 2'}
])

Product.where "price > 100 and created_at > '2018-04-10'"  # It returns ActiveRecord::Relation
Product.where('price > ? and created_at > ?', 100, '2018-04-10')
Product.where('price > :price and created_at > :created_at', price: 100, created_at: '2018-04-10')

User.find_or_initialize_by(name: 'kenny')
User.first_or_initialize(name: 'kenny')

User.first
User.last

Product.order('title DESC').pluck(:title)
#=> ['Simple PHP', 'Ruby is our friend', 'Python']

Product.find_by_sql("SELECT title, price FROM products WHERE price > 20")
#=> [#<Product id: nil, title: 'Ruby is our friend', price: 135>]


# Aggregate functions
Product.average(:price)
Product.maximum(:price)
Product.minimum(:price)
Product.sum(:price)
Product.count #=> 5
```

### Scopes

```ruby
class Order < ApplicationRecord
  scope :last_n_days, ->(days) { where('updated < ?', days) }
  scope :checks, -> { where(pay_type: :check) }
end

orders = Order.last_n_days(5)
```

## Other methods

```ruby
Order.column_names
#=> ['id', 'name', 'email', 'created_at', 'updated_at']

Order.columns_hash['name']
#=> #<ActiveRecord::ConnectionAdapters::PostgreSQLColumn:0x002123 @name="name",
@table_name="orders", @null=true, @default=nil, @max_identifier_length=63>

product = Product.last
product.price.class #=> BigDecimal
product.price #=> 0.26e2
product.price_before_type_cast #=> '26.00'  # Note: we added suffix '_before_type_cast' to get raw value of attribute
product.attributes
#=> {'id'=>17, 'title'=>'Ruby', 'description'=>'Ruby rocks here!', 'price'=>200}
product.attribute_names
#=> ['id', 'title', 'description', 'price']
product.attribute_present? :price  #=> true

user.superuser?  #=> Rails adds method with question mark to boolean attrs.
```

### Update record

```ruby
product.update(price: 10, title: 'Nice Ruby')
#=> true  Note: this method will update record in database

Product.update(15, title: 'Mighty PHP', price: 30)
```

### Delete record

`delete()` method bypass callback and validation functions:

```ruby
Order.delete(1000) #=> 0
Order.delete(5, 1000) #=> 1
Product.where('price > 1000').delete_all
```

`destroy()` method run all callbacks and validations.

```ruby
order = Order.find_by(name: 'Kenny')
order.destroy

Order.destroy(1)
Order.where('created_at > ?', Date.current + 30.days).destroy_all
```

### Pass block to Order.new

If you want to create and save an order without creating a new local variable:

```ruby
Order.new do |o|
  o.name = 'Kenny'
  o.address = 'Some street, 123'
  o.save
end
```

## Transactions

```ruby
Account.transaction do
  account1.deposit(5)
  account2.withdraw(5)
end
```
