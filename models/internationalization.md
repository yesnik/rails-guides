# Internationalization

## Config

Edit `config/routes.rb`:

```
Rails.application.routes.draw do
  scope '(:locale)' do
    resources :orders
    root 'store#index', as: 'store_index', via: :all
  end
end
```

As you see '(:locale)' is in parentheses, which is the way to say that it's optional.

And now we're ready to extract the locale from the params and make it available to the app:

```ruby
class ApplicationContorller < ActionController::Base
  before_aciton :set_i18n_locale_from_params

  def set_i18n_locale_from_params
  	return unless params[:locale]

  	if I18n.available_locales.map(&:to_s).include?(params[:locale])
  	  I18n.locale = params[:locale]
  	else
      flash.now[:notice] = "#{params[:locale]} translation not available"
      logger.error flash.now[:notice]
    end
  end
end
```

## Use html in yml locale files

```
ru:
  store:
    index:
      title_html: Книжная <b>полка</b>
```

Note: `_html` suffix allows us to use html in translation value.

In `app/views/store/index.html.erb`:

```
<h1><%= t('.title_html') %></h1>
```

## Translation key for partial

In partial `app/views/orders/_form.html.erb` we can use this expression:

```
<%= t('.submit') %>
```

And en.yml:

```
en:
  orders:
    form:
      submit: Place order
```
