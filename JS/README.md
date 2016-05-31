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

### Click animation button
``` js
animationType = 'var1 var2';
animationEnd = 'webkitAnimationEnd mozAnimationEnd MSAnimationEnd oanimationend animationend';

$('.btn').on('click', function() {
	$('input').addClass(animationType).one(animationEnd, function(){
		$(this).removeClass(animationType);
	});
});
```

### scrol disabled init
função para scroll do banner inicial
``` js
// Function para Disabled scroll
// left: 37, up: 38, right: 39, down: 40,
// spacebar: 32, pageup: 33, pagedown: 34, end: 35, home: 36
var keys = [37, 38, 39, 40];

function preventDefault(e) {
  e = e || window.event;
  if (e.preventDefault)
      e.preventDefault();
  e.returnValue = false;  
}

function keydown(e) {
  for (var i = keys.length; i--;) {
    if (e.keyCode === keys[i]) {
      preventDefault(e);
      return;
    }
  }
}

function wheel(e) {
  preventDefault(e);
}

function disable_scroll() {
  if (window.addEventListener) {
      window.addEventListener('DOMMouseScroll', wheel, false);
  }
  window.onmousewheel = document.onmousewheel = wheel;
  document.onkeydown = keydown;
}

function enable_scroll() {
    if (window.removeEventListener) {
        window.removeEventListener('DOMMouseScroll', wheel, false);
    }
    window.onmousewheel = document.onmousewheel = document.onkeydown = null;  
}

// banner scroll

	var lastScrollTop = 0;
	bannerScroll = $(document.getElementById('banner-home'));
	bannerScrollHeight = bannerScroll.height();
	flag = true;
	$(window).scroll(function(event){
	   var st = $(this).scrollTop();
	   if (st > lastScrollTop){
	    console.log('down');

			if ( st == 0 || st <= bannerScrollHeight ) {
				if (flag) {
	        disable_scroll();
			    $('html, body').animate({
			        scrollTop: $("#solucoes").offset().top + 20
			    }, 400, function(){
			    	enable_scroll();
			    });
			    flag = false;
			    // if (!flag) {
			    // 	return false;
			    // }
			    
				}
			}

	   } else {
	      console.log('up');
	      if (!flag) {
	      	flag = true;
	      }
	   }
	   lastScrollTop = st;
	});
```

## Scroll opacity
``` js

	var fadeStart=100, // Inicio do fade
	    fadeUntil=600, // final do fade
	    fading = $(document.querySelector('.header--bg'));
  
  $(window).on('scroll', function(){
    var offset = $(document).scrollTop(),
      opacity=1; //atribui o fade
    if( offset<=fadeStart ){
      opacity=0;
    } else if ( offset<=fadeUntil ){
      // opacity=1-offset/fadeUntil;   Inverte
      opacity=offset/fadeUntil;
    }
    fading.css('opacity',opacity);
  });
```

## new disabled scroll
``` js

// left: 37, up: 38, right: 39, down: 40,
// spacebar: 32, pageup: 33, pagedown: 34, end: 35, home: 36
var keys = {37: 1, 38: 1, 39: 1, 40: 1};

function preventDefault(e) {
  e = e || window.event;
  if (e.preventDefault)
      e.preventDefault();
  e.returnValue = false;  
}

function preventDefaultForScrollKeys(e) {
    if (keys[e.keyCode]) {
        preventDefault(e);
        return false;
    }
}

function disableScroll() {
  if (window.addEventListener) // older FF
      window.addEventListener('DOMMouseScroll', preventDefault, false);
  window.onwheel = preventDefault; // modern standard
  window.onmousewheel = document.onmousewheel = preventDefault; // older browsers, IE
  window.ontouchmove  = preventDefault; // mobile
  document.onkeydown  = preventDefaultForScrollKeys;
}

function enableScroll() {
    if (window.removeEventListener)
        window.removeEventListener('DOMMouseScroll', preventDefault, false);
    window.onmousewheel = document.onmousewheel = null; 
    window.onwheel = null; 
    window.ontouchmove = null;  
    document.onkeydown = null;  
}
```


