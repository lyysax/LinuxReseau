# TP2
## Lisa Anton et Paul Grienenberger

## I. Exploration locale en solo
### 1. Affichage d'informations sur la pile TCP/IP locale

Sur windows la commande opur afficher la paserelle par défaut est 
ipconfig
 OU 
ipconfig /all 
Et pour Linux 
ip route
 OU 
ip a
### Interface wifi
Nom: Wireless LAN adpater Wi-Fi
Adresse MAC : A0-59-50-0F-22-97 <br>
Adresse IP : 10.33.73.95
Adresse de réseau: 10.33.64.0
Adresse de broadcast: 10.33.79.255

### Interface Ethernet (virtuelle)
#### 1
Nom: Ethernet adapter vEthernet (WSL (Hyper-V firewall)) <br>
Adresse MAC :00-15-5D-83-46-CC <br>
Adresse IP : 172.20.0.1
Adresse de réseau: 172.20.0.0
Adresse de broadcast: 172.20.15.255

#### 2
Nom: Ethernet adapter Ethernet 2
Adresse MAC: 0A-00-27-00-00-0F
Adresse ip: 192.168.56.1
Adresse de réseau: 192.168.56.0
Adresse de broadcast: 192.168.56.255

### En passant par l'interfacr graphique
Settings --> Network and internet --> Wifi --> Properties

#### Question: à quoi sert la gateway dans le réseau d'Ingésup ?
La gateway permet de communiquer avec toutes les autres machines qui ne sont pas dans le même sous-réseau que mon ordinateur.

### 2. Modification des informations 
#### A/B/C
Première IP disponible: 10.33.79.254
Dernière IP disponible: 10.33.79.254

Commande pour afficher les IP actuellement utilisées
```
sudo apt install nmap
```
puis
``` 
nmap -sn 192.168.1.0/24
```
### II. Exploration locale en duo
#### 3. Création du réseau

PC1 → 172.16.18.11/24

PC2 → 172.16.18.12/24

#### 4. Utilisation d'un des deux comme gateway

