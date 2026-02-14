# TP 3 Cryptographie avec openSSL
## I.Exploration en solo
### 1.Base64
Je compare les fichiers et je constate que:
- La taille est différente
- file_bin est un fichier binaire brut, file_bin_b64 est encodée en Base64
- 
Après avoir décodé le fichier base64, les fichiers binaires sont identiques

### 2. AES (Chiffrement symétrique)

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
  
## II.
### A.Base64
#### 1.Génération d'un fichier binaire
Créer un fichier(100ko) puis vérifier sa taille
```
openssl rand -out data.bin 102400
ls -l data.bin
```
#### 2.Encodage
Encoder en data.b64
```
openssl base64 -in data.bin -out data.b64
```
Afficher le contenu
```
cat data.b64
```
Comparer la taille de data.bin et data.b64
```
ls- l data.bin data.b64
```
#### 3. Décodage
Décoder le fichier data.b64 pour obtenir un fichier data_restored.bin
```
openssl base64 -d -in data.b64 -out data_restored.bin
```
Vérifier que data.bin et data_restored.bin sont strictement identique
```
diff -s data.bin data_restored.bin
```
#### 4. Questions
1. Base64 sert seulement à coder les données en texte. Il n'empêche pas de les lires ou de récupérer facilement les données.
2. La taille change car 3 octets binaires sont transformés en 4 caractères BAse64, donc augmente la taille du fichier.
3. Environ 33%
4. Comparer le contenu binaire avec cmp ou diff -s.

### B. Chiffrement symétrique
#### 1. Création d'un message
```
nano confidentiel.txt
```
#### 2. Chiffrement
```
openssl enc -aes-256-cbc -salt -pbkdf2 -iter 100000 -md sha256 -in confidentiel.txt -out confidentiel.enc
```
```
file confidentiel.enc
```
#### 3. Déchiffrement
```
openssl enc -d -aes-256-cbc -pbkdf2 -iter 100000 -md sha256 -in confidentiel.enc -out confidentiel_dechiffre.txt
```
```
nano confidentiel_dechiffre.txt
```
#### 4. Analyse
```
openssl enc -aes-256-cbc -salt -pbkdf2 -iter 100000 -md sha256 -in confidentiel.txt -out confidentiel2.enc
```
```
diff -s confidentiel.enc confidentiel2.enc
```
#### 5. Questions
1. Ils sont différents car le chiffrement utilise un sel aléatoire pour chaque exécution.
2. Le fichier ne pourra pas être déchiffré correctement et le contenu sera illisible.
3. Ça sert à renforcer le mot de passe en générant une clé solide.
4. L'encodage transforme les données pour les rendres lisibles mais n'est pas vraiment sécurisé. Alors que le chiffrement transforme les données pour les protéger, il faut un mot de passe pour récuperer le contenu.

### C. Cryptographie asymétrique
#### 1. Génération de clés
```
openssl genpkey -algorithm RSA -aes-256-cbc -out rsa_private.pem -pkeyopt rsa_keygen_bits:2048
```
```
openssl rsa -pubout -in rsa_private.pem -out rsa_public.pem
```
```
openssl rsa -in rsa_private.pem -text -noout
```
```
openssl rsa -pubin -in rsa_public.pem -text -noout
```
La clé publique ne demande pas de mot de passe alors que la privée oui.
La cl" privée contient plus d'informations que la publique.

#### 2. Chiffrement asymétrique
```
nano secret.txt
```
```
openssl pkeyutl -encrypt -pubin -inkey rsa_public.pem -in secret.txt -out secret.enc
```
```
openssl pkeyutl -decrypt -inkey rsa_private.pem -in secret.enc -out secret_dechiffre.txt
```
```
cat secret_dechiffre.txt
```
#### 3. Question
1. La clé privée permet de déchiffrer tout ce qui a été chiffré avec la clé publique, alors tout le contenu devient lisible par n'importe qui.
2. RSA est lent et limité en taille.
3. La clé publique contient le modulus et l'exponent public; La clé privé contient modulus, exponent public, exponent privé, facteurs premiers
4. Le modulus (n) est utilisé pour toutes les opérations mathématiques de chiffrement et déchiffrement.
5. Je ne sais pas

### D. Signature numérique
#### 1. Création et signature
```
nano contrat.txt
```
```
openssl dgst -sha256 contrat.txt
```
```
openssl dgst -sha256 -sign rsa_private.pem -out contrat.sig contrat.txt
```
#### 2. Vérification
```
openssl dgst -sha256 -verify rsa_public.pem -signature contrat.sig contrat.txt
```
```
nano contrat.txt
openssl dgst -sha256 -verify rsa_public.pem -signature contrat.sig contrat.txt
```
#### 3. Questions
1. La vérification échoue et il est marqué "Verification Failure".
2. Toute modiciation après l'empreinte fait que la singature ne correspond plus.
3. Le hachage transorme le fichier en une empreinte unique et qui ne change pas. Ca permet donc de détecter toute modification.
4. Signature numérique: est authentique, mais le fichier reste lisible
   Chiffrement: rend le fichier confidentiel, illisible sans la clé.
