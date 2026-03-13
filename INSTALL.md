# Prérequis techniques

Nous avons mis en place 4 VMs pour ce projet :

- _VM 1_ : PROJET 1 - CLIENT WINDOWS 11
  - _OS_ : Windows 11
  - _Compte & Mot de passe_ : Wilder, Azerty1*
  - _Adresse Ip_ : 172.16.10.10
  - _Masque_ : 255.255.255.0

---

- _VM 2_ : PROJET 1 - CLIENT UBUNTU
  - _OS_ : Ubuntu 24.04
  - _Compte & Mot de passe_ : wilder, Azerty1*
  - _Adresse Ip_ : 172.16.10.20
  - _Masque_ : 255.255.255.0

---

- _VM 3_ : PROJET 1 - WindowsServer
  - _OS_ : Windows Server 2025 GUI
  - _Compte & Mot de passe_ : Administrator, Azerty1*
  - _Adresse Ip_ : 172.16.10.5
  - _Masque_ : 255.255.255.0

---

- _VM 4_ : PROJET 1 - DebianServer
  - _OS_ : Debian 13 CLI
  - _Compte & Mot de passe_ : Root, Azerty1*
  - _Adresse Ip_ : 172.16.10.6
  - _Masque_ : 255.255.255.0

Les machines virtuelles sont déployées sous l'hyperviseur VirtualBox. Pour garantir l'étanchéité de l'environnement de test et permettre une analyse de trafic non filtrée (indispensable pour les scans de ports), la configuration suivante a été appliquée sur l'interface de la VM :

* **Mode d'accès :** Réseau interne (`intnet`)
* **Mode Promiscuité :** "Autoriser tout"

![Configuration Réseau VirtualBox](Ressources/Ouverture-port-debian/SCREENSHOTS_DEBIAN/01_config_reseau_Vbox.png)

# Tâche principale 

# Installation/Mise en place de la solution

## Préparation des cibles : Création de services vulnérables sur WIN01 (Windows 11)

### Objectif technique
Afin que notre outil de cartographie (Nmap) puisse détecter des ports ouverts, il ne suffit pas de désactiver le pare-feu. Un port réseau n'apparaît comme "ouvert" que si une application ou un service est en cours d'exécution et "écoute" le trafic entrant sur ce port. 

Pour simuler un environnement d'entreprise réaliste et vulnérable, nous avons volontairement activé deux services critiques et fréquemment ciblés par les attaquants sur notre client Windows 11 (IP : 172.16.10.10) : le partage de fichiers (SMB) et le Bureau à distance (RDP).

### Étape 1 : Activation du protocole SMB (Port TCP 445)

Le protocole SMB (Server Message Block) est utilisé pour le partage de ressources sur le réseau.
* **Procédure de mise en place :**
1. Ouverture de l'Explorateur de fichiers sur le lecteur `C:`.
   
![](/Ressources/Ouverture-port-windows/Ouverture-de-l-Explorateur-de-fichiers-sur-le-lecteur-C.png)

2. Création d'un dossier nommé `Partage_Projet`

![](/Ressources/Ouverture-port-windows/Creation-d-un-dossier-nomme-partage_Projet.png)

3. Accès aux **Propriétés** du dossier > Onglet **Partage** > **Partage avancé**.
  
![](/Ressources/Ouverture-port-windows/Activation-de-l-option-Partager-ce-dossier-et-validation.png)

4. Activation de l'option **Partager ce dossier** et validation.

* **Résultat :** Le système d'exploitation lance le service de partage et se met en écoute sur le port **445**.

### Étape 2 : Activation du protocole RDP (Port TCP 3389)

Le protocole RDP (Remote Desktop Protocol) permet la prise de contrôle à distance de la machine.
* **Procédure de mise en place :**

1. Accès aux **Paramètres** de Windows 11 > **Système** > **Bureau à distance**.
  
![](/Ressources/Ouverture-port-windows/Acces-aux-Parametres-de-Windows11-Systeme-Bureau-a-distance.png)

2. Bascule du commutateur sur **Activé**.
3. Validation de l'avertissement de sécurité Windows.
   
![](/Ressources/Ouverture-port-windows/Bascule-du-commutateur-sur-Active-et-validation.png)

* **Résultat :** Le service de bureau à distance est démarré et écoute sur le port **3389**.

### Étape 3 : Validation locale de l'ouverture des ports
Avant de procéder aux tests distants avec notre client Linux, nous avons vérifié localement que les services répondaient bien. 
Depuis une Invite de commandes (en tant qu'administrateur), la commande suivante a été exécutée :
`netstat -ano | findstr "LISTENING"`

![](/Ressources/Ouverture-port-windows/Verification-local-des-ports-ouverts-win11.png)

**Résultat attendu :** Les lignes indiquant l'écoute sur `0.0.0.0:445` et `0.0.0.0:3389` sont bien présentes, confirmant que la cible est prête à être analysée.

## Préparation des cibles : Création de services vulnérables sur SRVLX01 (Debian 13 Serveur)

### Étape 1 : Service HTTP (Apache2) - Port 80
Installation du serveur Web Apache pour simuler une interface de gestion non sécurisée.

**Commande :** `apt update && apt install apache2 -y`

![Installation Apache2](Ressources/Ouverture-port-debian/SCREENSHOTS_DEBIAN/04_install_apache.png)

### Étape 2 : Service FTP (vsftpd) - Port 21
Déploiement du service de transfert de fichiers vsftpd pour simuler un vecteur d'exfiltration de données en clair (protocole non chiffré).

**Commande :** `apt install vsftpd -y`

![Installation vsftpd](Ressources/Ouverture-port-debian/SCREENSHOTS_DEBIAN/05_install_ftp.png)

### Audit et Vérification de la Surface d'Exposition
La validation du paramétrage repose sur une analyse locale des sockets et un scan distant de conformité.

### Étape 3 : Analyse locale des sockets (ss)
La commande ss -tunlp confirme que les services Apache et vsftpd sont en état d'écoute (state LISTEN) sur les ports respectifs 80 et 21.

**Commande :** `ss -tunlp`

![Analyse des ports en écoute](Ressources/Ouverture-port-debian/SCREENSHOTS_DEBIAN/06_ports_locaux.png)

### Étape 4 : Scan de reconnaissance distant (Nmap)
Un scan de découverte a été réalisé depuis la machine attaquante du laboratoire (UBU01 - 172.16.10.20). Les résultats valident l'ouverture effective et la visibilité des ports 21 (FTP) et 80 (HTTP) depuis le réseau.

**Commande :** `nmap 172.16.10.6`

![Résultat du scan Nmap](Ressources/Ouverture-port-debian/SCREENSHOTS_DEBIAN/07_audit_nmap_final.png)

### Étape  5: Test de réponse applicative (HTTP)
La connectivité est confirmée par l'accès à la page par défaut d'Apache depuis le navigateur de la machine d'attaque, prouvant que le service est fonctionnel.

![Test de connexion Web](Ressources/Ouverture-port-debian/SCREENSHOTS_DEBIAN/08_test_apache_web.png)

### Étape 5.1 : Test de bannière de service (FTP)
Pour valider le fonctionnement du service FTP sur le port 21, j'utilise l'outil Netcat. Cette commande permet de récupérer la bannière du service et confirme que le serveur est prêt à établir une session de transfert.

**Commande :** `nc -vn 172.16.10.6 21`

![Test de connexion 220 vsFTPd](Ressources/Ouverture-port-debian/SCREENSHOTS_DEBIAN/08_test_apache_web.png)

### Conclusion : 
La cible SRVLX01 est opérationnelle. L'isolation réseau est effective et les services vulnérables sont correctement exposés.

## Préparation des cibles : Création de services vulnérables sur SRVWIN01 (Windows Serveur 2025)

### Objectif : 
- Paramétrer les VM cibles en y laissant des failles afin d'utiliser les logiciel nmap et netcat (installer sur notre VM linux) pour analyser ces failles et les cartographier
- Pour ce faire , nous allons ouvrir **des ports** dans le pare feu-windows

### Nom des ports à ouvrir
- port 139 (netbios)
- port 445 (SMB)

### Étape 1 : ouvrez le panneau de commande 
- Rechercher "Control Panel" dans la barre de recherche Windows.
<img width="1278" height="864" alt="etape 1 failles" src="https://github.com/user-attachments/assets/19916f54-ecc1-4b6e-9e17-4453fd49808d" />

### Étape 2 : Sélectionnez le système et la sécurité 
- Cliquer sur "System and Security"
<img width="1278" height="864" alt="etapes 2" src="https://github.com/user-attachments/assets/ef6d0e46-4715-4831-bc66-aed663419bdc" />

### Étape 3 : Choisissez le pare-feu de Windows Defender 
- Cliquer sur "Windows Defender Firewall"
<img width="1278" height="864" alt="3" src="https://github.com/user-attachments/assets/ae05c101-1336-478a-a130-cff407885ffb" />

### Étape 4 : Sélectionnez Paramètres avancés 
- Sur le coté gauche , cliquer sur "Advanced settings"
<img width="1278" height="864" alt="4" src="https://github.com/user-attachments/assets/2af70704-b0be-41d8-9a97-e1e76aef6b99" />

### Étape 5 : Créez une nouvelle règle
- Choisissez d'abord les Règles entrantes (réguler le trafic entrant) puis les Règles sortantes (réguler le trafic sortant) pour chaque ports
- Puis sur la droite , cliquer sur "new Rule"
 <img width="927" height="864" alt="derniere" src="https://github.com/user-attachments/assets/a5c5f051-aa07-471f-b5bd-6db74dae106a" />

### Étape 6 : Sélectionnez le port
- Choisir Port Pour créer une règle pour un port spécifique
<img width="1278" height="864" alt="66" src="https://github.com/user-attachments/assets/286ddce6-cea8-457d-bb7c-6307f2391d54" />

### Étape 7 : Choisissez TCP 
- Ajouter les ports manuelement dans la zone de texte
<img width="1278" height="864" alt="6" src="https://github.com/user-attachments/assets/87f874c7-4295-4ffc-b7c3-69118cbc2473" />

### Étape 8 : Autoriser la connexion
- Décidez si vous souhaitez autoriser la connexion via ce port
<img width="1278" height="864" alt="1" src="https://github.com/user-attachments/assets/d7521159-d68d-446e-abf7-8e4aed598103" />

### Étape 9 : Spécifiez les options de la règle
<img width="1278" height="864" alt="2" src="https://github.com/user-attachments/assets/278b00f7-d0ce-4dae-84da-91c9f18b3a27" />


### Étape 10 : nom et description
- Ajouter un nom a la régle et une description si vous le souhaiter
<img width="1278" height="864" alt="33" src="https://github.com/user-attachments/assets/e2840296-8328-4bc3-b934-b5adab3f570b" />


#### ( ps : executer ces étapes pour chaque ports )

### Étape finale : Verification avec nmap sur ma VM linux
<img width="775" height="272" alt="VirtualBox_UBU01_12_03_2026_21_25_42" src="https://github.com/user-attachments/assets/532eadf8-de90-4fe5-8f7c-ec283e8c9a30" />


- Nous voyons bien nos 2 port (139 et 445 )

- **Resultat** : Nos 2 ports sont ouvert !

# Tâche secondaire :

### Code source du script `nmap.sh`

Voici le script bash que nous avons développé pour automatiser nos requêtes :

```bash
#!/bin/bash
#demandons a l'utilisateur quel script il veut utilisateur

echo "Quel type de scan voulez vous faire?" && echo "0: TCP" && echo "1: UDP" && echo "2: SYN only" && echo "3: Ping/host"
read type
echo "Quelle vitesse voules vous utiliser(0-4)? Du moins bruyant au plus agressif"
read vitesse
echo "Quel niveau d'info voulez vous?(0-3)Du moins détaillé au plus detaillé."
read verb
echo "Quelle option voulez vous?" && echo "0: toute" && echo "1: OS" && echo "2: version de service"
read opt
read -p "Rentrez l'ip cible" ip

#type de scan
if (($type >= 0 && $type <= 3))
then
type_array=("-sT" "-sU" "-sS" "-sn")
type2=${type_array[$type]}
else 
echo "erreur de saisie"
exit 1
fi

#vitesse du scan
if (($vitesse >= 0 && $vitesse <= 5 ))
then
vitesse_array=("-T0" "-T1" "-T2" "-T3" "-T4 -F")
vitesse2=${vitesse_array[$vitesse]}
    if (($vitesse > 2))
    then
    read -p "Attention vous allez attirer l'attention! Voulez vous poursuivre? Y/N" reponse
        if [[ $reponse != "Y" ]]
            then echo "terminé!" && exit 1
        fi
    fi
else
echo "erreur de saisie"
exit 1
fi

#verbosite
if (($verb >= 0 && $verb <= 3))
then
verb_array=("" "-v" "-vv" "-vvv")
verb2=${verb_array[$verb]}
else 
echo "erreur de saisie"
exit 1
fi

#options
if (($opt >= 0 && $opt <= 2))
then
opt_array=("-A" "-O" "-sV")
opt2=${opt_array[$opt]}
else
echo "erreur de saisie"
exit 1
fi

test= sudo nmap $type2 $vitesse2 $verb2 $opt2 $ip
echo "$test"
```
