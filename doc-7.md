# Veille Informationnelle – OpenMediaVault

Ce document présente une veille informationnelle sur OpenMediaVault, une solution NAS (Network Attached Storage) libre et basée sur Debian. Il synthétise les informations clés, l’évolution du projet, ses fonctionnalités, les mises à jour récentes ainsi que quelques pistes sur les évolutions potentielles à venir.

---

## 1. Introduction

**OpenMediaVault (OMV)** est une distribution Linux dédiée à la mise en place de serveurs de stockage en réseau (NAS). Conçue pour être simple d’utilisation grâce à une interface web, elle est particulièrement adaptée aux environnements domestiques et aux petites structures.

---

## 2. Présentation du projet

- **Origine et Objectifs :**  
  Créé par Volker Theile en 2009, OpenMediaVault est né du besoin d’avoir une solution NAS sous Linux offrant plus de flexibilité qu’une solution basée sur FreeBSD.  
  [Voir plus sur Wikipédia (FR)](https://fr.wikipedia.org/wiki/OpenMediaVault) <!-- :contentReference[oaicite:0]{index=0} -->

- **Base Technique :**  
  - Basée sur Debian  
  - Licence GPL v3  
  - Gestion des mises à jour via APT et dpkg  
  - Interface web pour l’administration

- **Cibles d’utilisation :**  
  Principalement destinée aux particuliers, aux petites entreprises et aux bureaux à domicile.

---

## 3. Historique et évolution

OpenMediaVault a connu de nombreuses versions, chacune portant un nom de code inspiré de l’univers de *Dune*. Quelques étapes marquantes :

- **OMV 0.2 – Ix (2011)**  
  Première version publiée.

- **OMV 0.3 – Omnius (2012)**  
  Introduction d’une interface web multilingue et d’un système de gestion des droits via ACL.

- **OMV 0.4 – Fedaykin (2012)**  
  Première évolution importante avec des améliorations de l’interface et des fonctionnalités.

- **OMV 1.0 – Kralizec (2014)**  
  Optimisation pour les matériels moins puissants et refonte de l’architecture des plugins.

- **OMV 2.0 – Stone Burner (2015)**  
  Introduction d’un nouveau framework pour la WebGUI.

- **Versions ultérieures : Erasmus, Arrakis, Usul, Shaitan**  
  Chaque version apporte des améliorations en termes de performance, de sécurité et d’ergonomie.

- **OMV 7 – Sandworm (2024)**  
  La dernière version stable en date, basée sur Debian 12, qui intègre de nombreuses améliorations (gestion automatisée des mises à jour, authentification via header HTTP, optimisation de la détection SMART, etc.).  
  [Voir le changelog officiel](https://www.openmediavault.org/?p=3663) <!-- :contentReference[oaicite:1]{index=1} -->

---

## 4. Fonctionnalités principales

OpenMediaVault offre un ensemble complet de fonctionnalités pour gérer un NAS :

- **Système et Réseau :**
  - Gestion de la date/heure via NTP  
  - Configuration réseau (IPv4, IPv6, DNS, Firewall, Wake-on-LAN)  
  - Notifications par email et rapports de diagnostic

- **Stockage :**
  - Support de divers périphériques (HDD, SSD, clés USB)  
  - Surveillance via S.M.A.R.T.  
  - Gestion des RAID logiciels (0, 1, 5, 6, 10, etc.) et des systèmes de fichiers (EXT3/4, XFS, BTRFS, etc.)  
  - Gestion des quotas et de l’énergie pour les disques

- **Services intégrés :**
  - Protocoles réseau (SMB/CIFS, NFS, FTP, rsync, TFTP, SSH)  
  - Gestion des utilisateurs, groupes et ACL  
  - Interface web multilingue et extensibilité via plugins

---

## 5. Mises à jour récentes

Les dernières informations issues du site officiel et du dépôt GitHub indiquent :

- **OMV 7.7.0 et 7.7.1 (2025)**  
  - **Améliorations de l’interface utilisateur** (mise à jour des fichiers de langue, refonte de certains éléments UI)  
  - **Refactoring des appels RPC** pour une meilleure gestion des systèmes de fichiers  
  - **Correctifs spécifiques** (gestion du dépôt backports, traitement des chaînes tokenisées)  
  - Mise à jour de plugins associés (OMV-apt, OMV-k8s, OMV-usbbackup)  
  [Voir le changelog GitHub](https://github.com/openmediavault/openmediavault/blob/master/deb/openmediavault/debian/changelog) <!-- :contentReference[oaicite:2]{index=2} -->

Les mises à jour sont diffusées régulièrement, assurant une évolution continue du projet.

---

## 6. Évolutions potentielles et mises à jour à venir

Bien qu’aucune feuille de route officielle détaillée ne soit publiée, plusieurs axes de développement ressortent des discussions sur le forum et des issues GitHub :

- **Intégration et gestion des conteneurs :**  
  Poursuite de l’amélioration des solutions basées sur Podman et Docker pour une gestion optimisée des applications conteneurisées.

- **Optimisation du support de BTRFS :**  
  Renforcement des fonctionnalités pour exploiter pleinement les avantages de BTRFS (snapshots, RAID intégré, etc.).

- **Amélioration de la gestion RAID et des interfaces réseau :**  
  Optimisation de la configuration et de la stabilité des ensembles RAID et des interfaces réseau, en particulier dans les environnements complexes.

- **Évolution de l’interface utilisateur :**  
  Nouvelle ergonomie, ajout de widgets et options de personnalisation pour une expérience utilisateur encore plus intuitive.

- **Sécurité et automatisation des mises à jour :**  
  Renforcement des mécanismes de sécurité et de notifications, avec une automatisation accrue des mises à jour via des outils comme `unattended-upgrades`.

Ces axes de développement sont pilotés par l’équipe principale (Volker Theile et quelques contributeurs) et par la communauté. Pour suivre ces évolutions, il est recommandé de consulter régulièrement le [forum officiel d’OMV](https://forum.openmediavault.org/) et le [dépôt GitHub](https://github.com/openmediavault/openmediavault/issues).

---

## 7. Plugins et Écosystème

- **Modularité :**  
  OMV est conçu pour être extensible via des plugins, permettant d’ajouter des fonctionnalités supplémentaires telles que :
  - ClamAV (antivirus)  
  - OneDrive (synchronisation cloud)  
  - Podman (gestion de conteneurs)  
  - USB Backup (sauvegarde automatique sur disques USB)  
  - Etc.  
  [Voir OMV-Extras sur GitHub](https://github.com/openmediavault-plugins) <!-- :contentReference[oaicite:3]{index=3} -->

- **Communauté et Contributions :**  
  - Une communauté active d’utilisateurs et de développeurs  
  - Suivi des issues et propositions de nouvelles fonctionnalités via GitHub et le forum

---

## 8. Ressources et Communauté

- **Site officiel :** [www.openmediavault.org](https://www.openmediavault.org)  
- **Documentation :** Accessible sur le site officiel et via le [wiki OMV-Extras](https://wiki.omv-extras.org/)  
- **Forum :** [Forum OpenMediaVault](https://forum.openmediavault.org/)  
- **Dépôt GitHub :** [openmediavault/openmediavault](https://github.com/openmediavault/openmediavault)  
- **Wikipédias :**  
  - [Wikipedia (FR)](https://fr.wikipedia.org/wiki/OpenMediaVault)  
  - [Wikipedia (EN)](https://en.wikipedia.org/wiki/OpenMediaVault)

---

## 9. Conclusion

OpenMediaVault continue d’évoluer pour répondre aux besoins d’un large public, allant des utilisateurs domestiques aux petites entreprises. La régularité des mises à jour, la richesse des fonctionnalités et la modularité grâce aux plugins en font une solution NAS robuste et adaptable.  
Les axes d’évolution identifiés laissent entrevoir de futures améliorations, notamment en matière d’intégration de conteneurs, de support avancé de BTRFS et d’optimisation de l’interface utilisateur. Restez informé via les forums et GitHub pour suivre les nouveautés.

---

*Ce document est amené à évoluer au fil des nouvelles publications et contributions de la communauté.*
