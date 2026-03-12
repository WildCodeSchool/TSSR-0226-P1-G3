## création de vulnerabilité sur notre VM cible SRVWIN01 (Windows server)

### objectif : 
- paramétrer les VM cibles en y laissant des failles afin d'utiliser les logiciel nmap et netcat (installer sur notre VM linux) pour analyser ces failles et les cartographier
- pour ce faire , nous allons ouvrir **des ports** dans le pare feu-windows

### nom des ports à ouvrir
- port 139 (TCP)
- port 445 (SMB)

## étape 1 : ouvrez le panneau de commande 
- Rechercher "Control Panel" dans la barre de recherche Windows.
<img width="1278" height="864" alt="etape 1 failles" src="https://github.com/user-attachments/assets/19916f54-ecc1-4b6e-9e17-4453fd49808d" />


## étape 2 : Sélectionnez le système et la sécurité 
- cliquer sur "System and Security"
<img width="1278" height="864" alt="etapes 2" src="https://github.com/user-attachments/assets/ef6d0e46-4715-4831-bc66-aed663419bdc" />


## étape 3 : Choisissez le pare-feu de Windows Defender 
- cliquer sur "Windows Defender Firewall"
<img width="1278" height="864" alt="3" src="https://github.com/user-attachments/assets/ae05c101-1336-478a-a130-cff407885ffb" />


## étape 4 : Sélectionnez Paramètres avancés 
- sur le coté gauche , cliquer sur "Advanced settings"
<img width="1278" height="864" alt="4" src="https://github.com/user-attachments/assets/2af70704-b0be-41d8-9a97-e1e76aef6b99" />


## étape 5 : Créez une nouvelle règle
- choisissez d'abord les Règles entrantes (réguler le trafic entrant) puis les Règles sortantes (réguler le trafic sortant) pour chaque ports
- Puis sur la droite , cliquer sur "new Rule"
 <img width="927" height="864" alt="derniere" src="https://github.com/user-attachments/assets/a5c5f051-aa07-471f-b5bd-6db74dae106a" />

 

## étape 6 : Sélectionnez le port
- Choisir Port Pour créer une règle pour un port spécifique
<img width="1278" height="864" alt="66" src="https://github.com/user-attachments/assets/286ddce6-cea8-457d-bb7c-6307f2391d54" />



## étape 7 : Choisissez TCP 
- ajouter les ports manuelement dans la zone de texte
<img width="1278" height="864" alt="6" src="https://github.com/user-attachments/assets/87f874c7-4295-4ffc-b7c3-69118cbc2473" />


## étape 8 : Autoriser la connexion
- Décidez si vous souhaitez autoriser la connexion via ce port
<img width="1278" height="864" alt="1" src="https://github.com/user-attachments/assets/d7521159-d68d-446e-abf7-8e4aed598103" />



## étape 9 : Spécifiez les options de la règle
<img width="1278" height="864" alt="2" src="https://github.com/user-attachments/assets/278b00f7-d0ce-4dae-84da-91c9f18b3a27" />



## étape 10 : nom et description
- ajouter un nom a la régle et une description si vous le souhaiter
<img width="1278" height="864" alt="33" src="https://github.com/user-attachments/assets/e2840296-8328-4bc3-b934-b5adab3f570b" />



#### ( ps : executer ces étapes pour chaque ports )

## étape finale : Verification avec nmap sur ma VM linux
<img width="775" height="272" alt="VirtualBox_UBU01_12_03_2026_21_25_42" src="https://github.com/user-attachments/assets/2a0704c3-19ec-4b0b-becf-727969d615e8" />


- nous voyons bien nos 2 port (139 et 445 )

- **resultat** : Nos 2 ports sont ouvert !
