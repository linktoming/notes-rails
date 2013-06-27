### Set NO Documentation During Gem Installation
1. Creating a gem configuration file.

    ```bash
    $ subl ~/.gemrc
    ``` 

1. Suppressing the ri and rdoc Documentation in .gemrc

    ```bash
    install: --no-rdoc --no-ri
    update: --no-rdoc --no-ri
    ```

### Create a Rails app
```bash
$ rails new app_name
```
    
### Gemfile Syntax

``` Ruby
source 'https://rubygems.org'

gem 'rails', '3.2.13'

# Bundle edge Rails instead:
# gem 'rails', :git => 'git://github.com/rails/rails.git'

gem 'sqlite3'


# Gems used only for assets and not required
# in production environments by default.
group :assets do
  gem 'sass-rails',   '~> 3.2.3'
  gem 'coffee-rails', '~> 3.2.2'

  gem 'uglifier', '>= 1.2.3'
end

gem 'jquery-rails'

# To use ActiveModel has_secure_password
# gem 'bcrypt-ruby', '~> 3.0.0'

# To use Jbuilder templates for JSON
# gem 'jbuilder'

# Use unicorn as the web server
# gem 'unicorn'

# Deploy with Capistrano
# gem 'capistrano'

# To use debugger
# gem 'ruby-debug19', :require => 'ruby-debug'
```
###  Create and Use gemsets
```bash
$ rvm use 1.9.3@rails3tutorial2ndEd --create --default
Using /Users/mhartl/.rvm/gems/ruby-1.9.3 with gemset rails3tutorial2ndEd
```
### Install the Gems
```bash
$ bundle update
$ bundle install
Fetching source index for https://rubygems.org/
.
.
.
```
[Bundler Best Practices](http://viget.com/extend/bundler-best-practices) said the following:
> 1. Always use bundle install  
> 1. If you need to upgrade a dependency that Bundler is already managing, use bundle update <gem>.  
> 1. Don't run bundle update unless you want all of your gems to be upgraded.

### Install Gems only for Development and Testing
```bash
$ bundle install --without production
```
> The --without production option prevents the local installation of any production gems, 
> which in this case is just pg. (Because the only gem we’ve added is restricted to a production 
> environment, right now this command doesn’t actually install any additional local gems, but it’s 
> needed to update Gemfile.lock since that’s what Heroku uses to infer the gem requirements of our application.)

### Start Local Web Server
```bash
$ rails server
=> Booting WEBrick
=> Rails application starting on http://0.0.0.0:3000
=> Call with -d to detach
=> Ctrl-C to shutdown server
```

### Heroku Realted Tasks
```bash
# login
$ heroku login
# create an application in heroku
$ heroku create
# push to heroku
$ git push heroku master
# open deployed heroku app in brower
$ heroku open
# rename a heroku app
$ heroku rename railstutorial
# run db migrate after push to heroku with db changes
$ heroku run rake db:migrate
```

### Use Direct IP Address For Heroku 
*This may be userful for Chinese users behind the GFW*

Put the following lines in /etc/ssh/ssh_config
```bash
Host heroku.com
User mingming
Hostname 107.21.95.3
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa
port 22
```
Before this, use the following command to detect which IP is working
```bash
$ ssh -v git@heroku.com
```
### Use Scaffolding to Generate Data Modules
```bash
$ rails generate scaffold User name:string email:string
$ rails generate scaffold Micropost content:string user_id:integer
$ bundle exec rake db:migrate
```
### List the Tasks of Rake
```bash
# List only the database related taskes
$ bundle exec rake -T db
# List all the tasks
$ bundle exec rake -T
```

### Add Validation for Data Models
```ruby
# app/models/micropost.rb
class Micropost < ActiveRecord::Base
  attr_accessible :content, :user_id
  validates :content, :length => { :maximum => 140 }
end
```
### Add Relationships Between Data Models
```ruby
# app/models/user.rb
class User < ActiveRecord::Base
  attr_accessible :email, :name
  has_many :microposts
end
```

```ruby
# app/models/micropost.rb
class Micropost < ActiveRecord::Base
  attr_accessible :content, :user_id

  belongs_to :user

  validates :content, :length => { :maximum => 140 }
end
```
### Play with Rails Console
```bash
$ rails console
>> first_user = User.first
=> #<User id: 1, name: "Michael Hartl", email: "michael@example.org",
created_at: "2011-11-03 02:01:31", updated_at: "2011-11-03 02:01:31">
>> first_user.microposts
=> [#<Micropost id: 1, content: "First micropost!", user_id: 1, created_at:
"2011-11-03 02:37:37", updated_at: "2011-11-03 02:37:37">, #<Micropost id: 2,
content: "Second micropost", user_id: 1, created_at: "2011-11-03 02:38:54",
updated_at: "2011-11-03 02:38:54">]
>> exit
```
### Class Inheritance
```ruby
class User < ActiveRecord::Base
  .
  .
  .
end
```
