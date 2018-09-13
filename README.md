# Wordpress
Wordpress Scripts

Woocommerce
Most functions found in:
/wp-content/plugins/woocommerce/includes/wc-template-functions.php


// Change Order By
add_filter('woocommerce_catalog_orderby', 'wc_customize_product_sorting');

function wc_customize_product_sorting($sorting_options){
$sorting_options = array(
'menu_order' => __( 'Sorting', 'woocommerce' ),
'popularity' => __( 'Popularity', 'woocommerce' ),
'rating' => __( 'Sort by average rating', 'woocommerce' ),
'date' => __( 'Date Added', 'woocommerce' ),
'price' => __( 'Sort by price: low to high', 'woocommerce' ),
'price-desc' => __( 'Sort by price: high to low', 'woocommerce' ),
);

return $sorting_options;
}


You can solve this by finding

ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_520_ci;
in your .sql file, and swapping it with

ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_general_ci;


// Create Custom Post Type for Work
// add_action( 'init', 'create_post_type' );
// function create_post_type() {
// register_post_type( 'custom_post',
// array(
// 'thumbnail',
// 'labels' => array(
// 'name' => __( 'Custom' ),
// 'singular_name' => __( 'Custom' )
// ),
// 'public' => true,
// 'has_archive' => true,
// 'rewrite' => array('slug' => 'custom'),
// 'taxonomies' => array('category', 'post_tag'),
// 'supports' => array( 'thumbnail' )
// )
// );
// };

Include Areas (looped) in page

Include - include(locate_template('partials/sections/pagename.php'));

Folder Structure
- Partials
     - Modules
     - Sections


Echo Featured Image as Background from Shop Categories
<?php
$target_post_id = '358'; // Echo Featured Image as Background from Shop Categories
$feat_image = wp_get_attachment_url(get_post_thumbnail_id($target_post_id));
echo '<li class="banner" style="background: url('. $feat_image.'); background-size: cover;">'; ?>


If page by ID

<?php if( $post->ID == 775) { ?>
<?php } ?>


Custom Post Type with Slick Slider

<?php
// args
$args = array(
// 'numberposts' => -1,
'post_type' => 'testimonials',
);
// query
$the_query = new WP_Query( $args );
?>
<div class="testimonials-slider">
<?php if( $the_query->have_posts('') ): ?>
<?php while( $the_query->have_posts('') ) : $the_query->the_post(); ?>
<div class="testimonials-slide">
<?php the_post_thumbnail(); ?>
<h4><a href="<?php the_permalink(); ?>"><?php the_title(); ?></a></h4>
<?php custom_length_excerpt(75); ?>
</div>
<?php endwhile; ?>
</div>
<?php endif; ?>

<?php wp_reset_query(); // Restore global post data stomped by the_post(). ?>



Repeater Loop

<?php if (have_rows('name')): ?>
<?php while (have_rows('name')): the_row(); ?>
<?php
$backgroundchoice = get_sub_field('background');
$image = get_sub_field('logo');
$title = get_sub_field('title');
$text = get_sub_field('text');
$link = get_sub_field('link');
$btn = get_sub_field('btn');
$background = get_post_meta($post->ID, '', true);
?>
<div class="col-xs-6 accreditations_certificates <?php echo $backgroundchoice; ?>">
<div class="thumb"><img class="img-responsive" src="<?php echo $image['url']; ?>" alt="<?php echo $image['alt'] ?>" /></div>
<div class="content">
<h3><?php echo $title; ?></h3>
<p><?php echo $text; ?></p>
<a class="btn" href="http://<?php echo $link; ?>" target="_blank"><?php echo $btn; ?></a>
</div>
</div>
<?php endwhile; ?>
<?php endif; ?>




ACF

<?php if( get_field('hobbies') ) { ?>
<?php echo '<p class="large">Hobbies include</p>' . '<p>' . get_field('hobbies') . '</p>' ; ?>
<?php } ?>

<?php if( have_rows('training_development') ): ?>

  <?php while( have_rows('training_development') ): the_row();
  // vars
  $link = get_sub_field('link');
  $title = get_sub_field('title');
  ?>

  <?php
  if (get_sub_field('title')) {
  ?>
  <a href="<?php the_sub_field('link'); ?>" target="_blank" class="list-group-item"> <?php the_sub_field('title'); ?></a>

  <?php
  } else {
  the_sub_field('title');
  }
  ?>

  <?php endwhile; ?>

  <?php endif; ?>

  <?php the_field('associates'); ?>



<?php if (have_rows('online_academy')): ?>
  <?php while (have_rows('online_academy')): the_row(); ?>
  <?php if ($oa_title= get_sub_field('oa_title')) { ?>
  <?php echo $oa_title ; ?></a>
  <?php } else {
  echo ['oa_title'];
  } ?>
  <?php endwhile; ?>
  <?php endif; ?>




Custom Field 
<div class="col-md-12"> 
  <a class="btn wht floatLeft marginRight" href="<?php echo get_post_meta($post->ID, 'directions', true); ?>" alt="Sorry, No Directions"> 
  View on map</a> 
  <a class="btn wht floatLeft" href="<?php $venue_email = get_post_meta($post->ID, 'venue_email', true); 
  if ($venue_email) { ?> 
  <?php echo $venue_email; ?> 
  <?php } 
  else { ?> 
  No Current Venue 
  <?php } ?>">Email Venue</a> 
  </div><!--/col--> 


Get Template Dir for Images 
<?php echo get_template_directory_uri(); ?> 

<?php get_template() ?> 

<?=get_home_url();?> 

Navigation 
<?php if ( has_nav_menu( 'primary' ) ) : ?> 
<button id="menu-toggle" class="menu-toggle"><?php _e( 'Menu', ‘THEME’ ); ?></button> 
<div id="site-header-menu" class="site-header-menu"> 
<nav id="site-navigation" class="main-navigation" role="navigation" aria-label="<?php esc_attr_e( 'Primary Menu', ‘THEME’ ); ?>"> 
<?php 
wp_nav_menu( array( 
'theme_location' => 'primary', 
'menu_class' => 'primary-menu', 
) ); 
?> 
</nav><!-- .main-navigation --> 
</div><!-- .site-header-menu --> 
<?php endif; ?> 



- Post Area 

<?php query_posts('category_name=news' . '&orderby=date&order=DEC' . 'cat=-3&showposts=3'); ?> 
<?php if (have_posts()): while (have_posts()) : the_post(); ?> 
<?php the_post_thumbnail(); ?> 
<h2><?php the_title(); ?></h2> 
<?php the_date(); ?> 
<?php the_excerpt(); ?> 
<?php the_content(); ?> 

// If No Content Added
<?php 
    // Get the content
    $content = get_the_content();

    if(trim($content) == "") // Check if the string is empty or only whitespace
    {
        echo "Static content";
        get_template_part('slug','name');
    }
    else
    {
        // Apply filters the same as the_content() does: 
        $content = apply_filters('the_content', $content);
        $content = str_replace(']]>', ']]&gt;', $content);
        echo $content;
    }
?>


<?php edit_post_link('Edit This area'); ?>
<?php endwhile; 
  else: ?> 
<p><strong><?php _e( 'Sorry, nothing to display.'); ?></strong></p> 
<?php endif; ?> 


Specific to page 
<?php get_header(); ?> <?php if (is_home()) { // we're on the home page, so let's show a picture of our new kitten! echo "<img src='/images/new_kitty.jpg' alt='Our new cat, Rufus!' />"; // and now back to our regularly scheduled home page } ?> 

Add Shortcode To Page 
<?php echo do_shortcode('[SHORTCODE]'); ?> 

Custom Fields 
<?php echo get_post_meta($post->ID, 'key', true); ?> 

Custom post Types
<?php $args = array( 'post_type' => 'testimonials', 'posts_per_page' => 10 );
  $loop = new WP_Query( $args );
  while ( $loop->have_posts() ) : $loop->the_post();
  the_title();
  echo '<div class="entry-content">';
  the_content();
  echo '</div>';
  endwhile;
  ?>

Plugins 
Custom Post Type UI 
SEO 
Debug Bar 

Add Wordpress User FTP 
function wpb_admin_account(){ 
$user = 'Username'; 
$pass = 'Password'; 
$email = 'email@domain.com'; 
if ( !username_exists( $user ) && !email_exists( $email ) ) { 
$user_id = wp_create_user( $user, $pass, $email ); 
$user = new WP_User( $user_id ); 
$user->set_role( 'administrator' ); 
} } 
add_action('init','wpb_admin_account');
