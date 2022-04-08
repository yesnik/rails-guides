# Relations

## belongs_to

```ruby
class Answer < ApplicationRecord
  belongs_to :question
  belongs_to :author, class_name: 'User'
end
```

Table `answers`: 

- `question_id` - foreign key to the column `questions.id`
- `author_id` - foreign key to the column `users.id`
