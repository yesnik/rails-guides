# Rake

## middleware

Show list of middlewares that Rails provides for Rails apps:

```
rake middleware
```

## rackup

Start rake application:

```
rackup store.ru
```

## Rake tasks

Rakefile can be found in root directory of Rails app.
All rake tasks we have to place in `lib/tasks` directory. File must has .rake extension:
`/lib/tasks/db_backup.rake`.

This command will show you all dependent rake tasks:

```
rake --trace --dry-run db:setup
```
