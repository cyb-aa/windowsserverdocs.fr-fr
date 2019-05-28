---
Title: 'Réplication DFS : Forum Aux Questions (FAQ)'
ms.date: 06/18/2014
ms.prod: windows-server-threshold
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 3782667e54f5e6b52c07645704b95fc9e7409a27
ms.sourcegitcommit: ed27ddbe316d543b7865bc10590b238290a2a1ad
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65476072"
---
# <a name="dfs-replication-frequently-asked-questions-faq"></a>Réplication DFS : Forum Aux Questions (FAQ)


Mise à jour : 30 avril 2019

S'applique à : Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Ce forum aux questions sur la réplication de système de fichiers distribués (DFS, Distributed File System) (également appelé DFS-R ou DFSR) pour Windows Server.

Pour plus d'informations sur les espaces de noms DFS, consultez [Espaces de noms DFS : Forum aux Questions](https://technet.microsoft.com/en-us/library/ee404780).

Pour plus d’informations sur les nouvelles fonctionnalités de la réplication DFS, consultez les rubriques suivantes :

  - [Espaces de noms DFS et la vue d’ensemble de la réplication DFS](http://technet.microsoft.com/en-us/library/jj127250) (dans Windows Server 2012)  
      
  - [Quelles sont les nouveautés dans le système de fichiers distribués](https://technet.microsoft.com/en-us/library/ee307957) rubrique dans [modifications apportées aux fonctionnalités de Windows Server 2008 vers Windows Server 2008 R2](https://technet.microsoft.com/en-us/library/dd391932)  
      
  - [Système de fichiers distribués](https://technet.microsoft.com/en-us/library/cc753479) rubrique dans [modifications apportées aux fonctionnalités de Windows Server 2003 avec SP1 pour Windows Server 2008](https://technet.microsoft.com/en-us/library/cc753208)  
      

Pour obtenir la liste des modifications récentes apportées à cette rubrique, consultez la section [Historique des modifications](#change-history) de cette rubrique.

      

## <a name="interoperability"></a>Interopérabilité

### <a name="can-dfs-replication-communicate-with-frs"></a>La réplication DFS peut communiquer avec FRS ?

Non. La réplication DFS ne communique pas avec le Service de réplication de fichiers (FRS). La réplication DFS et FRS peuvent s’exécuter sur le même serveur en même temps, mais ils ne doivent jamais être configurés pour répliquer les dossiers ou les sous-dossiers même, car cela peut entraîner une perte de données.

### <a name="can-dfs-replication-replace-frs-for-sysvol-replication"></a>La réplication DFS peut remplacer le FRS pour la réplication SYSVOL

Oui, la réplication DFS peut remplacer FRS pour la réplication SYSVOL sur des serveurs exécutant Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008. Les serveurs exécutant Windows Server 2003 R2 ne prennent pas en charge à l’aide de la réplication DFS pour répliquer le dossier SYSVOL.

Pour plus d’informations sur la réplication SYSVOL à l’aide de la réplication DFS, voir le [Guide de Migration de réplication SYSVOL : Réplication FRS vers DFS](https://technet.microsoft.com/en-us/library/dd640019).

### <a name="can-i-upgrade-from-frs-to-dfs-replication-without-losing-configuration-settings"></a>Puis-je mettre à niveau à partir de FRS vers la réplication DFS sans perdre les paramètres de configuration ?

Oui. Pour migrer la réplication de FRS vers la réplication DFS, consultez les documents suivants :

  - Pour migrer la réplication des dossiers autres que le dossier SYSVOL, consultez [Guide des opérations DFS : Migration de FRS vers la réplication DFS](http://go.microsoft.com/fwlink/?linkid=192776) et [FRS2DFSR – un FRS à l’utilitaire de Migration DFSR](http://go.microsoft.com/fwlink/?linkid=195437) (http://go.microsoft.com/fwlink/?LinkID=195437).  
      
  - Pour migrer la réplication du dossier SYSVOL à la réplication DFS, voir [Guide de Migration de réplication SYSVOL : Réplication FRS vers DFS](https://technet.microsoft.com/en-us/library/dd640019).  
      

### <a name="can-i-use-dfs-replication-in-a-mixed-windowsunix-environment"></a>Puis-je utiliser la réplication DFS dans un environnement mixte de Windows/UNIX ?

Oui. Bien que la réplication DFS ne prend en charge la réplication du contenu entre des serveurs exécutant Windows Server, les clients UNIX peuvent accéder aux partages de fichiers sur les serveurs Windows. Pour ce faire, installez les Services pour les systèmes de fichiers réseau (NFS) sur le serveur de réplication DFS.

Vous pouvez également utiliser la fonctionnalité de client SMB/CIFS incluse dans de nombreux clients UNIX d’accéder directement le fichier Windows partage, bien que cette fonctionnalité est souvent limitée ou nécessite des modifications à l’environnement Windows (par exemple, la désactivation de la signature SMB à l’aide de Stratégie de groupe).

La réplication DFS interagit avec NFS sur un serveur exécutant un système d’exploitation Windows, mais vous ne pouvez pas répliquer un point de montage NFS.

### <a name="can-i-use-the-volume-shadow-copy-service-with-dfs-replication"></a>Puis-je utiliser le Service de cliché instantané de Volume avec la réplication DFS ?

Oui. La réplication DFS est prise en charge sur les volumes de Volume Shadow Copy Service (VSS) et captures instantanées peuvent être restaurées correctement avec le Client de Versions précédentes.

### <a name="can-i-use-windowsbackup-ntbackupexe-to-remotely-back-up-a-replicated-folder"></a>Puis-je utiliser Windows Backup (Ntbackup.exe) à distance sauvegarder un dossier répliqué ?

Non, à l’aide de Windows Backup (Ntbackup.exe) sur un ordinateur exécutant Windows Server 2003 ou version antérieure vers sauvegarder le contenu d’un dossier répliqué sur un ordinateur exécutant Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008 n'est pas pris en charge.

Pour sauvegarder des fichiers qui sont stockés dans un dossier répliqué, utiliser sauvegarde Windows Server ou Microsoft® System Center Data Protection Manager. Pour plus d’informations sur les fonctionnalités de sauvegarde et de restauration dans Windows Server 2008 R2 et Windows Server 2008, consultez [sauvegarde et restauration](https://technet.microsoft.com/en-us/library/Cc754097). Pour plus d’informations, consultez [System Center Data Protection Manager](http://go.microsoft.com/fwlink/?linkid=182261) (http://go.microsoft.com/fwlink/?LinkId=182261).

### <a name="do-file-system-policies-impact-dfs-replication"></a>Stratégies système de fichiers impact sur la réplication DFS ?

Oui. Ne configurez pas les stratégies système de fichiers sur les dossiers répliqués. La stratégie de système de fichiers réapplique les autorisations NTFS à chaque intervalle d’actualisation de stratégie de groupe. Cela peut entraîner les violations de partage, car un fichier ouvert n’est pas répliqué jusqu'à ce que le fichier est fermé.

### <a name="does-dfs-replication-replicate-mailboxes-hosted-on-microsoft-exchange-server"></a>La réplication DFS réplique des boîtes aux lettres hébergées sur Microsoft Exchange Server ?

Non. La réplication DFS ne peut pas être utilisée pour répliquer les boîtes aux lettres hébergées sur Microsoft Exchange Server.

### <a name="does-dfs-replication-support-file-screens-created-by-file-server-resource-manager"></a>La réplication DFS prend en charge les filtres de fichiers créés par le Gestionnaire de ressources du serveur de fichiers ?

Oui. Toutefois, le paramètres de filtrage de fichier de File Server Resource Manager (FSRM) doit correspondre aux deux extrémités de la réplication. En outre, la réplication DFS a son propre mécanisme de filtre pour les fichiers et dossiers que vous pouvez utiliser pour exclure certains fichiers et les types de fichiers de la réplication.

Meilleures pratiques pour implémenter les quotas ou les filtres de fichiers sont les suivantes :

  - Le dossier DfsrPrivate masqué ne doit pas être soumis à des quotas ou les filtres de fichiers.  
      
  - Fichiers filtrés ne doivent pas exister dans n’importe quel dossier répliqué avant que le filtrage est activé.  
      
  - Aucun dossier ne peut dépasser le quota avant que le quota soit activé.  
      
  - Vous devez utiliser des quotas impératifs avec précaution. Il est possible pour les membres individuels d’un groupe de réplication à rester au sein d’un quota avant la réplication, mais la dépasser lors de la réplication de fichiers. Par exemple, si un utilisateur copie un fichier de 10 mégaoctets (Mo) sur le serveur A (qui est alors à la limite inconditionnelle) et un autre utilisateur copie un fichier de 5 Mo sur le serveur B, lors de la prochaine réplication se produit, les deux serveurs dépassera le quota de 5 mégaoctets. Cela peut entraîner la réplication DFS effectue de nouvelles tentatives réplication des fichiers, à l’origine des trous dans le vecteur de version et les éventuels problèmes de performances.  
      

### <a name="is-dfs-replication-cluster-aware"></a>Cluster de la réplication DFS est prenant en charge ?

Oui, la réplication DFS dans Windows Server 2012 R2, Windows Server 2012 et Windows Server 2008 R2 inclut la possibilité d’ajouter un cluster de basculement en tant que membre d’un groupe de réplication. Pour plus d’informations, consultez [ajouter un Cluster de basculement à un groupe de réplication](http://go.microsoft.com/fwlink/?linkid=155085) (http://go.microsoft.com/fwlink/?LinkId=155085). Le service de réplication DFS sur les versions de Windows avant Windows Server 2008 R2 n’est pas conçu pour coordonner avec un cluster de basculement, et le service ne basculent pas vers un autre nœud.


> [!NOTE]
> La réplication DFS ne prend pas en charge la réplication de fichiers sur les Volumes partagés de Cluster. 
<br>


### <a name="is-dfs-replication-compatible-with-data-deduplication"></a>La réplication DFS n’est compatible avec la déduplication des données ?

Oui, la réplication DFS peut répliquer des dossiers sur des volumes qui utilisent la déduplication des données dans Windows Server.

### <a name="is-dfs-replication-compatible-with-ris-and-wds"></a>La réplication DFS n’est compatible avec RIS et WDS ?

Oui. La réplication DFS réplique les volumes sur lesquels le stockage d’Instance unique (SIS) est activé. SIS est utilisée par les Services d’Installation à distance (RIS), les Services de déploiement Windows (WDS) et Windows Storage Server.

### <a name="is-it-possible-to-use-dfs-replication-with-offline-files"></a>Il est possible d’utiliser la réplication DFS avec fichiers hors connexion ?

Vous pouvez en toute sécurité utiliser la réplication DFS et des fichiers hors connexion ensemble dans des scénarios quand il existe un seul utilisateur à la fois qui écrit dans les fichiers. Cela est utile pour les utilisateurs qui voyagent entre deux succursales et souhaitez être en mesure d’accéder à leurs fichiers à deux branches ou while hors connexion. Fichiers hors connexion met en cache les fichiers localement pour une utilisation hors connexion et la réplication DFS réplique les données entre chaque succursale.

N’utilisez pas la réplication DFS avec fichiers hors connexion dans un environnement multi-utilisateur, car la réplication DFS ne fournit pas de n’importe quel mécanisme de verrouillage distribuée ou une fonctionnalité d’extraction de fichiers. Si deux utilisateurs modifient le même fichier en même temps sur des serveurs différents, la réplication DFS déplace l’ancien fichier vers le DfsrPrivate\\ConflictandDeleted dossier (situé sous le chemin d’accès local du dossier répliqué) lors de la prochaine réplication.

### <a name="what-antivirus-applications-are-compatible-with-dfs-replication"></a>Quelles sont les applications antiviruss sont compatibles avec la réplication DFS ?

Les applications antiviruss peuvent provoquer une réplication excessive si leurs activités d’analyse à modifier les fichiers dans un dossier répliqué. Pour plus d’informations, [test Antivirus l’interopérabilité d’Application avec la réplication DFS](http://go.microsoft.com/fwlink/?linkid=73990) (http://go.microsoft.com/fwlink/?LinkId=73990).

### <a name="what-are-the-benefits-of-using-dfs-replication-instead-of-windows-sharepoint-services"></a>Quels sont les avantages de l’utilisation de la réplication DFS au lieu de Windows SharePoint Services ?

Windows® SharePoint® Services fournit une cohérence étroite sous la forme d’une fonctionnalité de récupération de fichier qui n’est pas le cas de la réplication DFS. Si vous êtes inquiet à plusieurs personnes modifiant le fichier de même, nous recommandons à l’aide de Windows SharePoint Services. Windows SharePoint Services 2.0 avec Service Pack 2 est disponible en tant que partie de Windows Server 2003 R2. Windows SharePoint Services peut être téléchargés à partir du site Web de Microsoft ; Il n’est pas inclus dans les versions plus récentes de Windows Server. Toutefois, si vous répliquez des données sur plusieurs sites et les utilisateurs ne seront pas modifier les mêmes fichiers en même temps, la réplication DFS fournit la bande passante et une gestion simplifiée.

## <a name="limitations-and-requirements"></a>Configuration requise et limitations

### <a name="can-dfs-replication-replicate-between-branch-offices-without-a-vpn-connection"></a>La réplication DFS peut répliquer entre succursales sans une connexion VPN ?

Oui, en supposant qu’il existe une liaison réseau étendu (WAN) privée (pas à Internet) se connectant les succursales. Toutefois, vous devez ouvrir les ports appropriés dans les pare-feux externes. La réplication DFS utilise le mappeur de point de terminaison RPC (port 135) et un port éphémère attribué de façon aléatoire au-dessus de 1024. Vous pouvez utiliser la **Dfsrdiag** outil en ligne de commande pour spécifier un port statique au lieu du port éphémère. Pour plus d’informations sur la façon de spécifier le mappeur de point de terminaison RPC, consultez [article 154596](http://go.microsoft.com/fwlink/?linkid=73991) dans la Base de connaissances Microsoft (http://go.microsoft.com/fwlink/?LinkId=73991).

### <a name="can-dfs-replication-replicate-files-encrypted-with-the-encrypting-file-system"></a>La réplication DFS peut répliquer des fichiers chiffrés avec le système de fichiers EFS ?

Non. La réplication DFS ne réplique pas les fichiers ou dossiers qui sont chiffrées à l’aide de système de fichiers EFS (Encrypting File System). Si un utilisateur crypte un fichier qui a été précédemment répliqué, la réplication DFS supprime le fichier à partir de tous les autres membres du groupe de réplication. Cela garantit que la copie disponible uniquement du fichier est une version chiffrée sur le serveur.

### <a name="can-dfs-replication-replicate-outlook-pst-or-microsoft-office-access-database-files"></a>La réplication DFS peut répliquer des Outlook .pst ou des fichiers de base de données Microsoft Office Access ?

La réplication DFS peut répliquer en toute sécurité les fichiers de Microsoft Access et les fichiers du dossier personnel de Microsoft Outlook (.pst) uniquement si elles sont stockées pour l’archivage et ne sont pas accessibles sur le réseau à l’aide d’un client tel que Outlook ou d’accès (pour ouvrir .pst ou accès fichiers, tout d’abord copier les fichiers vers un périphérique de stockage local). Les raisons sont les suivantes :

  - Ouverture de fichiers .pst sur des connexions réseau peut entraîner une altération des données dans les fichiers .pst. Pour plus d’informations sur les raisons pour lesquelles des fichiers .pst ne peut pas en toute sécurité accessibles à partir d’un réseau, consultez [article 297019](http://go.microsoft.com/fwlink/?linkid=125363) dans la Base de connaissances Microsoft (http://go.microsoft.com/fwlink/?LinkId=125363).  
      
  - fichiers .pst et accès ont tendance à rester ouverte pendant de longues périodes de temps pendant accédée par un client tel que Outlook ou Office Access. Cela empêche la réplication DFS de répliquer ces fichiers, jusqu'à ce qu’ils sont fermés.  
      

### <a name="can-i-use-dfs-replication-in-a-workgroup"></a>Puis-je utiliser la réplication DFS dans un groupe de travail ?

Non. La réplication DFS s’appuie sur Active Directory® Domain Services pour la configuration. Il ne fonctionne que dans un domaine.

### <a name="can-more-than-one-folder-be-replicated-on-a-single-server"></a>Plus d’un dossier peut être répliqué sur un seul serveur ?

Oui. La réplication DFS peut répliquer de nombreux dossiers entre les serveurs. Assurez-vous que chaque dossier répliqué possède une valeur unique racine du chemin d’accès et qu’ils ne se chevauchent pas. Par exemple, D:\\Sales et D:\\comptabilité peut être les chemins d’accès racine pour les deux dossiers répliqués, mais D:\\Sales et D:\\Sales\\rapports ne peut pas être les chemins d’accès racine pour les deux dossiers répliqués.

### <a name="does-dfs-replication-require-dfs-namespaces"></a>La réplication DFS nécessite d’espaces de noms DFS ?

Non. La réplication DFS et des espaces de noms DFS peuvent être utilisé séparément ou ensemble. En outre, la réplication DFS peut être utilisée pour répliquer les espaces de noms DFS autonome, ce qui n’était pas possible avec FRS.

### <a name="does-dfs-replication-require-time-synchronization-between-servers"></a>La réplication DFS nécessite la synchronisation horaire entre les serveurs ?

Non. La réplication DFS ne nécessite pas explicitement la synchronisation horaire entre serveurs. Toutefois, la réplication DFS nécessite que les horloges des serveurs correspondent étroitement. Les horloges des serveurs doivent être définies dans les cinq minutes de l’autre (par défaut) pour l’authentification Kerberos fonctionne correctement. Par exemple, la réplication DFS utilise des horodatages pour déterminer quel fichier est prioritaire en cas de conflit. Heures précises sont également importants pour le garbage collection, planifications et autres fonctionnalités.

### <a name="does-dfs-replication-support-replicating-an-entire-volume"></a>La réplication DFS prend-il en charge la réplication d’un volume entier ?

Oui. Toutefois, vous devez d’abord installer Windows Server 2003 Service Pack 2 ou le correctif logiciel. Pour plus d’informations, consultez [article 920335](http://go.microsoft.com/fwlink/?linkid=76776) dans la Base de connaissances Microsoft (http://go.microsoft.com/fwlink/?LinkId=76776). En outre, la réplication d’un volume entier peut entraîner les problèmes suivants :

  - Si le volume contient un fichier de pagination Windows, la réplication échoue et consigne l’événement DFSR 4312 dans le journal des événements système.  
      
  - La réplication DFS définit les attributs système et caché sur le dossier répliqué sur les serveurs de destination. Cela se produit parce que Windows applique les attributs système et caché dans le dossier racine de volume par défaut. Si le chemin d’accès local du dossier répliqué sur les serveurs de destination est également une racine du volume, aucune modification supplémentaire n’est apportées aux attributs de dossier.  
      
  - Lors de la réplication d’un volume qui contient le dossier du système Windows, la réplication DFS reconnaît le dossier % Windir% et ne le réplique pas. Toutefois, la réplication DFS réplique des dossiers utilisés par les applications non Microsoft, susceptibles de provoquer l’échec des applications sur les serveurs de destination si les applications ont des problèmes d’interopérabilité avec la réplication DFS.  
      

### <a name="does-dfs-replication-support-rpc-over-http"></a>La réplication DFS prend-elle en charge les RPC sur HTTP ?

Non.

### <a name="does-dfs-replication-work-across-wireless-networks"></a>La réplication DFS fonctionne sur des réseaux sans fil ?

Oui. La réplication DFS est indépendante du type de connexion.

### <a name="does-dfs-replication-work-on-refs-or-fat-volumes"></a>La réplication DFS fonctionne sur ReFS ou les volumes FAT ?

Non. La réplication DFS prend en charge les volumes formatés avec le système de fichiers NTFS uniquement ; le système de fichiers résilient (ReFS) et le système de fichiers FAT ne sont pas pris en charge. La réplication DFS nécessite NTFS, car il utilise le journal des modifications NTFS et d’autres fonctionnalités de NTFS système de fichiers.

### <a name="does-dfs-replication-work-with-sparse-files"></a>La réplication DFS fonctionne avec les fichiers partiellement alloués ?

Oui. Vous pouvez répliquer des fichiers partiellement alloués. Le **Sparse** attribut est conservé sur le membre de réception.

### <a name="do-i-need-to-log-in-as-administrator-to-replicate-files"></a>J’ai besoin pour vous connecter en tant qu’administrateur pour répliquer des fichiers ?

Non. La réplication DFS est un service qui s’exécute sous le compte système local, vous n’avez pas besoin de se connecter en tant qu’administrateur à répliquer. Toutefois, vous devez être un administrateur de domaine ou un administrateur local des serveurs de fichiers affectés pour apporter des modifications à la configuration de la réplication DFS.

Pour plus d’informations, consultez « exigences de sécurité de la réplication DFS et délégation » dans le [déléguer la capacité à gérer la réplication DFS](http://go.microsoft.com/fwlink/?linkid=182294) (http://go.microsoft.com/fwlink/?LinkId=182294).

### <a name="how-can-i-upgrade-or-replace-a-dfs-replication-member"></a>Comment puis-je mettre à niveau ou remplacer un membre de la réplication DFS ?

Mettre à niveau ou remplacer un membre de la réplication DFS, consultez le blog sur la clé Ask blog de l’équipe de Services d’annuaire : [Remplacement DFSR de matériel ou système d’exploitation](http://blogs.technet.com/b/askds/archive/2010/09/10/series-wrap-up-and-downloads-replacing-dfsr-member-hardware-or-os.aspx).

### <a name="is-dfs-replication-suitable-for-replicating-roaming-profiles"></a>La réplication DFS est approprié pour la réplication des profils itinérants ?

Oui. Certains scénarios sont pris en charge lors de la réplication des profils utilisateur itinérants. Pour plus d’informations sur les scénarios pris en charge, consultez [Support instruction autour répliquées données de Microsoft de profil utilisateur](http://go.microsoft.com/fwlink/?linkid=201282) (http://go.microsoft.com/fwlink/?LinkId=201282).

### <a name="is-there-a-file-character-limit-or-limit-to-the-folder-depth"></a>Y a-t-il une limite de caractères de fichier ou d’une limite à la profondeur de dossier ?

Windows et la réplication DFS prennent en charge les chemins d’accès de dossier avec jusqu'à des milliers de 32 caractères. La réplication DFS n’est pas limitée aux chemins d’accès de dossier de 260 caractères.

### <a name="must-members-of-a-replication-group-reside-in-the-same-domain"></a>Membres d’un groupe de réplication doivent résider dans le même domaine ?

Non. Groupes de réplication peuvent s’étendre sur les domaines d’une forêt unique, mais pas entre des forêts différentes.

### <a name="what-are-the-supported-limits-of-dfs-replication"></a>Quelles sont les limites prises en charge de la réplication DFS ?

La liste suivante fournit un ensemble d’instructions d’évolutivité qui ont été testés par Microsoft sur Windows Server 2012 R2 :

  - Taille de tous les fichiers répliqués sur un serveur : 100 To.  
      
  - Nombre de fichiers répliqués sur un volume : 70 millions.  
      
  - Taille de fichier maximale : 250 Go.  
      


> [!IMPORTANT]
> Lors de la création de groupes de réplication avec un grand nombre ou la taille des fichiers nous vous recommandons d’exportation d’un clone de la base de données et à l’aide de techniques d’amorçage préalable pour réduire la durée de la réplication initiale. Pour plus d’informations, consultez <A href="http://blogs.technet.com/b/filecab/archive/2013/08/21/dfs-replication-initial-sync-in-windows-server-2012-r2-attack-of-the-clones.aspx">synchronisation initiale de la réplication DFS dans Windows Server 2012 R2 : L’attaque des Clones</A>. 
<br>


La liste suivante fournit un ensemble d’instructions d’évolutivité qui ont été testés par Microsoft sur Windows Server 2012, Windows Server 2008 R2 et Windows Server 2008 :

  - Taille de tous les fichiers répliqués sur un serveur : 10 téraoctets.  
      
  - Nombre de fichiers répliqués sur un volume : 11 millions.  
      
  - Taille de fichier maximale : 64 Go.  
      


> [!NOTE]
> Il n’est plus une limite au nombre de groupes de réplication, les dossiers répliqués, les connexions ou les membres du groupe de réplication. 
<br>


Pour obtenir la liste des recommandations d’évolutivité qui ont été testés par Microsoft pour Windows Server 2003 R2, consultez [les instructions de l’évolutivité de la réplication DFS](http://go.microsoft.com/fwlink/?linkid=75043) (http://go.microsoft.com/fwlink/?LinkId=75043).

### <a name="when-should-i-not-use-dfs-replication"></a>Cas ne dois-je pas utiliser la réplication DFS ?

N’utilisez pas la réplication DFS dans un environnement où plusieurs utilisateurs mettre à jour ou modifier les mêmes fichiers simultanément sur différents serveurs. Cela peut entraîner la réplication DFS déplacer des copies des fichiers en conflit pour le DfsrPrivate masqué\\ConflictandDeleted dossier.

Lorsque plusieurs utilisateurs doivent modifier les mêmes fichiers en même temps sur des serveurs différents, utilisez la fonctionnalité d’extraction de fichiers de Windows SharePoint Services pour vous assurer que seul un utilisateur travaille sur un fichier. Windows SharePoint Services 2.0 avec Service Pack 2 est disponible en tant que partie de Windows Server 2003 R2. Windows SharePoint Services peut être téléchargés à partir du site Web de Microsoft ; Il n’est pas inclus dans les versions plus récentes de Windows Server.

### <a name="why-is-a-schema-update-required-for-dfs-replication"></a>Pourquoi est une mise à jour de schéma requise pour la réplication DFS ?

La réplication DFS utilise des nouveaux objets dans le contexte de nommage de domaine des Services de domaine Active Directory pour stocker les informations de configuration. Ces objets sont créés lorsque vous mettez à jour le schéma Active Directory Domain Services. Pour plus d’informations, consultez [réviser les exigences pour la réplication DFS](http://go.microsoft.com/fwlink/?linkid=182264) (http://go.microsoft.com/fwlink/?LinkId=182264).

## <a name="monitoring-and-management-tools"></a>Outils de surveillance et gestion

### <a name="can-i-automate-the-health-report-to-receive-warnings"></a>Puis-je automatiser le rapport d’intégrité pour recevoir des avertissements ?

Oui. Il existe trois façons d’automatiser les rapports d’intégrité :

  - Utiliser le module de DFSR Windows PowerShell inclus dans Windows Server 2012 R2 ou DfsrAdmin.exe conjointement avec les tâches planifiées pour générer régulièrement des rapports d’intégrité. Pour plus d’informations, consultez [automatisation des rapports d’intégrité de la réplication DFS](http://go.microsoft.com/fwlink/?linkid=74010) (http://go.microsoft.com/fwlink/?LinkId=74010).  
      
  - Utilisez le Pack d’administration de réplication DFS pour System Center Operations Manager pour créer des alertes qui sont basées sur les conditions spécifiées.  
      
  - Utiliser le fournisseur WMI de réplication DFS pour écrire des alertes.  
      

### <a name="can-i-use-microsoft-system-center-operations-manager-to-monitor-dfs-replication"></a>Puis-je utiliser Microsoft System Center Operations Manager pour surveiller la réplication DFS ?

Oui. Pour plus d’informations, consultez le [Pack d’administration de réplication DFS pour System Center Operations Manager 2007](http://go.microsoft.com/fwlink/?linkid=182265) dans du Microsoft Download Center (http://go.microsoft.com/fwlink/?LinkId=182265).

### <a name="does-dfs-replication-support-remote-management"></a>La réplication DFS prend en charge la gestion à distance ?

Oui. La réplication DFS prend en charge la gestion à distance à l’aide de la console de gestion DFS et la **ajouter un groupe de réplication** commande. Par exemple, sur le serveur A, vous pouvez vous connecter à un groupe de réplication défini dans la forêt avec des serveurs A et B en tant que membres.

La gestion DFS est incluse dans Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008 et Windows Server 2003 R2. Pour gérer la réplication DFS à partir d’autres versions de Windows, utilisez le Bureau à distance ou le [à distance serveur outils d’Administration pour Windows 7](https://technet.microsoft.com/en-us/library/Ee449475).


> [!IMPORTANT]
> Pour afficher ou gérer des groupes de réplication qui contiennent des dossiers répliqués en lecture seule ou les membres qui sont des clusters de basculement, vous devez utiliser la version de la gestion DFS qui est inclus avec Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, le <a href="http://go.microsoft.com/fwlink/p/?linkid=238560">Outils d’Administration de serveur distant pour Windows 8</a>, ou le <a href="https://technet.microsoft.com/en-us/library/ee449475">outils d’Administration de serveur distant pour Windows 7</a>. 
<br>


### <a name="do-ultrasound-and-sonar-work-with-dfs-replication"></a>Ultrasound et Sonar fonctionnent avec la réplication DFS ?

Non. La réplication DFS a son propre ensemble d’outils de surveillance et de diagnostic. Ultrasound et Sonar sont uniquement capables de surveillance FRS.

### <a name="how-can-files-be-recovered-from-the-conflictanddeleted-or-preexisting-folders"></a>Comment les fichiers peuvent être récupérées à partir des dossiers ConflictAndDeleted ou PreExisting ?

Pour récupérer des fichiers perdus, restaurer les fichiers à partir du dossier système de fichiers ou un dossier partagé à l’aide de l’historique des fichiers, le **restaurer les versions précédentes** commande dans l’Explorateur de fichiers, ou en restaurant les fichiers à partir de la sauvegarde. Pour récupérer des fichiers directement à partir du dossier ConflictAndDeleted ou PreExisting, utilisez le `Get-DfsrPreservedFiles` et `Restore-DfsrPreservedFiles` applets de commande Windows PowerShell (inclus avec le module DFSR dans Windows Server 2012 R2), ou le [RestoreDFSR](http://code.msdn.microsoft.com/restoredfsr) exemple de script à partir de MSDN Code Gallery. Ce script est prévu uniquement pour la récupération d’urgence et n’est fourni en tant que-l’état, sans garantie.

### <a name="is-there-a-way-to-know-the-state-of-replication"></a>Existe-t-il un moyen de connaître l’état de réplication ?

Oui. Il existe plusieurs façons pour surveiller la réplication :

  - La réplication DFS a un pack d’administration pour System Center Operations Manager qui assure une surveillance proactive.  
      
  - La gestion DFS a un rapport de diagnostic de l’emploi pour le backlog de réplication, l’efficacité de réplication et le nombre de fichiers et dossiers dans un groupe de réplication donné.  
      
  - Le module DFSR Windows PowerShell dans Windows Server 2012 R2 contient des applets de commande pour le démarrage des tests de la propagation et écriture de la propagation et l’intégrité des rapports. Pour plus d’informations, consultez [Distributed fichier système réplication applets de commande Windows PowerShell](http://technet.microsoft.com/library/dn296601.aspx).  
      
  - Dfsrdiag.exe est un outil de ligne de commande qui peut générer un nombre de backlog ou du déclencheur un test de propagation. Ces deux rapports indiquent l’état de réplication. La propagation vous indique si les fichiers sont répliquées vers tous les nœuds. Backlog vous montre combien de fichiers doivent toujours à la réplication avant que les deux ordinateurs sont synchronisés. Le nombre de backlog est le nombre de mises à jour un membre de groupe de réplication n’a pas été traité. Sur les ordinateurs exécutant Windows Server 2012 R2, Windows Server 2012 ou Windows Server 2008 R2, Dfsrdiag.exe peut également afficher les mises à jour la réplication DFS est en cours de réplication.  
      
  - Scripts peuvent utiliser WMI pour collecter des informations de backlog, manuellement ou via MOM.  
      

## <a name="performance"></a>Performances

### <a name="does-dfs-replication-support-dial-up-connections"></a>La réplication DFS prend en charge les connexions à distance ?

Bien que la réplication DFS fonctionne à la vitesse d’accès à distance, il peut obtenir retardée si grand nombre de modifications à répliquer. Si de petites modifications sont apportées aux fichiers existants, la réplication DFS avec la Compression RDC (Remote Differential) fournira un bien meilleures performances que de copier le fichier directement.

### <a name="does-dfs-replication-perform-bandwidth-sensing"></a>La réplication DFS effectue la détection de la bande passante ?

Non. La réplication DFS n’effectue pas la détection de la bande passante. Vous pouvez configurer la réplication DFS pour utiliser une quantité limitée de la bande passante sur une base par connexion (limitation de bande passante). Toutefois, la réplication DFS ne pas réduire davantage l’utilisation de la bande passante si l’interface réseau devienne saturée, et la réplication DFS peut saturer le lien sur de courtes périodes. La bande passante de la limitation avec la réplication DFS n’est pas totalement exacte, car la réplication DFS limite la bande passante par la limitation des appels RPC. Par conséquent, différentes mémoires tampons dans les niveaux inférieurs de la pile de réseau (y compris RPC) peuvent interférer, à l’origine des pics de trafic réseau.

### <a name="does-dfs-replication-throttle-bandwidth-per-schedule-per-server-or-per-connection"></a>La réplication DFS limiter la bande passante conformément à la planification, par serveur ou par connexion ?

Si vous configurez la limitation de bande passante lors de la spécification de la planification, toutes les connexions pour ce groupe de réplication utilise ce paramètre pour la limitation de bande passante. Limitation de bande passante peut également être définie comme un paramètre de niveau de la connexion à l’aide de la gestion DFS.

### <a name="does-dfs-replication-use-active-directory-domain-services-to-calculate-site-links-and-connection-costs"></a>La réplication DFS utilise des Services de domaine Active Directory pour calculer des liens de sites et les coûts de connexion ?

Non. La réplication DFS utilise la topologie définie par l’administrateur, qui est indépendant de l’évaluation des coûts de site Active Directory Domain Services.

### <a name="how-can-i-improve-replication-performance"></a>Comment puis-je améliorer les performances de réplication ?

Pour en savoir plus sur les différentes méthodes de réglage des performances de réplication, consultez [paramétrage des performances de réplication dans DFSR](http://blogs.technet.com/b/askds/archive/2010/03/31/tuning-replication-performance-in-dfsr-especially-on-win2008-r2.aspx) sur le [poser le blog de l’équipe de Services d’annuaire](http://blogs.technet.com/b/askds/).

### <a name="how-does-dfs-replication-avoid-saturating-a-connection"></a>Comment la réplication DFS n’éviter la saturation d’une connexion ?

Dans la réplication DFS, vous définissez la bande passante maximale que vous souhaitez utiliser sur une connexion, et le service gère ce niveau de l’utilisation du réseau. Ceci diffère de la Background Intelligent Transfer Service (BITS) et la réplication DFS de ne pas saturer la connexion si vous configurer comme il convient.

Néanmoins, la limitation de bande passante n’est pas précis à 100 % et la réplication DFS peut saturer le lien pour de courtes périodes. Il s’agit, car la réplication DFS limite la bande passante par la limitation des appels RPC. Étant donné que ce processus dépend de plusieurs mémoires tampons dans les niveaux inférieurs de la pile réseau, y compris RPC, le trafic de réplication a tendance à voyager en rafales qui peuvent saturer parfois les liens de réseau.

La réplication DFS dans Windows Server 2008 inclut plusieurs améliorations de performances, comme indiqué dans [système de fichiers distribués](https://technet.microsoft.com/en-us/library/Cc753479), une rubrique dans [modifications apportées aux fonctionnalités de Windows Server 2003 avec SP1 pour Windows Server 2008](https://technet.microsoft.com/en-us/library/cc753208).

### <a name="how-does-dfs-replication-performance-compare-with-frs"></a>Comment les performances de la réplication DFS se distingue-t-il avec FRS ?

La réplication DFS est beaucoup plus rapide que FRS, en particulier lorsque de petites modifications sont apportées à des fichiers volumineux et la compression différentielle à distance est activé. Par exemple, avec la compression différentielle à distance, une petite modification à une présentation MB PowerPoint® 2 peut entraîner uniquement 60 kilo-octets (Ko) qui sont envoyées sur le réseau, une économie de 97 % dans les octets transférés.

RDC n’est pas utilisé sur des fichiers est inférieure à 64 Ko et peuvent ne pas être bénéfique sur des réseaux locaux à haut débit où la bande passante réseau n’est pas conflits. Compression différentielle à distance peut être désactivé sur une fonction de la connexion à l’aide de la gestion DFS.

### <a name="how-frequently-does-dfs-replication-replicate-data"></a>La fréquence à laquelle la réplication DFS réplique des données ?

Les données sont répliquées conformément à la planification que vous définissez. Par exemple, vous pouvez définir la planification à intervalles de 15 minutes, sept jours par semaine. Au cours de ces intervalles, la réplication est activée. La réplication démarre dès qu’une modification de fichier est détectée (généralement en quelques secondes).

La planification de groupe de réplication peut-être être définie en heure universelle coordonnée (UTC) alors que la planification de la connexion est définie sur l’heure locale du membre de réception. Prendre en compte quand le groupe de réplication s’étend sur plusieurs fuseaux horaires. Heure locale signifie que l’heure du membre de l’hébergement de la connexion entrante. La planification affichée de la connexion entrante et la connexion sortante correspondante reflètent les différences de fuseau horaire lorsque la planification est définie sur l’heure locale.

### <a name="how-much-of-my-servers-system-resources-will-dfs-replication-consume"></a>La quantité de ressources de système de mon serveur consommera la réplication DFS ?

Le disque, mémoire et les ressources processeur utilisées par la réplication DFS dépendent de plusieurs facteurs, notamment le nombre et la taille des fichiers, des taux de changement, nombre de membres du groupe de réplication et de nombre de dossiers répliqués. En outre, certaines ressources sont plus difficile à estimer. Par exemple, la technologie de moteur de stockage Extensible (ESE) utilisée pour la base de données de la réplication DFS peut consommer un grand pourcentage de mémoire disponible, ce qui libère l’à la demande. Les applications autres que la réplication DFS peuvent être hébergées sur le même serveur en fonction de la configuration du serveur. Toutefois, lorsque vous hébergez plusieurs applications ou les rôles de serveur sur un seul serveur, il est important de tester cette configuration avant de l’implémenter dans un environnement de production.

### <a name="what-happens-if-a-wan-link-fails-during-replication"></a>Que se passe-t-il si un lien WAN échoue lors de la réplication ?

Si la connexion tombe en panne, la réplication DFS essayera de répliquer alors que la planification est ouverte. Il y aura également des erreurs de connectivité indiqués dans le journal des événements la réplication DFS qui peuvent être récolté à l’aide de MOM (proactive à l’aide d’alertes) et le rapport de contrôle d’intégrité de réplication DFS (réactive, par exemple, quand un administrateur exécute il).

## <a name="remote-differential-compression-details"></a>Détails de la Compression différentielle à distance

### <a name="are-changes-compressed-before-being-replicated"></a>Modifications sont compressées avant d’en cours de réplication ?

Oui. Les parties modifiées des fichiers sont compressés avant d’être envoyés pour tous les fichiers de types à l’exception des suivants (qui sont déjà compressés) : .wma, .wmv, .zip, .jpg, .mpg, .mpeg, .m1v, .mp2, .mp3, .mpa, .cab, .wav, .snd, .au, .asf, .wm, .avi, .z, .gz, .tgz et .frx. Paramètres de compression pour ces types de fichiers ne sont pas configurables dans Windows Server 2003 R2.

### <a name="can-an-administrator-turn-off-rdc-or-change-the-threshold"></a>Un administrateur peut désactiver la compression différentielle à distance ou modifier le seuil ?

Oui. Vous pouvez désactiver la compression différentielle à distance via la page de propriétés d’une connexion donnée. La désactivation RDC peut réduire la latence d’utilisation et la réplication du processeur sur les liens de réseau local (LAN) rapide qui n’ont aucune contrainte de la bande passante ou des groupes de réplication qui se composent principalement de fichiers inférieurs à 64 Ko. Si vous choisissez de désactiver la compression différentielle à distance sur une connexion, tester l’efficacité de la réplication avant et après la modification pour vérifier que vous avez amélioré les performances de la réplication.

Vous pouvez modifier le seuil de taille de compression différentielle à distance à l’aide de la **Dfsradmin connexion définie** commande, le fournisseur WMI de réplication DFS, ou en modifiant manuellement le fichier XML de configuration de fichiers.

### <a name="does-rdc-work-on-all-file-types"></a>Compression différentielle à distance fonctionne sur tous les types de fichier ?

Oui. RDC calcule les différences au niveau du bloc, quelle que soit le type de données de fichier. Toutefois, la compression différentielle à distance fonctionne plus efficacement sur certains types de fichiers comme documents Word, des fichiers PST et des images de disque dur virtuel.

### <a name="how-does-rdc-work-on-a-compressed-file"></a>La compression différentielle à distance ne fonctionne pas sur un fichier compressé ?

La réplication DFS utilise la compression différentielle à distance, qui calcule les blocs dans le fichier qui ont été modifiés et envoie uniquement les blocs sur le réseau. La réplication DFS n’a pas besoin tout savoir sur le contenu du fichier, seuls les blocs ont été modifiés.

### <a name="is-cross-file-rdc-enabled-when-upgrading-to-windows-server-enterprise-edition-or-datacenter-edition"></a>Est compression RDC inter-fichiers activé lors de la mise à niveau vers Windows Server Enterprise Edition ou Datacenter Edition ?

Les éditions Standard de Windows Server ne gèrent pas compression RDC inter-fichiers. Toutefois, il est automatiquement activé lorsque vous mettez à niveau vers une édition qui prend en charge la compression RDC inter-fichiers, ou si un membre de la connexion de réplication s’exécute une édition prise en charge. Pour obtenir la liste des éditions prise en charge de compression RDC inter-fichiers, consultez les éditions de la prise en charge du système d’exploitation Windows compression RDC inter-fichiers ?

### <a name="is-rdc-true-block-level-replication"></a>Est la réplication au niveau du bloc true RDC ?

Non. RDC est un protocole d’usage général pour la compression de transfert de fichiers. La réplication DFS utilise la compression différentielle à distance sur les blocs au niveau du fichier, et non au niveau du bloc de disque. RDC divise un fichier en blocs. Pour chaque bloc dans un fichier, il calcule une signature qui est un petit nombre d’octets qui peut représenter le plus grand bloc. Le jeu de signatures est transféré à partir du serveur au client. Le client compare les signatures de serveur à son propre. Le client puis les demandes que le serveur envoie uniquement les données pour les signatures qui ne sont pas déjà sur le client.

### <a name="what-happens-if-i-rename-a-file"></a>Que se passe-t-il si je renomme un fichier ?

La réplication DFS renomme le fichier sur tous les autres membres du groupe de réplication lors de la prochaine réplication. Les fichiers sont suivies à l’aide d’un ID unique, par conséquent, vous renommez un fichier et le déplacement que du fichier dans le réplica n’a aucun effet sur la capacité de la réplication DFS pour répliquer un fichier.

### <a name="what-is-cross-file-rdc"></a>Ce qui est compression RDC inter-fichiers ?

Inter-fichiers RDC permet la réplication DFS utiliser la compression différentielle à distance même quand un fichier portant le même nom n’existe pas au niveau du client. Inter-fichiers RDC utilise une heuristique pour déterminer les fichiers qui sont similaires au fichier qui doit être répliquée et blocs utilise des fichiers similaires qui sont identiques dans le fichier de réplication afin de réduire la quantité de données transférées sur le réseau étendu. Inter-fichiers RDC peut utiliser des blocs de jusqu'à cinq fichiers similaires dans ce processus.

Pour utiliser compression RDC inter-fichiers, un membre de la connexion de réplication doit exécuter une édition de Windows prend en charge la compression RDC inter-fichiers. Pour obtenir la liste des éditions prise en charge de compression RDC inter-fichiers, consultez les éditions de la prise en charge du système d’exploitation Windows compression RDC inter-fichiers ?

### <a name="what-is-rdc"></a>Quelle est la compression différentielle à distance ?

Compression différentielle à distance (RDC) est un protocole client-serveur qui peut être utilisé pour mettre à jour efficacement les fichiers sur un réseau de bande passante limitée. RDC détecte les insertions, suppressions et réorganisations de données dans les fichiers, l’activation de la réplication DFS de répliquer uniquement les modifications lorsque les fichiers sont mis à jour. RDC est utilisé uniquement pour les fichiers qui sont de 64 Ko ou plus par défaut. Compression différentielle à distance peut utiliser une version antérieure d’un fichier portant le même nom dans le dossier répliqué ou dans le DfsrPrivate\\dossier ConflictandDeleted (situé sous le chemin d’accès local du dossier répliqué).

### <a name="when-is-rdc-used-for-replication"></a>Lorsque la compression différentielle à distance est utilisée pour la réplication ?

RDC est utilisé lorsque le fichier dépasse un seuil de taille minimale. Ce seuil de taille est de 64 Ko par défaut. Une fois un fichier dépasse ce seuil a été répliqué, des versions mises à jour du fichier toujours utilisent RDC, sauf si une grande partie du fichier est modifiée ou la compression différentielle à distance est désactivée.

### <a name="which-editions-of-the-windows-operating-system-support-cross-file-rdc"></a>Les éditions de la prise en charge du système d’exploitation Windows compression RDC inter-fichiers ?

Pour utiliser compression RDC inter-fichiers, un membre de la connexion de réplication doit exécuter une édition du système d’exploitation Windows prend en charge la compression RDC inter-fichiers. Le tableau suivant indique les éditions du système d’exploitation Windows prennent en charge compression RDC inter-fichiers.

### <a name="cross-file-rdc-availability-in-editions-of-the-windows-operating-system"></a>Inter-fichiers disponibilité RDC dans les éditions du système d’exploitation Windows

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th>Version du système d'exploitation</th>
<th>Standard Edition</th>
<th>Édition Entreprise</th>
<th>Datacenter Edition</th>
</tr>
</thead>
<tbody>
<tr class="even">
<td><p>Windows Server 2012 R2</p></td>
<td><p>Oui*</p></td>
<td><p>Non disponible</p></td>
<td><p>Oui*</p></td>
</tr>
<tr class="odd">
<td><p>Windows Server 2012</p></td>
<td><p>Oui</p></td>
<td><p>Non disponible</p></td>
<td><p>Oui</p></td>
</tr>
<tr class="even">
<td><p>Windows Server 2008 R2</p></td>
<td><p>Non</p></td>
<td><p>Oui</p></td>
<td><p>Oui</p></td>
</tr>
<tr class="odd">
<td><p>Windows Server 2008</p></td>
<td><p>Non</p></td>
<td><p>Oui</p></td>
<td><p>Non</p></td>
</tr>
<tr class="even">
<td><p>Windows Server 2003 R2</p></td>
<td><p>Non</p></td>
<td><p>Oui</p></td>
<td><p>Non</p></td>
</tr>
</tbody>
</table>

\* Vous pouvez éventuellement désactiver la compression RDC sur Windows Server 2012 R2 inter-fichiers.

## <a name="replication-details"></a>Détails de la réplication

### <a name="can-i-change-the-path-for-a-replicated-folder-after-it-is-created"></a>Puis-je modifier le chemin d’accès d’un dossier répliqué après sa création ?

Non. Si vous devez modifier le chemin d’accès d’un dossier répliqué, vous devez le supprimer de la gestion DFS et ajoutez-le comme un nouveau dossier répliqué. La réplication DFS utilise ensuite la Compression différentielle à distance (RDC) pour effectuer une synchronisation qui détermine si les données sont les mêmes sur l’envoi et la réception des membres. Elle ne réplique pas à nouveau toutes les données dans le dossier.

### <a name="can-i-configure-which-file-attributes-are-replicated"></a>Configurer les attributs de fichier à répliquer ?

Non, vous ne pouvez pas configurer les attributs de fichier, la réplication DFS réplique.

Pour obtenir la liste des valeurs d’attribut et leurs descriptions, consultez [les attributs de fichier](http://go.microsoft.com/fwlink/?linkid=182268) sur MSDN (http://go.microsoft.com/fwlink/?LinkId=182268).

Les valeurs d’attribut suivantes sont définies à l’aide de la `SetFileAttributes dwFileAttributes` (fonction) et ils sont répliqués par la réplication DFS. Modifications apportées aux valeurs de ces attributs déclenchent la réplication des attributs. Le contenu du fichier n’est pas répliqué, sauf si le contenu change également. Pour plus d’informations, consultez [SetFileAttributes fonction](http://go.microsoft.com/fwlink/?linkid=182269) dans MSDN library (http://go.microsoft.com/fwlink/?LinkId=182269).

  - FICHIER\_ATTRIBUT\_HIDDEN  
      
  - FICHIER\_ATTRIBUT\_EN LECTURE SEULE  
      
  - FICHIER\_ATTRIBUT\_SYSTÈME  
      
  - FICHIER\_ATTRIBUT\_PAS\_CONTENU\_INDEXÉ  
      
  - FICHIER\_ATTRIBUT\_HORS CONNEXION  
      

Les valeurs d’attribut suivantes sont répliquées par la réplication DFS, mais ils ne déclenchent pas de réplication.

  - FICHIER\_ATTRIBUT\_ARCHIVE  
      
  - FICHIER\_ATTRIBUT\_NORMAL  
      

Les valeurs d’attribut de fichier suivantes également déclenchent la réplication, même si elles ne peuvent pas être définies à l’aide de la `SetFileAttributes` (fonction) (utilisez la `GetFileAttributes` (fonction) pour afficher les valeurs d’attribut).

  - FICHIER\_ATTRIBUT\_D’ANALYSE\_POINT  
      

> [!NOTE]
> La réplication DFS ne réplique pas les valeurs d’attribut point d’analyse, sauf si la balise d’analyse est IO_REPARSE_TAG_SYMLINK. Fichiers avec les balises d’analyse IO_REPARSE_TAG_DEDUP, IO_REPARSE_TAG_SIS ou IO_REPARSE_TAG_HSM sont répliquées en tant que fichiers normaux. Toutefois, la balise d’analyse et les mémoires tampons de données d’analyse ne sont pas répliquées vers d’autres serveurs, car le point d’analyse fonctionne uniquement sur le système local. 
<br>

  - FICHIER\_ATTRIBUT\_COMPRESSÉS  
      
  - FICHIER\_ATTRIBUT\_ENCRYPTED  
      

> [!NOTE]
> La réplication DFS ne réplique pas les fichiers qui sont chiffrés à l’aide du système de fichiers EFS (Encrypting File System). La réplication DFS réplique les fichiers qui sont chiffrées à l’aide de logiciels non Microsoft, mais uniquement si elle ne définit pas la valeur d’attribut FILE_ATTRIBUTE_ENCRYPTED sur le fichier. 
<br>

  - FICHIER\_ATTRIBUT\_SPARSE\_FICHIER  
      
  - FICHIER\_ATTRIBUT\_DIRECTORY  
      

La réplication DFS ne réplique pas le fichier\_attribut\_valeur temporaire.

### <a name="can-i-control-which-member-is-replicated"></a>Puis-je contrôler le membre qui est répliqué ?

Oui. Vous pouvez choisir une topologie lorsque vous créez un groupe de réplication. Vous pouvez également sélectionner **aucune topologie** et configurer manuellement les connexions une fois que le groupe de réplication a été créé.

### <a name="can-i-seed-a-replication-group-member-with-data-prior-to-the-initial-replication"></a>Puis-je amorcer un membre du groupe de réplication avec des données avant la réplication initiale ?

Oui. La réplication DFS prend en charge la copie des fichiers vers un membre du groupe de réplication avant la réplication initiale. Cet « préparation du » peut considérablement réduire la quantité de données répliquées pendant la réplication initiale.

La réplication initiale n’a pas besoin répliquer le contenu lorsque les fichiers diffèrent uniquement par les attributs réels ou les horodatages. Un attribut réel est un attribut qui peut être défini par la fonction Win32 `SetFileAttributes`. Pour plus d’informations, consultez [SetFileAttributes fonction](http://go.microsoft.com/fwlink/?linkid=182269) dans MSDN library (http://go.microsoft.com/fwlink/?LinkId=182269). Si deux fichiers diffèrent par les autres attributs, telles que la compression, le contenu du fichier est répliqué.

Pour préparer un membre de groupe de réplication, copiez les fichiers dans le dossier approprié sur les serveurs de destination, créez le groupe de réplication, puis choisissez un membre principal. Choisissez le membre qui a des fichiers les plus récents que vous souhaitez répliquer, car le contenu du membre principal est considéré comme faisant « autorité ». Cela signifie que pendant la réplication initiale, les fichiers du membre principal remplace toujours autres versions des fichiers sur les autres membres du groupe de réplication.

Pour plus d’informations sur le préamorçage et le clonage de la base de données DFSR, voir [synchronisation initiale de la réplication DFS dans Windows Server 2012 R2 : L’attaque des Clones](http://blogs.technet.com/b/filecab/archive/2013/08/21/dfs-replication-initial-sync-in-windows-server-2012-r2-attack-of-the-clones.aspx).

Pour plus d’informations sur la réplication initiale, consultez [créer un groupe de réplication](https://technet.microsoft.com/en-us/library/cc725893).

### <a name="does-dfs-replication-overcome-common-file-replication-service-issues"></a>La réplication DFS surmonter les problèmes courants de Service de réplication de fichiers ?

Oui. La réplication DFS permet de résoudre trois problèmes FRS courants :

  - Encapsulages : La réplication DFS permet de récupérer à partir de l’enveloppe de journal à la volée. Chaque fichier ou dossier existant sera marqué comme journalWrap et vérifiée pour le système de fichiers avant la réplication est activée à nouveau. Lors de la récupération, ce volume n’est pas disponible pour la réplication dans les deux sens.  
      
  - Réplication excessive : Pour empêcher la réplication excessive, la réplication DFS utilise un système de crédits.  
      
  - Dossiers modifiés : Pour empêcher les noms de dossier transformée, la réplication DFS stocke les données conflictuelles dans un DfsrPrivate masqué\\dossier ConflictandDeleted (situé sous le chemin d’accès local du dossier répliqué). Par exemple, la création de plusieurs dossiers simultanément avec des noms identiques sur différents serveurs répliqués à l’aide de FRS, FRS renommer l’ou les dossiers plus anciens. La réplication DFS déplace à la place le plus anciennes ou les dossiers dans le dossier local en conflit et supprimés.  
      

### <a name="does-dfs-replication-replicate-files-in-chronological-order"></a>La réplication DFS réplique les fichiers dans l’ordre chronologique ?

Non. Les fichiers peuvent être répliquées en désordre.

### <a name="does-dfs-replication-replicate-files-that-are-being-used-by-another-application"></a>La réplication DFS réplique les fichiers qui sont utilisés par une autre application ?

Si une application ouvre un fichier et crée un verrou de fichier dessus (afin d’empêcher sa utilisé par d’autres applications lorsqu’il est ouvert), la réplication DFS ne répliquera pas le fichier jusqu'à ce qu’il est fermé. Si l’application ouvre le fichier avec accès en lecture-partage, le fichier peut toujours être répliqué.

### <a name="does-dfs-replication-replicate-ntfs-file-permissions-alternate-data-streams-hard-links-and-reparse-points"></a>La réplication DFS répliquer les autorisations de fichiers NTFS, les autres flux de données, les liens physiques et des points d’analyse ?

  - La réplication DFS réplique les autorisations de fichiers NTFS et les autres flux de données.  
      
  - Microsoft ne prend pas en charge la création de liens physiques NTFS vers ou à partir de fichiers dans un dossier répliqué – cela peut entraîner des problèmes de réplication avec les fichiers affectés. Fichiers de lien physique sont ignorés par la réplication DFS et ne sont pas répliquées. Les points de jonction également ne sont pas répliquées, et la réplication DFS enregistre l’événement 4406 pour chaque point de jonction qu’il rencontre.  
      
  - Les seuls points d’analyse répliqués par la réplication DFS sont celles qui utilisent les e/s\_d’analyse\_balise\_balise de lien symbolique ; Toutefois, la réplication DFS ne garantit pas que la cible d’un lien symbolique est également répliquée. Pour plus d’informations, consultez le [poser le blog de l’équipe de Services d’annuaire](http://blogs.technet.com/b/askds/archive/2011/09/30/friday-mail-sack-super-slo-mo-edition.aspx).  
      
  - Fichiers avec les e/s\_d’analyse\_balise\_la déduplication, les e/s\_d’analyse\_balise\_SIS ou e/s\_d’analyse\_balise\_HSM d’analyse balises sont répliquées. en tant que fichiers normaux. La balise d’analyse et les mémoires tampons de données d’analyse ne sont pas répliquées vers d’autres serveurs, car le point d’analyse fonctionne uniquement sur le système local. Par conséquent, la réplication DFS de répliquer des dossiers sur des volumes qui utilisent la déduplication des données dans Windows Server 2012, ou le stockage d’Instance unique (SIS), toutefois, les informations de la déduplication des données sont gérées séparément par chaque serveur sur lequel le service de rôle est activé.  
      

### <a name="does-dfs-replication-replicate-timestamp-changes-if-no-other-changes-are-made-to-the-file"></a>La réplication DFS réplique les modifications d’horodatage si aucune autre modification n’est apportées au fichier ?

Non, la réplication DFS ne réplique pas les fichiers dont la seule modification est une modification apportée à l’horodatage. En outre, l’horodateur modifié n’est pas répliquée aux autres membres du groupe de réplication, sauf si les autres modifications sont apportées au fichier.

### <a name="does-dfs-replication-replicate-updated-permissions-on-a-file-or-folder"></a>Est la réplication DFS réplique des autorisations mises à jour sur un fichier ou un dossier ?

Oui. La réplication DFS réplique les modifications de permission pour les fichiers et dossiers. Seule la partie du fichier associé avec la liste de contrôle d’accès (ACL) est répliquée, bien que la réplication DFS doit toujours lire l’intégralité du fichier dans la zone de transit.


> [!NOTE]
> Modifier des listes ACL sur un grand nombre de fichiers peut avoir un impact sur les performances de réplication. Toutefois, lorsque vous utilisez la compression différentielle à distance, la quantité de données transférées est proportionnelle à la taille de l’ACL, pas la taille du fichier entier. La quantité de trafic de disque est toujours proportionnelle à la taille des fichiers, car les fichiers doivent être lues vers et depuis le dossier intermédiaire. 
<br>


### <a name="does-dfs-replication-support-merging-text-files-in-the-event-of-a-conflict"></a>La réplication DFS prend en charge en cas de conflit, la fusion des fichiers de texte ?

La réplication DFS ne fusionne pas les fichiers lorsqu’il existe un conflit. Toutefois, il tente de conserver l’ancienne version du fichier dans le DfsrPrivate masqué\\dossier ConflictandDeleted sur l’ordinateur où le conflit a été détecté.

### <a name="does-dfs-replication-use-encryption-when-transmitting-data"></a>La réplication DFS utilise le chiffrement lors de la transmission des données ?

Oui. La réplication DFS utilise des connexions de l’appel de procédure distante (RPC) avec le chiffrement.

### <a name="is-it-possible-to-disable-the-use-of-encrypted-rpc"></a>Est-il possible de désactiver l’utilisation d’un appel RPC chiffré ?

Non. Le service de réplication DFS utilise des appels de procédure distante (RPC) sur TCP pour répliquer les données. Pour sécuriser les transferts de données sur Internet, le service de réplication DFS est conçu pour toujours utiliser la constante de niveau d’authentification, `RPC_C_AUTHN_LEVEL_PKT_PRIVACY`. Cela garantit que la communication RPC sur Internet est toujours chiffrée. Par conséquent, il n’est pas possible de désactiver l’utilisation d’un appel RPC chiffré par le service de réplication DFS.

Pour plus d’informations, consultez les sites Web Microsoft suivants :

  - [Référence technique de RPC](http://go.microsoft.com/fwlink/?linkid=182278)  
      
  - [À propos de la Compression différentielle à distance](http://go.microsoft.com/fwlink/?linkid=182279)  
      
  - [Constantes au niveau de l’authentification](http://go.microsoft.com/fwlink/?linkid=182280)  
      

### <a name="how-are-simultaneous-replications-handled"></a>Comment sont gérées les réplications simultanées ?

Il existe un gestionnaire de mise à jour par le dossier répliqué. Mettre à jour de travail de gestionnaires indépendamment les uns des autres.

Par défaut, il s’agit d’un maximum de 16 (quatre dans Windows Server 2003 R2), de téléchargements simultanés sont partagés entre toutes les connexions et les groupes de réplication. Étant donné que les connexions et les mises à jour du groupe de réplication ne sont pas sérialisés, il n’existe aucun ordre spécifique dans lequel les mises à jour sont reçues. Si deux planifications sont ouverts, mises à jour sont généralement reçus et installés à partir de ces deux connexions en même temps.

### <a name="how-do-i-force-replication-or-polling"></a>Comment forcer la réplication ou l’interrogation ?

Vous pouvez forcer la réplication immédiatement à l’aide de la gestion DFS, comme décrit dans [modifier les planifications de réplication](https://technet.microsoft.com/en-us/library/Cc732278). Vous pouvez également forcer la réplication à l’aide de la `Sync-DfsReplicationGroup` applet de commande incluse dans le module PowerShell de DFSR introduit avec Windows Server 2012 R2, ou le **Dfsrdiag SyncNow** commande. Vous pouvez forcer l’interrogation à l’aide de la `Update-DfsrConfigurationFromAD` applet de commande, ou le **Dfsrdiag PollAD** commande.

### <a name="is-it-possible-to-configure-a-quiet-time-between-replications-for-files-that-change-frequently"></a>Est-il possible de configurer une durée entre réplications pour les fichiers qui changent fréquemment ?

Non. Si la planification est ouverte, la réplication DFS répliquer les modifications comme il Remarque qu’eux. Il n’existe aucun moyen de configurer une durée pour les fichiers.

### <a name="is-it-possible-to-configure-one-way-replication-with-dfs-replication"></a>Il est possible de configurer une réplication unidirectionnelle avec la réplication DFS ?

Oui. Si vous utilisez Windows Server 2012 ou Windows Server 2008 R2, vous pouvez créer un dossier répliqué en lecture seule qui réplique le contenu via une connexion à sens unique. Pour plus d’informations, consultez [rendre un dossier répliqué accessible en lecture seule sur un membre particulier](http://go.microsoft.com/fwlink/?linkid=156740) (http://go.microsoft.com/fwlink/?LinkId=156740).

Nous ne prenons pas en charge la création d’une connexion de réplication unidirectionnelle avec la réplication DFS dans Windows Server 2008 ou Windows Server 2003 R2. Cela pourrait provoquer de nombreux problèmes, notamment les erreurs de topologie de contrôle d’intégrité, mise en lots des problèmes et des problèmes avec la base de données de réplication DFS.

Si vous utilisez Windows Server 2008 ou Windows Server 2003 R2, vous pouvez simuler une connexion à sens unique en effectuant les actions suivantes :

  - Formation des administrateurs pour apporter des modifications uniquement sur l’ou les serveurs que vous souhaitez désigner en tant que serveurs principaux. Puis les modifications seront répliquées vers les serveurs de destination.  
      
  - Configurer les autorisations de partage sur les serveurs de destination, afin que les utilisateurs finaux n’ont pas d’autorisations en écriture. Si aucune modification n’est autorisées sur les serveurs de succursale, rien à répliquer, simulation d’une connexion à sens unique et en conservant l’utilisation du réseau étendu est faible.  
      

### <a name="is-there-a-way-to-force-a-complete-replication-of-all-files-including-unchanged-files"></a>Existe-t-il un moyen de forcer une réplication complète de tous les fichiers, y compris les fichiers inchangés ?

Non. Si la réplication DFS considère les fichiers identiques, il ne réplique les. Si les fichiers modifiés n’ont pas été répliquées, la réplication DFS vont être répliqués automatiquement en cas de configuré pour ce faire. Pour remplacer le calendrier configuré, utilisez la méthode WMI **ForceReplicate()** . Toutefois, il s’agit uniquement remplacer une planification, et il n’impose pas de réplication de fichiers identiques ou non.

### <a name="what-happens-if-the-primary-member-suffers-a-database-loss-during-initial-replication"></a>Que se passe-t-il si le membre principal subit une perte de la base de données pendant la réplication initiale ?

Pendant la réplication initiale, les fichiers du membre principal seront toujours prioritaire dans la résolution des conflits qui se produit si les membres de réception possédant des différentes versions de fichiers sur le membre principal. La désignation de membre principal est stockée dans les Services de domaine Active Directory, et la désignation est effacée après que le membre principal est prêt à répliquer, mais avant que tous les membres de la réplication de groupe de réplication.

Si la réplication initiale échoue ou que le service de réplication DFS redémarre pendant la réplication, le membre principal voit la désignation de membre principal dans la base de données locale de la réplication DFS et de tentatives de la réplication initiale. Si la base de données de réplication DFS du membre principal est perdu après l’effacement de la désignation principale dans les Services de domaine Active Directory, mais avant que tous les membres du groupe de réplication effectuer la réplication initiale, tous les membres du groupe de réplication ne parviennent pas à répliquer le dossier, car aucun serveur n’est désigné comme le membre principal. Si cela se produit, utilisez le **Dfsradmin appartenance /Set /isprimary:true** commande sur le serveur membre principal pour restaurer la désignation de membre principal manuellement.

Pour plus d’informations sur la réplication initiale, consultez [créer un groupe de réplication](https://technet.microsoft.com/en-us/library/cc725893).


> [!WARNING]
> La désignation de membre principal est utilisée uniquement pendant le processus de réplication initiale. Si vous utilisez le <STRONG>Dfsradmin</STRONG> commande pour spécifier un membre principal pour un dossier répliqué après la réplication est terminée, la réplication DFS ne désigne pas le serveur en tant que membre principal dans les Services de domaine Active Directory. Toutefois, si la base de données de la réplication DFS sur le serveur sont affectées par la suite irréversible altération ou perte de données, le serveur tente d’effectuer une réplication initiale que le membre principal, au lieu de récupérer ses données à partir d’un autre membre de la réplication groupe. Essentiellement, le serveur devient un serveur principal non autorisé, ce qui peut provoquer des conflits. Pour cette raison, spécifier le membre principal manuellement uniquement si vous êtes certain que la réplication initiale a échoué irrémédiablement. 
<br>


### <a name="what-happens-if-the-replication-schedule-closes-while-a-file-is-being-replicated"></a>Que se passe-t-il si la planification de réplication se ferme pendant un fichier est en cours de réplication ?

Si la compression différentielle à distance (RDC) est activée sur la connexion, de réplication d’un fichier de taille supérieure à 64 Ko qui a commencé la réplication immédiatement avant la fermeture de la planification entrante (ou la modification à **aucune bande passante**) continue lorsque le planifier s’ouvre (ou les modifications apportées à un élément autre que **aucune bande passante**). La réplication continue à partir de l’état qu'où il se trouvait lors de la réplication s’est arrêté.

Si la compression différentielle à distance est désactivée, la réplication DFS redémarre entièrement le transfert de fichiers. Cela peut retarder lorsque le fichier est disponible sur le membre de réception.

### <a name="what-happens-when-two-users-simultaneously-update-the-same-file-on-different-servers"></a>Que se passe-t-il lorsque deux utilisateurs mettre à jour simultanément le même fichier sur différents serveurs ?

Lorsque la réplication DFS détecte un conflit, il utilise la version du fichier qui a été enregistré en dernier. Il déplace l’autre fichier dans le DfsrPrivate\\dossier ConflictandDeleted (sous le chemin d’accès local du dossier répliqué sur l’ordinateur qui a résolu le conflit). Il y reste jusqu'à ce que le nettoyage du dossier en conflit et supprimés, ce qui se produit lorsque le dossier en conflit et supprimés dépasse la taille configurée ou la réplication DFS détecte une erreur d’espace disque à l’emploi. Le dossier en conflit et supprimés n’est pas répliqué, et cette méthode de résolution des conflits évite de répertoires modifiés qui était possible dans FRS.

Lorsqu’un conflit se produit, la réplication DFS enregistre un événement d’information dans le journal des événements la réplication DFS. Cet événement ne nécessite pas d’action de l’utilisateur pour les raisons suivantes :

  - Il n’est pas visible aux utilisateurs (il est uniquement visible pour les administrateurs de serveur).  
      
  - La réplication DFS traite le dossier en conflit et supprimés comme un cache. Lorsqu’un seuil de quota est atteint, il nettoie certains de ces fichiers. Il n’existe aucune garantie que les fichiers en conflit seront enregistrés.  
      
  - Le conflit peut se trouver sur un serveur différent de l’origine du conflit.  
      

## <a name="staging"></a>Création intermédiaire

### <a name="does-dfs-replication-continue-staging-files-when-replication-is-disabled-by-a-schedule-or-bandwidth-throttling-quota-or-when-a-connection-is-manually-disabled"></a>La réplication DFS poursuit le mise en lots de fichiers lors de la réplication est désactivée par une planification ou le quota de limitation de bande passante, ou lorsqu’une connexion est désactivée manuellement ?

Non. La réplication DFS ne poursuit pas les fichiers de scène en dehors des heures de réplication planifiée, si le quota de limitation de bande passante a été dépassé, ou lorsque les connexions sont désactivées.

### <a name="does-dfs-replication-prevent-other-applications-from-accessing-a-file-during-staging"></a>La réplication DFS n’empêche d’autres applications d’accéder à un fichier pendant le transit ?

Non. La réplication DFS ouvre les fichiers d’une façon qui ne bloque pas les utilisateurs ou des applications à partir de l’ouverture des fichiers dans le dossier de réplication. Cette méthode est appelée « verrouillage opportuniste ».

### <a name="is-it-possible-to-change-the-location-of-the-staging-folder-with-the-dfs-management-tool"></a>Il est possible de modifier l’emplacement du dossier intermédiaire avec l’outil de gestion DFS ?

Oui. L’emplacement du dossier intermédiaire est configuré sur le **avancé** onglet de la **propriétés** boîte de dialogue pour chaque membre d’un groupe de réplication.

### <a name="when-are-files-staged"></a>Lorsque les fichiers intermédiaires ?

Fichiers intermédiaires sur le membre d’envoi lorsque le membre de réception demande le fichier (à moins que le fichier est de 64 Ko ou plus petit) comme indiqué dans le tableau suivant. Si la Compression différentielle à distance (RDC) est désactivé sur la connexion, le fichier est en attente, sauf si elle est de 256 Ko ou plus petits. Fichiers intermédiaires également sur le membre de réception comme ils sont transférés s’ils sont moins de 64 Ko, bien que vous pouvez configurer ce paramètre entre 16 Ko et 1 Mo. Si la planification est fermée, les fichiers ne sont pas mises en lots.

### <a name="the-minimum-file-sizes-for-staging-files"></a>Les tailles de fichier minimale pour les fichiers intermédiaires

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th> </th>
<th>RDC est activée</th>
<th>Compression différentielle à distance désactivé</th>
</tr>
</thead>
<tbody>
<tr class="even">
<td><p>Membre d’envoi</p></td>
<td><p>64 KO</p></td>
<td><p>256 KO</p></td>
</tr>
<tr class="odd">
<td><p>Membre de réception</p></td>
<td><p>64 Ko par défaut</p></td>
<td><p>64 Ko par défaut</p></td>
</tr>
</tbody>
</table>

### <a name="what-happens-if-a-file-is-changed-after-it-is-staged-but-before-it-is-completely-transmitted-to-the-remote-site"></a>Que se passe-t-il si un fichier est modifié une fois qu’il est en attente, mais avant d’être complètement transmis sur le site distant ?

Si n’importe quelle partie du fichier est déjà en cours de transmission, la réplication DFS continue la transmission. Si le fichier est modifié avant le début de la réplication DFS transmettre le fichier, la version la plus récente du fichier est envoyée.

## <a name="change-history"></a>Historique des modifications


<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Date</th>
<th>Description</th>
<th>Reason</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>15 novembre 2018</p></td>
<td><p>Mise à jour pour Windows Server 2019.</p></td>
<td><p>Nouveau système d’exploitation.</p></td>
</tr>
<tr class="even">
<td><p>9 octobre 2013</p></td>
<td><p>Mise à jour le quelles sont les limites de prise en charge de la réplication DFS ? section avec les résultats des tests sur Windows Server 2012 R2.</p></td>
<td><p>Mises à jour pour la dernière version de Windows Server</p></td>
</tr>
<tr class="odd">
<td><p>30 janvier 2013</p></td>
<td><p>Ajouté la réplication DFS est continuer la mise en lots de fichiers lors de la réplication est désactivée par une planification ou le quota de limitation de bande passante, ou lorsqu’une connexion est désactivée manuellement ? entrée.</p></td>
<td><p>Questions des clients</p></td>
</tr>
<tr class="even">
<td><p>31 octobre 2012</p></td>
<td><p>Modifié le quelles sont les limites de prise en charge de la réplication DFS ? entrée à augmenter leur nombre testé des fichiers répliqués sur un volume.</p></td>
<td><p>Retour d'expérience du client</p></td>
</tr>
<tr class="odd">
<td><p>15 août 2012</p></td>
<td><p>Modifié est la réplication DFS réplique NTFS fichier autorisations, les autres flux de données, les liens physiques et des points d’analyse ? points d’entrée pour clarifier la façon dont la réplication DFS gère les liens physiques et d’analyse.</p></td>
<td><p>Commentaires à partir du Support technique</p></td>
</tr>
<tr class="even">
<td><p>13 juin 2012</p></td>
<td><p>Permet de modifier le travail est la réplication DFS sur ReFS ou les volumes FAT ? entrée à ajouter une discussion de références.</p></td>
<td><p>Retour d'expérience du client</p></td>
</tr>
<tr class="odd">
<td><p>25 avril 2012</p></td>
<td><p>Modifié est la réplication DFS réplique NTFS fichier autorisations, les autres flux de données, les liens physiques et des points d’analyse ? entrée à clarifier la façon dont la réplication DFS gère les liens physiques.</p></td>
<td><p>Réduire le risque de confusion</p></td>
</tr>
<tr class="even">
<td><p>30 mars 2011</p></td>
<td><p>Modifié le peut la réplication DFS réplique Outlook .pst ou les fichiers de base de données Microsoft Office Access ? entrée pour corriger l’impact potentiel de l’utilisation de la réplication DFS avec les fichiers .pst et accès.</p>
<p>Ajouté comment améliorer les performances de la réplication ?</p></td>
<td><p>Questions des clients sur l’entrée précédente, ce qui incorrectement indiqué que la réplication .pst ou accéder aux fichiers peut endommager la base de données de réplication DFS.</p></td>
</tr>
<tr class="odd">
<td><p>26 janvier 2011</p></td>
<td><p>Ajouté comment peuvent récupérer les fichiers sur le ConflictAndDeleted ou préexistant dossiers ?</p></td>
<td><p>Retour d'expérience du client</p></td>
</tr>
<tr class="even">
<td><p>20 octobre 2010</p></td>
<td><p>Ajouté comment puis-je mettre à niveau ou remplacer un membre de la réplication DFS ?</p></td>
<td><p>Retour d'expérience du client</p></td>
</tr>
</tbody>
</table>

