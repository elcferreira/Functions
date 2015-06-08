# Functions-JS
Repositorio para funcoes Js


### Verifição IE
Verificar se o navegador é IE
``` js
  function msieversion() {

        var ua = window.navigator.userAgent;
        var msie = ua.indexOf("MSIE ");

        if (msie > 0 || !!navigator.userAgent.match(/Trident.*rv\:11\./))      // If Internet Explorer, return version number
           return true;
        else                 // If another browser, return 0
            return false;
```

### Bloginfo in JS
Inserir bloginfo, no arquivo .js
No functions.php
``` php
<!-- teoria -->
function custom_init_js()
{
    wp_enqueue_script('jquery');
    wp_localize_script('jquery', 'custom', array(
        'templateDir' => get_bloginfo('template_url')));
}
add_action('get_header', 'custom_init_js');

<!-- aplicada -->
//quando for registrar o JS externo
wp_register_script('plugins-ajax-nov', get_template_directory_uri() . '/js/plugins-ajax-novidade.js', array(), '1.0');
wp_enqueue_script('plugins-ajax-nov');
  wp_localize_script('plugins-ajax-nov', 'custom', array(
  'templateDir' => get_bloginfo('template_url')));
```

No JS
``` js
$(document).ready(function(){

    // Login
    $('.login-form').on('submit', function(e){
        e.preventDefault();
        dataString = $(this).serialize() + '&ajax=1';
        $.ajax ({
            type: "POST",
            url: custom.templateDir + "/inc/do-login.php",
            data: dataString,
            cache: false,
            success: function(data)
            {

            }
        });
    });
```
