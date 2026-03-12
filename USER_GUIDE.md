






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
