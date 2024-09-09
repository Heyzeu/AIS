# Algorythmes
### Les algorithmes suivants ne devraient plus être utilisés :

    
* AES 128 avec ECB 
* 3DES : 
* SHA1 
* MD5 
* RSA avec PKCS1 

### Algorithmes sûrs
* AES 256 avec mécanisme XTS
* SHA2
* RSA avec OAEP

## Grâce à un script, générer une clé de chiffrement **AES256** ainsi que les IV avec le destinataire. Partagez là avec votre destinataire en essayant de préserver sa confidentialité.

### Script en powershell

#### Génération de la clef
```powershell
# Générer une clé AES-256
$key = [System.Security.Cryptography.Aes]::Create()
$key.KeySize = 256
$key.GenerateKey()
$key.GenerateIV()

# Convertir la clé et l'IV en base64 pour faciliter leur manipulation
$keyBase64 = [Convert]::ToBase64String($key.Key)
$ivBase64 = [Convert]::ToBase64String($key.IV)

# Afficher la clé et l'IV
Write-Output "Clé AES-256 : $keyBase64"
Write-Output "IV : $ivBase64"

# Sauvegarder la clé et l'IV dans des fichiers
$keyBase64 | Out-File -FilePath "C:\Faes_key.txt"
$ivBase64 | Out-File -FilePath "C:\aes_iv.txt"

```
## Comment générer une **clé de chiffrement** de manière sure ? Quel est le risque si les **IV** sont toujours les mêmes ?

Cette commande génère une clé sécurisée en utilisant l'algorithme AES:

$key = [System.Security.Cryptography.Aes]::Create().Key

Si l'IV est toujours le même, cela présente plusieurs risques, comme :

* Faiblesse contre les attaques : Un attaquant pourrait identifier des motifs et déduire des informations sur les données chiffrées.
 
* Chiffrement déterministe : Si les mêmes données sont chiffrées avec la même clé et le même IV, le résultat sera toujours identique. Cela permet à un attaquant de deviner si des données spécifiques ont été chiffrées.

Il est essentiel de générer un IV aléatoire à chaque chiffrement et de le transmettre avec les données chiffrées.
## Comment pourrait-on s'assurer de l'intégrité du message et de l'authenticité du destinataire ?

* Signature numérique : L'émetteur signe le message avec sa clé privée pour assurer son authenticité.
* HMAC : Un HMAC est ajouté pour garantir que le message n'a pas été modifié (intégrité).
* Clé publique : Le destinataire utilise la clé publique de l'émetteur pour vérifier la signature.

Ainsi, on s'assure que :

* Authenticité : Le message vient bien du bon émetteur (via signature numérique).
* Intégrité : Le message n'a pas été modifié (via HMAC ou signature).

## le message suivant a été intercepté: `"prggr grpuavdhr f'nccryyr yr puvsserzrag qr prnfre, vy a'rfg cyhf hgvyvft nhwbheq'uhv, pne crh ftphevft"`, il semble vulnérable à une attaque en fréquences ou une attaque par force brute. Déchiffrez-le !

Message: "cette technique s'appelle le chiffrement de ceaser, il n'est plus utilisg aujourd'hui, car peu sgcurisg"

https://www.dcode.fr/chiffre-cesar

## Nous suspectons qu'un adversaire a implémenté une backdoor dans notre logiciel de messagerie sécurisé, pourtant nous utilisons AES-CBC (extrait des logs ci-dessous). Trouvez ce qui ne convient pas dans le chiffrement de ces messages.

```txt
Bob: '>s\x06\x14\x0c\xa7\xa6\x88\xd5[+i\xcc/J\xf7'
Alice: "3\x01\xeb\xcah\xf6\x1f\xc2[\xf9}P'A\xe0\xd5"
Bob: '\xf7\xb0\xc5\xccO\xab&\xee\xa4&6N?V\xbd\x85\x94b\xee\xc5\x18\x1f9\xe7\xe5\xe0\xffyf\xab\xfb\xb9
Alice: '\xde@=\x1ed\xc0Qe\x0fK=\x1c\xb3$\xd9\xcb'
Bob: '\xce\xbf\x0e\\\x8aX\x1c \xb2v\x97\xf5<\x86M\x86\x0c\xa1j\xa0\xe6\xa9\x11\xf9AyZ\xda9\x94ec'
Alice: "\xde@=\x1ed\xc0Qe\x0fK=\x1c\xb3$\xd9\xcb"
Bob: '\xfb\x0cc\xb0/\xd4:\xde\xe7a\x95_L\x8d\x108\xac\xff\xcep\x8e&\xcfq6ym\x0c\xf6\xccI\xed'
Alice: '\xee\xcb\xd0\x9aRt;\x12\xca\xfe\r\x01MN>\xde'
Bob: '\xab\x8aX\xef\xd4\xf3\x88a\x1a\x96\r\xec\x17\xe6s"\x94\xec6\xe0\xff \x82\xa1\xb4\xe2\xc1\x08\r!T\x89\xe2B\x1d^\xf7l\xd8\xc9\xa4\xcd\xa5\x8e\xb3\x1d\x1f\xe7'
Alice: '\xee\xcb\xd0\x9aRt;\x12\xca\xfe\r\x01MN>\xde'
Alice: '\x1f\xafV4\xcb\x116N\xc5.\xa8\xdfM\xcf\xda\x02\x98\xbb\x04\x04C}N{\xf95\x05e\xc6\xf9\xbe,'
Bob: '\xde@=\x1ed\xc0Qe\x0fK=\x1c\xb3$\xd9\xcb'
```
Bob et Alice utilisent la même clef, on peut remarquer des messages similaires.

## Nous avons intercepté le message suivant: `b'\xd72U\xc03.\xda\x99Q\xb5\x020\xc4\xb8\x16\xc6\xfa-\xb9U+\xda\\\x126L\xf3~\xbd8\x12q\x02?\x80\xeaVI\xa9\xe1'`. 

La première partie de la **Clé de 16 octets** est: `b'12345678bien'` et l'algorithme utilisé est celui-ci:

```python
from cryptography.hazmat.primitives.ciphers import Cipher, algorithms, modes
from cryptography.hazmat.primitives import padding
from cryptography.hazmat.backends import default_backend

def des_encrypt(key, plaintext):
    cipher = Cipher(algorithms.TripleDES(key), modes.ECB(), backend=default_backend())
    encryptor = cipher.encryptor()
    padder = padding.PKCS7(64).padder()
    padded_data = padder.update(plaintext) + padder.finalize()
    ct = encryptor.update(padded_data) + encryptor.finalize()
    return ct

def des_decrypt(key, ciphertext):
    cipher = Cipher(algorithms.TripleDES(key), modes.ECB(), backend=default_backend())
    decryptor = cipher.decryptor()
    pt = decryptor.update(ciphertext) + decryptor.finalize()
    unpadder = padding.PKCS7(64).unpadder()
    unpadded_data = unpadder.update(pt) + unpadder.finalize()
    return unpadded_data
```

Quel était le message transmis ?

Message déchiffré: DES n'est plus sur de nors jours!

Voici mon code python:
```python
from cryptography.hazmat.primitives.ciphers import Cipher, algorithms, modes
from cryptography.hazmat.primitives import padding
from cryptography.hazmat.backends import default_backend

def des_decrypt(key, ciphertext):
    cipher = Cipher(algorithms.TripleDES(key), modes.ECB(), backend=default_backend())
    decryptor = cipher.decryptor()
    pt = decryptor.update(ciphertext) + decryptor.finalize()
    unpadder = padding.PKCS7(64).unpadder()
    unpadded_data = unpadder.update(pt) + unpadder.finalize()
    return unpadded_data
ciphertext = b'\xd72U\xc03.\xda\x99Q\xb5\x020\xc4\xb8\x16\xc6\xfa-\xb9U+\xda\\\x126L\xf3~\xbd8\x12q\x02?\x80\xeaVI\xa9\xe1'

#déduction de la suite de la clef en fonction des 8 octets restants
key = b'12345678bienjoue'


plaintext = des_decrypt(key, ciphertext)
print("Message déchiffré:", plaintext.decode('utf-8')) 
```


Mon code python qui génère la clef partiel automatiquement

```python

from cryptography.hazmat.primitives.ciphers import Cipher, algorithms, modes
from cryptography.hazmat.primitives import padding
from cryptography.hazmat.backends import default_backend

def des_decrypt(key, ciphertext):
    cipher = Cipher(algorithms.TripleDES(key), modes.ECB(), backend=default_backend())
    decryptor = cipher.decryptor()
    pt = decryptor.update(ciphertext) + decryptor.finalize()
    unpadder = padding.PKCS7(64).unpadder()
    unpadded_data = unpadder.update(pt) + unpadder.finalize()
    return unpadded_data

ciphertext = b'\xd72U\xc03.\xda\x99Q\xb5\x020\xc4\xb8\x16\xc6\xfa-\xb9U+\xda\\\x126L\xf3~\xbd8\x12q\x02?\x80\xeaVI\xa9\xe1'

partial_key = b'12345678bien'  

def find_key():
    characters = "abcdefghijklmnopqrstuvwxyz"
    
    for c1 in characters:
        for c2 in characters:
            for c3 in characters:
                for c4 in characters:
                    yield c1 + c2 + c3 + c4


for key_suffix in find_key():
    key = partial_key + key_suffix.encode()  
    
    try:
        decrypted_message = des_decrypt(key, ciphertext)
        print(f"Key: {key_suffix} - Message: {decrypted_message}")
    except Exception as e:
        
        continue
