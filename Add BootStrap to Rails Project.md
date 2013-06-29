1. Add less-rails-bootstrap and therubyracer to Gemfile
```ruby
gem 'less-rails-bootstrap'
gem 'therubyracer'
```
2. Require Bootstrap in app/assets/stylesheets/application.css
```ruby
*= require twitter/bootstrap
```
3. Require Bootstrap in app/assets/javascript/application.js
```bash
//= require twitter/bootstrap
```
### Reference
[Adding Twitter's Bootstrap CSS to your Rails app](http://www.opinionatedprogrammer.com/2011/11/twitter-bootstrap-on-rails/)
