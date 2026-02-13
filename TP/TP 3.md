# TP 3 Cryptographie avec openSSL
## I.Exploration en solo
### 1.Base64

Photo 1
Je compare les fichiers et je constate que:
- La taille est différente
- file_bin est un fichier binaire brut, file_bin_b64 est encodée en Base64

Photo 2
Après avoir décodé le fichier base64, les fichiers binaires sont identiques

### 2. AES (Chiffrement symétrique)

Fichier binaire
Photo 3

Fichier chiffré et encodé en base64
Photo 4

### 3. RSA (Chiffrement asymétrique)
La commande pour protéger notre paire de clés RSA avec un chiffrement AES:
```
openssl rsa -in cle_ynov.pem -aes256 -out cle_ynov_protegee.pem
```
Pour vérifier on fait la commande:
```
cat cle_ynov_protegee.pem
```
Photo 5

Dans les paramètres de la clé publique, on peux observer:
- La taille de la clé: 2048 bits
- Le modulus
- L'exposant public e
  
Photo 6

## II.
### A.Base64
#### 1.Génération d'un fichier binaire
Créer un fichier(100ko) puis vérifier sa taille
```
openssl rand -out data.bin 102400
ls -l data.bin
```
#### 2.Encodage
