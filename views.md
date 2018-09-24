# Views

All instance variables of the controller are also available in the template. 
This is how actions transfer data to the templates.

## .debug

We can use debug method to dispaly the contents of the session in the view:

```
Session: <%= debug(session) %>
```

## .controller

The current controller object is accessible in view.
This allows the template to call any public method in the controller.

## .base_path

The path to the base directory of the templates.

## Render partial

### Example 1

In template we can use:

```ruby
= render @post
```

It's the same as:

```ruby
= render partial: 'posts/post', collection: @posts
```

### Example 2

Instead of this:

File `app/views/carts/show.html.erb`:

```ruby
<% @cart.line_items.each do |line_item| %>
  <tr>
    <td><%= line_item.quantity %></td>
  </tr>
<% end %>
```

We can write this:

```ruby
<%= render @cart.line_items %>
```

Also create partial `app/views/line_items/_line_item.html.erb`:

```ruby
<tr>
  <td><%= line_item.quantity %></td>
</tr>
```

*Note*: the partial is named line_item, so inside the partial we expect to have a variable called `line_item`.


## button_to

This helper allows us to create delete button for destroy action:

```ruby
<%= button_to 'X', @item, method: :delete, data: {confirm: 'Are you sure?'} %>
```

## Render javascript

In controller `app/controllers/line_items_controller.rb`:

```ruby
class LineItemsController < ApplicationController
  def destroy
    @line_item.destroy

    respond_to do |format|
      format.html { redirect_to store_index_url, notice: 'Line item was deleted' }
      format.js { @line_items = @cart.line_items }
    end
  end
end
```

In view `app/views/line_items/destroy.js.erb`:

```javascript
$('#cart').html('<%= j render @line_items %>')
```

*Note*: It will render `views/line_items/_line_item.html.erb`
