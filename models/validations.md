## Validations

The model layer is the gatekeeper between the world of code and the database.
Nothing to do with our app comes out of the database or gets stored into the database that doesn't first
go throuth the model. This makes models an ideal place to put validations.
If a model checks it before writing to the database, the database will be protected from bad data.

```ruby
class Product
  validates :title, :description, presence: true

  validates :points, numericality: true
  validates :games_played, numericality: { only_integer: true }
end
```

## Enum type

```ruby
class Order < ApplicationRecord
  enum pay_type: {
  	'Check' => 0,
  	'Credit card' => 1,
  	'Purchase order' => 2
  }
end
```

*Note*: Table orders has column pay_type with type Integer.
