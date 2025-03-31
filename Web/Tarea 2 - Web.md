# Includes

Can you get the flag? Go to this [website](http://saturn.picoctf.net:64022/) and see what you can discover.
Is there more code than what the inspector initially shows?

1. Ingresamos a la página
2. Revisamos el código fuente
3. Revisamos el archivo style.css y obtenemos la primera parte de la bandera
```
picoCTF{1nclu51v17y_1of2_
```
4. Revisamos el archivo script.js y obtenemos la segunda parte de la bandera
```
f7w_2of2_df589022}
```
5. Completamos la bandera
```
picoCTF{1nclu51v17y_1of2_f7w_2of2_df589022}
```



# Inspect HTML

Can you get the flag? Go to this [website](http://saturn.picoctf.net:57737/) and see what you can discover.
What is the web inspector in web browsers?

1. Ingresamos a la página
2. Revisamos el código fuente
3. Encontramos la bandera en un comentario
```
picoCTF{1n5p3t0r_0f_h7ml_1fd8425b}
```


# IntroToBurp

Try [here](http://titan.picoctf.net:53581/) to find the flag
Try using burpsuite to intercept request to capture the flag.
Try mangling the request, maybe their server-side code doesn't handle malformed requests very well

1. Ingresamos a la página
2. Inspeccionar
3. Network
4. Resend
5. Modificamos las solicitudes para que sean POST
6. Reenviamos la solicitud
7. Obtenemos la bandera en la respuesta 
```
picoCTF{#0TP_Bypvss_SuCc3$S_3e3ddc76}
```


# Local Authority

Can you get the flag? Go to this [website](http://saturn.picoctf.net:61120/) and see what you can discover.

1. Ingresamos a la página
2. Revisamos el código fuente en busca de una referencia al archivo secure.js. Encontramos el usuario y la contraseña
```
function checkPassword(username, password)
{
  if( username === 'admin' && password === 'strongPassword098765' )
  {
    return true;
  }
  else
  {
    return false;
  }
}
```
4. Ingresamos con el usuario y la contraseña. Obtenemos la bandera
```
picoCTF{j5_15_7r4n5p4r3n7_b0c2c9cb}
```


# Power Cookie

Can you get the flag? Go to this [website](http://saturn.picoctf.net:57137/) and see what you can discover.
Do you know how to modify cookies?

1. Ingresamos a la página
2. Enviamos la solicitud
3. Revisamos las cookies
```
isAdmin = 0
```
4. Modificamos el valor de la cookie isAdmin
```
isAdmin = 1
```
5. Refrescamos la página y obtenemos la bandera
```
picoCTF{gr4d3_A_c00k13_65fd1e1a}
```


# Roboto Sans

The flag is somewhere on this web application not necessarily on the website. Find it. Check [this](http://saturn.picoctf.net:57012/) out.

1. Ingresamos a la página
2. Revisamos el código pero no encontramos nada
3. Buscamos en el archivo robots.tx
```
User-agent *
Disallow: /cgi-bin/
Think you have seen your flag or want to keep looking.

ZmxhZzEudHh0;anMvbXlmaW
anMvbXlmaWxlLnR4dA==
svssshjweuiwl;oiho.bsvdaslejg
Disallow: /wp-admin/
```
4. Convertimos desde base64, sólo el renglón con ' \=\=' porque los otros dos tienen caracteres inválidos. Obtenemos la bandera
```
picoCTF{Who_D03sN7_L1k5_90B0T5_032f1c2b}
```


# Secrets

We have several pages hidden. Can you find the one with the flag? The website is running [here](http://saturn.picoctf.net:63457/).
folders folders folders

1. Ingresamos a la página
2. Revisamos el código. Encontramos un folder sospechosamente llamado "secret"
3. Ingresamos al folder secret
```
http://saturn.picoctf.net:55344/secret
```
4. Volvemos a revisar el código y encontramos otra carpeta llamada "hidden". Ingresamos a la carpeta
```
http://saturn.picoctf.net:55344/secret/hidden/
```
5. Encontramos otra subcarpeta llamada "superhidden"
```
http://saturn.picoctf.net:55344/secret/hidden/superhidden/
```
6. Revisamos el código fuente y encontramos la bandera
```
<h3 class="flag">picoCTF{succ3ss_@h3n1c@10n_790d2615}</h3>
```


# SQLiLite

Can you login to this website? Try to login [here](http://saturn.picoctf.net:61858/).
`admin` is the user you want to login as.

1. Ingresamos a la página
2. Intentamos ingresar con el usuario admin, pero no podemos
3. Usamos una inyección sql
```
user: admin
password: ' or 1==1;
```
4. Ingresamos
5. Revisamos el código fuente
6. Encontramos la bandera
```
picoCTF{L00k5_l1k3_y0u_solv3d_it_ec8a64c7}
```


# Unminify

I don't like scrolling down to read the code of my website, so I've squished it. As a bonus, my pages load faster! Browse [here](http://titan.picoctf.net:49277/), and find the flag!
Try CTRL+U / ⌘+U in your browser to view the page source. You can also add 'view-source:' before the URL, or try `curl <URL>` in your shell.
Minification reduces the size of code, but does not change its functionality.
What tools do developers use when working on a website? Many text editors and browsers include formatting.

1. Ingresamos a la página
2. Inspeccionar
3. Encontramos la bandera en el HTML
```
<p class="picoCTF{pr3tty_c0d3_d9c45a0b}"></p>
```


# WebDecode

Do you know how to use the web inspector? Start searching [here](http://titan.picoctf.net:53733/) to find the flag
Use the web inspector on other files included by the web page
The flag may or may not be encoded

1. Ingresamos a la página
2. Revisamos el código fuente
3. Encontramos una línea sospechosa
```
<section class="about" notify_true="cGljb0NURnt3ZWJfc3VjYzNzc2Z1bGx5X2QzYzBkZWRfZGYwZGE3Mjd9">
```
4. Desencriptamos el mensaje y obtenemos la bandera
```
picoCTF{web_succ3ssfully_d3c0ded_df0da727}
```