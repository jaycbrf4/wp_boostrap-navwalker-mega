wp-bootstrap-navwalker-Mega
======================

**A custom WordPress nav walker class to fully implement the Bootstrap 3.0+ navigation style with Mega Menu items in a custom theme using the WordPress built in menu manager.**

http://prntscr.com/h7206f <- screenshot of menu

http://prntscr.com/h720sy < - screenshot of widgets screen

http://prntscr.com/h721hh <- screenshot of menu screen


This version adds the support for multi level dropdowns with Mega Menu type nav items

NOTE
----
This is a utility class that is intended to format your WordPress theme menu with the correct syntax and classes to utilize the Bootstrap dropdown navigation, and does not include the required Bootstrap JS files. You will have to include them manually. 

Installation
------------
Place **wp_bootstrap-mega-navwalker.php** in your WordPress theme folder `/wp-content/your-theme/`

Open your WordPress themes **functions.php** file  `/wp-content/your-theme/functions.php` and add the following code:


		// Register Custom Navigation Walker
		require_once('wp_bootstrap-mega-navwalker.php');


Usage
------------
Update your `wp_nav_menu()` function in `header.php` to use the new walker by adding a "walker" item to the wp_nav_menu array.

```php
 <?php
          $args = array(
		  'theme_location' => 'mega_menu',
		  'depth' => 0,
		  'menu_class'  => 'nav navbar-nav',
		  'walker'  => new BootstrapNavMenuWalker()
          );
    wp_nav_menu($args);
        ?>
```

Your menu will now be formatted with the correct syntax and classes to implement Bootstrap dropdown navigation. 

You will also want to declare your new menu in your `functions.php` file.


	register_nav_menus( array(
	'mega_menu'   => __( 'Mega Menu', 'your-theme' ),
	) );

Now you will need to add the Widget that will contain the mega-menu item's HTML. In your functions file add this code to the register sidebars function:

    //register MegaMenu widget if the Mega Menu is set as the menu location
    $location = 'mega_menu';
    $css_class = 'mega-menu-parent';
    $locations = get_nav_menu_locations();
    if ( isset( $locations[ $location ] ) ) {
      $menu = get_term( $locations[ $location ], 'nav_menu' );
      if ( $items = wp_get_nav_menu_items( $menu->name ) ) {
        foreach ( $items as $item ) {
          if ( in_array( $css_class, $item->classes ) ) {
            register_sidebar( array(
              'id'   => 'mega-menu-item-' . $item->ID,
              'description' => 'Mega Menu items',
              'name' => $item->title . ' - Mega Menu',
              'before_widget' => '<li id="%1$s" class="mega-menu-item">',
              'after_widget' => '</li>', 

            ));
          }
        }
      }
    }
    
    
Typically the menu is wrapped with additional markup, here is an example of a ` navbar-fixed-top` menu that collapse for responsive navigation.


	<nav class="navbar navbar-inverse navbar-fixed-top">
	  <div class="container">
		<div class="navbar-header">
		       <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#bootstrap-nav-collapse">
				<span class="sr-only">Toggle navigation</span>
				<span class="icon-bar"></span>
				<span class="icon-bar"></span>
				<span class="icon-bar"></span>
			</button>

	 <a class="navbar-brand" id="logo" href="<?php echo site_url(); ?>"><img src="<?php header_image(); ?>" height="<?php echo get_custom_header()->height; ?>" width="<?php echo get_custom_header()->width; ?>" alt="" class="img-responsive logo"/></a>

		</div><!-- /. navbar-header -->
		    <!-- Collect the nav links from WordPress -->
	  <div class="collapse navbar-collapse" id="bootstrap-nav-collapse">         
	  <?php $args = array(
		  'theme_location' => 'mega_menu',
		  'depth' => 0,
		  'menu_class'  => 'nav navbar-nav',
		  'walker'  => new BootstrapNavMenuWalker()
		  );
	    wp_nav_menu($args);
	  ?>
	  </div><!-- ./collapse -->
		  </div><!-- /.container -->
		</nav>




Displaying the Menu 
-------------------
To display the menu you must associate your menu with your theme location. You can do this by selecting your theme location in the *Theme Locations* list wile editing a menu in the WordPress menu manager and selecting mega-menu.

Displaying the Mega Menu
-------------------
To display the mega menu items add the class "mega-menu-parent" to the menu item in the WordPress menu manager. This will create a new widget in the widgets section with the same name as the nav parent. Add a Custom HTML widget and code your mega menu items using default bootstrap containers, rows and cols. Add a search widget or any other widget, style the CSS by the item's ID.

Additional CSS to get the menu to display correctly:

	/**Mega Menu **/
	.navbar .container {
	    position: relative;
	}
	.navbar-nav, .navbar .collapse, .navbar-nav li {
	  position: static;
	}
	.menu-item-has-children, .menu-item-has-children .dropdown-menu {
	  left: auto;
	}
	.menu-item-has-children .dropdown-menu {
	  width: auto !important;
	}

And optional css for a hover/fade inUp effect

	@media (min-width: 767px) {
    /** 
    make dropdown active on hover and fade in - 
    change to 992px for alt breakpoint and swap bootstrap.css files 
    **/

	  ul.nav li.dropdown > ul.dropdown-menu{
	    visibility:hidden;
	    display:block;
	    opacity:0;
	    margin-top: 10px !important;
	    transition: all  0.5s ease-in-out;
	  }
	  ul.nav li.dropdown:hover > ul.dropdown-menu{
	    visibility:visible;
	    opacity: 1;
	    display: block;
	    margin-top: 0 !important; 
	    border-bottom-right-radius: 16px;
	    border-bottom-left-radius: 16px;
	  } 


Based on WPBootstrapNavwalker by by johnmegahan https://gist.github.com/1597994, Emanuele 'Tex' Tessore https://gist.github.com/3765640

