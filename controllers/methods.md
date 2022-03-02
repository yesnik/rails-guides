
## GET params

```
id=1				{id: '1'}
user[name]=kenny		{user: {name: 'kenny'}}
user[address][city]=Moscow	{user: {address: {city: 'Moscow'}}}
```

## Strong Parameters

This feature helps Rails to filter `params` passed to controller action.

```ruby
class ArticlesController < ApplicationController
  # ...

  def create
    @article = Article.new(article_params)

    if @article.save
      redirect_to @article
    else
      render :new, status: :unprocessable_entity
    end
  end

  private
    def article_params
      params.require(:article).permit(:title, :body)
    end
end
```

## logger in controllers

Every controller has a `logger` attribute. We can use it to record a message at the error logging level:

```ruby
class CartsController < ApplicationController
  rescue_from ActiveRecord::RecordNotFound, with: :invalid_cart

  private

  def invalid_cart
    logger.error "Attempt to access invalid cart: #{params[:id]}"
    redirect_to store_index_url, notice: 'Invalid cart'
  end
end
```

Here `:notice` param specifies a message to be stored in the flash as a notice.

## redirect_to

```ruby
redirect_to store_index_url, notice: 'Logged out'
```

## Methods in controller

```
action_name #=> 'index'
cookies
headers - hash of HTTP headrs that will be used in the response.
params  - a hash-like object containing request params: params[:id] or params['id']
request - the incoming request object. Methods:
	request.get? #=> true
	request.post?
	request.request_method #=> 'GET'
	request.xhr? #=> nil
	request.url #=> 'http://127.0.0.1:3000/'
	request.query_string
	request.remote_ip
response
session
logger  - it is available throughout Action Pack
```

## send_data

It sends a data stream to the client.

```ruby
def sales_graph
  png_data = Sales.plot_for(Date.today.month)
  send_data(png_data, type: 'image/png', disposition: 'inline')
end
```

## send_file

```ruby
def send_secret_file
  send_file '/files/secret_list'
  headers['Content-Description'] = 'Top secret'
end
```

## render

```ruby
render(action: 'index')
render(template: 'products/description')
render(file: 'shared/agreement')
```

## layout

Layouts are controller specific. If the current request is being handled by a controller called `store`, 
Rails will by default look for a layout called store in the `app/views/layouts` directory.
Layout `layouts/application.html.erb` will be applied to all controllers 
that don't otherwise have a layout defined for them.

We can qualify which actions will have the layout applied to them using the `:only` and `:except` qualifiers:

```ruby
class StoreController < ApplicationController
  layout 'standard', except: [:rss, :atom]
end
```

Specifying a layout of `nil` turns off layouts for a controller.

We can create method containing logic for defining layout:

```ruby
class StoreController < ApplicationController
  layout :find_layout

  protected

  def find_layout
    if Store.closed?
      'store_down'
    else
      'standard'
    end
  end
end
```

Individual actions can choose to render using a specific layout:

```ruby
def rss
  render layout: false
end

def checkout
  render layout: 'layouts/simple'
end
```
