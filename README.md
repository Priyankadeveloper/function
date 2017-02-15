add_action( 'init', 'codex_our_services' );

function codex_our_services() {
 $labels = array(
  'name'               => _x( 'Our Services', 'post type general name', 'your-plugin-textdomain' ),
  'singular_name'      => _x( 'Our Services', 'post type singular name', 'your-plugin-textdomain' ),
  'menu_name'          => _x( 'Our Services', 'admin menu', 'your-plugin-textdomain' ),
  'name_admin_bar'     => _x( 'Our Services', 'add new on admin bar', 'your-plugin-textdomain' ),
  'add_new'            => _x( 'Add New', 'Our Services', 'your-plugin-textdomain' ),
  'add_new_item'       => __( 'Add New Our Services', 'your-plugin-textdomain' ),
  'new_item'           => __( 'New Our Services', 'your-plugin-textdomain' ),
  'edit_item'          => __( 'Edit Our Services', 'your-plugin-textdomain' ),
  'view_item'          => __( 'View Our Services', 'your-plugin-textdomain' ),
  'all_items'          => __( 'All Our Services', 'your-plugin-textdomain' ),
  'search_items'       => __( 'Search Our Services', 'your-plugin-textdomain' ),
  'parent_item_colon'  => __( 'Parent Our Services:', 'your-plugin-textdomain' ),
  'not_found'          => __( 'No Our Services found.', 'your-plugin-textdomain' ),
  'not_found_in_trash' => __( 'No Our Services found in Trash.', 'your-plugin-textdomain' )
 );

 $args = array(
  'labels'             => $labels,
                'description'        => __( 'Description.', 'your-plugin-textdomain' ),
  'public'             => true,
  'publicly_queryable' => true,
  'show_ui'            => true,
  'show_in_menu'       => true,
  'query_var'          => true,
  'rewrite'            => array( 'slug' => 'ourservices' ),
  'capability_type'    => 'post',
  'has_archive'        => true,
  'hierarchical'       => false,
  'menu_position'      => null,
  'supports'           => array( 'title', 'editor', 'author', 'thumbnail', 'excerpt', 'comments' )
 );

 register_post_type( 'ourservices', $args );
}

add_action( 'init', 'our_state_categories' );

function our_state_categories() {
 register_taxonomy(
  'servicescate',
  'ourservices',
  array(
   'label' => __( 'Categories' ),
   'rewrite' => array( 'slug' => 'servicecate' ),
   'hierarchical' => true,
  )
 );
}
