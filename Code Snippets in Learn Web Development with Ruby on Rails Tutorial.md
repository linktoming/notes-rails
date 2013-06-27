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

    $ rails new app_name

    
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
```
