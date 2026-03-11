
### Préparation des cibles : Création de services vulnérables sous Debian 13 serveur

### Objectif technique
Afin que notre outil de cartographie (Nmap) puisse détecter des ports ouverts, il faut ouvrir les ports sur le serveur distant. Un port réseau n'apparaît comme "ouvert" que si une application ou un service est en cours d'exécution et "écoute" le trafic entrant sur ce port. 

Pour simuler un port vulnérable sur ce serveur nous utiliserons le logiciel Ncat qui nous permettra d’écouter en permanence sur ce port.

### Faille 1 : Ouverture d'un port TCP

On lance Ncat à l'ecoute du port 700 par exemple grâce à cette commande:
   
![screen1.png](Ouverture du port 700)

On s'assure de l'ouverture du port  avec la commande: 



3.  On peut tester que Nmap  voit bien le port en le scannant depuis la VM d'attaque:
  


### Faille 2 : Activation du protocole UDP (Port TCP 3389

De la même façon on lance Ncat à l’écoute cette fois d'un port UDP grâce à cette commande et on l'envoie en background:

![Capture d’écran du 2026-03-10 21-03-34.png](Ouverture du port UDC)

On s'assure de l'ouverture du port  avec la commande: 

