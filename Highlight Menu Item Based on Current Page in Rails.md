1. Use Helper
    
    First, add a helper
    ```Ruby
    def is_active?(page_name)
      "active" if params[:action] == page_name
    end
    ```
    Then, in erb
    ```erb
    link_to 'Home', '/', :class => is_active?("index")
    link_to 'About', '/about', :class => is_active?("about")
    link_to 'contact', '/contact', :class => is_active?("contact")
    ```
2. Use Jquery
    ```javascript
    $(document).ready(function() {  
        $('.home-page-menu-item a').each(function() {
            if ($(this).attr('href') == $(location).attr('pathname')){   
                $(this).parent().addClass("active")     
            }
        }) 
    }) 
    ```
3. Use link_to_unless_current Rails Helper
    ```erb
    <ul>
        <li>
            <%= link_to_unless_current "Home", root_path do link_to "Home", root_path, :class => "current" end %>
        </li>
    </ul>
    ```
    
References

1. <http://capotej.com/post/52081481/highlight-link-based-on-current-page-in-rails/>
2. <http://api.rubyonrails.org/classes/ActionView/Helpers/UrlHelper.html#method-i-link_to_unless_current>
3. <http://www.liquidfoot.com/2010/09/28/add-highlight-link-to-current-page-in-rails/>
4. <http://mark.stratmann.me/content_items/simple-nav-menu-highlighting-in-rails-3-2-and-twitter-bootstrap>
