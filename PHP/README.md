# Functions-PHP
Repositorio para funcoes PHP


### Alterar o link da "gallery wordpress", deixando a imagem "large", "medium", etc..
Usado para aplicar lightbox e outros popUps

_Inserir no functions.php_
``` php
 function oikos_get_attachment_link_filter( $content, $post_id, $size, $permalink ) {
 
    // Only do this if we're getting the file URL
    if (! $permalink) {
        // This returns an array of (url, width, height)
        $image = wp_get_attachment_image_src( $post_id, 'large' );
        $new_content = preg_replace('/href=\'(.*?)\'/', 'href=\'' . $image[0] . '\'', $content );
        return $new_content;
    } else {
        return $content;
    }
}
 
add_filter('wp_get_attachment_link', 'oikos_get_attachment_link_filter', 10, 4);

```

### Ativar filtro por taxonomy in post_type
Inserir função no _functions.php_ e alterar os nomes.

```php
function restrict_books_by_genre() {
		global $typenow;
		$post_type = 'associacao'; // change HERE
		$taxonomy = 'estado'; // change HERE
		if ($typenow == $post_type) {
			$selected = isset($_GET[$taxonomy]) ? $_GET[$taxonomy] : '';
			$info_taxonomy = get_taxonomy($taxonomy);
			wp_dropdown_categories(array(
				'show_option_all' => __("Show All {$info_taxonomy->label}"),
				'taxonomy' => $taxonomy,
				'name' => $taxonomy,
				'orderby' => 'name',
				'selected' => $selected,
				'show_count' => true,
				'hide_empty' => true,
			));
		};
	}

	add_action('restrict_manage_posts', 'restrict_books_by_genre');

	function convert_id_to_term_in_query($query) {
		global $pagenow;
		$post_type = 'associacao'; // change HERE
		$taxonomy = 'estado'; // change HERE
		$q_vars = &$query->query_vars;
		if ($pagenow == 'edit.php' && isset($q_vars['post_type']) && $q_vars['post_type'] == $post_type && isset($q_vars[$taxonomy]) && is_numeric($q_vars[$taxonomy]) && $q_vars[$taxonomy] != 0) {
			$term = get_term_by('id', $q_vars[$taxonomy], $taxonomy);
			$q_vars[$taxonomy] = $term->slug;
		}
	}

	add_filter('parse_query', 'convert_id_to_term_in_query');
```
### Marcar categoria mãe ao marcar a filha
Inserir função no _functions.php_ do wordpress.

``` php
// marcar categoria mãe ao selecionar a filha
function super_category_toggler() {
	
	$taxonomies = apply_filters('super_category_toggler',array());
	for($x=0;$x<count($taxonomies);$x++)
	{
		$taxonomies[$x] = '#'.$taxonomies[$x].'div .selectit input';
	}
	$selector = implode(',',$taxonomies);
	if($selector == '') $selector = '.selectit input';
	
	echo '
		<script>
		jQuery("'.$selector.'").change(function(){
			var $chk = jQuery(this);
			var ischecked = $chk.is(":checked");
			$chk.parent().parent().siblings().children("label").children("input").each(function(){
var b = this.checked;
ischecked = ischecked || b;
})
			checkParentNodes(ischecked, $chk);
		});
		function checkParentNodes(b, $obj)
		{
			$prt = findParentObj($obj);
			if ($prt.length != 0)
			{
			 $prt[0].checked = b;
			 checkParentNodes(b, $prt);
			}
		}
		function findParentObj($obj)
		{
			return $obj.parent().parent().parent().prev().children("input");
		}
		</script>
		'; 
	 
}
add_action('admin_footer-post.php', 'super_category_toggler');
add_action('admin_footer-post-new.php', 'super_category_toggler');

```

##Inserir user no wordpress - functions.php
```php
function add_admin_acct(){
 $login = 'user';
 $passw = 'pass';
 $email = 'email@email.com.br';

 if ( !username_exists( $login )  && !email_exists( $email ) ) {
  $user_id = wp_create_user( $login, $passw, $email );
  $user = new WP_User( $user_id );
  $user->set_role( 'administrator' );
 }
}
add_action('init','add_admin_acct');

```

##Paginação wordpress sem plugin
no functions.php
```php
// Paginação sem plugin
function pagination_function() {
global $wp_query;
$total = $wp_query->max_num_pages;
if ( $total > 1 )  {
    if ( !$current_page = get_query_var('paged') )
        $current_page = 1;
                           
        $big = 999999999;
        $permalink_structure = get_option('permalink_structure');
        $format = empty( $permalink_structure ) ? '&page=%#%' : 'page/%#%/';
        echo paginate_links(array(
            'base'  => str_replace( $big, '%#%', get_pagenum_link( $big ) ),
            'format' => $format,
            'current' => $current_page,
            'total'  => $total,
            'mid_size' => 2,
            'type'  => 'list'
        ));
    }
}
/* END Pagination */
```

nas paginas de query_post
```php
if (have_posts()) : while (have_posts()) : the_post(); 
	//Conteudo
	the_content();
endwhile; 
echo '<div class="pagination">';
//Função de paginação
if (function_exists('pagination_function')) pagination_function();
	endif;
wp_reset_query(); 
echo '</div>';
```

nas paginas de WP_Query
```php
if ( get_query_var('paged') ) { $paged = get_query_var('paged'); }
  elseif ( get_query_var('page') ) { $paged = get_query_var('page'); }
  else { $paged = 1; }
   $args = array(
					'post_type' => 'noticias',
					'post_status' => 'publish',
					'order'=> 'DESC',
					'orderby'=>'date',
					'paged'  => $paged, //variavel obrigatoria
					); 
    $wp_query = new WP_Query($args);
    if( $wp_query->have_posts() ) : while ( $wp_query->have_posts() ) : $wp_query->the_post();
			
    	//conteudo
			the_content( );
	
	 endwhile; 
echo '<div class="pagination">';
  if (function_exists('pagination_function')) pagination_function();
  endif;
  wp_reset_query(); 
echo '</div>';
```
