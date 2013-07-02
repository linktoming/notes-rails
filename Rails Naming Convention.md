```
Model Naming Convention

Table: orders
Class: Order
File: /app/models/order.rb
Primary Key: id
Foreign Key: customer_id
Link Tables: items_orders

Controller Naming Convention

Class: OrdersController
File: /app/controllers/orders_controller.rb
Layout: /app/layouts/orders.html.erb

View Naming Convention

Helper: /app/helpers/orders_helper.rb
Helper Module: OrdersHelper
Views: /app/views/orders/… (list.html.erb for example)

Tests Naming Convention

Unit: /test/unit/order_test.rb
Functional: /test/functional/orders_controller_test.rb
Fixtures: /test/fixtures/orders.yml
```

Reference: [Ruby and Rails Naming Conventions](http://itsignals.cascadia.com.au/?p=7)
