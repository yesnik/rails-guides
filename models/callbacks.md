
## Callbacks

ActiveRecord defines 16 callbacks:

* Creating an Object

```
before_validation
after_validation
before_save
  around_save
  before_create
    around_create
  after_create
after_save
after_commit/after_rollback
```

* Updating an Object

```
before_validation
after_validation
before_save
  around_save
  before_update
    around_update
  after_update
after_save
after_commit/after_rollback
```

* Destroying an Object

```
before_destroy
  around_destroy
after_destroy
after_commit/after_rollback

after_initialize
after_find
after_touch
```

### Class for model callbacks

A handler class is a class that defines callback methods (before_save(), before_validate(), etc.)

```ruby
class CreditCardCallbacks
  def before_validation(model)
    model.cc_number.gsub!(/[-\s]/, '')
  end
end
```

In our model classes we can arrange for this shared callback to be invoked:

```ruby
class Order < ApplicationRecord
  before_validation CreditCardCallbacks.new
end

class Subscription < ApplicationRecord
  before_validation CreditCardCallbacks.new
end
```

### Rollback in after_destroy

File `app/models/user.rb`:

```ruby
class User
  after_destroy :ensure_user_exists

  class Error < StandardError; end

  protected

  def ensure_user_exists
    raise Error.new 'Cannot delete last user' if User.count.zero?
  end
end
```

File `app/controllers/users_controller.rb`

```ruby
class UsersController < ApplicationController
  
  def destroy
    # ...
  end

  rescue_from 'User::Error' do |exception|
    redirect_to users_url, notice: exception.message
  end
end
```

This exception:
- it causes an automatic rollback, because it's raised inside a transaction
- it signals the error back to the controller, where we use a `resque_from` block
to handle it and report the error to the user in the notice.

Raise `ActiveRecord::Rollback` exception if you want only to abort the transaction.
