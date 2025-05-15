# The numbers

The Numbers picoCTF Crypto

The flag is in the format PICOCTF{}

1. Descargamos el archivo
```
wget https://jupiter.challenges.picoctf.org/static/f209a32253affb6f547a585649ba4fda/the_numbers.png
```

2. Abrimos la imagen y vemos que contiene una secuencia de números y caracteres
```
16 9 3 15 3 20 6 { 20 8 5 14 21 13 2 5 18 19 13 1 19 15 14 }
```

3. Parece que cada número corresponde a una letra. Hacemos un script de python para probar
```python
enc_flag = "16 9 3 15 3 20 6 { 20 8 5 14 21 13 2 5 18 19 13 1 19 15 14 }".split()
ALPHABET = "ABCDEFGHIJKLMNOPQRSTUVWXYZ"
flag = "".join(ALPHABET[int(num)-1] if num.isnumeric() else num for num in enc_flag)
print(flag)
```

4. Encontramos la bandera
```
python cyrpto_the_numbers.py

PICOCTF{THENUMBERSMASON}
```



# 13

Cryptography can be easy, do you know what ROT13 is? `cvpbPGS{abg_gbb_onq_bs_n_ceboyrz}`

This can be solved online if you don't want to do it by hand!

1. Parece que la cadena que nos dan está cifrada en ROT13. Usamos un script en python
```python
import codecs
texto_cifrado = "cvpbPGS{abg_gbb_onq_bs_n_ceboyrz}"
texto_descifrado = codecs.decode(texto_cifrado, "rot_13")
print(texto_descifrado)
```

2. Obtenemos la bandera
```
python3 crypto_13.py
picoCTF{not_too_bad_of_a_problem}
```



# Cesar

Decrypt this [message](https://jupiter.challenges.picoctf.org/static/6385b895dcb30c74dbd1f0ea271e3563/ciphertext).

caesar cipher [tutorial](https://learncryptography.com/classical-encryption/caesar-cipher)

El **cifrado César** es un método de sustitución en el que cada letra del texto original se desplaza un número fijo de posiciones en el alfabeto. Es uno de los cifrados más simples y fue utilizado por **Julio César** para proteger mensajes militares.

1. Descargamos el archivo
```
wget https://jupiter.challenges.picoctf.org/static/6385b895dcb30c74dbd1f0ea271e3563/ciphertext
```

2. Abrimos el archivo y observamos.
```
cat ciphertext

picoCTF{dspttjohuifsvcjdpoabrkttds}
```

3. Usamos un script de python
```python
import string
import re

abc = string.ascii_letters

encriptado = open('ciphertext', 'r').read()
encriptado = re.findall('\{(.*?)\}', encriptado)[0]

rot = 25
salida = ''

for car in encriptado:
	salida += abc[(abc.find(car) + rot) % 26]

print(salida)
```

4. Obtenemos la bandera
```
picoCTF{crossingtherubiconzaqjsscr}
```



# Easy 1

The one time pad can be cryptographically secure, but not when you know the key. Can you solve this? We've given you the encrypted flag, key, and a table to help `UFJKXQZQUNB` with the key of `SOLVECRYPTO`. Can you use this [table](https://jupiter.challenges.picoctf.org/static/1fd21547c154c678d2dab145c29f1d79/table.txt) to solve it?.

Submit your answer in our flag format. For example, if your answer was 'hello', you would submit 'picoCTF{HELLO}' as the flag.
Please use all caps for the message.

Se debe buscar cada letra del texto cifrado en la tabla usando la clave. Encontramos la letra correspondiente en la parte superior de la columna. Y obtenemos el mensaje oculto

```
cat table.txt

    A B C D E F G H I J K L M N O P Q R S T U V W X Y Z 
   +----------------------------------------------------
A | A B C D E F G H I J K L M N O P Q R S T U V W X Y Z
B | B C D E F G H I J K L M N O P Q R S T U V W X Y Z A
C | C D E F G H I J K L M N O P Q R S T U V W X Y Z A B
D | D E F G H I J K L M N O P Q R S T U V W X Y Z A B C
E | E F G H I J K L M N O P Q R S T U V W X Y Z A B C D
F | F G H I J K L M N O P Q R S T U V W X Y Z A B C D E
G | G H I J K L M N O P Q R S T U V W X Y Z A B C D E F
H | H I J K L M N O P Q R S T U V W X Y Z A B C D E F G
I | I J K L M N O P Q R S T U V W X Y Z A B C D E F G H
J | J K L M N O P Q R S T U V W X Y Z A B C D E F G H I
K | K L M N O P Q R S T U V W X Y Z A B C D E F G H I J
L | L M N O P Q R S T U V W X Y Z A B C D E F G H I J K
M | M N O P Q R S T U V W X Y Z A B C D E F G H I J K L
N | N O P Q R S T U V W X Y Z A B C D E F G H I J K L M
O | O P Q R S T U V W X Y Z A B C D E F G H I J K L M N
P | P Q R S T U V W X Y Z A B C D E F G H I J K L M N O
Q | Q R S T U V W X Y Z A B C D E F G H I J K L M N O P
R | R S T U V W X Y Z A B C D E F G H I J K L M N O P Q
S | S T U V W X Y Z A B C D E F G H I J K L M N O P Q R
T | T U V W X Y Z A B C D E F G H I J K L M N O P Q R S
U | U V W X Y Z A B C D E F G H I J K L M N O P Q R S T
V | V W X Y Z A B C D E F G H I J K L M N O P Q R S T U
W | W X Y Z A B C D E F G H I J K L M N O P Q R S T U V
X | X Y Z A B C D E F G H I J K L M N O P Q R S T U V W
Y | Y Z A B C D E F G H I J K L M N O P Q R S T U V W X
Z | Z A B C D E F G H I J K L M N O P Q R S T U V W X Y

```

1. Hacemos un script en python
```python
def vigenere_decrypt(ciphertext, key):
    alphabet = "ABCDEFGHIJKLMNOPQRSTUVWXYZ"
    decrypted_text = ""
    key_repeated = (key * (len(ciphertext) // len(key) + 1))[:len(ciphertext)]

    for c, k in zip(ciphertext, key_repeated):
        decrypted_text += alphabet[(alphabet.index(c) - alphabet.index(k)) % 26]

    return decrypted_text

mensaje_cifrado = "UFJKXQZQUNB"
clave = "SOLVECRYPTO"
print("Texto descifrado:", vigenere_decrypt(mensaje_cifrado, clave))
```

2. Obtenemos la bandera
```
picoCTF{CRYPTOISFUN}
```



# la cifra de

I found this cipher in an old book. Can you figure out what it says? Connect with `nc jupiter.challenges.picoctf.org 58295`.

There are tools that make this easy.
Perhaps looking at history will help

1. Nos conectamos al puerto
```
nc jupiter.challenges.picoctf.org 58295
Encrypted message:
Ne iy nytkwpsznyg nth it mtsztcy vjzprj zfzjy rkhpibj nrkitt ltc tnnygy ysee itd tte cxjltk

Ifrosr tnj noawde uk siyyzre, yse Bnretèwp Cousex mls hjpn xjtnbjytki xatd eisjd

Iz bls lfwskqj azycihzeej yz Brftsk ip Volpnèxj ls oy hay tcimnyarqj dkxnrogpd os 1553 my Mnzvgs Mazytszf Merqlsu ny hox moup Wa inqrg ipl. Ynr. Gotgat Gltzndtg Gplrfdo 

Ltc tnj tmvqpmkseaznzn uk ehox nivmpr g ylbrj ts ltcmki my yqtdosr tnj wocjc hgqq ol fy oxitngwj arusahje fuw ln guaaxjytrd catizm tzxbkw zf vqlckx hizm ceyupcz yz tnj fpvjc hgqqpohzCZK{m311a50_0x_a1rn3x3_h1ah3xf966878l}

Tnj qixxe wkqw-duhfmkseej ipsiwtpznzn uk l puqjarusahjeii htpnjc hubpvkw, hay rldk fcoaso 1467 be Qpot Gltzndtg Fwbkwei.

Zmp Volpnèxj Nivmpr ox ehkwpfuwp surptorps ifwlki ehk Fwbkwei Jndc uw Llhjcto Htpnjc.

It 1508, Ozhgsyey Ycizmpmozd itapnzjo tnj do-ifwlki eahzwa xjntg (f xazwtx uk dhokeej fwpnfmezx) ehgy hoaqo lgypr hj l cxneiifw curaotjyt uk ehk Atgksèce Inahkw.

Merqlsu’x deityd htzkrje avupaxjo it 1555 fd a itytosfaznzn uk ehk ktryy. Ehk qzwkw saraps uk ehk fwpnfmezx lrk szw ymtfzjo rklflgwwy, hze tnj llvmlbkyd ati ehk nydkc wezypry fce sniej gj mkfys uk l mtjxotnn kkd ahxfde, cmtcn hln hj oilkprkse woys eghs cuwceyuznjjyt.
```

Parece que la bandera está cifrada
```
hgqqpohzCZK{m311a50_0x_a1rn3x3_h1ah3xf966878l}
```

2. Usamos una receta en CyberChef
```
Recipe: Vigenère Decode
Kev: flag

Output: picoCTF{b311a50_0r_v1gn3r3_c1ph3ra966878a}
```



# Tapping

Theres tapping coming in from the wires. What's it saying `nc jupiter.challenges.picoctf.org 48247`.

What kind of encoding uses dashes and dots?
The flag is in the format PICOCTF{}

1. Nos conectamos al pueto
```
nc jupiter.challenges.picoctf.org 48247
.--. .. -.-. --- -.-. - ..-. { -- ----- .-. ... ...-- -.-. ----- -.. ...-- .---- ... ..-. ..- -. .---- ..--- -.... .---- ....- ...-- ---.. .---- ---.. .---- } 
```

2. Hacemos un script en Python
```python
morse_dict = {
    ".-": "A", "-...": "B", "-.-.": "C", "-..": "D", ".": "E",
    "..-.": "F", "--.": "G", "....": "H", "..": "I", ".---": "J",
    "-.-": "K", ".-..": "L", "--": "M", "-.": "N", "---": "O",
    ".--.": "P", "--.-": "Q", ".-.": "R", "...": "S", "-": "T",
    "..-": "U", "...-": "V", ".--": "W", "-..-": "X", "-.--": "Y",
    "--..": "Z", "-----": "0", ".----": "1", "..---": "2", "...--": "3",
    "....-": "4", ".....": "5", "-....": "6", "--...": "7", "---..": "8",
    "----.": "9"
}

def morse_decode(morse_code):
    return "".join(morse_dict.get(code, code) for code in morse_code.split())

mensaje_cifrado = ".--. .. -.-. --- -.-. - ..-. { -- ----- .-. ... ...-- -.-. ----- -.. ...-- .---- ... ..-. ..- -. .---- ..--- -.... .---- ....- ...-- ---.. .---- ---.. .---- }"
print("Texto descifrado:", morse_decode(mensaje_cifrado))
```

3. Encontramos la bandera
```
python3 crypto_morse.py

Texto descifrado: PICOCTF{M0RS3C0D31SFUN1261438181}
```



# waves over lambda 

We made a lot of substitutions to encrypt this. Can you decrypt it? Connect with `nc jupiter.challenges.picoctf.org 13758`.

Flag is not in the usual flag format

1. Nos conectamos al puerto
```
nc jupiter.challenges.picoctf.org 13758
-------------------------------------------------------------------------------
uxhwiltp ayiy gp kxmi zdlw - ziyvmyhuk_gp_u_xqyi_dlnfol_ohqtzitlkm
-------------------------------------------------------------------------------
ldyryk zkxoxixqgtua elilnlcxq slp tay tagio pxh xz zkxoxi blqdxqgtua elilnlcxq, l dlho xshyi sydd ehxsh gh xmi ogptigut gh agp xsh olk, lho ptgdd iynynfyiyo lnxhw mp xsghw tx agp wdxxnk lho tilwgu oylta, sagua albbyhyo tagityyh kylip lwx, lho sagua g paldd oypuigfy gh gtp bixbyi bdluy. zxi tay biypyht g sgdd xhdk plk talt tagp dlhoxshyizxi px sy mpyo tx uldd agn, ldtaxmwa ay aliodk pbyht l olk xz agp dgzy xh agp xsh yptltyslp l ptilhwy tkby, kyt xhy biyttk ziyvmyhtdk tx fy nyt sgta, l tkby lfjyut lho qgugxmp lho lt tay plny tgny pyhpydypp. fmt ay slp xhy xz taxpy pyhpydypp byipxhp sax liy qyik sydd ulblfdy xz dxxeghw lztyi taygi sxidodk lzzlgip, lho, lbbliyhtdk, lztyi hxtaghw ydpy. zkxoxi blqdxqgtua, zxi ghptlhuy, fywlh sgta hyrt tx hxtaghw; agp yptlty slp xz tay pnlddypt; ay ilh tx oghy lt xtayi nyh'p tlfdyp, lho zlptyhyo xh tayn lp l txlok, kyt lt agp oylta gt lbbyliyo talt ay alo l amhoiyo taxmplho ixmfdyp gh alio ulpa. lt tay plny tgny, ay slp ldd agp dgzy xhy xz tay nxpt pyhpydypp, zlhtlptguld zyddxsp gh tay saxdy ogptigut. g iybylt, gt slp hxt ptmbgogtktay nljxigtk xz taypy zlhtlptguld zyddxsp liy paiyso lho ghtyddgwyht yhxmwafmt jmpt pyhpydypphypp, lho l byumdgli hltgxhld zxin xz gt.
```

2. Copiamos el texto. Usamos [guballa.de](https://guballa.de/) para Substitution Solver
```
nc jupiter.challenges.picoctf.org 13758 | xclip -selection c
```

3. Obtenemos la bandera, recordemos que no está en el formato tradicional
```
Key
abcdefghijklmnopqrstuvwxyz     This clear text ...  seynawcjvrqfkobzltgmupihxd     ... maps to this cipher text
```

```
congrats here is your flag - frequency_is_c_over_lambda_dnvtfrtayu
```



# interencdec

Can you get the real meaning from this file. Download the file here. 

Engaging in various decoding processes is of utmost importance

1. Descargamos el archivo
```
wget https://artifacts.picoctf.net/c_titan/108/enc_flag
```

2. Abrimos el archivo
```
cat enc_flag

YidkM0JxZGtwQlRYdHFhR3g2YUhsZmF6TnFlVGwzWVROclgyZzBOMm8yYXpZNWZRPT0nCg==
```

3. Lo convertimos desde base 64
```
base64 -d enc_flag

b'd3BqdkpBTXtqaGx6aHlfazNqeTl3YTNrX2g0N2o2azY5fQ=='
```

4. Extraemos la subcadena entre comillas y la volvemos a convertir desde base 64
```
echo "d3BqdkpBTXtqaGx6aHlfazNqeTl3YTNrX2g0N2o2azY5fQ==" | base64 -d 

wpjvJAM{jhlzhy_k3jy9wa3k_h47j6k69}
```

5. El resultado lo convertimos desde chiper caesar
```python
def caesar_decrypt(ciphertext, shift):
    decrypted_text = ""
    for char in ciphertext:
        if char.isalpha():
            shifted = ord(char) - shift
            if char.islower():
                decrypted_text += chr(shifted) if shifted >= ord('a') else chr(shifted + 26)
            else:
                decrypted_text += chr(shifted) if shifted >= ord('A') else chr(shifted + 26)
        else:
            decrypted_text += char
    return decrypted_text

mensaje_cifrado = "wpjvJAM{jhlzhy_k3jy9wa3k_h47j6k69}"
for shift in range(1, 26):
    print(f"Desplazamiento {shift}: {caesar_decrypt(mensaje_cifrado, shift)}")
```

6. Obtenemos la bandera
```
Desplazamiento 7: picoCTF{caesar_d3cr9pt3d_a47c6d69}
```