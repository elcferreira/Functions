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
### Hide scroll down, show scroll up
Função js para ocultar quando o scroll for para baixo e mostrar quando o scroll subir

Para usar deve-se trocar o elemento _header_
``` js
var didScroll;
var lastScrollTop = 0;
var delta = 5;
var navbarHeight = $('header').outerHeight();

$(window).scroll(function(event){
    didScroll = true;
});

setInterval(function() {
    if (didScroll) {
        hasScrolled();
        didScroll = false;
    }
}, 250);

function hasScrolled() {
    var st = $(this).scrollTop();
    
    // Make sure they scroll more than delta
    if(Math.abs(lastScrollTop - st) <= delta)
        return;
    
    // If they scrolled down and are past the navbar, add class .nav-up.
    // This is necessary so you never see what is "behind" the navbar.
    if (st > lastScrollTop && st > navbarHeight){
        // Scroll Down
        $('header').fadeOut(300);
    } else {
        // Scroll Up
        if(st + $(window).height() < $(document).height()) {
            $('header').fadeIn(300);
        }
    }
    
    lastScrollTop = st;
}
```

### POP UP de compartilhamento
**AVISO** - Nunca coloque esta função dentro do $(document).ready <br>
script usado para o pop up de compartilhamento <br>
no HTML
``` html
<a href="#" taget="_blank" onclick="return abrirPopup('https://www.facebook.com/sharer/sharer.php?u=<?php the_permalink() ?>', 700, 400);">Facebook</a>

<a href="#" taget="_blank" onclick="return abrirPopup('https://twitter.com/intent/tweet?url=<?php the_permalink() ?>', 700, 400);">Twitter</a>

<a href="#" taget="_blank" onclick="return abrirPopup('https://plus.google.com/share?url=<?php the_permalink() ?>', 700, 400);">Google+</a>
```

no JS
``` js
//function para abrir o pop up de compartilhar
  function abrirPopup(url,w,h) {
  var newW = w + 100;
  var newH = h + 100;
  var left = (screen.width-newW)/2;
  var top = (screen.height-newH)/2;
  var newwindow = window.open(url, 'name', 'width='+newW+',height='+newH+',left='+left+',top='+top);
  newwindow.resizeTo(newW, newH);
  //posiciona o popup no centro da tela
  newwindow.moveTo(left, top);
  newwindow.focus();
  return false;
}
```

### Scroll page
Função simples para localizar se houve scroll na pagina
``` js
    $(window).scroll(function(){
       valor_scroll = $(document).scrollTop();
       // console.log(valor_scroll);
       if(valor_scroll >= 30){
       	$('header').addClass('header-ativo');
       }
       else{
					$('header').removeClass('header-ativo');
       }
    });
```

