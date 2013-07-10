> # Naming Conventions
> 
> ## Ruby Naming Convention
> 
> Ruby uses the first character of the name to help it determine it’s intended use.
> 
> Local Variables
> underscores rather than camelBack for multiple word names, e.g. mileage, variable_xyz
> 
> Instance Variables
> lowercase letter should be used after the @, e.g. @colour 
> 
> Instance Methods
> letters, e.g. paint, close_the_door
> 
> Class Variables
> letters, e.g. @@colour
> 
> Constant 
> convention named using all uppercase letters and underscores between words, e.g. THIS_IS_A_CONSTANT
> 
> Class and Module 
>  module Encryption, class MixedCase
> 
> Global Variables
> Starts with a dollar ($) sign followed by other characters, e.g. $global
> 
> ## Rails Naming Convention
> 
> Rails use the same naming convention as Ruby with some additions:
> 
> Variable 
> order_amount, total
> 
> Class and Module 
> InvoiceItem
> 
> Database Table
> plural, e.g. invoice_items, orders
> 
> Model 
> multiple capitalised words, the table name is assumed to have underscores between these words.
> 
> Controller
> /app/controllers directory.
> 
> Files, Directories and other pluralization
> other conventions will apply:
> 
> directory
> Rails will look for view template files for the controller in the app/views/orders directory
> app/views/layouts directory
> be created in the /test/functional directory
> Primary Key
> The primary key of a table is assumed to be named id.
> 
> Foreign Key
> order_id in the items table where we have items linked to the orders table.
> 
> Many to Many Link Tables
> with the table names in alphabetical order, for example items_orders.
> 
> Automated Record Timestamps
> and time, use :created_on and :updated_on.
> 
> ## Naming Convention Summary 
> 
> ### Model Naming Convention
> 
> Table: orders  
> Class: Order  
> File: /app/models/order.rb  
> Primary Key: id  
> Foreign Key: customer_id  
> Link Tables: items_orders  
> 
> ### Controller Naming Convention
> 
> Class: OrdersController  
> File: /app/controllers/orders_controller.rb  
> Layout: /app/layouts/orders.html.erb  
> 
> ### View Naming Convention
> 
> Helper: /app/helpers/orders_helper.rb  
> Helper Module: OrdersHelper  
> Views: /app/views/orders/… (list.html.erb for example)  
> 
> ### Tests Naming Convention
> 
> Unit: /test/unit/order_test.rb  
> Functional: /test/functional/orders_controller_test.rb  
> Fixtures: /test/fixtures/orders.yml   


Reference: [Ruby and Rails Naming Conventions](http://itsignals.cascadia.com.au/?p=7)
