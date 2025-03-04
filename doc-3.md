# Comparaison Approfondie des Solutions RAID

Dans le cadre d’un **cluster multi-site** conçu pour offrir une **haute disponibilité** (HA) et des **migrations en temps réel**, nous avons déployé un serveur **OpenMediaVault** en tant que solution NAS. L’ensemble est hébergé sur un **stockage distribué Linstor/DRBD**, permettant ainsi la réplication immédiate des données entre les différents nœuds du cluster.

- **Sur le serveur Dell PowerEdge (dell1)**, nous avons mis en place un **RAID 1 matériel** via la carte RAID intégrée. Cette configuration assure une première couche de redondance locale : en cas de défaillance d’un des disques, le serveur reste opérationnel.
- **Au niveau du cluster**, les données sont ensuite **répliquées en temps réel** grâce à **DRBD/Linstor**. Cette réplication garantit une disponibilité accrue et autorise la migration à chaud de la VM OpenMediaVault, sans interruption de service.  
  Résultat : si un site ou un nœud venait à rencontrer un problème, la solution NAS resterait accessible via un autre nœud du cluster.
- **Interconnexion Sécurisée** : Le serveur OpenMediaVault est relié de l’infrastructure personnelle de Dorian et Rémi à celle d’Assurmer grâce à Tailscale, assurant ainsi une interconnexion sécurisée et fluide entre les différents environnements.

Ce document propose une **analyse détaillée** des configurations RAID les plus courantes (RAID 1, RAID 5, RAID 6 et RAID 10), afin de clarifier les choix techniques qui ont guidé notre implémentation et de présenter les avantages et inconvénients de chaque solution. Vous comprendrez ainsi comment le RAID 1 matériel sur dell1, combiné à la réplication multi-nœuds via DRBD/Linstor, apporte une **sécurité** et une **flexibilité** optimales pour nos besoins de stockage en environnements critiques.cument présente une analyse détaillée des configurations RAID courantes (RAID 1, RAID 5, RAID 6 et RAID 10), afin d’expliquer les choix effectués et de situer les avantages et inconvénients de chaque solution.

---

## Tableau Comparatif des Configurations RAID

| **Type RAID** | **Description** | **Avantages** | **Inconvénients** | **Nombre Minimum de Disques** |
|---------------|-----------------|---------------|-------------------|-------------------------------|
| **RAID 1**    | Duplication exacte des données sur deux disques (mirroring). | - Redondance élevée<br>- Récupération rapide en cas de défaillance<br>- Mise en œuvre simple | - Capacité de stockage effective divisée par deux<br>- Investissement matériel élevé | 2 |
| **RAID 5**    | Répartition des données et de la parité sur tous les disques. | - Bonne utilisation de l'espace de stockage<br>- Tolérance à la défaillance d’un disque<br>- Compromis entre performance et sécurité | - Performances en écriture impactées par le calcul de parité<br>- Reconstruction longue en cas de défaillance | 3 |
| **RAID 6**    | Extension du RAID 5 avec une double parité, permettant de tolérer deux défaillances simultanées. | - Tolérance à deux défaillances<br>- Sécurité renforcée pour les environnements critiques | - Impact important sur les performances en écriture<br>- Besoin d’un nombre plus élevé de disques | 4 |
| **RAID 10**   | Combinaison du mirroring (RAID 1) et du striping (RAID 0) pour optimiser performance et redondance. | - Excellentes performances en lecture et écriture<br>- Haute redondance<br>- Récupération rapide | - Capacité effective limitée à 50 % du total<br>- Coût plus élevé en raison du nombre de disques requis | 4 |

---

## Détail des Configurations RAID

### RAID 1
Le RAID 1 consiste à copier identiquement les données d’un disque sur un autre. Chaque écriture est dupliquée en temps réel, assurant une haute disponibilité en cas de défaillance d’un disque.

- **Avantages :**
  - **Redondance Maximale :** Si un disque tombe en panne, l'autre contient une copie intégrale des données.
  - **Simplicité :** Facile à mettre en place et à gérer.
  - **Amélioration Potentielle de la Lecture :** Possibilité d'effectuer des lectures en parallèle.
- **Inconvénients :**
  - **Capacité Effective Réduite :** La moitié de la capacité totale est utilisée pour la duplication.
  - **Coût :** Nécessite l’achat de deux fois plus de disques pour obtenir la capacité de stockage utile équivalente.

### RAID 5
Le RAID 5 distribue les données ainsi que les informations de parité sur l’ensemble des disques de l’array. La parité permet de reconstruire les données en cas de défaillance d’un disque.

- **Avantages :**
  - **Efficacité de Stockage :** Seul l’équivalent d’un disque est utilisé pour la parité, maximisant ainsi la capacité disponible.
  - **Tolérance à la Défaillance :** Permet de continuer à fonctionner même si un disque échoue.
  - **Bon Compromis :** Allie performance en lecture, capacité de stockage et redondance.
- **Inconvénients :**
  - **Impact sur les Écritures :** Le calcul de la parité ralentit les opérations d’écriture.
  - **Temps de Reconstruction :** En cas de défaillance, la reconstruction de l’array peut être longue et affecter les performances globales.

### RAID 6
Le RAID 6 est similaire au RAID 5 mais ajoute une deuxième couche de parité. Cela permet au système de tolérer la défaillance simultanée de deux disques.

- **Avantages :**
  - **Tolérance Accrue :** Protection contre la défaillance de deux disques, essentielle pour les environnements où la continuité du service est critique.
  - **Sécurité Supplémentaire :** Convient aux infrastructures nécessitant une haute disponibilité.
- **Inconvénients :**
  - **Performance en Écriture :** Les calculs supplémentaires pour la double parité peuvent considérablement ralentir les écritures.
  - **Utilisation de l’Espace :** La capacité effective est réduite, et l’overhead par disque augmente par rapport au RAID 5.
  - **Configuration Plus Complexe :** Nécessite un plus grand nombre de disques et une surveillance accrue.

### RAID 10
Le RAID 10 combine les avantages du RAID 1 et du RAID 0. Il duplique les données (mirroring) puis les répartit (striping) sur plusieurs disques pour améliorer la performance.

- **Avantages :**
  - **Excellente Performance :** Le striping permet des opérations de lecture et d’écriture rapides.
  - **Haute Redondance :** Chaque donnée est dupliquée, offrant une sécurité similaire au RAID 1.
  - **Reconstruction Rapide :** La récupération en cas de panne est généralement plus rapide.
- **Inconvénients :**
  - **Capacité Réduite :** Comme pour le RAID 1, seulement 50 % de la capacité totale est utilisable.
  - **Coût Élevé :** Exige un nombre minimum de disques plus important, ce qui peut augmenter les coûts.
  - **Investissement en Matériel :** Idéal pour des environnements exigeants, mais peut ne pas être rentable pour des applications à faible charge.

---

## Facteurs à Considérer Lors du Choix d'une Configuration RAID

1. **Performance :**  
   - **Lecture vs. Écriture :** Le RAID 10 offre d'excellentes performances globales, tandis que le RAID 5 et RAID 6 peuvent présenter des limitations sur les écritures à cause du calcul de parité.
   - **Charge de Travail :** Pour des applications nécessitant des vitesses de traitement élevées (bases de données transactionnelles, etc.), le RAID 10 est souvent privilégié.

2. **Tolérance aux Pannes et Sécurité des Données :**  
   - **Redondance Maximale :** Le RAID 1 et RAID 10 offrent une redondance complète via le mirroring.
   - **Tolérance aux Défaillances Multiples :** Le RAID 6 convient mieux aux environnements critiques où la tolérance à la défaillance de deux disques est indispensable.

3. **Efficacité du Stockage et Coût :**  
   - **Utilisation de l’Espace :** Le RAID 5 et RAID 6 optimisent l’espace disponible, contrairement au mirroring pur (RAID 1/10).
   - **Investissement Matériel :** Les RAID 1 et RAID 10 exigent un doublement des disques, augmentant le coût global.

4. **Complexité de Gestion :**  
   - **Maintenance et Surveillance :** RAID 5/6 demandent une surveillance accrue, surtout pendant la reconstruction.
   - **Simplicité :** Le RAID 1 est facile à mettre en œuvre et à gérer, idéal pour des petites structures ou des applications simples.

---

## Mise en Pratique dans Notre Infrastructure

### Choix et Déploiement

1. **RAID 1 Matériel sur le Serveur Dell (dell1)**  
   Nous avons opté pour un **RAID 1** via la carte RAID intégrée au Dell PowerEdge, assurant une première couche de redondance locale. En cas de défaillance d’un disque, le serveur continue de fonctionner, protégeant immédiatement les données.

2. **Stockage Distribué Linstor/DRBD**  
   Pour renforcer la tolérance aux pannes et permettre une haute disponibilité multi-nœuds, nous utilisons **DRBD/Linstor**. Ainsi, le RAID 1 local du serveur Dell1 s’intègre dans un système distribué répliquant les données sur l’ensemble du cluster.

3. **OpenMediaVault Hébergé sur le Cluster**  
   Nous déployons **OpenMediaVault** en tant que VM sur ce stockage distribué, faisant office de NAS. Les utilisateurs bénéficient alors d’un point d’accès unique pour leurs données, tandis que la redondance matérielle et logicielle assure une disponibilité élevée.

---

## Cas d'Utilisation Typiques

- **RAID 1 :**  
  Adapté aux besoins de redondance simple et immédiate, comme sur notre Dell PowerEdge pour une première sécurisation des données.

- **RAID 5 :**  
  Populaire pour un bon compromis entre capacité, performance et tolérance à la panne, dans des environnements de stockage partagés (serveurs de fichiers, etc.).

- **RAID 6 :**  
  Recommandé pour les infrastructures critiques, où la perte de deux disques simultanés ne doit pas interrompre le service.

- **RAID 10 :**  
  Idéal pour des charges de travail intensives (bases de données transactionnelles, etc.) nécessitant rapidité et haute redondance.

---

## Conclusion

Le **RAID 1 matériel** sur Dell1, combiné au **stockage distribué Linstor/DRBD**, offre une solution solide et redondante pour héberger **OpenMediaVault**. Cette architecture conjugue :

- **Redondance locale** (mirroring matériel RAID 1)  
- **Haute disponibilité multi-nœuds** (DRBD/Linstor)  
- **Simplicité d’administration NAS** (OpenMediaVault)

Le choix d’un type de RAID dépendra néanmoins des besoins spécifiques de performance, de tolérance aux pannes et de coûts. Dans notre cas, **RAID 1** assure la duplication immédiate sur Dell1, tandis que **DRBD/Linstor** gère la réplication et la résilience globale du cluster, offrant ainsi un niveau de protection élevé pour les données critiques.
