# Insp3ct0r

Kishor Balan tipped us off that the following code may need inspection: http://jupiter.challenges.picoctf.org:9670
How do you inspect web code on a browser?
There's 3 parts

1. Ingresar a la página mediante el link
2. Click derecho
	- **Forma 1**
		1. Inspeccionar
		2. Sources
	
	- **Forma 2**
		1. Ver Código Fuente (Ctrl + U)
	
	- **Forma 3**
		1. Inspeccionar
		2. Elements
		Aquí se puede modificar la página
3. Encontramos la primera parte de la bandera en un comentario en HTML
```html
<!doctype html>
<html>
  <head>
    <title>My First Website :)</title>
    <link href="https://fonts.googleapis.com/css?family=Open+Sans|Roboto" rel="stylesheet">
    <link rel="stylesheet" type="text/css" href="mycss.css">
    <script type="application/javascript" src="myjs.js"></script>
  </head>

  <body>
    <div class="container">
      <header>
	<h1>Inspect Me</h1>
      </header>

      <button class="tablink" onclick="openTab('tabintro', this, '#222')" id="defaultOpen">What</button>
      <button class="tablink" onclick="openTab('tababout', this, '#222')">How</button>
      
      <div id="tabintro" class="tabcontent">
	<h3>What</h3>
	<p>I made a website</p>
      </div>

      <div id="tababout" class="tabcontent">
	<h3>How</h3>
	<p>I used these to make this site: <br/>
	  HTML <br/>
	  CSS <br/>
	  JS (JavaScript)
	</p>
	<!-- Html is neat. Anyways have 1/3 of the flag: picoCTF{tru3_d3 -->
      </div>
      
    </div>
    
  </body>
</html>
```
5. Encontramos la segunda parte en la hoja de estilos css
```css
div.container {
    width: 100%;
}

header {
    background-color: black;
    padding: 1em;
    color: white;
    clear: left;
    text-align: center;
}

body {
    font-family: Roboto;
}

h1 {
    color: white;
}

p {
    font-family: "Open Sans";
}

.tablink {
    background-color: #555;
    color: white;
    float: left;
    border: none;
    outline: none;
    cursor: pointer;
    padding: 14px 16px;
    font-size: 17px;
    width: 50%;
}

.tablink:hover {
    background-color: #777;
}

.tabcontent {
    color: #111;
    display: none;
    padding: 50px;
    text-align: center;
}

#tabintro { background-color: #ccc; }
#tababout { background-color: #ccc; }

/* You need CSS to make pretty pages. Here's part 2/3 of the flag: t3ct1ve_0r_ju5t */
```
6. Encontramos la última parte de la bandera en el script de Java.
```javascript
function openTab(tabName,elmnt,color) {
    var i, tabcontent, tablinks;
    tabcontent = document.getElementsByClassName("tabcontent");
    for (i = 0; i < tabcontent.length; i++) {
	tabcontent[i].style.display = "none";
    }
    tablinks = document.getElementsByClassName("tablink");
    for (i = 0; i < tablinks.length; i++) {
	tablinks[i].style.backgroundColor = "";
    }
    document.getElementById(tabName).style.display = "block";
    if(elmnt.style != null) {
	elmnt.style.backgroundColor = color;
    }
}

window.onload = function() {
    openTab('tabintro', this, '#222');
}

/* Javascript sure is neat. Anyways part 3/3 of the flag: _lucky?2e7b23e3} */
```
7. Unimos las tres partes de la bandera
```
picoCTF{tru3_d3t3ct1ve_0r_ju5t_lucky?2e7b23e3}
```

**Fuente:**
https://youtu.be/f1infpFomIM?si=U2yucBhRO-aFkPFt

# where are the robots

Can you find the robots? `https://jupiter.challenges.picoctf.org/problem/56830/` http://jupiter.challenges.picoctf.org:56830
What part of the website could tell you where the creator doesn't want you to look?

1. Ingresar a la página
2. Buscamos en el código fuente pero a simple vista no encontramos nada
3. Buscamos 'Archivos robots' en Google
Un archivo robots.txt indica a los rastreadores de los buscadores a qué URLs de tu sitio pueden acceder. Principalmente, se utiliza para evitar que las solicitudes que recibe tu sitio lo sobrecarguen; **no es un mecanismo para impedir que una página web aparezca en Google**. Si quieres que una página web no aparezca en Google, bloquea la indexación con `noindex` o protege la página con una contraseña. Aun con el archivo robots.txt es posible acceder manualmente al sitio y averiguar qué hay ahí.
4. Accedemos manualmente al archivo robots.txt
```
https://jupiter.challenges.picoctf.org/problem/56830/robots.txt

output:
User-agent: *
Disallow: /1bb4c.html
```
La liga `/1bb4c.html` no está indexada en el HTML original, pero nada evita que podamos ingresar a ella.
5. Ingresamos la liga oculta.
```
https://jupiter.challenges.picoctf.org/problem/56830//1bb4c.html

output: Guess you found the robots  
picoCTF{ca1cu1at1ng_Mach1n3s_1bb4c}
```

**Fuentes:**
https://developers.google.com/search/docs/crawling-indexing/robots/intro?hl=es
https://youtu.be/LRgg3Kcnbuw?si=vK5RANuzQoGSzDeb

# logon

The factory is hiding things from all of its users. Can you login as Joe and find what they've been looking at? `https://jupiter.challenges.picoctf.org/problem/13594/` http://jupiter.challenges.picoctf.org:13594
Hmm it doesn't seem to check anyone's password, except for Joe's?

1. Ingresamos a la página
2. Intentamos ingresar con el usuario Joe, pero no es posible
3. La pista dice que sólo revisa la contraseña de Joe, así que intentamos ingresar con un usuario y contraseña inventados, y entramos.
4. Regresamos a la parte de login
5. Inspeccionar
6. Network
7. Repetimos paso 3
8. Headers
9. Observamos los request cliente-servidor
10. Click en flag con status de 200
11. Podemos ver los Headers de solicitud y de respuesta
12. Vamos a Cookies
13. El reto consiste en cambiar el valor de la cookie 'admin' de False a True
14. Instalamos la extensión 'Cookie Editor' en el navegador
15. Usamos 'Cookie Editor' para modificar el valor de la cookie admin
16. Hacemos logut
17. Volvemos a ingresar
18. Obtenemos la bandera
```
picoCTF{th3_c0nsp1r4cy_l1v3s_d1c24fef}
```

El usuario no debe tener acceso a las cookies para evitar modificarlas. Las cookies no deben almacenar datos de acceso a no ser que sea de manera encriptada. En este ejercicio, la cookie estaba en texto plano y por eso fue sencillo encontrar la banera.


**Fuentes:**
- [Video explicación](https://www.youtube.com/watch?v=P2njyHWhu1U&list=PLDo9DMLZyP6kTZ8Td37-LdbAx4-yNfHBl&index=3)
- [https](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbGdvVVJ3QnVlTXBCcGYtNFNDbUMtQXBwRmQ3d3xBQ3Jtc0tsRF9ITEZzWG5PTUE5NWV1MmI1R29LRDYzOFpOQVBHdExRTFExdWMwRU1EZWlBc2VwR2Z0MnhPMDNrWDhuQVZRS3NjTXl0cEl3XzRyWjY5S053UVpic21hdllWRGtUNmdpSlZ5Y3p5VHQ2N1ZZd1BLNA&q=https%3A%2F%2Fdeveloper.mozilla.org%2Fen-US%2Fdocs%2FWeb%2FHTTP&v=P2njyHWhu1U)
- [https headers](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqazNOSTFkbWNnZlhHVDlXUFBHcWswQjF4c3FlQXxBQ3Jtc0tta3M1TllDRkQtbE5LakdSRWxfbmxKMnR2cFpTODIwZDRJaG1hd2xfclNGNDB1VERaRklYWnExS3NnaW9vZEEwcC1CVmZSQ1Rxcnd5ZTlWa2JTSm5jemNYVEpIN1NXMU5fdDNmV0ZCNXh6Zkh4bkJ6TQ&q=https%3A%2F%2Fdeveloper.mozilla.org%2Fen-US%2Fdocs%2FWeb%2FHTTP%2FHeaders&v=P2njyHWhu1U)
- [request headers](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbEhqVnMyVVJpZGpFM091ZUNBVWJKNFNFZ2Rsd3xBQ3Jtc0tuNEpiVVB2QUU2V2NoZXJmTzBWNUdGdU16NzdYNG9XN1FRQjBJNUl2anMtZGttUnQ4bEE1MkVjQ29nQl9falpZX0ZuaGo2RlFKZ0FDM3VJcEhkaVQySVhiNkpGN1NCcWVzWEo1QzZGYnktRVZXTnBIbw&q=https%3A%2F%2Fdeveloper.mozilla.org%2Fen-US%2Fdocs%2FGlossary%2FRequest_header&v=P2njyHWhu1U)
- [request methods](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqa2QwZ3FMQmRQa2VtdXpXRDhGU2tTWTFxWWEtQXxBQ3Jtc0tuUFJkUy1WYXFFRFQxY1EwVl9ESElvek9BczUzRWdRYjFuRktHZ2FWSUZ2SXNnX01WTHI5OVdDR0h1M2tMVmhEMlowMVdQSkdxZmlKUW5NSVpReGR4NE54YXVoVHBOR19SLVlFdHFNZXV4TVR6ZGdoQQ&q=https%3A%2F%2Fdeveloper.mozilla.org%2Fen-US%2Fdocs%2FWeb%2FHTTP%2FMethods&v=P2njyHWhu1U)
- [response headers](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbVNnWFNCRHFqVVNMWWtBWkhmdEhPZTRVQXVxQXxBQ3Jtc0tuLWJKdEo4dldxc3Q5MnctZjEwUWlQRFNLVmtLb0hUSEFqbFQtbThZcDVCNk1mc1lhN1BSTnh6cXgyVFU0NjY2YUxmTGNSZGhTU0dYZzMtNnZYUWVhc1dVWHJFMl9fRF9SbExQbHM0RnNRbFRxcGVhRQ&q=https%3A%2F%2Fdeveloper.mozilla.org%2Fen-US%2Fdocs%2FGlossary%2FResponse_header&v=P2njyHWhu1U)
- [response codes](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbWVOWExVZnk4dHgtU3g5c1R4OXdnREFQcGJaUXxBQ3Jtc0tuU0ZwMlEyU3JMR1h3ZDlBM2gwa3BlTkhzZHBCMHJSSjRjM214NzRsZEZjYXE5Vkh2MnNtZ3UyaGw5WXA5UnVQMG8wN3VOTlFway1rNzZmc2dEem81LXVRdS1DU2ZSN2VQNU9kdTVNeG9OUXJvRTk0bw&q=https%3A%2F%2Fdeveloper.mozilla.org%2Fen-US%2Fdocs%2FWeb%2FHTTP%2FStatus&v=P2njyHWhu1U)
- [cookies](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqa2M5ZEVfLUtQS09KR0JDN0NHZ3pSU3h3THFRd3xBQ3Jtc0tuMFRsNjdXVDVPUkJfTWFkX1ZNRzNZazNrbXItSjJWajh0NHF2eWtXcWk2bS1UZVhoNzZEanFPTXdpZDkyRXNHTmRFaE9rcGJDTzhTNnRxWFFWZFRRc0V2TzRyYzRYLXZKQjFGQmNjMnJPVTJhRzBRRQ&q=https%3A%2F%2Fen.wikipedia.org%2Fwiki%2FHTTP_cookie&v=P2njyHWhu1U)


# dont-use-client-side

Can you break into this super secure portal? `https://jupiter.challenges.picoctf.org/problem/17682/` http://jupiter.challenges.picoctf.org:17682
Never trust the client

**Fuentes:**
- [client-side vs server-side](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqa1A3X2ltZnlzSkFFWHJUdTk1NHF0cHBPdXVQUXxBQ3Jtc0ttd0ZmZlc2LWQxQmNQNnhzSU0xYjNXSGRZMENMcUMzbm8zcmJ2NVJEbjktby03YW10a2ZBeGQzVk5kQ1ZGU0F2b2UwcUd2UFNMaVhaZFVKQnZQQ2VBajgzOGxGOWU3bVpjSFRYYXJrZV9QR1ptM2xKZw&q=https%3A%2F%2Fwww.educative.io%2Fanswers%2Fclient-side-vs-server-side&v=19Hkmb1Guzk)
- [mozilla debugger](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbWUxbG4zRXZVM25vQ05NQS1nN2VST1JfZVBWUXxBQ3Jtc0tuVld2RmswNUdWaDh3ZElGMkZSWUlTdGdETGt1VUdCaDY1ODJFY3k4TlgtT1c0RmNoVjRpb1U4dWtnRzc0RzdTUW9IWHFva2gxT2RpeGRqemFuNEdnQ1padzc0QjNFdHR3VkFBOEZ2Y3ZHUDcxR1NYYw&q=https%3A%2F%2Fdeveloper.mozilla.org%2Fes%2Fdocs%2FTools%2FDebugger&v=19Hkmb1Guzk)

1. Ingresamos a la página
2. No tenemos la contraseña así que debemos ver cómo se hace la verificación
3. Vamos al código fuente
4. En este ejercicio, el programador dejó el código de validación de la contraseña del lado del cliente
```html
<script type="text/javascript">
function verify() {
	checkpass = document.getElementById("pass").value;
	split = 4;
	if (checkpass.substring(0, split) == 'pico') {
		if (checkpass.substring(split*6, split*7) == '706c'){
			if (checkpass.substring(split, split*2) == 'CTF{') {
				if (checkpass.substring(split*4, split*5) == 'ts_p') {
					if (checkpass.substring(split*3, split*4) == 'lien') {
						if (checkpass.substring(split*5, split*6) == 'lz_b') {
							if (checkpass.substring(split*2, split*3) == 'no_c') {
								if (checkpass.substring(split*7, split*8) == '5}') {alert("Password Verified")
								}
							}
						}
					}
				}
			}
		}
	}
else {
	alert("Incorrect password");
	}
}
</script>
```
5. Obtenemos la bandera
```
picoCTF{no_clients_plz_b706c5}
```

El password se almacena en un arreglo de caracteres. Se toma una sub cadena desde la posición 0 hasta la posición `split - 1 ` y lo compara con la palabra 'pico'. Después compara la segunda sub cadena desde la posición `split` hasta `(split*2)-1`. 


# picobrowser

This website can be rendered only by **picobrowser**, go and catch the flag! `https://jupiter.challenges.picoctf.org/problem/28921/` http://jupiter.challenges.picoctf.org:28921
You don't need to download a new web browser

## Desde navegador

1. Abrimos el link de la página
2. La página detecta que no estamos en el navegador correcto
3. Click derecho
4. Inspeccionar
5. Network
6. Hacemos la solicitud
7. Seleccionamos flag con status 200
8. El Header Request le da toda la información de identificación al servidor
```
user-agent:
Mozilla/5.0 (X11; Linux x86_64; rv:128.0) Gecko/20100101 Firefox/128.0
```
9. Click en Block o Resend
10. Modificamos el 'user-agent'
```
user-agent:
picobrowser
```
11. Reenviamos la petición
12. Vemos la respuesta en Respond
13. Obtenemos la bandera
```
picoCTF{p1c0_s3cr3t_ag3nt_84f9c865}
```

## Desde consola

1. Comano curl
```
curl -s https://jupiter.challenges.picoctf.org/problem/28921/flag -H 'user-agent: picobrowser' | grep picoCTF 
```
- `-s` modo silencioso
- `-H` editar el header
2. Obtenemos la bandera
```
<p style="text-align:center; font-size:30px;"><b>Flag</b>: <code>picoCTF{p1c0_s3cr3t_ag3nt_84f9c865}</code></p>
```


**Fuentes:**
- [Video explicación](http://youtube.com/watch?v=9d6-N0oJwOk&list=PLDo9DMLZyP6kTZ8Td37-LdbAx4-yNfHBl&index=5)
- [user-agent](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbThobUFFMDRwQVVxV1k0LVpVQmRBWmNzZHFXd3xBQ3Jtc0tsZkV6ampVOXB2TnpoT3NIN0xUdmVOM0VJMU52U1h1R2lqZ2l4UnpYZmtxMjRxYzZ0UjZlbkJWNHZ5U3pwd09vT2szTzJCeXFqQ3RUY29EWERyTG0wY2ktblk3RlZqRWVtbE5xSFgydDhOdUFOeEJZcw&q=https%3A%2F%2Fdeveloper.mozilla.org%2Fes%2Fdocs%2FWeb%2FHTTP%2FHeaders%2FUser-Agent&v=9d6-N0oJwOk)
- [modifcar headers](https://requestly.com/blog/modify-headers-in-https-requests-and-responses-in-chrome-firefox-safari/)


# Client-side-again

Can you break into this super secure portal? `https://jupiter.challenges.picoctf.org/problem/6353/` http://jupiter.challenges.picoctf.org:6353
What is obfuscation?

1. Ingresamos a la página
2. No tenemos la contraseña
3. Inspeccionar
4. Debuger
5. Vemos que el código está ofuscado
	```javascript
var _0x5a46=['0a029}','_again_5','this','Password\x20Verified','Incorrect\x20password','getElementById','value','substring','picoCTF{','not_this'];(function(_0x4bd822,_0x2bd6f7){var _0xb4bdb3=function(_0x1d68f6){while(--_0x1d68f6){_0x4bd822['push'](_0x4bd822['shift']());}};_0xb4bdb3(++_0x2bd6f7);}(_0x5a46,0x1b3));var _0x4b5b=function(_0x2d8f05,_0x4b81bb){_0x2d8f05=_0x2d8f05-0x0;var _0x4d74cb=_0x5a46[_0x2d8f05];return _0x4d74cb;};function verify(){checkpass=document[_0x4b5b('0x0')]('pass')[_0x4b5b('0x1')];split=0x4;if(checkpass[_0x4b5b('0x2')](0x0,split*0x2)==_0x4b5b('0x3')){if(checkpass[_0x4b5b('0x2')](0x7,0x9)=='{n'){if(checkpass[_0x4b5b('0x2')](split*0x2,split*0x2*0x2)==_0x4b5b('0x4')){if(checkpass[_0x4b5b('0x2')](0x3,0x6)=='oCT'){if(checkpass[_0x4b5b('0x2')](split*0x3*0x2,split*0x4*0x2)==_0x4b5b('0x5')){if(checkpass['substring'](0x6,0xb)=='F{not'){if(checkpass[_0x4b5b('0x2')](split*0x2*0x2,split*0x3*0x2)==_0x4b5b('0x6')){if(checkpass[_0x4b5b('0x2')](0xc,0x10)==_0x4b5b('0x7')){alert(_0x4b5b('0x8'));}}}}}}}}else{alert(_0x4b5b('0x9'));}}
	```
6. Copiamos la parte del código que recibe la contraseña e intentamos desofuscarlo con jsnice
```javascript
/** @type {Array} */
var _0x5a46 = ["0a029}", "_again_5", "this", "Password Verified", "Incorrect password", "getElementById", "value", "substring", "picoCTF{", "not_this"];
(function(paths, opt_attributes) {
  /**
   * @param {number} val
   * @return {undefined}
   */
  var setter = function(val) {
    for (;--val;) {
      paths["push"](paths["shift"]());
    }
  };
  setter(++opt_attributes);
})(_0x5a46, 435);
/**
 * @param {string} key
 * @param {?} dataAndEvents
 * @return {?}
 */
var _0x4b5b = function(key, dataAndEvents) {
  /** @type {number} */
  key = key - 0;
  var label = _0x5a46[key];
  return label;
};
/**
 * @return {undefined}
 */
function verify() {
  checkpass = document[_0x4b5b("0x0")]("pass")[_0x4b5b("0x1")];
  /** @type {number} */
  split = 4;
  if (checkpass[_0x4b5b("0x2")](0, split * 2) == _0x4b5b("0x3")) {
    if (checkpass[_0x4b5b("0x2")](7, 9) == "{n") {
      if (checkpass[_0x4b5b("0x2")](split * 2, split * 2 * 2) == _0x4b5b("0x4")) {
        if (checkpass[_0x4b5b("0x2")](3, 6) == "oCT") {
          if (checkpass[_0x4b5b("0x2")](split * 3 * 2, split * 4 * 2) == _0x4b5b("0x5")) {
            if (checkpass["substring"](6, 11) == "F{not") {
              if (checkpass[_0x4b5b("0x2")](split * 2 * 2, split * 3 * 2) == _0x4b5b("0x6")) {
                if (checkpass[_0x4b5b("0x2")](12, 16) == _0x4b5b("0x7")) {
                  alert(_0x4b5b("0x8"));
                }
              }
            }
          }
        }
      }
    }
  } else {
    alert(_0x4b5b("0x9"));
  }
}
;
```
Aunque esté desofuscado el código no es del todo legible.
7. Vemos que la variable '\_0x5a46' parece tener fragmentos de la bandera, así que nos centraremos en ella. Lo copiamos y lo pegamos en Console (usar 'allow pasting' y luego 'Enter' en caso de que no nos permita pegar)
	```javascript
var _0x5a46 = ["0a029}", "_again_5", "this", "Password Verified", "Incorrect password", "getElementById", "value", "substring", "picoCTF{", "not_this"];
	```
8. Vemos lo que hay almacenado en la variable
```
_0x5a46

output: Array(10) [ "0a029}", "_again_5", "this", "Password Verified", "Incorrect password", "getElementById", "value", "substring", "picoCTF{", "not_this" ]
```
9. Armamos la bandera de manera manual buscando la lógica de la secuencia
```
_0x5a46[8] + _0x5a46[9] + _0x5a46[1] + _0x5a46[0]

output: "picoCTF{not_this_again_50a029}"
```


**Fuentes:**

- [video explicación](https://www.youtube.com/watch?v=rsPT722MkzQ&list=PLDo9DMLZyP6kTZ8Td37-LdbAx4-yNfHBl&index=6)
- [ javascript obfuscation](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbm1lSU82ZXFOMFg5V3E5TEJFSDU4RG05QW5hZ3xBQ3Jtc0ttVnRTNndoWWJlUjltbFpRcW9hQXhPTmxfMTZ6bHFELWY0bFp1b1hZa0ZzclBnclVDZVNUSklERS10cmczS3VCNWdDazRHczBuc3RjQVdfR0J0Ymo2OTBqckh2b1Y5SG1zbkIwXzJQWGl2TVgyMjJBbw&q=https%3A%2F%2Fjscrambler.com%2Fproducts%2Fcode-integrity%2Fjavascript-obfuscation&v=rsPT722MkzQ)
- [javascript deobfuscation](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqa2JmMGJqZnJpdUtVSHVHeXl6VmZIeFVqQTl5Z3xBQ3Jtc0tsZG9teE85SzJWWGg3ekJYVXk1VkVsTEZ6Skl2dWtBVnJGYjVqYnhENWlmZFU4Rm9sZS15ZzluVmZGck5QbUJxSTBrSzdCX3pjSFdPdGhCRUU3UU1IZE1zdzV1Q3VrZ2s0cXA5eFcxai1tRDI2dVd5RQ&q=http%3A%2F%2Fjsnice.org%2F&v=rsPT722MkzQ)
- [video obfuscation](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqa3U2Zkk3OEN6WXA3UllKQ1h0UTRjNkRpaDcyZ3xBQ3Jtc0ttb1lwNjRtMkpzWG1jMWJySFlmVXBDNTRYcEVpcU45TDFlMXNhTktrQUNoeWhGMjQzUmNUZE95RXBzZVpCX2VRbERncEQtU2oyZVQ1RWN3TWJoUmJ6UWVQU2JTeXRIWk16Q0VVbTREYjlFZVBoVXF0aw&q=https%3A%2F%2Fbeautifier.io%2F&v=rsPT722MkzQ)