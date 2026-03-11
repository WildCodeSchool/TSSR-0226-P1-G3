# **PROJET : Analyse et cartographie des ports réseau**
## Sommaire

- [Le projet](#le-projet)
- [Les membres du groupe et leurs rôles](#les-membres-du-groupe-et-leurs-rôles)
- [Choix techniques](#choix-techniques)
- [Difficultés rencontrées](#difficultés-rencontrées)
- [Solutions trouvées](#solutions-trouvées)
- [Amélioration possibles](#améliorations-possibles)
---
### Le projet
**Tâche principale** : Analyse de ports réseaux
- Depuis un client scanner les ports de plusieurs machines
- Récupérer le maximum d’informations

**Tâche secondaire** : Création de profils de scan personnalisés


---
### Les membres du groupe et leurs rôles

|            |                                                          **Sprint 1**                                                          |                                 **Sprint 2**                                 |
| :--------: | :----------------------------------------------------------------------------------------------------------------------------: | :--------------------------------------------------------------------------: |
| **Alexandre** |                                                        Scrum Master                                                         |                                                                              |
| **Xavier** |                                                           Product Owner                                                        |                                                                              |
| **Revine**  |                                                                                                                               |                                  Scrum Master                                |
| **Mohamed** |                                                                                                                               |                                  Product Owner                               |



---
### Choix techniques 

Pour mener à bien notre projet, nous avons utilisé :

| Rôle | Nom de la VM | OS | Adresse IP | Identifiant (Admin) |
| :--- | :--- | :--- | :--- | :--- |
| **Attaquant** | UBU01 | Ubuntu 24 LTS | 172.16.10.20 | wilder |
| **Cible 1** | WIN01 | Windows 11 Pro | 172.16.10.10 | Wilder |
| **Cible 2** | SRVWIN01 | Windows Server 2025 | 172.16.10.5 | Administrateur |
| **Cible 3** | SRVLX01 | Debian 13 CLI | 172.16.10.6 | root / wilder |
---



### Logiciels :

**1. Nmap**


**2. Netcat**

---
### Difficultés rencontrées 

- Ouvrir des ports sur Debian 13 Server

---
### Solutions trouvées 

- Utilisation de Ncat pour ouvrir les ports et de maintenir ouvert les ports pour la demonstration sur Debian 13 Server

---
### Améliorations possibles

- Paramètrage d'un serveur Apache
