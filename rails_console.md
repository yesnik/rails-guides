# Console commands

## rails about

Shows info about app's environment: 
Rails, Ruby, RubyGems, Rask versions, JS Runtime, Middleware, DB adapter etc.
This command runs from the `bin` directory. This command is a wrapper, or binstub,
for the Rails executable. It serves 2 purposes:

- it ensures that you're running with the correct version of every dependency;
- it speeds up the startup times of Rails commands by preloading your app.

## rails stats

Show number of controllers, models, methods, etc.

## rails new

```bash
rails new mysite -d postgresql # use PostgreSQL
rails new mysite -d mysql # use MySQL
rails new depot -T --skip-coffee # skip tests, coffescript
rails new todos-api --api -T # api app, skip tests
```

## rails db:create

This command creates databases for *development* and *test*. It uses `config/database.yml`.

## rails db:seed

Apply seeds to database.

## rails dev:cache

Toggle caching in dev environment

## rails generate

```bash
rails g model Item name done:boolean todo:references

# It will create: t.decimal :balance, precision: 10, scale: 2:
rails g Model Account balance:decimal{10-2}

rails g controller Admin::Book action1 action2

rails g scaffold User name password:digest

rails g channel products

# Generate default files for rspec-rails gem
rails g rspec:install

# It will create file lib/tasks/db_schema_migrations.rake
rails g task db_schema_migrations

# This command allows us to run scripts, that use our Rails app:
rails runner scripts/generate_orders.rb
```

## rails db:migrate

This command applies or reverts migration files located at `db/migrate`:

```
rails db:migrate  # Apply pending migrations
rails db:migrate VERSION=201804201030305020  # It will apply/rollback migrations to rewind DB to specified state.
rails db:migrate redo  # It will roll back one migration and rerun it.
rails db:migrate redo STEP=2  # It will roll back 2 last migrations and rerun them.
rails db:rollback  # Rollback latest migration

rails g migration remove_pay_type_from_orders pay_type:integer
rails g migration add_pay_type_to_orders pay_type:references

rails g Model Account balance:decimal{10-2}
# It will create: t.decimal :balance, precision: 10, scale: 2
```

## Other commands

```
rails console production  # Start rails console for production environment
rails -T  # Show all rake tasks
rails -D db:seed  # Show description of rake task
```
