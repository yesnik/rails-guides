# Routes

You can find routes via 2 ways:

- Go to page: http://127.0.0.1:3000/rails/info/routes
- In console: `rails routes`

Routes are defined at `config/routes.rb`.

## resources

Whenever we have a combination of routes, controller actions, and views that work together 
to perform CRUD operations on an entity, we call that entity a `resource`.

This method maps all of the conventional routes for a collection of resources, such as `articles`:

```ruby
resources :articles
```

If you don't want all 7 actions, you can exclude them:

```ruby
resources :comments, excerpt: [:update, :destroy]
```

The `resources` method also sets up URL and path helper methods:

```ruby
<a href="<%= article_path(article) %>">
  <%= article.title %>
</a>
```
Run `rails routes` to see all prefixes:

```ruby
Prefix Verb   URI Pattern                                                                                       
root 		GET    /				articles#index
articles 	GET    /articles(.:format)		articles#index
		POST   /articles(.:format)		articles#create
new_article 	GET    /articles/new(.:format)		articles#new
edit_article 	GET    /articles/:id/edit(.:format)	articles#edit
article 	GET    /articles/:id(.:format)		articles#show
		PATCH  /articles/:id(.:format)		articles#update
		PUT    /articles/:id(.:format)		articles#update
		DELETE /articles/:id(.:format)		articles#destroy
```

## Default route

```ruby
root to: 'products#index'
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
