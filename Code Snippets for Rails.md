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
# tell rails not to generate test directory associated with the default Test::Unit framework
$ rails new sample_app --skip-test-unit
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

> This is a remembered option which means next round we may just use *bundle install* and it'll include 
> *--without production* automatically.

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
# check the logs in heroku
$ heroku logs
# check the state of the app’s dynos
$ heroku ps
=== web: `rails server -p $PORT -e $RAILS_ENV`
web.1: up for 5s
# scale heroku web dynos
$ heroku ps:scale web=2
# run rails console in heroku
$ heroku run rails console
Running `rails console` attached to terminal... up, run.2591
Loading production environment (Rails 4.0.0)
irb(main):001:0>
# run rake tasks in heroku
$ heroku run rake db:migrate
```
More details on [Getting Started with Rails 4.x on Heroku](https://devcenter.heroku.com/articles/rails4-getting-started)
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

### Change Gem Source Location
```bash
$ gem sources --remove https://rubygems.org/
$ gem sources -a http://ruby.taobao.org/
$ gem sources -l
*** CURRENT SOURCES ***

http://ruby.taobao.org
```
### Change Bundle Source Location for Bundle Install
```bash
# in Gemfile, replace the first line with
source 'http://ruby.taobao.org/'
```
### Configure Rails to Use RSpec in Place of Test::Unit
```bash
$ rails generate rspec:install
```
### Generate Controller with Actions
```bash
#  use the option --no-test-framework to suppress the generation of the default RSpec tests,
$ rails generate controller StaticPages home help --no-test-framework
      create  app/controllers/static_pages_controller.rb
       route  get "static_pages/help"
       route  get "static_pages/home"
      invoke  erb
      create    app/views/static_pages
      create    app/views/static_pages/home.html.erb
      create    app/views/static_pages/help.html.erb
      invoke  helper
      create    app/helpers/static_pages_helper.rb
      invoke  assets
      invoke    coffee
      create      app/assets/javascripts/static_pages.js.coffee
      invoke    scss
      create      app/assets/stylesheets/static_pages.css.scss
```
### Undoing things
```bash
$ rails destroy controller FooBars baz quux
$ rails destroy model Foo
$ rake db:rollback
$ rake db:migrate VERSION=0
```

### Generate and Run RSpec Integration Test
```bash
$ rails generate integration_test static_pages
      invoke  rspec
      create    spec/requests/static_pages_spec.rb
      
$ bundle exec rspec spec/requests/static_pages_spec.rb
```
### Sample of RSpecs
```ruby
# spec/requests/static_pages_spec.rb
require 'spec_helper'

describe "Static pages" do

  describe "Home page" do

    it "should have the h1 'Sample App'" do
      visit '/static_pages/home'
      page.should have_selector('h1', :text => 'Sample App')
    end

    it "should have the title 'Home'" do
      visit '/static_pages/home'
      page.should have_selector('title',
                        :text => "Ruby on Rails Tutorial Sample App | Home")
    end
  end

  describe "Help page" do

    it "should have the h1 'Help'" do
      visit '/static_pages/help'
      page.should have_selector('h1', :text => 'Help')
    end

    it "should have the title 'Help'" do
      visit '/static_pages/help'
      page.should have_selector('title',
                        :text => "Ruby on Rails Tutorial Sample App | Help")
    end
  end

  describe "About page" do

    it "should have the h1 'About Us'" do
      visit '/static_pages/about'
      page.should have_selector('h1', :text => 'About Us')
    end

    it "should have the title 'About Us'" do
      visit '/static_pages/about'
      page.should have_selector('title',
                    :text => "Ruby on Rails Tutorial Sample App | About Us")
    end
  end
end
```
### Rails' provide function in Erb
```erb
<% provide(:title,'Home')%>
<!DOCTYPE html>
<html>
  <head>
    <title>Ruby on Rails Tutorial Sample App | <%= yield(:title)%></title>
  </head>
  <body>
    <h1>Sample App</h1>
    <p>
      This is the home page for the
      <a href="http://railstutorial.org/">Ruby on Rails Tutorial</a>
      sample application.
    </p>
  </body>
</html>
```
### Use Guard to Automate Test
Add Guard and growl to Gemfile
```ruby
# Gemfile
group :development, :test do
  gem 'sqlite3'
  gem 'rspec-rails', '2.11.0'
  gem 'guard-rspec', '1.2.1'
end

group :test do
  gem 'capybara', '1.1.2'
  gem 'rb-fsevent', '0.9.1', :require => false
  gem 'growl', '1.0.3'
end

```

Initialize Guard so that it works with RSpec and Use it
```bash
$ bundle exec guard init rspec
Writing new Guardfile to /Users/mhartl/rails_projects/sample_app/Guardfile
rspec guard added to Guardfile, feel free to edit it
$ bundle exec guard
```


