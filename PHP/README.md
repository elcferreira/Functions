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
add_action('restrict_manage_posts','my_restrict_manage_posts');

		function my_restrict_manage_posts() {
			global $typenow;

			if ($typenow=='your_custom_post_type'){
                         $args = array(
                             'show_option_all' => "Show All Categories",
                             'taxonomy'        => 'your_custom_taxonomy',
                             'name'               => 'your_custom_taxonomy'

                         );
				wp_dropdown_categories($args);
                        }
		}
add_action( 'request', 'my_request' );
function my_request($request) {
	if (is_admin() && $GLOBALS['PHP_SELF'] == '/wp-admin/edit.php' && isset($request['post_type']) && $request['post_type']=='your_custom_post_type') {
		$request['term'] = get_term($request['your_custom_taxonomy'],'your_custom_taxonomy')->name;

	}
	return $request;
}
```
