# WPCustomDashboardByRole
Customized Dashboard by User role

Plugin to user for creating user role : **Member**

Incase remove menu function is not available for particular role in wp
```PHP
 // Hook into the 'admin_menu' action to forcefully remove menu items based on user role
 add_action('admin_footer', 'force_remove_plugin_menu_by_role', 999);
 function force_remove_plugin_menu_by_role() 
 {
  
   $current_user = wp_get_current_user();
   
   if(in_array( 'sm_success_manager', (array) $current_user->roles )) {  ?>
       
    <script>
     
        function removeElementByRole( id_target ) { document.getElementById(id_target).remove(); }   

        // Media Bulk 
        removeElementByRole('menu-media');
        // Logo Slider
        removeElementByRole('menu-posts-gs-logo-slider');
        // Tools 
        //removeElementByRole('toplevel_page_mfs-scheduling');
        // Memebers 
        removeElementByRole('toplevel_page_members');
        // Divi
        removeElementByRole('toplevel_page_et_divi_options');
        // Settings 
        removeElementByRole('menu-settings');

    </script>
    
 <?php } 
 
 }

```

Get current WP Admin URL 

```PHP

 function wp_mfs_get_current_url() 
 {
    // Get the protocol (HTTP or HTTPS)
    $protocol = (!empty($_SERVER['HTTPS']) && $_SERVER['HTTPS'] !== 'off') ? 'https' : 'http';
    
    // Get the server name and port
    $server_name = $_SERVER['SERVER_NAME'];
    $server_port = $_SERVER['SERVER_PORT'];
    
    // Get the request URI (path + query string)
    $request_uri = $_SERVER['REQUEST_URI'];
    
    // Construct the full URL
    $url = $protocol . '://' . $server_name;
    
    // Include the port if it’s not the default for the protocol
    if (($protocol === 'http' && $server_port !== '80') || ($protocol === 'https' && $server_port !== '443')) {
        $url .= ':' . $server_port;
    }
    
    $url .= $request_uri;
    
    return $url;
 }

```

Redirect to admin aside to these URL in array

```PHP

 add_action('admin_init', 'custom_admin_redirect');
 function custom_admin_redirect() 
 {
    // Check if the current user is an admin
    if (is_user_logged_in() && current_user_can('manage_options')) {
        // Define the current page slug and the target page slug
        $current_page = wp_mfs_get_current_url() ;
        $user         = wp_get_current_user();
        
        // Only these URL in array can access of this user_role_name!
         $urls_to_check = array(
            admin_url(),                      // Base admin URL
            admin_url('admin.php?page=mfs-scheduling-entry'), 
            admin_url('admin.php?page=mfs-scheduling-done'),
            admin_url('profile.php'),
            admin_url('users.php?page=user_verification')
         );
       
        // else redirect to admin dashboard if attemp to access other page!
        if (in_array('user_role_name', (array) $user->roles) && !in_array($current_page, $urls_to_check) ) {
           wp_redirect(admin_url());
          exit;
        }
   
    }
 }

```
