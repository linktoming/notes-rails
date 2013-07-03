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

# Start the server in production mode
$ rails server --environment production

# Before start the server in production mode, we may need to setup the database for production first
$ rake db:migrate RAILS_ENV=production
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
### Add Validation for Data Models
```ruby
# app/models/micropost.rb
class Micropost < ActiveRecord::Base
  attr_accessible :content, :user_id
  
  validates :content, :length => { :maximum => 140 }
end

class User < ActiveRecord::Base 
  attr_accessible :name, :email

  validates :name, presence: true, length: { maximum: 100 }
  VALID_EMAIL_REGEX = /\A[\w+\-.]+@[a-z\d\-.]+\.[a-z]+\z/i
  validates :email, presence: true, length: { maximum: 100 }, 
      format: { with: VALID_EMAIL_REGEX }, 
      uniqueness:  { case_sensitive: false }
end
```
Show Error Messages
```ruby
>> user.errors.full_messages
=> ["Name can't be blank"]
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
### Use Active Record Callbacks
```ruby
class User < ActiveRecord::Base 
  attr_accessible :name, :email

  before_save { |user| user.email = email.downcase }
  .
  .
  .
end
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
### Generarte Models with Attributes
```bash
$ rails generate model User name:string email:string is_admin:boolean
```
### Use Migration to Modify Model/DB Structure
Generate the migration first
```bash
$ rails generate migration add_index_to_users_email
```
Then manually add the changes to db/migrate/[timestamp]_add_index_to_users_email.rb
```ruby
class AddIndexToUsersEmail < ActiveRecord::Migration
  def change
    add_index :users, :email, unique: true
  end
end
```
Finally, run the migration command
```bash
$ rake db:migrate
```

Add collumn to table
```bash
# it’s convenient to end the name with _to_users, since in this case Rails automatically constructs 
# a migration to add columns to the users table. Moreover, by including the second argument, we’ve 
# given Rails enough information to construct the entire migration for us.
$ rails generate migration add_password_digest_to_users password_digest:string
```
Above command will generate the migration file automatically
```ruby
# db/migrate/[ts]_add_password_digest_to_users.rb
class AddPasswordDigestToUsers < ActiveRecord::Migration
  def change
    add_column :users, :password_digest, :string
  end
end
```
### Undoing things
```bash
$ rails destroy controller FooBars baz quux
$ rails destroy model Foo
$ rake db:rollback
$ rake db:migrate VERSION=0
$ rake db:reset
```
### Play with Rails Console
```bash
# the default rails environment is development
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

# use the sandbox environment in which any changes will be rollback after the session
$ rails console --sandbox
Loading development environment in sandbox
Any modifications you make will be rolled back on exit
>> 

# use test evironment for the console
$ rails console test
```

### List the Tasks of Rake
```bash
# List only the database related taskes
$ bundle exec rake -T db
# List all the tasks
$ bundle exec rake -T

# Rake task for reset the database
$ rake db:reset
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
### Speeding up tests with Spork
Add the spork gem dependency to the Gemfile
```Gemfile
source 'https://rubygems.org'

gem 'rails', '3.2.13'
.
.
.
group :development, :test do
  .
  .
  .
  gem 'guard-spork', '1.2.0'
  gem 'childprocess', '0.3.6'
  gem 'spork', '0.9.2'
end
.
.
.
```

Then install Spork using bundle install
```bash
$ bundle install
```

Next, bootstrap the Spork configuration
```bash
$ bundle exec spork --bootstrap
```
Before running Spork, we can get a baseline for the testing overhead by timing our test suite as follows
```bash
$ time bundle exec rspec spec/requests/static_pages_spec.rb
......

6 examples, 0 failures

real    0m8.633s
user	0m7.240s
sys	0m1.068s
```
Start a Spork server in a seperate terminal window in application root
```bash
$ spork
```
Run test and timing it with --drb option
```bash
$ time bundle exec rspec spec/requests/static_pages_spec.rb --drb
......

6 examples, 0 failures

real    0m2.649s
user	0m1.259s
sys	0m0.258s
```

Configuring RSpec to automatically use Spork
```bash
# Adding --drb option to the .rspec file in the application’s root directory
--colour
--drb
```
Arrange Guard with Spork
```bash
$ guard init spork
```
Update Guardfile for Spork
```bash
require 'active_support/core_ext'

guard 'spork', :rspec_env => { 'RAILS_ENV' => 'test' } do
  watch('config/application.rb')
  watch('config/environment.rb')
  watch(%r{^config/environments/.+\.rb$})
  watch(%r{^config/initializers/.+\.rb$})
  watch('Gemfile')
  watch('Gemfile.lock')
  watch('spec/spec_helper.rb')
  watch('test/test_helper.rb')
  watch('spec/support/')
end

guard 'rspec', :version => 2, :all_after_pass => false, :cli => '--drb' do
  .
  .
  .
```
With that configuration in place, we can start Guard and Spork at the same time with the guard command:
```bash
$ guard
```
### Use Active Record to CRUD Records
```ruby
>> User.new
=> #<User id: nil, name: nil, email: nil, created_at: nil, updated_at: nil>

>> user = User.new(name: "Michael Hartl", email: "mhartl@example.com")
=> #<User id: nil, name: "Michael Hartl", email: "mhartl@example.com",
created_at: nil, updated_at: nil>

>> user.save
=> true

>> user
=> #<User id: 1, name: "Michael Hartl", email: "mhartl@example.com",
created_at: "2011-12-05 00:57:46", updated_at: "2011-12-05 00:57:46">

>> user.name
=> "Michael Hartl"
>> user.email
=> "mhartl@example.com"
>> user.updated_at
=> Tue, 05 Dec 2011 00:57:46 UTC +00:00

>> User.create(name: "A Nother", email: "another@example.org")
#<User id: 2, name: "A Nother", email: "another@example.org", created_at:
"2011-12-05 01:05:24", updated_at: "2011-12-05 01:05:24">
>> foo = User.create(name: "Foo", email: "foo@bar.com")
#<User id: 3, name: "Foo", email: "foo@bar.com", created_at: "2011-12-05
01:05:42", updated_at: "2011-12-05 01:05:42">

>> foo.destroy
=> #<User id: 3, name: "Foo", email: "foo@bar.com", created_at: "2011-12-05
01:05:42", updated_at: "2011-12-05 01:05:42">

# the destroies object is still in memery
>> foo
=> #<User id: 3, name: "Foo", email: "foo@bar.com", created_at: "2011-12-05
01:05:42", updated_at: "2011-12-05 01:05:42">

>> User.find(1)
=> #<User id: 1, name: "Michael Hartl", email: "mhartl@example.com",
created_at: "2011-12-05 00:57:46", updated_at: "2011-12-05 00:57:46">

>> User.find_by_email("mhartl@example.com")
=> #<User id: 1, name: "Michael Hartl", email: "mhartl@example.com",
created_at: "2011-12-05 00:57:46", updated_at: "2011-12-05 00:57:46">

>> User.first
=> #<User id: 1, name: "Michael Hartl", email: "mhartl@example.com",
created_at: "2011-12-05 00:57:46", updated_at: "2011-12-05 00:57:46">

>> User.all
=> [#<User id: 1, name: "Michael Hartl", email: "mhartl@example.com",
created_at: "2011-12-05 00:57:46", updated_at: "2011-12-05 00:57:46">,
#<User id: 2, name: "A Nother", email: "another@example.org", created_at:
"2011-12-05 01:05:24", updated_at: "2011-12-05 01:05:24">]

# Updating user objects
>> user           # Just a reminder about our user's attributes
=> #<User id: 1, name: "Michael Hartl", email: "mhartl@example.com",
created_at: "2011-12-05 00:57:46", updated_at: "2011-12-05 00:57:46">
>> user.email = "mhartl@example.net"
=> "mhartl@example.net"
>> user.save
=> true

>> user.update_attributes(name: "The Dude", email: "dude@abides.org")
=> true
>> user.name
=> "The Dude"
>> user.email
=> "dude@abides.org"
```
### Rake Tasks for Preparing your Application for Testing
```bash
# Recreate the test database from the current environment's database schema
$ rake db:test:clone    
# Recreate the test database from the development structure
$ rake db:test:clone_structure	
# Recreate the test database from the current schema.rb
$ rake db:test:load	
# Check for pending migrations and load the test schema
$ rake db:test:prepare	
# Empty the test database.
$ rake db:test:purge	
```
### %w[] technique for making arrays of strings,
```ruby
>> %w[foo bar baz]
=> ["foo", "bar", "baz"]
>> addresses = %w[user@foo.COM THE_US-ER@foo.bar.org first.last@foo.jp]
=> ["user@foo.COM", "THE_US-ER@foo.bar.org", "first.last@foo.jp"]
>> addresses.each do |address|
?>   puts address
>> end
user@foo.COM
THE_US-ER@foo.bar.org
first.last@foo.jp
```

### Add Debug Information to the Site Layout
Add debug info in app/views/layouts/application.html.erb
```erb
<!DOCTYPE html>
<html>
  .
  .
  .
  <body>
    <%= render 'layouts/header' %>
    <div class="container">
      <%= yield %>
      <%= render 'layouts/footer' %>
      <%= debug(params) if Rails.env.development? %>
    </div>
  </body>
</html>
```
to make the debug info prettier, add the css to
app/assets/stylesheets/custom.css.scss
```scss
@import "bootstrap";

/* mixins, variables, etc. */

$grayMediumLight: #eaeaea;

@mixin box_sizing {
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
  box-sizing: border-box;
}
.
.
.

/* miscellaneous */

.debug_dump {
  clear: both;
  float: left;
  width: 100%;
  margin-top: 45px;
  @include box_sizing;
}
```
