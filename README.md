# serveur-authentification

Contexte:Vous êtes salarié par Egnom. Cette PME est un prestataire de services informatiques qui assure diverses missions liées à l’infrastructure matérielle pour le compte d’organisations dépourvues de personnel dédié à ces taches. L’association des jeunes d’Arnac la Poste souhaite mettre en place, en libre-service pour ses adhérents, une salle informatique.

Objective:Les PC de cette salle permettront l’accès à l’Internet via le réseau filaire du bâtiment. Cette salle n’étant pas surveillée en permanence, n’importe qui peut débrancher une des prises RJ 45 murales et relier une machine tierce au réseau du bâtiment.


IP - 172.30.3.15/16
Gateway - 172.30.0.254
DHCP - 172.30.0.10 et 172.30.0.100


| Système d'opération | Hoste | Edits | Site téléchargement |
| --- | --- | --- | --- |
| `Windows 10` | hoste actuel | Change l'adresse IP | https://www.microsoft.com/fr-fr/software-download/windows10 |
| `Linux Mint` |Virtual Box | Accès par pont | https://www.linuxmint.com/edition.php?id=299  |
| `OPNsense` | Virtual Box | Accès par pont | https://opnsense.org/download/ |
⚠ FAIT ATTENTION A NE PAS UTILISER PFSENSE! Beaucoup de gens échouent en utilisant cette méthode, Opensense est beaucoup plus simple.

## FreeRADIUS install and setup
```
apt-get update
apt-get install freeradius
```

```
updatedb
locate clients.conf
nano /etc/freeradius/3.0/clients.conf
```

```
client OPNSENSE {
ipaddr = 192.168.15.11
secret = 445
}
```

```
locate freeradius | grep users
nano /etc/freeradius/3.0/users
```

```
admin Cleartext-Password := "1234"
```

```
service freeradius restart
```

```
freeradius -CX
```


## OPNsense setup

System > Servers
Nom descriptif : RADIUS  
Type: RADIUS  
Nom d’hôte ou adresse IP - 172.30.3.x
Secret partagé - Le client Radius a partagé le secret (445)  
Services offerts - Authentification et comptabilité  
Port d’authentification - 1812  
Port d’Acconting - 1813

Vous devez modifier l’adresse IP du serveur Radius.

Vous devez modifier le secret partagé pour refléter le secret partagé de votre client Radius.

Faire un test.


## Failed projects:
Windows server / Server manager:
![[capture 1.PNG]]

![[serverless-server.PNG]]


Free Radius:

![[daloradius.PNG]]
# Sources
https://techexpert.tips/fr/opnsense-fr/opnsense-authentification-radius-a-laide-de-freeradius/


https://www.youtube.com/watch?v=KSVH1CwLZqU