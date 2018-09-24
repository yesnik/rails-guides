# Routes

You can find routes via 2 ways:

- Go to page: http://127.0.0.1:3000/rails/info/routes
- In console: rails routes

If you don't want all 7 actions, you can exclude them:

In `routes.rb`:

```
resources :comments, excerpt: [:update, :destroy]
```

## Routing concerns

Concerns allow you to capture the common behavior:

```ruby
concern :reviewable do
	resources :reviews
end

resources :products, concern: :reviewable
resources :products, concern: :users
```
