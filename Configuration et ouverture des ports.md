
## Préparation des cibles : Création de services vulnérables sous Debian 13 serveur

### Objectif technique
Afin que notre outil de cartographie (Nmap) puisse détecter des ports ouverts, il faut ouvrir les ports sur le serveur distant. Un port réseau n'apparaît comme "ouvert" que si une application ou un service est en cours d'exécution et "écoute" le trafic entrant sur ce port. 

Pour simuler un port vulnérable sur ce serveur nous utiliserons le logiciel Ncat qui nous permettra d’écouter en permanence sur ce port.

### Faille 1 : Ouverture d'un port TCP

On lance Ncat à l'ecoute du port 700 par exemple grâce à cette commande:
   
Configuration-et-ouverture-de-port-sous-Debian-13-serveur

![port700 ouvert](https://github.com/WildCodeSchool/TSSR-0226-P1-G3/blob/Configuration-et-ouverture-de-port-sous-Debian-13-serveur/tcpouvert.png)

  
### Faille 2 : Activation du protocole UDP (Port TCP 3389)

De la même façon on lance Ncat à l’écoute cette fois d'un port UDP grâce à cette commande et on l'envoie en background:

![Port UDP](https://github.com/WildCodeSchool/TSSR-0226-P1-G3/blob/Configuration-et-ouverture-de-port-sous-Debian-13-serveur/ouverture%20port%20UDP.png](https://github.com/WildCodeSchool/TSSR-0226-P1-G3/blob/Configuration-et-ouverture-de-port-sous-Debian-13-serveur/udpouvert.png)

### Verdification de l'ouverture des ports

La commande suivante permet de lister les port ouverts et donc de verifier qu'on a bien ouvert les ports:

![Rendu des ports ouverts](https://github.com/WildCodeSchool/TSSR-0226-P1-G3/blob/Configuration-et-ouverture-de-port-sous-Debian-13-serveur/rendu.png)


### Scan de verification avec Nmap depuis la VM d'attaque:

On va bien sur verifier que l'on arrive bien à scanner les port que l'onvient d'ouvrir. Dans la capture ci dessous on voit que le port 700 est bien ouvert en TCP et le port 400 en UDP.


![Rendu Nmap](https://github.com/WildCodeSchool/TSSR-0226-P1-G3/blob/Configuration-et-ouverture-de-port-sous-Debian-13-serveur/rendunmap.png)


