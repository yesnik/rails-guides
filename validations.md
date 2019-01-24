# Validations

```ruby
class Partner < ApplicationRecord
  validates :name, :surname, presence: true
  
  validates :login, length: { minimum: 2, maximum: 15 }
  validates :password, length: { in: 8..15 }
  validates :pin, length: { is: 4 }
  
  validates :balance, numericality: true
  validates :clients_found, numericality: { only_integer: true }
  
  validates :email, uniqueness: true
end
```
