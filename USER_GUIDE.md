






### Étape finale : Verification avec nmap sur ma VM linux
<img width="775" height="272" alt="VirtualBox_UBU01_12_03_2026_21_25_42" src="https://github.com/user-attachments/assets/f06c4757-be1a-44bd-91dc-e8c774957503" />


- Nous voyons bien nos 2 port (139 et 445 )

- **Resultat** : Nos 2 ports sont ouvert !







## Étape 4 : Scan de reconnaissance distant (Nmap)
Un scan de découverte a été réalisé depuis la machine attaquante du laboratoire (UBU01 - 172.16.10.20). Les résultats valident l'ouverture effective et la visibilité des ports 21 (FTP) et 80 (HTTP) depuis le réseau.

**Commande :** `nmap 172.16.10.6`

![Résultat du scan Nmap](Ressources/Ouverture-port-debian/SCREENSHOTS_DEBIAN/07_audit_nmap_final.png)

### Étape  5: Test de réponse applicative (HTTP)
La connectivité est confirmée par l'accès à la page par défaut d'Apache depuis le navigateur de la machine d'attaque, prouvant que le service est fonctionnel.

![Test de connexion Web](Ressources/Ouverture-port-debian/SCREENSHOTS_DEBIAN/08_test_apache_web.png)

### Étape 5.1 : Test de bannière de service (FTP)
La réponse du service est confirmée par la récupération de la bannière vsFTPd via l'outil Netcat depuis la machine d'attaque, prouvant que le service de transfert de fichiers est opérationnel et prêt à l'emploi.

![Test de connexion 220 vsFTPd](Ressources/Ouverture-port-debian/SCREENSHOTS_DEBIAN/09_test_service_FTP.png)

