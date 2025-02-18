
# Big Zip

Unzip this archive and find the flag.

Can grep be instructed to look at every file in a directory and its subdirectories?

1. Descargamos el archivo
	```
	gwet LINK-ARCHIVO
	```

2. Desempaquetamos el archivo y mostramos el contenido en pantalla. Si no se usa `-p` sólo se mostraran los subdirectorios, pero no el contenido.
	```
	unzip -p NOMBRE-ARCHIVO.zip
	```

3. Buscamos la bandera
	```
	| grep pico
	```


# First Find

Unzip this archive and find the file named 'uber-secret.txt'

1. Descargamos el archivo
	```
	gwet LINK-ARCHIVO
	```

2. Desempaquetamos el archivo y lo guardamos en un directorio destino.
	```
	unzip files.zip -d directorio_destino
	```

3. Buscamos el archivo .txt en los subdirectorios y extraemos la bandera.
	```
	find directorio_destino -type f -name "archivo.txt" -exec grep -i "pico" {} \;
	```
Buscamos recursivamente en cada subdirectorio  el archivo .txt y por cada archivo encontrado se ejecuta grep para buscar la palabra "pico". -i asegura que no distinga mayúsculas de nimúsculas.


# Magikarp Ground Mission

Do you know how to move between directories and read files in the shell? Start the container, `ssh` to it, and then `ls` once connected to begin. Login via `ssh` as `ctf-player` with the password, `6d448c9c`

Finding a cheatsheet for bash would be really helpful!

1. Conectar por SSH con el usuario, puerto y contraseña designados.
	```
	ssh [url/ctf-player@venus.picoctf.net] -p [puero/52762]
	password: 6d448c9c
	```

2. Usar ls para listar los archivos
	```
	ls
	```

3. Se encontraron dos archivos: 
	1of3.flag.txt
	instructions-to-2of3.txt
	Podemos entender que necesitaremos buscar en 3 archivos para encontrar la bandera.

4. Buscamos en el primer archivo
	```
	cat 1of3.flag.txt
	output: picoCTF{xxsh_
	```

5. Buscamos en el otro archivo
	```
	cat instructions-to-2of3.txt
	output: Next, go to the root of all things, more succinctly `/`
	```

6. Listamos lo que haya en la raíz del sistema de archivos
	```
	ls /
	output: 2of3.flag.txt  bin  boot  dev  etc  home  instructions-to-3of3.txt  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
	```

7. Nos movemos a la raíz
	```
	cd /
	```

8. Buscamos entro de 2of3.flag.txt
	```
	cat 2of3.flag.txt
	output: 0ut_0f_\/\/4t3r_
	```

9. Abrimos instructions-to-3of3.txt
	```
	cat instructions-to-3of3.txt
	output: Lastly, ctf-player, go home... more succinctly `~`
	```

10. Nos movemos al directorio Home.
	```
	cd ~
	output: 3of3.flag.txt
	```

11. Abrimos el último archivo para terminar de formar la bandera.
```
cat 3of3.flag.txt
output: 5190b070}
```

``picoCTF{xxsh_0ut_0f_\/\/4t3r_5190b070}


# repetitions

Can you make sense of this file? Multiple decoding is always good.

1. Descargamos el archivo
	```
	gwet LINK
	```

2. En este caso el archivo tiene una extensión desconocida, así que copiamos el contenido y lo guardamos en un archivo txt.
	```
	cat enc_flag > encoded.txt
	```

3. Este ejercicio requiere decodificar varias veces el archivo. Nos ayudamos de un bucle.
	```
	while true; do
	    base64 --decode encoded.txt > decoded.txt 2>/dev/null
	    if grep -iq "pico" decoded.txt; then
	        echo "Flag found!"
	        cat decoded.txt
	        break
	    fi
	    mv decoded.txt encoded.txt
	done
	
	```
Iniciamos un bucle infinito que se ejecutará hasta encontrar la palabra clave "pico". Decodificamos en base 64 lo almacenado en enconded.txt y lo guardamos en decoded.txt. 2>/dev/null asegura que cualquier mensaje de error sea ignorado y no se muestre en la terminal, esto resulta útil para la sobreescritura de archivos. Verificamos si "pico" está en el archivo descifrado. Si se encuentra la bandera, se imprime "Flag found!" y el contenido descifrado que debería ser la bandera. Se interrumpe la ejecución con break. Si la palabra no fue encontrada, mv sobreescribe el contenido de decoded.txt en encoded.txt para seguir con la siguiente iteración. done termina el bucle.

4. Obtenemos la bandera.
```
Flag found!
picoCTF{base64_n3st3d_dic0d!n8_d0wnl04d3d_73494190}
```


# Static ain't always noise

Can you look at the data in this binary: static? This BASH script might help!

```
wget static
wget BASH
file static
chmod +x static
./static
cat ltdis.sh
bash ltdis.sh
nano ltdis.sh
bash ltdis.sh static
cat static.ltdis.x86_64.txt | grep pico
cat static.ltdis.strings.txt | grep pico
```

Otra forma:
``strings static | grep pico``



# Based

To get truly 1337, you must understand different data encodings, such as hexadecimal or binary. Can you get the flag from this program to prove you are on the way to becoming 1337? Connect with  `nc jupiter.challenges.picoctf.org 15130`. I hear python can convert things. It might help to have multiple windows open.

Como primera opción se podría usar CyberChef para cambiar entre bases. 

Como segunda opción se pueden usar scripts en Python.

Convertir de binario a ASCII
```python
def binary_to_ascii(binary_string):
    binary_values = binary_string.split()  
    ascii_text = ''.join(chr(int(b, 2)) for n in binary_values)  
    return ascii_text

binary_input = "01110011 01101100 01110101 01100100 01100111 01100101" # sludge
print(binary_to_ascii(binary_input))
```
- El binario requiere primero dividir la cadena en bloques de **8 bits (1 byte)**.
- Luego, cada bloque binario se convierte a decimal (`int(b, 2)`) y después a ASCII (`chr()`).

Convertir de octal a ASCII
```python
def octal_to_ascii(octal_string):
	octal_values = octal_string.split()  
	ascii_text = ''.join(chr(int(b, 8)) for n in octal_values) 
	return ascii_text

octal_input = "143 150 141 151 162" # chair
print(octal_to_ascii(octal_input))
```
- En octal cada **3 dígitos octales** forman **1 byte**.
- Se usa `int(octal_string, 8)` para convertir a decimal y luego `chr()`.

Convertir de hexadecimal a ASCII
```python
def hex_to_ascii(hex_string):
	ascii_text = bytes.fromhex(hex_string).decode('utf-8')  
	return ascii_text

hex_input = "736c75646765" # sludge
print(hex_to_ascii(hex_input))
```
- Hexadecimal es una representación compacta donde cada **2 dígitos** representan **1 byte (8 bits)**.
- Python proporciona un método directo: bytes.fromhex(hex_string).decode('utf-8')

- **Hexadecimal ya está alineado con los bytes** (cada 2 dígitos = 1 byte), así que `bytes.fromhex()` lo convierte directamente sin necesidad de dividir la cadena.
- **Binario y octal requieren conversión manual** porque sus agrupaciones **no siempre coinciden con los bytes** usados en ASCII.

# Strings it

Can you find the flag in file without running it? 
https://linux.die.net/man/1/strings

```
wget https://jupiter.challenges.picoctf.org/static/5bd86036f013ac3b9c958499adf3e2e2/strings
strings -n 10 strings | grep pico
```


# Wave a flag

Can you invoke help flags for a tool or binary? This program has extraordinarily helpful information...

```wget https://mercury.picoctf.net/static/fc1d77192c544314efece5dd309092e3/warm
file warm
chmod +x warm
./warm
./warm -h
```

El comando -h nos ayuda a dirigirnos a help.


# Useless

There's an interesting script in the user's home directoryThe work computer is running SSH. We've been given a script which performs some basic calculations, explore the script and find a flag. 

```
ssh -p 62098 picoplayer@saturn.picoctf.net
ls 
./useless
ls -la
cat useless
./useless hola 1 2
man useless
```

Al revisar el código, vemos que si se envía alguna entrada invalida, nos dice que revisemos el manual, para ello usamos el comando man.


# Tab, Tab, Attack

Using tabcomplete in the Terminal will add years to your life, esp. when dealing with long rambling directory structures and filenames: Addadshashanammu.zip

```
wget LINK

ls 

file A \[tab] 
output: Addadshashanammu.zip: Zip archive data, at least v1.0 to extract, compression method=store

ls -R
output: Muestra todos los archivos.

unzip \[tab]

cd A\[tab] hasta que ya no se pueda

ls

./[tab] para ejecutar
```


o también
```
unzip Addadshashanammu.zip
strings \[ruta] | grep pico
```

rm -rf /
NUNCA USAR. BORRA TODO LO QUE HAYA EN LA COMPUTADORA




