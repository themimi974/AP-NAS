## Document 4 : Procédure d’installation et de configuration de la solution NAS (OpenMediaVault)

### 1. Introduction  
Ce document décrit pas à pas la procédure d’installation d’OpenMediaVault (OMV) sur une machine Debian disposant de deux disques.

### 2. Pré-requis  
- Une machine Debian (physique ou virtuelle) déjà installée.  
- Deux disques :  
  - Un pour l’OS Debian.  
  - Un pour le stockage des données.  
- Droits administrateur (sudo) pour installer OMV.  
- Connexion Internet fonctionnelle pour télécharger le script d’installation.

### 3. Installation d’OpenMediaVault  
1. **Ouvrir un terminal** sur la machine Debian.  
2. **Exécuter la commande** suivante pour installer OMV :  
   ```bash
   wget -O - https://github.com/OpenMediaVault-Plugin-Developers/installScript/raw/master/install | sudo bash
   ```  
   *(Insérer image indiquant l’exécution de la commande d’installation)*

3. **Attendre la fin de l’installation** : le script va installer tous les paquets nécessaires et configurer OMV.  
4. Une fois l’installation terminée, **OpenMediaVault sera opérationnel**.  
   *(Insérer capture d’écran de la page de connexion OMV)*

### 4. Connexion à l’interface Web OMV  
- **Ouvrir un navigateur** et accéder à l’adresse IP de la machine Debian (ex. : `http://<IP_de_la_machine>/`).  
- Par défaut, les identifiants sont :  
  - **Utilisateur :** `admin`  
  - **Mot de passe :** `openmediavault`  
- Il est **fortement recommandé** de changer le mot de passe `admin` dès la première connexion.

### 5. Préparer le disque de stockage  
Avant de créer un partage, il faut **effacer et formater** le disque qui sera utilisé pour le stockage.  

1. Dans l’interface OMV, allez dans **Storage > Disks**.  
2. Sélectionnez le disque destiné au partage.  
3. Choisissez l’option **Wipe** (effacement) pour réinitialiser ce disque.  
   *(Insérer image montrant l’effacement du disque)*

### 6. Créer le système de fichiers  
1. Toujours dans **Storage**, cliquez sur **File Systems**.  
2. Cliquez sur **Create** pour formater le disque avec le système de fichiers souhaité (ex. EXT4).  
3. Une fois créé, sélectionnez la nouvelle entrée et cliquez sur **Mount** pour monter ce système de fichiers.  
4. Appliquez les changements en validant la configuration (un bandeau “Pending changes” apparaît pour confirmer).  
   *(Insérer image illustrant la création et le montage du système de fichiers)*

### 7. Créer un partage  
1. Allez dans **Storage > Shared Folders**.  
2. Cliquez sur **Add** (ou l’icône « + ») pour créer un nouveau dossier partagé.  
   - **Name** : Indiquez le nom du partage (ex. : `PartagePublic`).  
   - **File System** : Sélectionnez le système de fichiers précédemment monté.  
   - **Relative Path** : Laissez OMV proposer un chemin ou personnalisez-le (ex. : `/PartagePublic`).  
   - **Permissions** : Configurez les droits selon l’usage (par exemple, lecture seule pour le partage public ou lecture/écriture pour des groupes spécifiques).  
3. Sauvegardez et **appliquez les changements**.  
   *(Insérer image montrant le bouton de création du partage)*

### 8. Rendre le partage public  
Pour autoriser un accès public en lecture seule (ou avec les droits souhaités) :

1. Rendez-vous dans **Services > SMB/CIFS > Shares**.  
2. Cliquez sur **Add** (ou l’icône « + ») et sélectionnez le **Shared Folder** créé précédemment.  
3. Dans la section **Public**, choisissez la configuration adaptée (ex. : cochez l’option « Guest allowed » pour un accès public).  
4. Enregistrez et appliquez les changements.  
   *(Insérer image montrant les paramètres pour un partage public)*

### 9. Vérification du partage  
Une fois ces étapes réalisées, testez l’accès depuis un autre ordinateur :  
- **Windows** : Ouvrez l’explorateur et entrez `\\IP_OMV\NomDuPartage`.  
- **Linux** : Dans un explorateur de fichiers, saisissez `smb://IP_OMV/NomDuPartage`.  
- **macOS** : Dans le Finder, utilisez `Aller > Se connecter au serveur` et entrez `smb://IP_OMV/NomDuPartage`.  
   *(Insérer image illustrant la vérification du fonctionnement du partage)*

### 10. Activer les répertoires personnels  
Pour permettre à chaque utilisateur de disposer d’un dossier personnel :

1. Dans **Access Rights Management > Users > Settings**, cochez la case **Enable user home directories** (ou l’option « Répertoire personnel »).  
2. Sélectionnez le **Shared Folder** qui servira de base pour ces répertoires.  
3. Enregistrez et appliquez les changements.  
   *(Insérer image montrant l’option "Répertoire personnel" cochée)*

### 11. Créer un utilisateur de test  
1. Allez dans **Access Rights Management > Users**.  
2. Cliquez sur **Add** (ou l’icône « + »).  
3. Renseignez le nom d’utilisateur (ex. : `remi`) et un mot de passe.  
4. Enregistrez et appliquez les changements.  
   *(Insérer image présentant la liste des utilisateurs et le bouton pour créer un nouvel utilisateur)*  
   *(Insérer image illustrant la création de l’utilisateur "remi" avec les champs Nom et Mot de passe)*

### 12. Vérification du dossier personnel  
Une fois l’utilisateur créé, OMV génère automatiquement un dossier personnel pour `remi` dans le **Shared Folder** configuré pour les homes.  
- Vérifiez soit via l’interface OMV, soit en vous connectant en tant que `remi` via SMB (ex. : `\\IP_OMV\remi`).  
   *(Insérer image montrant le partage avec le dossier personnel accessible)*

### 13. Conclusion  
Vous disposez désormais d’un **serveur NAS fonctionnel** sous OpenMediaVault, avec :  
- Un partage public (configuré en lecture seule ou selon vos droits définis).  
- Un répertoire personnel automatiquement créé pour chaque utilisateur (ici, l’utilisateur `remi` a son dossier personnel).
