# Super SSH

Using a Secure Shell (SSH) is going to be pretty important.Can you `ssh` as `ctf-player` to `titan.picoctf.net` at port `61887` to get the flag?You'll also need the password `f3b61b38`. If asked, accept the fingerprint with `yes`.If your device doesn't have a shell, you can use: [https://webshell.picoctf.org](https://webshell.picoctf.org/)If you're not sure what a shell is, check out our Primer: [https://primer.picoctf.com/#the_shell](https://primer.picoctf.com/#_the_shell)
[https://linux.die.net/man/1/ssh](https://linux.die.net/man/1/ssh)
You can try logging in 'as' someone with `<user>`@titan.picoctf.net
How could you specify the port?
Remember, passwords are hidden when typed into the shell

```
ssh -p 61887 ctf-player@`titan.picoctf.net`
password: f3b61b38
```


# runme.py

Run the runme.py script to get the flag. Download the script with your browser or with wget in the webshell. Download runme.py Python script
If you have Python on your computer, you can download the script normally and run it. Otherwise, use the `wget` command in the webshell.
To use `wget` in the webshell, first right click on the download link and select 'Copy Link' or 'Copy Link Address'
Type everything after the dollar sign in the webshell: `$ wget` , then paste the link after the space after `wget` and press enter. This will download the script for you in the webshell so you can run it!
Finally, to run the script, type everything after the dollar sign and then press enter: `$ python3 runme.py` You should have the flag now!

En caso de no tener Python:
```
wget LINK
python3 ARCHIVO.py
```

En caso de sí tener Python:
```
ARCHIVO.py
```


# Codebook

Run the Python script `code.py` in the same directory as `codebook.txt`. On the webshell, use `ls` to see if both files are in the directory you are in. The `str_xor` function does not need to be reverse engineered for this challenge.

```
wget ARCHIVO.py
wget ARCHIVO.txt
ls (para comprobar que ambos estén en el mismo directorio)
python3 ARCHIVO.py (para ejecutar)
```


# convertme.py

Run the Python script and convert the given number from decimal to binary to get the flag. Look up a decimal to binary number conversion app on the web or use your computer's calculator! The `str_xor` function does not need to be reverse engineered for this challenge. If you have Python on your computer, you can download the script normally and run it. Otherwise, use the `wget` command in the webshell. To use `wget` in the webshell, first right click on the download link and select 'Copy Link' or 'Copy Link Address'. Type everything after the dollar sign in the webshell: `$ wget` , then paste the link after the space after `wget` and press enter. This will download the script for you in the webshell so you can run it! Finally, to run the script, type everything after the dollar sign and then press enter: `$ python3 convertme.py` 

```
wget LINK-ARCHIVO
python3 convertme.py
If 51 is in decimal base, what is it in binary base?
Answer: 00110011
That is correct! Here's your flag: picoCTF{4ll_y0ur_b4535_762f748e}
```


# fixme1.py

Fix the syntax error in this Python script to print the flag. Indentation is very meaningful in Python. To view the file in the webshell, do: `$ nano fixme1.py`. To exit `nano`, press Ctrl and x and follow the on-screen prompts. The `str_xor` function does not need to be reverse engineered for this challenge.

1. Descargamos el script de Python
	```
	wget LINK-ARCHIVO.py
	nano ARCHIVO.py
	```

2. Abrimos el script
	```python
import random

def str_xor(secret, key):
	#extend key to secret length
	new_key = key
	i = 0
	while len(new_key) < len(secret):
		new_key = new_key + key[i]
		i = (i + 1) % len(key)        
	return "".join([chr(ord(secret_c) ^ ord(new_key_c)) for (secret_c,new_key_c) in zip(secret,new_key)])


flag_enc = chr(0x15) + chr(0x07) + chr(0x08) + chr(0x06) + chr(0x27) + chr(0x21) + chr(0x23) + chr(0x15) + chr(0x5a) + chr(0x07) + chr(0x00) + chr(0x46) + chr(0x0b) + chr(0x1a) + chr(0x5a) +> [el código sigue para dar la bandera]

  
flag = str_xor(flag_enc, 'enkidu')
print('That is correct! Here\'s your flag: ' + flag)
	```

	Vemos que el error está en la sangría en donde se llama al ``print`` y lo corregimos.
	```python
flag = str_xor(flag_enc, 'enkidu')
print('That is correct! Here\'s your flag: ' + flag)
	```

3. Ejecutamos el programa y obtenemos la bandera.
```
python3 fixme1.py
That is correct! Here's your flag: picoCTF{1nd3nt1ty_cr1515_09ee727a}
```


# fixme2.py

Fix the syntax error in the Python script to print the flag. Are equality and assignment the same symbol? To view the file in the webshell, do: `$ nano fixme2.py` To exit `nano`, press Ctrl and x and follow the on-screen prompts. The `str_xor` function does not need to be reverse engineered for this challenge.

1. Descargamos el script y lo abrimos.
	```
	wget LINK-ARCHIVO
	nano fixme2.py
	```

2. Notamos que hay un error de sintaxis en el ``if``, así que lo corregimos.
	```python
import random

def str_xor(secret, key):
	#extend key to secret length
	new_key = key
	i = 0
	while len(new_key) < len(secret):
		new_key = new_key + key[i]
		i = (i + 1) % len(key)        
	return "".join([chr(ord(secret_c) ^ ord(new_key_c)) for (secret_c,new_key_c) in zip(secret,new_key)])


flag_enc = chr(0x15) + chr(0x07) + chr(0x08) + chr(0x06) + chr(0x27) + chr(0x21) + chr(0x23) + chr(0x15) + chr(0x58) + chr(0x18) + chr(0x11) + chr(0x41) + chr(0x09) + chr(0x5f) + chr(0x1f) +>

  
flag = str_xor(flag_enc, 'enkidu')

# Check that flag is not empty
if flag = "":
  print('String XOR encountered a problem, quitting.')
else:
  print('That is correct! Here\'s your flag: ' + flag)
	```

	```python
if flag == "":
  print('String XOR encountered a problem, quitting.')
else:
  print('That is correct! Here\'s your flag: ' + flag)
	```

3. Guardamos los cambios y ejecutamos el archivo para obtener la bandera.
```
ctrl + X
Y
ENTER
python3 fixme2.py
picoCTF{3qu4l1ty_n0t_4551gnm3nt_4863e11b}
```


# PW Crack 1

Can you crack the password to get the flag?
Download the password checker here and you'll need the encrypted flag in the same directory too.
To view the file in the webshell, do: `$ nano level1.py`
To exit `nano`, press Ctrl and x and follow the on-screen prompts.
The `str_xor` function does not need to be reverse engineered for this challenge.

1. Descargar el programa para desencriptar y la bandera encriptada.
	```
	wget LINK-ARCHIVO.py
	wget LINK-ARCHIVO.txt
	```

2. Ejecutamos el programa en Pyhton.
	```
	python3 ARCHIVO.py
	```
	Nos pide  una contraseña que en el planteamiento del problema no se menciona.

3. Abrimos el script de Pyhton para buscar la contraseña.
	```
	nano ARCHIVO.py
	```

4. Buscamos la contraseña.
	```python
### THIS FUNCTION WILL NOT HELP YOU FIND THE FLAG --LT ########################
def str_xor(secret, key):
	#extend key to secret length
	new_key = key
	i = 0
	while len(new_key) < len(secret):
		new_key = new_key + key[i]
		i = (i + 1) % len(key)        
	return "".join([chr(ord(secret_c) ^ ord(new_key_c)) for (secret_c,new_key_c) in zip(secret,new_key)])
###############################################################################

flag_enc = open('level1.flag.txt.enc', 'rb').read()

def level_1_pw_check():
	user_pw = input("Please enter correct password for flag: ")
	if( user_pw == "691d"):
		print("Welcome back... your flag, user:")
		decryption = str_xor(flag_enc.decode(), user_pw)
		print(decryption)
		return
	print("That password is incorrect")

level_1_pw_check()
	```
	Vemos que la contraseña es ``691d

5. Volvemos a ejecutar el script e ingresamos la contraseña para obtener la bandera.
	```
	python3 ARCHIVO.py
	Please enter correct password for flag: 691d
	Welcome back... your flag, user:
	picoCTF{545h_r1ng1ng_56891419}
	```


# PW Crack 2

Can you crack the password to get the flag?
Download the password checker here and you'll need the encrypted flag in the same directory too.
Does that encoding look familiar? The `str_xor` function does not need to be reverse engineered for this challenge.

1. Descargar el programa para desencriptar y la bandera encriptada.
	```
	wget LINK-ARCHIVO.py
	wget LINK-ARCHIVO.txt
	```

2.  Ejecutamos el programa en Pyhton.
	```
	python3 ARCHIVO.py
	```
	Nos pide  una contraseña que en el planteamiento del problema no se menciona.

3. Buscamos la contraseña.
	```python
### THIS FUNCTION WILL NOT HELP YOU FIND THE FLAG --LT ########################
def str_xor(secret, key):
	#extend key to secret length
	new_key = key
	i = 0
	while len(new_key) < len(secret):
		new_key = new_key + key[i]
		i = (i + 1) % len(key)        
	return "".join([chr(ord(secret_c) ^ ord(new_key_c)) for (secret_c,new_key_c) in zip(secret,new_key)])
###############################################################################

flag_enc = open('level2.flag.txt.enc', 'rb').read()

def level_2_pw_check():
	user_pw = input("Please enter correct password for flag: ")
	if( user_pw == chr(0x33) + chr(0x39) + chr(0x63) + chr(0x65) ):
		print("Welcome back... your flag, user:")
		decryption = str_xor(flag_enc.decode(), user_pw)
		print(decryption)
		return
	print("That password is incorrect")

level_2_pw_check()
	```
	Vemos que la contraseña está en hexadecimal, así que la convertimos a ASCII y obtenemos `39ce`.

4. Ejecutamos el script e ingresamos la contraseña para obtener la bandera.
	```
	python3 ARCHIVO.py
	Please enter correct password for flag: 39ce
	Welcome back... your flag, user:
	picoCTF{tr45h_51ng1ng_502ec42e}
	```


# PW Crack 3

Can you crack the password to get the flag?
Download the password checker here and you'll need the encrypted flag and the hash in the same directory too.
There are 7 potential passwords with 1 being correct. You can find these by examining the password checker script.

To view the level3.hash.bin file in the webshell, do: `$ bvi level3.hash.bin`.
To exit `bvi` type `:q` and press enter.
The `str_xor` function does not need to be reverse engineered for this challenge.

1. Descargamos la bandera encriptada, el programa que desencripta y la contraseña hash ya calculada.
	```
	wget ARCHIVO.py
	wget ARCHIVO.txt.enc
	wget ARCHIVO.hash.bin
	```

2. Abrimos el archivo con la contraseña hash.
	```
	bvi level3.hash.bin
	output: 02 6D 60 FF 9B 54 41 0B 34 35 B4 03 AF D2 26
	```

3. Abrimos el script de Python.
	```python
import hashlib

### THIS FUNCTION WILL NOT HELP YOU FIND THE FLAG --LT ########################
def str_xor(secret, key):
	#extend key to secret length
	new_key = key
	i = 0
	while len(new_key) < len(secret):
		new_key = new_key + key[i]
		i = (i + 1) % len(key)        
	return "".join([chr(ord(secret_c) ^ ord(new_key_c)) for (secret_c,new_key_c) in zip(secret,new_key)])
###############################################################################

flag_enc = open('level3.flag.txt.enc', 'rb').read()
correct_pw_hash = open('level3.hash.bin', 'rb').read()

def hash_pw(pw_str):
	pw_bytes = bytearray()
	pw_bytes.extend(pw_str.encode())
	m = hashlib.md5()
	m.update(pw_bytes)
	return m.digest()

def level_3_pw_check():
	user_pw = input("Please enter correct password for flag: ")
	user_pw_hash = hash_pw(user_pw)
	
	if( user_pw_hash == correct_pw_hash ):
		print("Welcome back... your flag, user:")
		decryption = str_xor(flag_enc.decode(), user_pw)
		print(decryption)
		return
	print("That password is incorrect")

level_3_pw_check()

# The strings below are 7 possibilities for the correct password. 
#   (Only 1 is correct)
pos_pw_list = ["8799", "d3ab", "1ea2", "acaf", "2295", "a9de", "6f3d"]
	```
	En el script se usa el algoritmo MD5 para generar un hash de cada contraseña y compararlo con un has almacenado previamente. El hash es una función que convierte datos de entrada en una cadena de longitud fija. Algunas de las funciones más comunes son MD5, SHA-1 y SHA-256. 
	Una vez que se encuentra la coincidencia, utiliza una función XOR para descifrar la bandera y la imprime. 
	
	La función ``str_xor(secret, key)`` extiende la clave para que tenga la misma longitud que el secreto y luego realiza una operación XOR entre el secreto y la clave.
	
	La función ``hash_pw(pw_str)`` convierte una contraseña en su equivalente hash MD5.

4. Automatizamos el proceso.
	```python
	def level_3_pw_check():  
	  
	# The strings below are 7 possibilities for the correct password.  
	# (Only 1 is correct)  
	pos_pw_list = ["f09e", "4dcf", "87ab", "dba8", "752e", "3961", "f159"]  
	  
	#user_pw = input("Please enter correct password for flag: ")  
	  
	for i in range(0, len(pos_pw_list)):  
	  
	user_pw = pos_pw_list[i]  
	user_pw_hash = hash_pw(user_pw)  
	  
	if( user_pw_hash == correct_pw_hash ):  
		print("Welcome back... your flag, user:")  
		decryption = str_xor(flag_enc.decode(), user_pw)  
		print(decryption)  
	return  
	  
	print("That password is incorrect")  
	  
	level_3_pw_check()
	```
	Movemos la lista de posibles contraseñas dentro de la función ``level_3_pw_check()`` y agregamos un ciclo ``for`` para que, en lugar de comparar con la contraseña ingresada por el usuario, lo haga de manera iterativa con las 7 posibles contraseñas hasta encontrar la correcta.

5. Ejecutamos el script y obtenemos la bandera.
	```
	python3 level3.py
	```


# Serpentine

Find the flag in the Python script! Try running the script and see what happens. In the webshell, try examining the script with a text editor like `nano`. To exit `nano`, press Ctrl and x and follow the on-screen prompts. The `str_xor` function does not need to be reverse engineered for this challenge.

1. Descargamos el script de Python.
	```
	gwet LINK-ARCHIVO.py
	```

2. Ejecutamos el programa.
	```
	python3 serpentine.py
	
	output:
	    Y
	  .-^-.
	 /     \      .- ~ ~ -.
	()     ()    /   _ _   `.                     _ _ _
	 \_   _/    /  /     \   \                . ~  _ _  ~ .
	   | |     /  /       \   \             .' .~       ~-. `.
	   | |    /  /         )   )           /  /             `.`.
	   \ \_ _/  /         /   /           /  /                `'
	    \_ _ _.'         /   /           (  (
	                    /   /             \  \
	                   /   /               \  \
	                  /   /                 )  )
	                 (   (                 /  /
	                  `.  `.             .'  /
	                    `.   ~ - - - - ~   .'
	                       ~ . _ _ _ _ . ~
	
	Welcome to the serpentine encourager!
	
	
	a) Print encouragement
	b) Print flag
	c) Quit
	
	What would you like to do? (a/b/c) a
	
	-----------------------------------------------------
	Look how far you've come!
	-----------------------------------------------------
	
	
	a) Print encouragement
	b) Print flag
	c) Quit
	
	What would you like to do? (a/b/c) b
	
	Oops! I must have misplaced the print_flag function! Check my source code!
	
	
	a) Print encouragement
	b) Print flag
	c) Quit
	
	What would you like to do? (a/b/c) c
	```

3. Abrimos el script
	```
	nano serpentine.py
	```

	```python
import random
import sys

def str_xor(secret, key):
	#extend key to secret length
	new_key = key
	i = 0
	while len(new_key) < len(secret):
		new_key = new_key + key[i]
		i = (i + 1) % len(key)        
	return "".join([chr(ord(secret_c) ^ ord(new_key_c)) for (secret_c,new_key_c) in zip(secret,new_key)])

flag_enc = chr(0x15) + chr(0x07) + chr(0x08) + chr(0x06) + chr(0x27) + chr(0x21) + chr(0x23) + chr(0x15) + chr(0x5c) + chr(0x01) + chr(0x57) + chr(0x2a) + chr(0x17) + chr(0x5e) + chr(0x5f) +>

def print_flag():
	flag = str_xor(flag_enc, 'enkidu')
	print(flag)

def print_encouragement():
	encouragements = ['You can do it!', 'Keep it up!', 
					'Look how far you\'ve come!']
	choice = random.choice(range(0, len(encouragements)))
	print('\n-----------------------------------------------------')
	print(encouragements[choice])
	print('-----------------------------------------------------\n\n')

def main():
	print(
	'''
	Y
	 .-^-.
	/     \      .- ~ ~ -.
	()     ()    /   _ _   `.                     _ _ _
	\_   _/    /  /     \   \                . ~  _ _  ~ .
	| |     /  /       \   \             .' .~       ~-. `.
	| |    /  /         )   )           /  /             `.`.
	\ \_ _/  /         /   /           /  /                `'
	\_ _ _.'         /   /           (  (
					/   /             \  \\
				   /   /               \  \\
				  /   /                 )  )
				 (   (                 /  /
				  `.  `.             .'  /
					`.   ~ - - - - ~   .'
					   ~ . _ _ _ _ . ~
	'''
	)
	print('Welcome to the serpentine encourager!\n\n')
	
	while True:
	print('a) Print encouragement')
	print('b) Print flag')
	print('c) Quit\n')
	choice = input('What would you like to do? (a/b/c) ')
	
	if choice == 'a':
		  print_encouragement()
	elif choice == 'b':
		  print('\nOops! I must have misplaced the print_flag function! Check my source code!\n\n')
	elif choice == 'c':
		  sys.exit(0)
	else:
		print('\nI did not understand "' + choice + '", input only "a", "b" or "c"\n\n')

if __name__ == "__main__":
	main()
	```

4. Modificamos la acción de la opción ``b`` para que llame a la función ``print_flag()`` y obtener la bandera decodificada.
	```python
	if choice == 'a':
		  print_encouragement()
	elif choice == 'b':
		  print_flag()
	elif choice == 'c':
		  sys.exit(0)
	else:
		print('\nI did not understand "' + choice + '", input only "a", "b" or "c"\n\n')
	```

5. Obtener la bandera.
```
What would you like to do? (a/b/c) b
picoCTF{7h3_r04d_l355_7r4v3l3d_ae0b80bd}
```
