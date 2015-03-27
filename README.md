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
function custom_init_js()
{
    wp_enqueue_script('jquery');
    wp_localize_script('jquery', 'custom', array(
        'templateDir' => get_bloginfo('template_url')));
}
add_action('get_header', 'custom_init_js');
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
