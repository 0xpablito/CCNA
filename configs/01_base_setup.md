# Configuration de Base et Sécurité
**Cible :** `ACC-01`, `ACC-02`, `CORE`,`DEPOT`,`R-EDGE`


### 📋 Script de configuration

### Identification et DNS
```
hostname CORE
no ip domain-lookup        ! Évite les blocages en cas d'erreur de frappe
```
### Sécurisation des accès 
```
enable secret cDawAG8!&E@F  ! Mot de passe du mode privilégié (haché)
service password-encryption ! Chiffre les mots de passe visibles dans la config
```
### Création d'un compte administrateur
```
! Utilisé pour la connexion SSH
username admin secret cDawAG8!&E@F
```
### Paramètres SSH 
```
ip domain-name pf.local      ! Définit le suffixe DNS nécessaire à la génération de la clé
crypto key generate rsa      ! Génère la clé de chiffrement (Choisir 1024 bits)
ip ssh version 2             ! Force l'usage du protocole SSH version 2 (plus sécurisé)
```   
### Configuration de la console (Accès physique) 
```
line console 0
 logging synchronous         ! Évite que les logs coupent la saisie
 login local                 ! Utilise le compte admin créé plus haut
 exec-timeout 5 0            ! Timeout de sécurité
```
### Configuration VTY (Accès distant)
```
line vty 0 15
 login local               ! Authentification via la base de données locale
 transport input ssh       ! Autorise uniquement le SSH (Telnet bloqué)
 exec-timeout 5 0          ! Déconnexion après 5 min d'inactivité
```
### Message d'avertissement 
```
banner motd #UNAUTHORIZED ACCESS TO THIS DEVICE IS PROHIBITED.#
