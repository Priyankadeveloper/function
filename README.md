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
------------------------------------
use custom field
-------------------------------------
<?php
if( !SLZSUNHOUSE_CORE_IS_ACTIVE){
	return;
}

get_header();

$html_options = array(
	'info_format'      => '<div class="property-info">%1$s</div>',
	'status_format'    => '<div class="info %2$s"><div class="type"><i class="'.esc_attr(SlzCore::get_theme_icons('status')).'"></i><span>%1$s</span></div></div>',
	'price_format'     => '<div class="info"><div class="price"><i class="'.esc_attr(SlzCore::get_theme_icons('price')).'"></i><span class="price-num">%1$s</span><span class="price-of-rent">%3$s%2$s</span></div></div>',
	'area_format'      => '<div class="info"><div class="acr"><i class="'.esc_attr(SlzCore::get_theme_icons('area')).'"></i><span>%1$s</span></div></div>',
	'edit_format'      => '<div class="info"><a href="%1$s" class="edit"><i class="fa fa-pencil"></i><span>'.esc_html__('edit', 'sunhouse').'</span></a></div>',
	'postfix_sep'      => '/',
);



while ( have_posts() ) :
	$model = new SlzCore_Property( $html_options );
	
	the_post();
	if ( post_password_required() ) {
		get_template_part( 'inc/content-password' );
		wp_reset_query();
		return;
	}
	$model->loop_index();
	$json_map = $model->get_single_map_location();
	$show_agent = $model->post_meta['show_agent'];
	$agent_slug = $model->post_meta['agent'];
	if( $show_agent && !empty( $agent_slug ) ) {
		$show_agent = true;
	} else {
		$show_agent = false;
	}
	//var_dump($model);
	// css to show/hide sidebar.
	$all_container_css = slzsunhouse_get_container_css( $show_agent );
	extract($all_container_css);
	if( $show_agent ) {
		$sidebar_css .= ' margin-top-2';
	}
	$attach = $model->get_single_attachments($model->post_meta['attachment_ids']);
?>
<div class="section nav-bar">
	<div class="nav-bar-wrapper">
		<div class="<?php echo esc_attr( $container_css ); ?>">
			<div class="row">
				<div class="<?php echo esc_attr( $content_css ); ?>">
					<ul class="detail-nav list-unstyled">
						
                                               <li class="nav-item description-btn">
							<figure><i class="<?php echo esc_attr(SlzCore::get_theme_icons('description'));?> sh-icon"></i>
								<figcaption><?php esc_html_e('Description', 'sunhouse');?></figcaption>
							</figure>
						</li>

						<li class="nav-item condition-btn">
							<figure><i class="<?php echo esc_attr(SlzCore::get_theme_icons('condition'));?> sh-icon"></i>
								<figcaption><?php esc_html_e('Key Details', 'sunhouse');?></figcaption>
							</figure>
						</li>
						
						
						<li class="nav-item" id="Flat-Sizes-list">
							<a href="#Flat-Sizes"><figure><i class="fa fa-home sh-icon" aria-hidden="true"></i>
								<figcaption>
<?php esc_html_e('
Flat Sizes', 'sunhouse');?></figcaption>
							</figure></a>
						</li>

                                                <li class="nav-item" id="Building-Amenities-list">
							<a href="#Building-Amenities"><figure><i class="fa fa-building sh-icon" aria-hidden="true"></i>

								<figcaption><?php esc_html_e('Building Amenities', 'sunhouse');?></figcaption>
							</figure></a>
						</li>


                                                <li class="nav-item" id="Floor-Plans-list">
							<a href="#Floor-Plans"><figure><i class="fa fa-home sh-icon" aria-hidden="true"></i>
								<figcaption><?php esc_html_e('Floor Plans', 'sunhouse');?></figcaption>
							</figure></a>
						</li>

                                                

                                                <li class="nav-item" id="Amenities-list">
							<a href="#Amenities"><figure><i class="fa fa-home sh-icon" aria-hidden="true"></i>
								<figcaption><?php esc_html_e('Amenities in the flat', 'sunhouse');?></figcaption>
							</figure></a>
						</li>

                                                <li class="nav-item" id="Vicinity-list">
							<a href="#Vicinity"><figure><i class="fa fa-home sh-icon" aria-hidden="true"></i>
								<figcaption><?php esc_html_e('Vicinity', 'sunhouse');?></figcaption>
							</figure></a>
						</li>

                                                <li class="nav-item" id="Location-list">
							<a href="#Location"><figure><i class="<?php echo esc_attr(SlzCore::get_theme_icons('location'));?> sh-icon"></i>
								<figcaption><?php esc_html_e('Location Plan', 'sunhouse');?></figcaption>
							</figure></a>
						</li>

                                               

						<?php //do_action('slzsunhouse_property_favorites');?>
					</ul>
				</div>
			</div>
		</div>
	</div>
</div>

<?php 
$termslists=wp_get_post_terms(get_the_ID(),'slz_property_type',array('fields'=>'all'));
foreach($termslists as $termsids){$termslugs=$termsids->slug; 
if($termslugs=='past-projects'){$flag=0; } else {$flag=1;}
}

?>

<div id="page-content" class="section property-detail">
	<div class="<?php echo esc_attr( $container_css ); ?>">
		<div id="post-<?php the_ID(); ?>" class="detail-wrapper row">
			<div class="<?php echo esc_attr( $content_css ); ?> detail-content">
				<?php $model->get_single_info();?>

				<!-- gallery -->
				<div class="col-md-12 title"><?php esc_html_e('image gallery', 'sunhouse');?></div>
				<div class="row gallery">
					<div class="col-md-12 gallery-item">
						<?php $model->get_single_gallery();?>
					</div>
				</div>
				
				<!-- Description -->
				<div class="col-md-12 title description-title"><?php esc_html_e('About Project', 'sunhouse');?></div>
				<div id="description" class="row description">
					<div class="col-md-12 des-text entry-content">
						<?php
						the_content( sprintf( '<a href="%s"><button class="btn-readmore">%s</button></a>',
								get_permalink(),
								esc_html__( 'Read more', 'sunhouse' )
						) );
						wp_link_pages( array( 'before' => '<div class="page-links"><span class="page-links-title">' . esc_html__( 'Pages:', 'sunhouse' ) . '</span>', 'after' => '</div>', 'link_before' => '<span>', 'link_after' => '</span>' ) );
						?>
					</div>
				</div>
				

								
				<?php $vicinity1=get_field('plot_area'); 
				if($vicinity){
				?>
				<div class="col-md-12 detail-content floorplans">
						<div class="col-md-12 title amenities-title">Key Details</div>
						<div class="vicinity_span"></div>

						<ul id="condition" class="list-unstyled list-inline condition">
						<li class="condition-item"><div class="info"><i class="fa fa-arrows-alt"></i><span> 
						<?php echo $vicinity2=get_field('plot_area'); ?></span></div></li>
						<div class="spacer"></div>
						<li class="condition-item"><div class="info"><i class="fa fa-map-marker sh-icon"></i>
						<span class="location"><?php echo $vicinity_train_icon_text=get_field('location');; ?></span></div></li>
						<div class="spacer"></div>

						<li class="condition-item"><div class="info"><i class="fa fa-money"></i><span>
						<?php echo $vicinity_train_icon_text=get_field('banks_approved'); ?></span></div></li>
						<div class="spacer"></div>
						

						
<li class="condition-item"><div class="info"><i class="fa fa-building-o" aria-hidden="true"></i><span>
						<?php echo $vicinity_train_icon_textcus=get_field('current_status'); ?></span></div></li>
						<div class="spacer"></div>
			<li class="condition-item"><div class="info"><i class="fa fa-home" aria-hidden="true"></i><span>			
<!--<span class="flates">--><?php echo $vicinity_train_icon_textflates=get_field('no_of_flats');; ?></span></div>
</li>
<div class="spacer">
</div>
<li class="condition-item">
<div class="info">
<i class="fa fa-home" aria-hidden="true"></i>
<span class="floor"><?php echo $vicinity_train_icon_textfloor=get_field('no_of_floors');; ?></span>
</div>
</li>
</span>
</div>

</ul>
<div class="spacer">
</div>
				<?php } ?>
				<?php if($flag!=0) { ?>
<!----------------Flat Sizes-------------------------->
				<?php $vicinityflatsize=get_field('flat_sizes'); 
				if($vicinityflatsize){
				?>

<div class="col-md-12 detail-content floorplans" style="" id="Flat-Sizes">
						<div class="col-md-12 title amenities-title" style="margin-bottom: 20px !important;">Flat Sizes</div>
						

						<ul id="condition" class="list-unstyled list-inline condition">
						<li class="condition-item"><div class="info"><i class="fa fa-home" aria-hidden="true"></i><span> 
						<?php echo $vicinityflatsizes=get_field('flat_sizes'); ?></span></div></li>
						<div class="spacer"></div>
						<li class="condition-item"><div class="info"><i class="fa fa-home" aria-hidden="true"></i><!--<i class="fa fa-map-marker sh-icon"></i>-->
						<span class="location"><?php echo $vicinity_train_icon_text2bhk=get_field('2.5_bhk');; ?></span></div></li>
						<div class="spacer"></div>
						<li class="condition-item"><div class="info"><i class="fa fa-home" aria-hidden="true"></i><!--<i class="fa fa-map-marker sh-icon"></i>-->
						<span class="location"><?php echo $vicinity_train_icon_text3bhk=get_field('3_bhk');; ?></span></div></li>
						<div class="spacer"></div>
						<li class="condition-item"><div class="info"><i class="fa fa-home" aria-hidden="true"></i><!--<i class="fa fa-map-marker sh-icon"></i>-->
						<span class="location"><?php echo $vicinity_train_icon_textsqft=get_field('1206_sq_ft');; ?></span></div></li>
						<div class="spacer"></div>
						<?php } } ?>
				
				<!-- Building amenities -->
		<div class="col-md-12 title amenities-title" style="margin-bottom: 30px !important; padding-top: 90px !important;" id="Building-Amenities"><?php esc_html_e('Building Amenities', 'sunhouse');?></div>
				<?php
				//echo "testings".$post->ID;
				$args = array('orderby' => 'name', 'order' => 'ASC', 'fields' => 'all');
				$term_list = wp_get_post_terms($post->ID, 'slz_property_amenity', $args);
				//var_dump($term_list);
				foreach($term_list as $amenitieslists){  // var_dump($amenitieslists); ?>
					<ul id="amenities" class="list-unstyled list-inline amenities">
						<li class="amenities-item">
						<i class="fa fa-check-square"></i>
						<span><?php echo $amenitieslists->name; ?></span>
						</li>
					</ul>
				<?php }
					
				?>
				
				<!-- Floor Plans -->
				<?php if($flag!=0) { ?>
				<?php 

					$floora = get_field('floora');
					$floorb = get_field('floorb');
					$squarefeet=get_field('floor_a_areas');
					$bedrooms=get_field('floor_a_bedrooms');
					$kitchens=get_field('floor_a__kitchens');
					
					$squarefeetb=get_field('floor_b__bedroom');
					$bedroomsb=get_field('floor_b__bathrooms');
					$kitchensb=get_field('floor_b__kitchens');
					
					
					if( !empty($floora) ): ?>
						<div class="col-md-12 detail-content floorplans" id="Floor-Plans">
						<div class="col-md-12 title amenities-title" style="margin-bottom: 50px !important;">Floor Plans</div>
						<div class="floor_tabs">
						<a href="javascript:void(0);" class="floorsa"><span>2.5 BHK & 3 BHK</span></a><br/>
						<a href="javascript:void(0);" class="floorsb"><span>2 BHK & 3 BHK</span></a><br/><br/>
						</div>
						<div class="col-md-12 floora_desc"><img src="<?php echo $floora['url']; ?>" alt="<?php echo $floora['alt']; ?>" /><br/><p class="floora_speci"><i class="icon-house_1"></i> <span><?php echo $squarefeet;?></span> <i class="icon-bed"></i> <span><?php echo $bedrooms;?>  - Bedrooms</span> <i class="icon flaticon-old"></i> <span><?php echo $kitchens;?>  - Kitchen</span></p></div>
						<div class="col-md-12 floorb_desc"><img src="<?php echo $floorb['url']; ?>" alt="<?php echo $floorb['alt']; ?>" /><br/><p class="floorb_speci"><i class="icon-house_1"></i> <span><?php echo $squarefeetb;?></span> <i class="icon-bed"></i> <span><?php echo $bedroomsb;?>   - Bedrooms</span> <i class="icon flaticon-old"></i><span><?php echo $kitchensb;?>  - Kitchen</span></p></div>
						</div>
						
					<?php endif; } ?>
				<strong>Contact Person Details</strong>

<p><label><div class="col-sm-12 col-md-12 col-lg-12 col-xs-12 div-name"><div class="col-sm-5 col-md-5 col-lg-5 col-xs-12">Name<span style="color:red;">*</span></div><div class="col-sm-7 col-md-7 col-lg-7 col-xs-12">[text* your-name placeholder ""]</div></div></label></p>


<p><label><div class="col-sm-12 col-md-12 col-lg-12 col-xs-12 div-Mobile"><div class="col-sm-5 col-md-5 col-lg-5 col-xs-12">Telephone<span style="color:red;">*</span></div><div class="col-sm-7 col-md-7 col-lg-7 col-xs-12">[number number-262]</div></div></label></p>

<p><label><div class="col-sm-12 col-md-12 col-lg-12 col-xs-12 div-email"><div class="col-sm-5 col-md-5 col-lg-5 col-xs-12">Email Id<span style="color:red;">*</span></div><div class="col-sm-7 col-md-7 col-lg-7 col-xs-12">[email* your-email placeholder ""]</div></div></label></p> 

<div class="title"><strong>Society / Plot Owner Details</strong></div>


<p><label><div class="col-sm-12 col-md-12 col-lg-12 col-xs-12 div-budget"><div class="col-sm-5 col-md-5 col-lg-5 col-xs-12">Society / Owner Name	<span style="color:red;"></span></div><div class="col-sm-7 col-md-7 col-lg-7 col-xs-12">[number* Budget placeholder ""]</label></div></div></p>

<p><label><div class="col-sm-12 col-md-12 col-lg-12 col-xs-12 div-projects"><div class="col-sm-5 col-md-5 col-lg-5 col-xs-12">Address<span style="color:red;"></span></div><div class="col-sm-7 col-md-7 col-lg-7 col-xs-12">
[text text-103]</label></div></div></p>

<p><label><div class="col-sm-12 col-md-12 col-lg-12 col-xs-12 div-projects"><div class="col-sm-5 col-md-5 col-lg-5 col-xs-12">City<span style="color:red;"></span></div><div class="col-sm-7 col-md-7 col-lg-7 col-xs-12">[text City]</label></div></div></p>

<p><label><div class="col-sm-12 col-md-12 col-lg-12 col-xs-12 div-projects"><div class="col-sm-5 col-md-5 col-lg-5 col-xs-12">ZipCode<span style="color:red;"></span></div><div class="col-sm-7 col-md-7 col-lg-7 col-xs-12">[text ZipCode]</label></div></div></p>



<p><label><div class="col-sm-12 col-md-12 col-lg-12 col-xs-12 div-projects"><div class="col-sm-5 col-md-5 col-lg-5 col-xs-12">Landmark-ifany<span style="color:red;"></span></div><div class="col-sm-7 col-md-7 col-lg-7 col-xs-12">[text Landmark-ifany]</label></div></div></p>

<p><label><div class="col-sm-12 col-md-12 col-lg-12 col-xs-12 div-projects"><div class="col-sm-5 col-md-5 col-lg-5 col-xs-12">CTSNumber<span style="color:red;"></span></div><div class="col-sm-7 col-md-7 col-lg-7 col-xs-12">[text CTSNumber]</label></div></div></p>

<p><label><div class="col-sm-12 col-md-12 col-lg-12 col-xs-12 div-projects"><div class="col-sm-5 col-md-5 col-lg-5 col-xs-12">Village<span style="color:red;"></span></div><div class="col-sm-7 col-md-7 col-lg-7 col-xs-12">[text Village]</label></div></div></p>

<p><label><div class="col-sm-12 col-md-12 col-lg-12 col-xs-12 div-projects"><div class="col-sm-5 col-md-5 col-lg-5 col-xs-12">Area of plot (in sq m)<span style="color:red;">*</span></div><div class="col-sm-7 col-md-7 col-lg-7 col-xs-12">[text Area]</label></div></div></p>

<div class="title"><strong>Total number of members</strong><span style="color:red;">*</span></div>

<p><label><div class="col-sm-12 col-md-12 col-lg-12 col-xs-12 div-projects"><div class="col-sm-5 col-md-5 col-lg-5 col-xs-12">a)Residential<span style="color:red;">*</span></div><div class="col-sm-7 col-md-7 col-lg-7 col-xs-12">[text Residential]</label></div></div></p>

<p><label><div class="col-sm-12 col-md-12 col-lg-12 col-xs-12 div-projects"><div class="col-sm-5 col-md-5 col-lg-5 col-xs-12">b)Commercial<span style="color:red;">*</span></div><div class="col-sm-7 col-md-7 col-lg-7 col-xs-12">[text Commercial]</label></div></div></p>

<p><label><div class="col-sm-12 col-md-12 col-lg-12 col-xs-12 div-projects"><div class="col-sm-5 col-md-5 col-lg-5 col-xs-12">c)Shops<span style="color:red;">*</span></div><div class="col-sm-7 col-md-7 col-lg-7 col-xs-12">[text Shops]</label></div></div></p>


<div class="title"><strong>Total Carpet area of tenaments of existing members / tenants / owners</strong><span style="color:red;"> *</span></div>


<p><label><div class="col-sm-12 col-md-12 col-lg-12 col-xs-12 div-projects"><div class="col-sm-5 col-md-5 col-lg-5 col-xs-12">a)Residential<span style="color:red;">*</span></div><div class="col-sm-7 col-md-7 col-lg-7 col-xs-12">[text Residentiale]</label></div></div></p>

<p><label><div class="col-sm-12 col-md-12 col-lg-12 col-xs-12 div-projects"><div class="col-sm-5 col-md-5 col-lg-5 col-xs-12">b)Commercial<span style="color:red;">*</span></div><div class="col-sm-7 col-md-7 col-lg-7 col-xs-12">[text Commerciale]</label></div></div></p>

<p><label><div class="col-sm-12 col-md-12 col-lg-12 col-xs-12 div-projects"><div class="col-sm-5 col-md-5 col-lg-5 col-xs-12">c)Shops<span style="color:red;">*</span></div><div class="col-sm-7 col-md-7 col-lg-7 col-xs-12">[text Shopse]</label></div></div></p><br/>

<p><label><div class="col-sm-12 col-md-12 col-lg-12 col-xs-12 div-projects div-projects1"><div class="col-sm-5 col-md-5 col-lg-5 col-xs-12">Whether conveyance has been executed in the favour of society?
(Applicable only for Societies)<span style="color:red;">*</span></div><div class="col-sm-7 col-md-7 col-lg-7 col-xs-12">[radio radio-801 default:1 "Yes" "No"]</label></div></div></p>


<p><label><div class="col-sm-12 col-md-12 col-lg-12 col-xs-12 div-projects div-projects1"><div class="col-sm-5 col-md-5 col-lg-5 col-xs-12">Do you have a copy of previous approved plan?<span style="color:red;">*</span></div><div class="col-sm-7 col-md-7 col-lg-7 col-xs-12">[radio radio-803 default:1 "Yes" "No"]</label></div></div></p>


<p><label><div class="col-sm-12 col-md-12 col-lg-12 col-xs-12 div-projects div-projects1"><div class="col-sm-5 col-md-5 col-lg-5 col-xs-12">In case of a society, whether resolution for re-development has been passed?<span style="color:red;">*</span></div><div class="col-sm-7 col-md-7 col-lg-7 col-xs-12">[radio radio-805 default:1 "Yes" "No"]</label></div></div></p>

<p><label><div class="col-sm-12 col-md-12 col-lg-12 col-xs-12 div-enquiry"><div class="col-sm-5 col-md-5 col-lg-5 col-xs-12">Comments<span style="color:red;"></span></div><div class="col-sm-7 col-md-7 col-lg-7 col-xs-12">[textarea Comments placeholder ""]</label></div></div></p>

<div class="col-sm-12 col-md-12 col-lg-12 col-xs-12 submits">
<div class="col-sm-5 col-md-5 col-lg-5 col-xs-12"></div>
<div class="col-sm-7 col-md-7 col-lg-7 col-xs-12" style="padding: 0px;">[submit "submit"][submit "Reset"]</div></div>






<!-- Flat amenities -->
				<?php if($flag!=0) { ?>
				<?php
				//echo $post->ID;
				$args = array('orderby' => 'name', 'order' => 'ASC', 'fields' => 'all');
				$term_lists = wp_get_post_terms($post->ID, 'flatamenities', $args); 
				//var_dump($term_lists); 
				if($term_lists){
				?>
				<div class="col-md-12 title amenities-title" style="margin-bottom: 30px !important; padding-top: 90px !important;" id="Amenities"><?php esc_html_e('Amenities in the flat', 'sunhouse');?></div>
				<?php 
				foreach($term_lists as $flooramenitieslists){  // var_dump($amenitieslists); ?>
					<ul id="amenities" class="list-unstyled list-inline amenities">
						<li class="amenities-item">
						<i class="fa fa-check-square"></i>
						<span><?php echo $flooramenitieslists->name; ?></span>
						</li>
					</ul>
				<?php }
				} }
				?>
								
				<!-- Vicinity -->
				<?php if($flag!=0) { ?>
				<?php $vicinityflats=get_field('vicinity_train_content'); 
				if($vicinityflats){
				?>
				<div class="col-md-12 detail-content floorplans" id="Vicinity">
						<div class="col-md-12 title amenities-title">Vicinity</div>
						<div class="vicinity_span"></div>
						<?php echo $vicinity=get_field('vicinity_train_content'); ?>
						<div class="spacer"></div><div class="trainicon">
						<i class="fa fa-train" aria-hidden="true"></i>
<span class="train"><?php echo $vicinity_train_icon_text=get_field('vicinity_train_icon_text');; ?></span>
</div>						
												<div class="spacer"></div>
						<div class="wroad">
						<i class="fa fa-car" aria-hidden="true"></i>						
						<span class="road"><?php echo $vicinity_train_icon_text=get_field('road/car_');; ?></span></div>
						<div class="spacer"></div>
						<div class="wrelgious">

						<i class="fa fa-car" aria-hidden="true"></i>						
						<span class="relgious"><?php echo $vicinity_train_icon_text=get_field('relgious');; ?></span></div>
						<div class="spacer"></div>
						
				</div>
				<?php } } ?>
				<!------------------------>

				
</div>
				<!----------------------->

				


	<!--map-->
	<?php $maps_locations=get_field('map');
    if($maps_locations){
	?>
<div class="col-md-12 detail-content floorplans" id="Location">
					<div class="col-md-12 title amenities-title">Location Plan</div>
					<div class="vicinity_span"></div>
<?php echo $vicinity_train_icon_text=get_field('map'); ?>
</div>
	<?php } ?>
	<!--map-->

				
				
				<!-- Enquiry About Project-->
				<?php  $enquiry_about_project=get_field('enquiry_about_project');
				if($enquiry_about_project){
				?>
				<div class="col-md-12 detail-content floorplans">
						<div class="col-md-12 title amenities-title">Enquiry About Project</div>
						<div class="vicinity_span"></div>
						<?php echo $enquiry_about_project=get_field('enquiry_about_project'); ?>
						
				</div>
				<?php } ?>
			



			
				

				

				
				<?php if( !empty( $attach ) ) :?>
				<!-- Attachments -->
				<div class="col-md-12 title"><?php esc_html_e('Attachments', 'sunhouse');?></div>
				<div class="row description">
					<?php echo '<div class="col-md-12 des-text"> ' . $attach . '</div>'?>
				</div>
				<?php endif;?>
				<?php $model->reset();?>
				<!--<div class="col-md-12 show-map">
					<i class="fa fa-map-marker sh-icon"></i>
					<a href="#" title="" class="show-text"><?php esc_html_e('Show Map', 'sunhouse');?></a>
					<a href="#" title="" class="hide-text"><?php esc_html_e('Hide Map', 'sunhouse');?></a>
				</div>-->
			</div>
			<div id='page-sidebar' class="sidebar <?php echo esc_attr( $sidebar_css )?> ">
				<?php
					// show agent box
					if( $show_agent ):
						echo do_shortcode('[slzcore_agent_list_sc layout="agent-widget" agent_slug="'. $model->post_meta['agent'] .'" contact_btn="yes"]');
					endif;
					// show sidebar
					if( $sidebar != 'none' ) :
						slzsunhouse_get_sidebar($sidebar_id);
					endif;
				?>
			</div>
		</div>
	</div>
</div>
<!-- show map -->
<!--<div id="single-map" class="section contact-map height-full-screen location-map" 
	data-im-url="<?php //echo esc_url(SLZCORE_MAP_MAKER)?>"
	data-json="<?php //echo htmlentities( $json_map, ENT_QUOTES, "utf-8" );?>" >
</div>-->
<?php endwhile;?>
<?php wp_reset_query();?>
<?php do_action('slzsunhouse_show_property_modal');?>
					<script type="text/javascript">
						jQuery(document).ready(function(){
						jQuery('.floorsa').click(function() {
						jQuery('.col-md-12.floora_desc').css('display','block');	
						jQuery('.col-md-12.floorb_desc').css('display','none');

						});


						jQuery('.floorsb').click(function() {
						jQuery('.col-md-12.floora_desc').css('display','none');	
						jQuery('.col-md-12.floorb_desc').css('display','block');	

						});


					});
					</script>
<style>

</style>
<?php get_footer(); ?>
