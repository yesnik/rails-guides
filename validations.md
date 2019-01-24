# Validations

```ruby
class Partner < ApplicationRecord
  validates :name, :surname, presence: true
  
  validates :login, uniqueness: true
  
  validates :description, length: { minimum: 20, maximum: 250 }
  validates :password, length: { in: 8..15 }
  validates :pin, length: { is: 4 }
  
  validates :balance, numericality: true
  validates :clients_found, numericality: { only_integer: true }
  
  validates :email, format: { with: /\A([^@\s]+)@((?:[-a-z0-9]+\.)+[a-z]{2,})\z/i }
end
```
