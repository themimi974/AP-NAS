Guide d’installation et de configuration d’un NAS OpenMediaVault avec RAID
Ce guide vous accompagnera pas à pas pour installer OpenMediaVault (OMV) et configurer un RAID sur votre serveur NAS.
OpenMediaVault est une solution NAS open source et légère, basée sur Debian, qui offre de nombreux services (SMB/CIFS, FTP, SSH, etc.) et une interface web très simple d’utilisation.

1. Pré-requis
Matériel :

Une machine ou un serveur compatible x86_64 (ou ARM selon la version).
Au moins deux disques durs (identiques de préférence) pour configurer un RAID.
Une clé USB ou un CD/DVD pour l’installation d’OpenMediaVault.
Une connexion réseau (Ethernet).
Téléchargement d’OpenMediaVault :

Rendez-vous sur la page officielle OpenMediaVault (ou sur un mirroir fiable).
Téléchargez l’ISO correspondant à votre architecture (généralement amd64).
Création du support d’installation :

Utilisez un outil tel que Rufus, Etcher ou unetbootin pour rendre votre clé USB bootable à partir du fichier ISO.
Si vous optez pour un CD/DVD, gravez simplement l’ISO.
2. Installation d’OpenMediaVault
Démarrez la machine sur le support d’installation :

Insérez la clé USB ou le CD/DVD.
Réglez l’ordre de démarrage (boot order) dans le BIOS/UEFI pour démarrer sur ce support.
Sélectionnez la langue et le clavier :

Choisissez la langue (Français) et la disposition du clavier (France, par exemple).
Installation du système :

Sélectionnez la cible d’installation (un disque dur ou un SSD principal).
Poursuivez les étapes de l’installateur Debian (renseignement du fuseau horaire, mot de passe root, configuration réseau, etc.).
Redémarrez :

Retirez le support d’installation.
Après le redémarrage, vous devriez avoir un système fonctionnel avec OpenMediaVault préinstallé.
Accès à l’interface web :

Sur un autre ordinateur du réseau, ouvrez un navigateur et tapez l’adresse IP de votre serveur (par exemple : http://192.168.x.x).
Par défaut, l’identifiant est admin et le mot de passe est openmediavault (vous pourrez le modifier par la suite).
3. Configuration initiale
Changer le mot de passe admin :

Dans la barre latérale, rendez-vous sur System > General Settings > Web Administrator Password.
Choisissez un nouveau mot de passe pour l’interface web.
Mettre à jour le système :

Allez dans System > Update Management.
Appliquez toutes les mises à jour disponibles.
Configuration réseau (si nécessaire) :

Dans System > Network > Interfaces, configurez votre adresse IP (DHCP ou statique).
Vérifiez que votre passerelle et vos DNS soient corrects.
4. Préparation des disques pour le RAID
Avant de créer votre RAID, assurez-vous d’avoir branché tous les disques prévus.
Attention : la création d’un RAID effacera toutes les données présentes sur les disques sélectionnés.

Identification des disques :

Dans l’interface d’OMV, cliquez sur Storage > Disks.
Vérifiez que tous vos disques durs sont bien détectés.
Effacer les partitions existantes (optionnel) :

Si vos disques contiennent d’anciennes partitions, vous pouvez les effacer pour repartir de zéro.
Dans Storage > Disks, sélectionnez chaque disque puis cliquez sur Wipe (choisir Quick ou Secure suivant vos besoins).
5. Création du RAID
Accédez à la section RAID Management :

Allez dans Storage > RAID Management.
Cliquez sur Create.
Choisissez le niveau de RAID :

RAID 1 (Mirroring) : pour la redondance (deux disques minimum).
RAID 5 (Parité distribuée) : pour un compromis entre performance et sécurité (3 disques minimum).
RAID 6 : variante de RAID 5 avec double parité (4 disques minimum).
RAID 10 (RAID 1+0) : nécessite au moins 4 disques, bonnes performances et bonne redondance.
Sélectionnez les disques :

Cochez les disques qui composeront votre volume RAID.
Donnez un nom (ex: NASRAID).
Validez.
Laissez le RAID se synchroniser :

Le processus de synchronisation peut prendre plusieurs heures selon la taille des disques.
Vous verrez une barre de progression dans RAID Management.
6. Création d’un système de fichiers
Une fois le RAID créé et synchronisé, il faut créer un système de fichiers (formatage logique) :

Allez dans File Systems :

Dans Storage > File Systems, cliquez sur Create.
Sélectionnez votre volume RAID (ex: /dev/md0).
Choisissez le système de fichiers (ext4, XFS, btrfs, …).
Donnez un nom (ex: RAID_Data).
Monter le système de fichiers :

Une fois le formatage terminé, cliquez sur Mount.
Sélectionnez le point de montage proposé ou personnalisez-le.
7. Créer et partager un dossier
Pour accéder à vos fichiers sur le réseau, vous devez créer un dossier partagé :

Création du Shared Folder :

Dans Access Rights Management > Shared Folders, cliquez sur Add.
Donnez un nom (ex : Documents).
Sélectionnez le volume correspondant à votre RAID précédemment monté.
Définissez les permissions (Read/Write pour le propriétaire, par exemple).
Activation des services de partage (SMB, FTP, etc.) :

Par exemple, pour activer SMB (Windows) :
Allez dans Services > SMB/CIFS.
Activez le service et configurez le groupe de travail (par exemple WORKGROUP).
Dans l’onglet Shares, ajoutez le “Shared Folder” créé plus tôt. Sélectionnez les permissions.
Accéder au partage :

Depuis un PC Windows, ouvrez l’explorateur de fichiers et tapez \\IP_de_votre_NAS\Documents.
Sur Linux, vous pouvez monter le partage SMB avec mount -t cifs //192.168.x.x/Documents /point/de/montage.
8. Gestion des utilisateurs et droits d’accès
Pour gérer les accès aux dossiers :

Création d’utilisateurs :

Dans Access Rights Management > User, cliquez sur Add.
Renseignez un nom d’utilisateur et mot de passe.
Gestion des groupes (si nécessaire) :

Dans Access Rights Management > Group, ajoutez ou modifiez vos groupes (ex : admins, users, etc.).
Attribution des permissions :

Dans Access Rights Management > Shared Folders, sélectionnez votre dossier.
Cliquez sur Privileges pour attribuer à chaque utilisateur ou groupe les droits de lecture, d’écriture, etc.
9. Maintenance et surveillance
Surveillance du RAID :

Dans Storage > RAID Management, vérifiez régulièrement l’état du RAID (Synchronisé, Dégradé, …).
En cas de disque défectueux, OMV vous enverra une alerte si la configuration de notifications est activée.
Notifications par mail :

Dans System > Notification, configurez un serveur SMTP pour recevoir des alertes (état RAID, température des disques, etc.).
Sauvegardes :

Songez toujours à faire des sauvegardes externes, même avec un RAID. Un RAID ne protège pas totalement des sinistres, virus, ou erreurs humaines.
10. Conclusion
Vous disposez désormais d’un NAS fonctionnel sous OpenMediaVault avec un volume RAID pour sécuriser vos données.
N’hésitez pas à explorer les autres services offerts par OMV (FTP, SSH, NFS, Docker, etc.) afin d’enrichir les fonctionnalités de votre NAS.
