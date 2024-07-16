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

## Comment pourrait-on s'assurer de l'intégrité du message et de l'authenticité du destinataire ? Ajouter cette fonctionnalité à l'aide d'un script ou d'un outil en CLI.


## le message suivant a été intercepté: `"prggr grpuavdhr f'nccryyr yr puvsserzrag qr prnfre, vy a'rfg cyhf hgvyvft nhwbheq'uhv, pne crh ftphevft"`, il semble vulnérable à une attaque en fréquences ou une attaque par force brute. Déchiffrez-le !

## Nous suspectons qu'un adversaire a implémenté une backdoor dans notre logiciel de messagerie sécurisé, pourtant nous utilisons AES-CBC, voici les logs :