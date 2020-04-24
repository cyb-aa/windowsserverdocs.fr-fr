---
title: 'Réplication DFS : Questions fréquentes (FAQ)'
ms.date: 06/18/2014
ms.prod: windows-server
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 1e11f6c596d7e5eb0bdf379adcf47d21e74e9f6b
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2020
ms.locfileid: "80815622"
---
# <a name="dfs-replication-frequently-asked-questions-faq"></a>Réplication DFS : Forum Aux Questions (FAQ)


Mise à jour : 30 avril 2019

S'applique à : Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Ce Forum aux questions (FAQ) répond aux questions relatives à la réplication du système de fichiers distribués (DFS) (également appelée DFS-R ou DFSR) pour Windows Server.

Pour plus d'informations sur les espaces de noms DFS, consultez [Espaces de noms DFS : Questions fréquentes](https://technet.microsoft.com/library/ee404780).

Pour plus d’informations sur les nouveautés de la réplication DFS, consultez les rubriques suivantes :

  - [Vue d’ensemble des espaces de noms DFS et de la réplication DFS](https://technet.microsoft.com/library/jj127250) (dans Windows Server 2012)  
      
  - Rubrique [Nouveautés du système de fichiers distribués (DFS)](https://technet.microsoft.com/library/ee307957) dans [Changements apportés aux fonctionnalités entre Windows Server 2008 et Windows Server 2008 R2](https://technet.microsoft.com/library/dd391932)  
      
  - Rubrique [Système de fichiers distribués (DFS)](https://technet.microsoft.com/library/cc753479) dans [Changements apportés aux fonctionnalités entre Windows Server 2003 avec SP1 et Windows Server 2008](https://technet.microsoft.com/library/cc753208)  
      

Pour obtenir la liste des modifications récentes apportées à cette rubrique, consultez la section [Historique des modifications](#change-history) de cette rubrique.

      

## <a name="interoperability"></a>Interopérabilité

### <a name="can-dfs-replication-communicate-with-frs"></a>La réplication DFS peut-elle communiquer avec le service FRS ?

Non. La réplication DFS ne communique pas avec le service de réplication de fichiers (FRS). La réplication DFS et le service FRS peuvent être exécutés simultanément sur le même serveur, mais ils ne doivent jamais être configurés pour répliquer les mêmes dossiers ou sous-dossiers, car cela peut entraîner une perte de données.

### <a name="can-dfs-replication-replace-frs-for-sysvol-replication"></a>La réplication DFS peut-elle remplacer le service FRS pour la réplication SYSVOL ?

Oui, la réplication DFS peut remplacer le service FRS pour la réplication SYSVOL sur les serveurs exécutant Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008. Les serveurs exécutant Windows Server 2003 R2 ne prennent pas en charge l’utilisation de la réplication DFS pour répliquer le dossier SYSVOL.

Pour plus d’informations sur la réplication de SYSVOL à l’aide de la réplication DFS, consultez [Guide de migration de la réplication SYSVOL : Réplication FRS vers la réplication DFS](https://technet.microsoft.com/library/dd640019).

### <a name="can-i-upgrade-from-frs-to-dfs-replication-without-losing-configuration-settings"></a>Puis-je effectuer une mise à niveau de la réplication FRS vers la réplication DFS sans perdre de paramètres de configuration ?

Oui. Pour migrer la réplication de FRS vers la réplication DFS, consultez les documents suivants :

  - Pour migrer la réplication de dossiers autres que le dossier SYSVOL, consultez [Guide des opérations DFS : Migration de FRS vers la réplication DFS](https://go.microsoft.com/fwlink/?linkid=192776) et [FRS2DFSR : un utilitaire de migration de FRS vers DFSR](https://go.microsoft.com/fwlink/?linkid=195437) (https://go.microsoft.com/fwlink/?LinkID=195437).  
      
  - Pour migrer la réplication du dossier SYSVOL vers la réplication DFS, consultez [Guide de migration de la réplication SYSVOL : Réplication FRS vers la réplication DFS](https://technet.microsoft.com/library/dd640019).  
      

### <a name="can-i-use-dfs-replication-in-a-mixed-windowsunix-environment"></a>Puis-je utiliser la réplication DFS dans un environnement mixte Windows/UNIX ?

Oui. Même si la réplication DFS prend uniquement en charge la réplication de contenu entre des serveurs exécutant Windows Server, les clients UNIX peuvent accéder aux partages de fichiers situés sur les serveurs Windows. Pour ce faire, installez les Services pour NFS (Network File System) sur le serveur de réplication DFS.

Vous pouvez également utiliser la fonctionnalité de client SMB/CIFS incluse dans de nombreux clients UNIX pour accéder directement aux partages de fichiers Windows, même si cette fonctionnalité est souvent limitée ou nécessite d’apporter des modifications à l’environnement Windows (comme la désactivation de la signature SMB à l’aide d’une stratégie de groupe).

La réplication DFS interagit avec NFS sur un serveur exécutant un système d’exploitation Windows Server, mais vous ne pouvez pas répliquer un point de montage NFS.

### <a name="can-i-use-the-volume-shadow-copy-service-with-dfs-replication"></a>Puis-je utiliser le service de cliché instantané des volumes (VSS) avec la réplication DFS ?

Oui. La réplication DFS est prise en charge sur les volumes VSS et les instantanés précédents peuvent être restaurés avec le client des versions précédentes.

### <a name="can-i-use-windowsbackup-ntbackupexe-to-remotely-back-up-a-replicated-folder"></a>Puis-je utiliser la Sauvegarde Windows (NTBackup.exe) pour sauvegarder à distance un dossier répliqué ?

Non, l’utilisation de la Sauvegarde Windows (NTBackup.exe) sur un ordinateur exécutant Windows Server 2003 ou une version antérieure pour sauvegarder le contenu d’un dossier répliqué sur un ordinateur exécutant Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008 n’est pas prise en charge.

Pour sauvegarder des fichiers stockés dans un dossier répliqué, utilisez la Sauvegarde Windows Server ou Microsoft&reg; System Center Data Protection Manager. Pour plus d’informations sur la fonctionnalité de sauvegarde et de récupération de Windows Server 2008 R2 et de Windows Server 2008, consultez [Sauvegarde et récupération](https://technet.microsoft.com/library/Cc754097). Pour plus d’informations, consultez [System Center Data Protection Manager](https://go.microsoft.com/fwlink/?linkid=182261) (https://go.microsoft.com/fwlink/?LinkId=182261).

### <a name="do-file-system-policies-impact-dfs-replication"></a>Les stratégies de système de fichiers affectent-elles la réplication DFS ?

Oui. Ne configurez pas des stratégies de système de fichiers sur des dossiers répliqués. La stratégie de système de fichiers réapplique les autorisations NTFS à chaque intervalle d’actualisation de la stratégie de groupe. Cela peut entraîner des violations de partage, car un fichier ouvert n’est pas répliqué tant qu’il n’est pas fermé.

### <a name="does-dfs-replication-replicate-mailboxes-hosted-on-microsoft-exchange-server"></a>La réplication DFS réplique-t-elle les boîtes aux lettres hébergées sur Microsoft Exchange Server ?

Non. La réplication DFS ne peut pas être utilisée pour répliquer les boîtes aux lettres hébergées sur Microsoft Exchange Server.

### <a name="does-dfs-replication-support-file-screens-created-by-file-server-resource-manager"></a>La réplication DFS prend-elle en charge les filtres de fichiers créés par le Gestionnaire des ressources du serveur de fichiers ?

Oui. Toutefois, les paramètres de filtrage de fichiers du Gestionnaire de ressources du serveur de fichiers (FSRM) doivent correspondre aux deux extrémités de la réplication. De plus, la réplication DFS dispose de son propre mécanisme de filtre pour les fichiers et les dossiers que vous pouvez utiliser pour exclure certains fichiers et types de fichiers de la réplication.

Voici les bonnes pratiques permettant d’implémenter des filtres de fichiers ou des quotas :

  - Le dossier masqué DfsrPrivate ne doit pas être soumis à des quotas ou à des filtres de fichiers.  
      
  - Il ne doit pas exister des fichiers filtrés dans un dossier répliqué avant l’activation du filtrage.  
      
  - Aucun dossier ne peut dépasser le quota avant l’activation du quota.  
      
  - Il convient d’être prudent lors de l’utilisation de quotas inconditionnels. Il est possible que des membres d’un groupe de réplication restent dans un quota avant la réplication, mais le dépassent quand des fichiers sont répliqués. Par exemple, si un utilisateur copie un fichier de 10 mégaoctets (Mo) sur le serveur A (qui est alors à la limite inconditionnelle) et qu’un autre utilisateur copie un fichier de 5 Mo sur le serveur B, quand la réplication suivante se produit, les deux serveurs dépassent le quota de 5 mégaoctets. Cela peut entraîner une nouvelle tentative sans fin de réplication des fichiers par la réplication DFS, provoquant des trous dans le vecteur de version et des problèmes de performances possibles.  
      

### <a name="is-dfs-replication-cluster-aware"></a>La réplication DFS est-elle adaptée aux clusters ?

Oui, dans Windows Server 2012 R2, Windows Server 2012 et Windows Server 2008 R2, la réplication DFS permet d’ajouter un cluster de basculement comme membre d’un groupe de réplication. Pour plus d’informations, consultez [Ajouter un cluster de basculement à un groupe de réplication](https://go.microsoft.com/fwlink/?linkid=155085) (https://go.microsoft.com/fwlink/?LinkId=155085). Dans les versions de Windows antérieures à Windows Server 2008 R2, le service de réplication DFS n’est pas conçu pour se coordonner avec un cluster de basculement, et le service n’est pas basculé vers un autre nœud.


> [!NOTE]
> La réplication DFS ne prend pas en charge la réplication de fichiers sur des volumes partagés de cluster. 
<br>


### <a name="is-dfs-replication-compatible-with-data-deduplication"></a>La réplication DFS est-elle compatible avec la déduplication des données ?

Oui, la réplication DFS peut répliquer des dossiers sur des volumes qui utilisent la déduplication des données dans Windows Server.

### <a name="is-dfs-replication-compatible-with-ris-and-wds"></a>La réplication DFS est-elle compatible avec RIS et WDS ?

Oui. La réplication DFS réplique les volumes sur lesquels le stockage d’instance simple (SIS, Single Instance Storage) est activé. SIS est utilisé par les services d’installation à distance (RIS, Remote Installation Services), les services de déploiement Windows (WDS, Windows Deployment Services) et Windows Storage Server.

### <a name="is-it-possible-to-use-dfs-replication-with-offline-files"></a>Est-il possible d’utiliser la réplication DFS avec la fonctionnalité Fichiers hors connexion ?

Vous pouvez utiliser la réplication DFS et la fonctionnalité Fichiers hors connexion de manière sécurisée dans des scénarios où il n’y a qu’un seul utilisateur à la fois qui écrit dans les fichiers. Cela est utile pour les utilisateurs qui se déplacent entre deux filiales et qui veulent pouvoir accéder à leurs fichiers dans l’une ou l’autre agence ou en mode hors connexion. La fonctionnalité Fichiers hors connexion met en cache les fichiers localement pour une utilisation hors connexion et la réplication DFS réplique les données entre les filiales.

N’utilisez pas la réplication DFS avec les Fichiers hors connexion dans un environnement multi-utilisateur, car la réplication DFS ne fournit pas de mécanisme de verrouillage distribué ou de fonction d’extraction de fichier. Si deux utilisateurs modifient en même temps le même fichier sur des serveurs différents, la réplication DFS déplace l’ancien fichier vers le dossier DfsrPrivate\\ConflictandDeleted (situé sous le chemin local du dossier répliqué) lors de la prochaine réplication.

### <a name="what-antivirus-applications-are-compatible-with-dfs-replication"></a>Quelles sont les applications antivirus compatibles avec la réplication DFS ?

Les applications antivirus peuvent entraîner une réplication excessive si leurs activités d’analyse modifient les fichiers d’un dossier répliqué. Pour plus d’informations, consultez [Test de l’interopérabilité de l’application anti-virus avec la réplication DFS](https://go.microsoft.com/fwlink/?linkid=73990) (https://go.microsoft.com/fwlink/?LinkId=73990).

### <a name="what-are-the-benefits-of-using-dfs-replication-instead-of-windows-sharepoint-services"></a>Quels sont les avantages de l’utilisation de la réplication DFS plutôt que Windows SharePoint Services ?

Contrairement à la réplication DFS, Windows&reg; SharePoint&reg; Services offre une stricte cohérence sous la forme d’une fonctionnalité d’extraction de fichier. Si vous êtes concerné par la modification du même fichier par plusieurs personnes, nous vous recommandons d’utiliser Windows SharePoint Services. Windows SharePoint Services 2.0 avec Service Pack 2 est disponible en tant que composant de Windows Server 2003 R2. Windows SharePoint Services peut être téléchargé à partir du site web Microsoft. Il n’est pas inclus dans les versions plus récentes de Windows Server. Toutefois, si vous répliquez des données sur plusieurs sites et que les utilisateurs ne modifieront pas les mêmes fichiers en même temps, la réplication DFS offre une bande passante plus importante et une gestion plus simple.

## <a name="limitations-and-requirements"></a>Limitations et exigences

### <a name="can-dfs-replication-replicate-between-branch-offices-without-a-vpn-connection"></a>La réplication DFS peut-elle être effectuée entre des succursales sans connexion VPN ?

Oui, en supposant qu’il existe une liaison de réseau étendu (WAN) privée (et pas Internet) connectant les succursales. Toutefois, vous devez ouvrir les ports appropriés dans les pare-feu externes. La réplication DFS utilise le mappeur de point de terminaison RPC (port 135) et un port éphémère affecté de façon aléatoire supérieur à 1024. Vous pouvez utiliser l’outil en ligne de commande **Dfsrdiag** pour préciser un port statique au lieu du port éphémère. Pour plus d’informations sur la façon de spécifier le mappeur de point de terminaison RPC, consultez l’[article 154596](https://go.microsoft.com/fwlink/?linkid=73991) dans la Base de connaissances Microsoft (https://go.microsoft.com/fwlink/?LinkId=73991).

### <a name="can-dfs-replication-replicate-files-encrypted-with-the-encrypting-file-system"></a>La réplication DFS peut-elle répliquer des fichiers chiffrés avec le système de fichiers EFS ?

Non. La réplication DFS ne réplique pas les fichiers ou dossiers qui sont chiffrés à l’aide du système de fichiers EFS. Si un utilisateur chiffre un fichier qui a été précédemment répliqué, la réplication DFS supprime le fichier de tous les autres membres du groupe de réplication. Cela garantit que la seule copie disponible du fichier est la version chiffrée sur le serveur.

### <a name="can-dfs-replication-replicate-outlook-pst-or-microsoft-office-access-database-files"></a>La réplication DFS peut-elle répliquer des fichiers de base de données Microsoft Office Access ou Outlook.pst ?

La réplication DFS peut répliquer de manière sécurisée des fichiers de dossiers personnels Microsoft Outlook (.pst) et des fichiers Microsoft Access uniquement s’ils sont stockés à des fins d’archivage et qu’ils ne font pas l’objet d’un accès sur le réseau à l’aide d’un client tel qu’Outlook ou Access. (Pour ouvrir des fichiers .pst ou Access, copiez d’abord les fichiers sur un dispositif de stockage local.) Les raisons pour cela sont les suivantes :

  - L’ouverture de fichiers .pst sur des connexions réseau peut entraîner une altération des données qu’ils contiennent. Pour plus d’informations sur les raisons pour lesquelles les fichiers .pst ne sont pas accessibles de manière sécurisée à partir d’un réseau, consultez l’[article 297019](https://go.microsoft.com/fwlink/?linkid=125363) de la Base de connaissances Microsoft (https://go.microsoft.com/fwlink/?LinkId=125363).  
      
  - Les fichiers .pst et Access ont tendance à rester ouverts pendant de longues durées tout en étant accessibles par un client comme Outlook ou Office Access. Cela empêche la réplication DFS de répliquer ces fichiers tant qu’ils ne sont pas fermés.  
      

### <a name="can-i-use-dfs-replication-in-a-workgroup"></a>Puis-je utiliser la réplication DFS dans un groupe de travail ?

Non. La réplication DFS s’appuie sur Active Directory&reg; Domain Services pour la configuration. Elle ne fonctionne que dans un domaine.

### <a name="can-more-than-one-folder-be-replicated-on-a-single-server"></a>Plusieurs dossiers peuvent-ils être répliqués sur un seul serveur ?

Oui. La réplication DFS peut répliquer de nombreux dossiers entre des serveurs. Vérifiez que chacun des dossiers répliqués a un chemin racine unique et qu’ils ne se chevauchent pas. Par exemple, D:\\Sales et D:\\Accounting peuvent être les chemins racines de deux dossiers répliqués, mais pas D:\\Sales et D:\\Sales\\Reports.

### <a name="does-dfs-replication-require-dfs-namespaces"></a>La réplication DFS nécessite-t-elle des espaces de noms DFS ?

Non. La réplication DFS et les espaces de noms DFS peuvent être utilisés séparément ou ensemble. De plus, la réplication DFS peut être utilisée pour répliquer des espaces de noms DFS autonomes, ce qui n’était pas possible avec le service FRS.

### <a name="does-dfs-replication-require-time-synchronization-between-servers"></a>La réplication DFS nécessite-t-elle une synchronisation de l’heure entre les serveurs ?

Non. La réplication DFS n’exige pas explicitement la synchronisation de l’heure entre les serveurs. Toutefois, les horloges des serveurs doivent correspondre étroitement. Pour que l’authentification Kerberos fonctionne correctement, les horloges des serveurs doivent être définies avec une différence maximale de cinq minutes entre elles (par défaut). Par exemple, la réplication DFS utilise des horodatages pour déterminer quel fichier est prioritaire en cas de conflit. Les heures précises sont également importantes pour le garbage collection, les planifications et d’autres fonctionnalités.

### <a name="does-dfs-replication-support-replicating-an-entire-volume"></a>La réplication DFS prend-elle en charge la réplication d’un volume entier ?

Oui. Toutefois, vous devez d’abord installer Windows Server 2003 Service Pack 2 ou le correctif logiciel. Pour plus d’informations, consultez l’[article 920335](https://go.microsoft.com/fwlink/?linkid=76776) dans la Base de connaissances Microsoft (https://go.microsoft.com/fwlink/?LinkId=76776). De plus, la réplication d’un volume entier peut provoquer les problèmes suivants :

  - Si le volume contient un fichier de pagination Windows, la réplication échoue et journalise l’événement DFSR 4312 dans le journal des événements système.  
      
  - La réplication DFS définit les attributs Système et Masqué sur le dossier répliqué situé sur le ou les serveurs de destination. Cela est dû au fait que Windows applique par défaut les attributs Système et Masqué au dossier racine du volume. Si le chemin local du dossier répliqué sur le ou les serveurs de destination est également une racine de volume, aucun autre changement n’est apporté aux attributs du dossier.  
      
  - Lors de la réplication d’un volume qui contient le dossier système Windows, la réplication DFS reconnaît le dossier %WINDIR% et ne le réplique pas. Toutefois, la réplication DFS réplique les dossiers utilisés par les applications non-Microsoft, ce qui peut entraîner l’échec des applications sur le ou les serveurs de destination si les applications présentent des problèmes d’interopérabilité avec la réplication DFS.  
      

### <a name="does-dfs-replication-support-rpc-over-http"></a>La réplication DFS prend-elle en charge RPC sur HTTP ?

Non.

### <a name="does-dfs-replication-work-across-wireless-networks"></a>La réplication DFS fonctionne-t-elle sur les réseaux sans fil ?

Oui. La réplication DFS est indépendante du type de connexion.

### <a name="does-dfs-replication-work-on-refs-or-fat-volumes"></a>La réplication DFS fonctionne-t-elle sur les volumes ReFS ou FAT ?

Non. La réplication DFS prend en charge les volumes formatés uniquement avec le système de fichiers NTFS ; le système ReFS (Resilient File System) et le système de fichiers FAT ne sont pas pris en charge. La réplication DFS nécessite NTFS car elle utilise le journal des modifications NTFS et d’autres fonctionnalités du système de fichiers NTFS.

### <a name="does-dfs-replication-work-with-sparse-files"></a>La réplication DFS fonctionne-t-elle avec des fichiers partiellement alloués ?

Oui. Vous pouvez répliquer des fichiers partiellement alloués. L’attribut **Sparse** (Partiellement alloué) est conservé sur le membre de réception.

### <a name="do-i-need-to-log-in-as-administrator-to-replicate-files"></a>Dois-je me connecter en tant qu’administrateur pour répliquer des fichiers ?

Non. La réplication DFS est un service qui s’exécute sous le compte système local. Vous n’avez donc pas besoin de vous connecter en tant qu’administrateur pour effectuer une réplication. Toutefois, vous devez être administrateur de domaine ou administrateur local des serveurs de fichiers concernés pour apporter des changements à la configuration de la réplication DFS.

Pour plus d’informations, consultez « Exigences et délégation de sécurité de la réplication DFS » dans [Déléguer la gestion de la réplication DFS](https://go.microsoft.com/fwlink/?linkid=182294) (https://go.microsoft.com/fwlink/?LinkId=182294).

### <a name="how-can-i-upgrade-or-replace-a-dfs-replication-member"></a>Comment puis-je mettre à niveau ou remplacer un membre de la réplication DFS ?

Pour mettre à niveau ou remplacer un membre de la réplication DFS, consultez le billet de blog suivant sur le blog Ask the Directory Services Team : [Replacing DFSR Member Hardware or OS](https://blogs.technet.com/b/askds/archive/2010/09/10/series-wrap-up-and-downloads-replacing-dfsr-member-hardware-or-os.aspx).

### <a name="is-dfs-replication-suitable-for-replicating-roaming-profiles"></a>La réplication DFS est-elle appropriée pour la réplication de profils itinérants ?

Oui. Certains scénarios sont pris en charge lors de la réplication de profils utilisateur itinérants. Pour plus d’informations sur les scénarios pris en charge, consultez [Déclaration de prise en charge de Microsoft concernant les données de profil utilisateur répliquées](https://go.microsoft.com/fwlink/?linkid=201282) (https://go.microsoft.com/fwlink/?LinkId=201282).

### <a name="is-there-a-file-character-limit-or-limit-to-the-folder-depth"></a>Existe-t-il une limite de caractères de fichier ou une limite concernant le niveau de dossiers ?

Windows et la réplication DFS prennent en charge les chemins de dossiers contenant jusqu’à 32 000 caractères. La réplication DFS n’est pas limitée aux chemins de dossiers de 260 caractères.

### <a name="must-members-of-a-replication-group-reside-in-the-same-domain"></a>Les membres d’un groupe de réplication doivent-ils se trouver dans le même domaine ?

Non. Les groupes de réplication peuvent s’étendre sur plusieurs domaines d’une même forêt, mais pas dans différentes forêts.

### <a name="what-are-the-supported-limits-of-dfs-replication"></a>Quelles sont les limites de la réplication DFS prises en charge ?

La liste suivante fournit un ensemble de recommandations concernant la scalabilité qui ont été testées par Microsoft et qui s’appliquent à Windows Server 2012 R2, Windows Server 2016 et Windows Server 2019

  - Taille de tous les fichiers répliqués sur un serveur : 100 téraoctets.  
      
  - Nombre de fichiers répliqués sur un volume : 70 millions.  
      
  - Taille de fichier maximale : 250 gigaoctets.  
      


> [!IMPORTANT]
> Lors de la création de groupes de réplication avec un grand nombre de fichiers ou des fichiers volumineux, nous vous recommandons d’exporter un clone de base de données et d’utiliser des techniques de prédéfinition pour réduire la durée de la réplication initiale. Pour plus d’informations, consultez [Synchronisation initiale de la réplication DFS dans Windows Server 2012 R2 : Attaque des clones](https://techcommunity.microsoft.com/t5/Storage-at-Microsoft/DFS-Replication-Initial-Sync-in-Windows-Server-2012-R2-Attack-of/ba-p/424877). 
<br>


La liste suivante fournit un ensemble de recommandations concernant la scalabilité qui ont été testées par Microsoft sur Windows Server 2012, Windows Server 2008 R2 et Windows Server 2008 :

  - Taille de tous les fichiers répliqués sur un serveur : 10 téraoctets.  
      
  - Nombre de fichiers répliqués sur un volume : 11 millions.  
      
  - Taille de fichier maximale : 64 gigaoctets.  
      


> [!NOTE]
> Il n’y a plus de limite au nombre de groupes de réplication, de dossiers répliqués, de connexions ou de membres de groupe de réplication. 
<br>


Pour obtenir la liste des recommandations concernant la scalabilité qui ont été testées par Microsoft pour Windows Server 2003 R2, consultez [Recommandations concernant la scalabilité de la réplication DFS](https://go.microsoft.com/fwlink/?linkid=75043) (https://go.microsoft.com/fwlink/?LinkId=75043).

### <a name="when-should-i-not-use-dfs-replication"></a>Quand ne dois-je pas utiliser la réplication DFS ?

N’utilisez pas la réplication DFS dans un environnement où plusieurs utilisateurs mettent à jour ou modifient simultanément les mêmes fichiers sur des serveurs différents. Cela peut entraîner le déplacement, par la réplication DFS, des copies en conflit des fichiers vers le dossier caché DfsrPrivate\\ConflictandDeleted.

Quand plusieurs utilisateurs doivent modifier simultanément les mêmes fichiers sur des serveurs différents, utilisez la fonctionnalité d’extraction de fichier de Windows SharePoint Services pour garantir qu’un seul utilisateur travaille sur un fichier. Windows SharePoint Services 2.0 avec Service Pack 2 est disponible en tant que composant de Windows Server 2003 R2. Windows SharePoint Services peut être téléchargé à partir du site web Microsoft. Il n’est pas inclus dans les versions plus récentes de Windows Server.

### <a name="why-is-a-schema-update-required-for-dfs-replication"></a>Pourquoi une mise à jour de schéma est-elle nécessaire pour la réplication DFS ?

La réplication DFS utilise de nouveaux objets dans le contexte de nommage de domaine d’Active Directory Domain Services pour stocker les informations de configuration. Ces objets sont créés quand vous mettez à jour le schéma Active Directory Domain Services. Pour plus d’informations, consultez [Passer en revue les exigences relatives à la réplication DFS](https://go.microsoft.com/fwlink/?linkid=182264) (https://go.microsoft.com/fwlink/?LinkId=182264).

## <a name="monitoring-and-management-tools"></a>Outils de supervision et de gestion

### <a name="can-i-automate-the-health-report-to-receive-warnings"></a>Puis-je automatiser le rapport d’intégrité pour recevoir des avertissements ?

Oui. Il existe trois façons d’automatiser les rapports d’intégrité :

  - Utilisez le module Windows PowerShell DFSR inclus dans Windows Server 2012 R2 ou DfsrAdmin.exe en conjonction avec l’option Tâches planifiées pour générer régulièrement des rapports d’intégrité. Pour plus d’informations, consultez [Automatisation des rapports d’intégrité de la réplication DFS](https://go.microsoft.com/fwlink/?linkid=74010) (https://go.microsoft.com/fwlink/?LinkId=74010).  
      
  - Utilisez le pack d’administration de la réplication DFS pour System Center Operations Manager afin de créer des alertes basées sur des conditions spécifiées.  
      
  - Utilisez le fournisseur WMI de la réplication DFS pour générer des scripts d’alerte.  
      

### <a name="can-i-use-microsoft-system-center-operations-manager-to-monitor-dfs-replication"></a>Puis-je utiliser Microsoft System Center Operations Manager pour superviser la réplication DFS ?

Oui. Pour plus d’informations, consultez le [pack d’administration de la réplication DFS pour System Center Operations Manager 2007](https://go.microsoft.com/fwlink/?linkid=182265) dans le Centre de téléchargement Microsoft (https://go.microsoft.com/fwlink/?LinkId=182265).

### <a name="does-dfs-replication-support-remote-management"></a>La réplication DFS prend-elle en charge la gestion à distance ?

Oui. La réplication DFS prend en charge la gestion à distance à l’aide de la console Gestion du système de fichiers distribués DFS et la commande **Ajouter un groupe de réplication**. Par exemple, sur le serveur A, vous pouvez vous connecter à un groupe de réplication défini dans la forêt avec les serveurs A et B comme membres.

La Gestion DFS est incluse avec Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008 et Windows Server 2003 R2. Pour gérer la réplication DFS à partir d’autres versions de Windows, utilisez le Bureau à distance ou les [Outils d’administration de serveur distant pour Windows 7](https://technet.microsoft.com/library/Ee449475).


> [!IMPORTANT]
> Pour consulter ou gérer des groupes de réplication qui contiennent des dossiers répliqués en lecture seule ou des membres qui sont des clusters de basculement, vous devez utiliser la version de la Gestion DFS qui est incluse avec Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, les <a href="https://go.microsoft.com/fwlink/p/?linkid=238560">Outils d’administration de serveur distant pour Windows 8</a> ou les <a href="https://technet.microsoft.com/library/ee449475">Outils d’administration de serveur distant pour Windows 7</a>. 
<br>


### <a name="do-ultrasound-and-sonar-work-with-dfs-replication"></a>Ultrasound et Sonar fonctionnent-ils avec la réplication DFS ?

Non. La réplication DFS dispose de son propre ensemble d’outils de supervision et de diagnostic. Ultrasound et Sonar sont uniquement capables de superviser le service FRS.

### <a name="how-can-files-be-recovered-from-the-conflictanddeleted-or-preexisting-folders"></a>Comment est-il possible de récupérer des fichiers à partir des dossiers ConflictAndDeleted ou PreExisting ?

Pour récupérer des fichiers perdus, restaurez les fichiers à partir du dossier du système de fichiers ou du dossier partagé à l’aide de l’historique des fichiers, de la commande **Restaurer les versions précédentes** de l’Explorateur de fichiers ou en restaurant les fichiers à partir d’une sauvegarde. Pour récupérer des fichiers directement à partir du dossier ConflictAndDeleted ou PreExisting, utilisez les applets de commande Windows PowerShell `Get-DfsrPreservedFiles` et `Restore-DfsrPreservedFiles` (incluses dans le module DFSR de Windows Server 2012 R2) ou l’exemple de script [RestoreDFSR](https://code.msdn.microsoft.com/restoredfsr) de MSDN Code Gallery. Ce script est destiné uniquement à la reprise d’activité après sinistre et est fourni en l’état, sans garantie.

### <a name="is-there-a-way-to-know-the-state-of-replication"></a>Existe-t-il un moyen de connaître l’état de la réplication ?

Oui. Il existe différentes manières de superviser une réplication :

  - La réplication DFS dispose d’un pack d’administration pour System Center Operations Manager qui permet une supervision proactive.  
      
  - La gestion DFS dispose d’un rapport de diagnostic intégré concernant le backlog de réplication, l’efficacité de la réplication et le nombre de fichiers et dossiers présent dans un groupe de réplication donné.  
      
  - Le module Windows PowerShell DFSR dans Windows Server 2012 R2 contient des applets de commande permettant de commencer des tests de propagation et d’écrire des rapports d’intégrité et de propagation. Pour plus d’informations, consultez [Applets de commande de réplication du système de fichiers distribués (DFS) dans Windows PowerShell](https://technet.microsoft.com/library/dn296601.aspx).  
      
  - Dfsrdiag.exe est un outil en ligne de commande qui peut générer un nombre de dans un backlog ou déclencher un test de propagation. Les deux affichent l’état de la réplication. La propagation vous indique si des fichiers sont en cours de réplication sur tous les nœuds. Le backlog vous indique le nombre de fichiers qui doivent encore être répliqués avant que deux ordinateurs soient synchronisés. Le nombre indiqué dans le backlog est le nombre de mises à jour qu’un membre de groupe de réplication n’a pas traitées. Sur les ordinateurs exécutant Windows Server 2012 R2, Windows Server 2012 ou Windows Server 2008 R2, Dfsrdiag.exe peut également afficher les mises à jour que la réplication DFS est en train de répliquer.  
      
  - Les scripts peuvent utiliser WMI pour collecter des informations sur les journaux de backlog (manuellement ou par le biais de MOM).  
      

## <a name="performance"></a>Performances

### <a name="does-dfs-replication-support-dial-up-connections"></a>La réplication DFS prend-elle en charge les connexions d’accès à distance ?

Même si la réplication DFS fonctionne à des vitesses d’accès à distance, elle peut être mise en backlog s’il y a un grand nombre de changements à répliquer. Si des changements mineurs sont apportés à des fichiers existants, la réplication DFS avec la compression différentielle à distance (RDC) fournit des performances nettement meilleures que la copie directe du fichier.

### <a name="does-dfs-replication-perform-bandwidth-sensing"></a>La réplication DFS effectue-t-elle la détection de la bande passante ?

Non. La réplication DFS n’effectue pas de détection de la bande passante. Vous pouvez configurer la réplication DFS pour utiliser une quantité limitée de bande passante en fonction de la connexion (limitation de la bande passante). Toutefois, elle ne réduit pas davantage l’utilisation de la bande passante si l’interface réseau est saturée, et elle peut saturer la liaison pendant de courtes périodes. La limitation de la bande passante avec la réplication DFS n’est pas tout à fait précise, car cette dernière limite la bande passante en limitant les appels RPC. Par conséquent, plusieurs mémoires tampons dans des niveaux inférieurs de la pile réseau (dont RPC) peuvent interférer, provoquant des augmentations significatives du trafic réseau.

### <a name="does-dfs-replication-throttle-bandwidth-per-schedule-per-server-or-per-connection"></a>La réplication DFS limite-t-elle la bande passante conformément à une planification, par serveur ou par connexion ?

Si vous configurez la limitation de la bande passante lors de la spécification de la planification, toutes les connexions de ce groupe de réplication utilisent ce paramètre pour la limitation de la bande passante. La limitation de la bande passante peut également être définie en tant que paramètre au niveau de la connexion à l’aide de la Gestion DFS.

### <a name="does-dfs-replication-use-active-directory-domain-services-to-calculate-site-links-and-connection-costs"></a>La réplication DFS utilise-t-elle Active Directory Domain Services pour calculer les liens de site et les coûts de connexion ?

Non. La réplication DFS utilise la topologie définie par l’administrateur, qui est indépendante du coût du site Active Directory Domain Services.

### <a name="how-can-i-improve-replication-performance"></a>Comment améliorer les performances de la réplication ?

Pour en savoir plus sur les différentes méthodes de réglage des performances de la réplication, consultez [Réglage des performances de la réplication dans DFSR](https://blogs.technet.com/b/askds/archive/2010/03/31/tuning-replication-performance-in-dfsr-especially-on-win2008-r2.aspx) sur le [blog Ask the Directory Services Team](https://blogs.technet.com/b/askds/).

### <a name="how-does-dfs-replication-avoid-saturating-a-connection"></a>Comment la réplication DFS évite la saturation d’une connexion ?

Dans la réplication DFS, vous définissez la bande passante maximale que vous voulez utiliser sur une connexion, et le service maintient ce niveau d’utilisation du réseau. C’est différent du Service de transfert intelligent en arrière-plan (BITS), et la réplication DFS ne sature pas la connexion si vous la définissez de manière appropriée.

Néanmoins, la limitation de la bande passante n’est pas précise à 100 % et la réplication DFS peut saturer la liaison pendant de courtes périodes. Cela est dû au fait que la réplication DFS limite la bande passante en limitant les appels RPC. Étant donné que ce processus repose sur plusieurs mémoires tampons dans des niveaux inférieurs de la pile réseau, dont RPC, le trafic de réplication tend à transiter par salves, ce qui peut, dans certains cas, saturer les liaisons réseau.

Dans Windows Server 2008, la réplication DFS comprend plusieurs améliorations des performances, comme indiqué dans la rubrique [Système de fichiers distribués (DFS)](https://technet.microsoft.com/library/Cc753479) figurant dans les [Changements apportés aux fonctionnalités entre Windows Server 2003 avec SP1 et Windows Server 2008](https://technet.microsoft.com/library/cc753208).

### <a name="how-does-dfs-replication-performance-compare-with-frs"></a>Quelles sont les performances de la réplication DFS par rapport à celles du service FRS ?

La réplication DFS est beaucoup plus rapide que le service FRS, en particulier quand des changements mineurs sont apportés à des fichiers volumineux et que la compression RDC est activée. Par exemple, avec la compression RDC, un changement mineur apporté à une présentation PowerPoint&reg; de 2 Mo peut entraîner l’envoi de seulement 60 Ko sur le réseau, soit une économie de 97 % au niveau des octets transférés.

La compression RDC n’est pas utilisée sur les fichiers dont la taille est inférieure à 64 Ko et n’est pas forcément bénéfique sur les réseaux locaux à haut débit où la bande passante réseau n’est pas utilisée. La compression RDC peut être désactivée par connexion à l’aide de la Gestion DFS.

### <a name="how-frequently-does-dfs-replication-replicate-data"></a>À quelle fréquence la réplication DFS réplique les données ?

Les données sont répliquées en fonction de la planification que vous définissez. Par exemple, vous pouvez définir la planification sur des intervalles de 15 minutes, sept jours par semaine. Pendant ces intervalles, la réplication est activée. La réplication démarre juste après la détection d’un changement de fichier (généralement après quelques secondes).

La planification du groupe de réplication peut être définie sur l’heure UTC (Universal Time Coordinate) tandis que la planification de la connexion est définie sur l’heure locale du membre de réception. Prenez cela en compte quand le groupe de réplication s’étend sur plusieurs fuseaux horaires. L’heure locale correspond à l’heure du membre qui héberge la connexion entrante. La planification affichée de la connexion entrante et de la connexion sortante correspondante reflète les différences de fuseau horaire quand la planification est définie sur l’heure locale.

### <a name="how-much-of-my-servers-system-resources-will-dfs-replication-consume"></a>Quelle quantité de ressources système de mon serveur la réplication DFS utilisera ?

Les ressources disque, mémoire et processeur utilisées par la réplication DFS dépendent d’un certain nombre de facteurs, notamment le nombre et la taille des fichiers, le taux de changements, le nombre de membres du groupe de réplication et le nombre de dossiers répliqués. De plus, certaines ressources sont plus difficiles à estimer. Par exemple, la technologie ESE (Extensible Storage Engine) utilisée pour la base de données de réplication DFS peut consommer un grand pourcentage de la mémoire disponible, qu’elle libère à la demande. Les applications autres que la réplication DFS peuvent être hébergées sur le même serveur en fonction de la configuration du serveur. Toutefois, quand vous hébergez plusieurs applications ou rôles de serveur sur un serveur unique, il est important de tester cette configuration avant de l’implémenter dans un environnement de production.

### <a name="what-happens-if-a-wan-link-fails-during-replication"></a>Que se passe-t-il si une liaison de réseau étendu (WAN) échoue pendant la réplication ?

Si la connexion est interrompue, la réplication DFS continuera à tenter d’effectuer la réplication pendant que la planification est ouverte. Des erreurs de connectivité seront également signalées dans le journal des événements de la réplication DFS. Elles peuvent être collectées à l’aide de MOM (de manière proactive par le biais d’alertes) et dans le rapport d’intégrité de la réplication DFS (de manière réactive, par exemple quand un administrateur l’exécute).

## <a name="remote-differential-compression-details"></a>Détails de la compression différentielle à distance (RDC)

### <a name="are-changes-compressed-before-being-replicated"></a>Les changements sont-ils compressés avant d’être répliqués ?

Oui. Les parties de fichiers changées sont compressées avant d’être envoyées, pour tous les types de fichiers, à l’exception des suivants (qui sont déjà compressés) : .wma, .wmv, .zip, .jpg, .mpg, .mpeg, .m1v, .mp2, .mp3, .mpa, .cab, .wav, .snd, .au, .asf, .wm, .avi, .z, .gz, .tgz et .frx. Les paramètres de compression pour ces types de fichiers ne sont pas configurables dans Windows Server 2003 R2.

### <a name="can-an-administrator-turn-off-rdc-or-change-the-threshold"></a>Un administrateur peut-il désactiver la compression RDC ou changer le seuil ?

Oui. Vous pouvez désactiver la compression RDC par le biais de la page de propriétés d’une connexion donnée. La désactivation de la compression RDC peut réduire l’utilisation du processeur et la latence de réplication sur les liens de réseau local (LAN) rapides qui n’ont pas de contraintes de bande passante ou pour les groupes de réplication qui se composent principalement de fichiers de taille inférieure à 64 Ko. Si vous choisissez de désactiver la compression RDC sur une connexion, testez l’efficacité de la réplication avant et après le changement pour vérifier que vous avez amélioré les performances de réplication.

Vous pouvez changer le seuil de taille de la compression RDC à l’aide de la commande **Dfsradmin Connection Set**, du fournisseur WMI de la réplication DFS ou en modifiant manuellement le fichier XML de configuration.

### <a name="does-rdc-work-on-all-file-types"></a>La compression RDC fonctionne-t-elle sur tous les types de fichiers ?

Oui. La compression RDC calcule les différences au niveau du bloc, quel que soit le type de données des fichiers. Toutefois, la compression RDC fonctionne plus efficacement sur certains types de fichiers comme les documents Word, les fichiers PST et les images VHD.

### <a name="how-does-rdc-work-on-a-compressed-file"></a>Comment fonctionne la compression RDC sur un fichier compressé ?

La réplication DFS utilise la compression RDC qui calcule les blocs du fichier ayant changé et envoie uniquement ces blocs sur le réseau. La réplication DFS n’a pas besoin de connaître le contenu du fichier, mais uniquement les blocs qui ont changé.

### <a name="is-cross-file-rdc-enabled-when-upgrading-to-windows-server-enterprise-edition-or-datacenter-edition"></a>La compression RDC interfichier est-elle activée lors de la mise à niveau vers Windows Server Enterprise Edition ou Datacenter Edition ?

Les éditions Standard de Windows Server ne prennent pas en charge la compression RDC interfichier. Toutefois, elle est automatiquement activée quand vous effectuez une mise à niveau vers une édition qui prend en charge la compression RDC interfichier, ou si un membre de la connexion de réplication exécute une édition prise en charge. Pour obtenir la liste des éditions qui prennent en charge la compression RDC interfichier, consultez « Quelles éditions du système d’exploitation Windows prennent en charge la compression RDC interfichier ? ».

### <a name="is-rdc-true-block-level-replication"></a>La compression RDC est-elle une véritable réplication au niveau du bloc ?

Non. La compression RDC est un protocole à usage général permettant de compresser le transfert de fichiers. La réplication DFS utilise la compression RDC sur les blocs au niveau du fichier, et non au niveau du bloc de disque. La compression RDC divise un fichier en blocs. Pour chaque bloc présent dans un fichier, elle calcule une signature, qui est un petit nombre d’octets pouvant représenter le bloc plus grand. L’ensemble des signatures est transféré du serveur au client. Le client compare les signatures du serveur à ses propres signatures. Le client demande ensuite au serveur d’envoyer uniquement les données correspondant aux signatures qu’il ne détient pas déjà.

### <a name="what-happens-if-i-rename-a-file"></a>Que se passe-t-il si je renomme un fichier ?

La réplication DFS renomme le fichier sur tous les autres membres du groupe de réplication lors de la réplication suivante. Les fichiers étant suivis à l’aide d’un ID unique, le fait de renommer un fichier et de le déplacer dans le réplica n’a aucun effet sur la capacité de la réplication DFS à répliquer un fichier.

### <a name="what-is-cross-file-rdc"></a>Qu’est-ce que la compression RDC interfichier ?

La compression RDC interfichier permet à la réplication DFS d’utiliser la compression RDC même s’il n’existe aucun fichier portant le même nom du côté client. La compression RDC interfichier utilise une méthode heuristique pour déterminer quels fichiers sont similaires au fichier qui doit être répliqué, et utilise les blocs des fichiers similaires qui sont identiques au fichier de réplication pour réduire la quantité de données transférées sur le réseau étendu. Dans ce processus, la compression RDC interfichier peut utiliser des blocs de cinq fichiers similaires au maximum.

Pour utiliser la compression RDC interfichier, un membre de la connexion de réplication doit exécuter une édition de Windows qui prend en charge la compression RDC interfichier. Pour obtenir la liste des éditions qui prennent en charge la compression RDC interfichier, consultez « Quelles éditions du système d’exploitation Windows prennent en charge la compression RDC interfichier ? ».

### <a name="what-is-rdc"></a>Qu’est-ce que la compression RDC ?

La compression différentielle à distance (RDC) est un protocole client-serveur qui peut être utilisé pour mettre à jour efficacement des fichiers sur un réseau à bande passante limitée. La compression RDC détecte les insertions, les suppressions et les réorganisations de données dans les fichiers, ce qui permet à la réplication DFS de répliquer uniquement les changements quand des fichiers sont mis à jour. Par défaut, la compression RDC est utilisée uniquement pour les fichiers de 64 Ko ou plus. La compression RDC peut utiliser une version antérieure d’un fichier portant le même nom dans le dossier répliqué ou dans le dossier DfsrPrivate\\ConflictandDeleted (situé sous le chemin local du dossier répliqué).

### <a name="when-is-rdc-used-for-replication"></a>Quand la compression RDC est utilisée pour la réplication ?

La compression RDC est utilisée quand le fichier dépasse un seuil de taille minimale. Ce seuil de taille est de 64 Ko par défaut. Une fois qu’un fichier ayant dépassé ce seuil a été répliqué, les versions mises à jour du fichier utilisent toujours la compression RDC, sauf si une grande partie du fichier est modifiée ou si la compression RDC est désactivée.

### <a name="which-editions-of-the-windows-operating-system-support-cross-file-rdc"></a>Quelles éditions du système d’exploitation Windows prennent en charge la compression RDC interfichier ?

Pour utiliser la compression RDC interfichier, un membre de la connexion de réplication doit exécuter une édition du système d’exploitation Windows qui prend en charge la compression RDC interfichier. Le tableau suivant indique quelles éditions du système d’exploitation Windows prennent en charge la compression RDC interfichier.

### <a name="cross-file-rdc-availability-in-editions-of-the-windows-operating-system"></a>Disponibilité de la compression RDC interfichier dans les éditions du système d’exploitation Windows

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
<th>Édition Datacenter</th>
</tr>
</thead>
<tbody>
<tr class="even">
<td><p>Windows Server 2012 R2</p></td>
<td><p><em>Oui</p></td>
<td><p>Non disponible</p></td>
<td><p>Oui</em></p></td>
</tr>
<tr class="odd">
<td><p>Windows Server 2012</p></td>
<td><p>Oui</p></td>
<td><p>Non disponible</p></td>
<td><p>Oui</p></td>
</tr>
<tr class="even">
<td><p>Windows Server 2008 R2</p></td>
<td><p>Non</p></td>
<td><p>Oui</p></td>
<td><p>Oui</p></td>
</tr>
<tr class="odd">
<td><p>Windows Server 2008</p></td>
<td><p>Non</p></td>
<td><p>Oui</p></td>
<td><p>Non</p></td>
</tr>
<tr class="even">
<td><p>Windows Server 2003 R2</p></td>
<td><p>Non</p></td>
<td><p>Oui</p></td>
<td><p>Non</p></td>
</tr>
</tbody>
</table>

\* Vous pouvez éventuellement désactiver la compression RDC interfichier sur Windows Server 2012 R2.

## <a name="replication-details"></a>Détails de la réplication

### <a name="can-i-change-the-path-for-a-replicated-folder-after-it-is-created"></a>Est-ce que je peux changer le chemin d’un dossier répliqué après l’avoir créé ?

Non. Si vous avez besoin de changer le chemin d’un dossier répliqué, vous devez le supprimer dans la Gestion DFS, puis l’ajouter de nouveau en tant que nouveau dossier répliqué. La réplication DFS utilise ensuite la compression différentielle à distance (RDC) pour effectuer une synchronisation qui détermine si les données sont identiques sur les membres d’envoi et les membres de réception. Elle ne réplique pas à nouveau toutes les données du dossier.

### <a name="can-i-configure-which-file-attributes-are-replicated"></a>Puis-je configurer les attributs de fichier qui sont répliqués ?

Non, vous ne pouvez pas configurer les attributs de fichier que la réplication DFS réplique.

Pour obtenir la liste des valeurs d’attribut et leurs descriptions, consultez [Attributs de fichier](https://go.microsoft.com/fwlink/?linkid=182268) sur MSDN (https://go.microsoft.com/fwlink/?LinkId=182268).

Les valeurs d’attribut suivantes sont définies à l’aide de la fonction `SetFileAttributes dwFileAttributes`, et elles sont répliquées par la réplication DFS. Les changements apportés à ces valeurs d’attribut déclenchent la réplication des attributs. Le contenu du fichier n’est pas répliqué à moins que le contenu change également. Pour plus d’informations, consultez [Fonction SetFileAttributes](https://go.microsoft.com/fwlink/?linkid=182269) dans MSDN Library (https://go.microsoft.com/fwlink/?LinkId=182269).

  - FILE\_ATTRIBUTE\_HIDDEN  
      
  - FILE\_ATTRIBUTE\_READONLY  
      
  - FILE\_ATTRIBUTE\_SYSTEM  
      
  - FILE\_ATTRIBUTE\_NOT\_CONTENT\_INDEXED  
      
  - FILE\_ATTRIBUTE\_OFFLINE  
      

Les valeurs d’attribut suivantes sont répliquées par la réplication DFS, mais elles ne déclenchent pas la réplication.

  - FILE\_ATTRIBUTE\_ARCHIVE  
      
  - FILE\_ATTRIBUTE\_NORMAL  
      

Les valeurs d’attribut de fichier suivantes déclenchent également la réplication, même si elles ne peuvent pas être définies à l’aide de la fonction `SetFileAttributes` (utilisez la fonction `GetFileAttributes` pour afficher les valeurs d’attribut).

  - FILE\_ATTRIBUTE\_REPARSE\_POINT  
      

> [!NOTE]
> La réplication DFS ne réplique pas les valeurs d’attribut de point d’analyse, sauf si la balise d’analyse est IO_REPARSE_TAG_SYMLINK. Les fichiers avec des balises d’analyse IO_REPARSE_TAG_DEDUP, IO_REPARSE_TAG_SIS ou IO_REPARSE_TAG_HSM sont répliqués comme les fichiers normaux. Toutefois, la balise d’analyse et les tampons de données d’analyse ne sont pas répliqués sur d’autres serveurs, car le point d’analyse fonctionne uniquement sur le système local. 
<br>

  - FILE\_ATTRIBUTE\_COMPRESSED  
      
  - FILE\_ATTRIBUTE\_ENCRYPTED  
      

> [!NOTE]
> La réplication DFS ne réplique pas les fichiers qui sont chiffrés à l’aide du système de fichiers EFS. La réplication DFS réplique les fichiers qui sont chiffrés à l’aide de logiciels non-Microsoft, mais uniquement si elle ne définit pas la valeur de l’attribut FILE_ATTRIBUTE_ENCRYPTED sur le fichier. 
<br>

  - FILE\_ATTRIBUTE\_SPARSE\_FILE  
      
  - FILE\_ATTRIBUTE\_DIRECTORY  
      

La réplication DFS ne réplique pas la valeur FILE\_ATTRIBUTE\_TEMPORARY.

### <a name="can-i-control-which-member-is-replicated"></a>Puis-je contrôler le membre qui est répliqué ?

Oui. Vous pouvez choisir une topologie quand vous créez un groupe de réplication. Vous pouvez également sélectionner **Aucune topologie** et configurer manuellement les connexions après la création du groupe de réplication.

### <a name="can-i-seed-a-replication-group-member-with-data-prior-to-the-initial-replication"></a>Puis-je prédéfinir un membre de groupe de réplication avec des données avant la réplication initiale ?

Oui. La réplication DFS prend en charge la copie de fichiers vers un membre de groupe de réplication avant la réplication initiale. Cette « prédéfinition » peut réduire considérablement la quantité de données répliquées pendant la réplication initiale.

La réplication initiale n’a pas besoin de répliquer le contenu quand les fichiers diffèrent uniquement par des horodatages ou des attributs réels. Un attribut réel est un attribut qui peut être défini par la fonction Win32 `SetFileAttributes`. Pour plus d’informations, consultez [Fonction SetFileAttributes](https://go.microsoft.com/fwlink/?linkid=182269) dans MSDN Library (https://go.microsoft.com/fwlink/?LinkId=182269). Si deux fichiers diffèrent par d’autres attributs, comme la compression, le contenu du fichier est répliqué.

Pour prédéfinir un membre de groupe de réplication, copiez les fichiers dans le dossier approprié sur le ou les serveurs de destination, créez le groupe de réplication, puis choisissez un membre principal. Choisissez le membre qui possède le plus grand nombre de fichiers à jour à répliquer, car le contenu du membre principal est considéré comme « faisant autorité ». Cela signifie que, lors de la réplication initiale, les fichiers du membre principal remplaceront toujours les autres versions des fichiers sur les autres membres du groupe de réplication.

Pour plus d’informations sur la prédéfinition et le clonage de la base de données DFSR, consultez [Synchronisation initiale de la réplication DFS dans Windows Server 2012 R2 : Attaque des clones](https://blogs.technet.com/b/filecab/archive/2013/08/21/dfs-replication-initial-sync-in-windows-server-2012-r2-attack-of-the-clones.aspx).

Pour plus d’informations sur la réplication initiale, consultez [Créer un groupe de réplication](https://technet.microsoft.com/library/cc725893).

### <a name="does-dfs-replication-overcome-common-file-replication-service-issues"></a>La réplication DFS résout-elle les problèmes courants liés au service de réplication de fichiers (FRS) ?

Oui. La réplication DFS résout trois problèmes courants liés à FRS :

  - Dépassements de taille de journal : la réplication DFS récupère immédiatement des dépassements de taille de journal. Chaque fichier ou dossier existant est marqué comme ayant fait l’objet d’un dépassement de taille de journal (journalWrap) et est vérifié par rapport au système de fichiers avant que la réplication soit réactivée. Pendant la récupération, ce volume n’est disponible pour la réplication dans aucun sens.  
      
  - Réplication excessive : pour éviter une réplication excessive, la réplication DFS utilise un système de crédits.  
      
  - Dossiers modifiés : pour empêcher les noms de dossier modifiés, la réplication DFS stocke les données en conflit dans un dossier masqué DfsrPrivate\\ConflictandDeleted (situé sous le chemin local du dossier répliqué). Par exemple, si plusieurs dossiers portant des noms identiques sur des serveurs différents répliqués à l’aide du service FRS sont créés simultanément, le service FRS renomme le ou les dossiers les plus anciens. La réplication DFS, quant à elle, déplace le ou les dossiers les plus anciens vers le dossier local des fichiers en conflit et supprimés.  
      

### <a name="does-dfs-replication-replicate-files-in-chronological-order"></a>La réplication DFS réplique-t-elle les fichiers par ordre chronologique ?

Non. Les fichiers peuvent être répliqués dans le désordre.

### <a name="does-dfs-replication-replicate-files-that-are-being-used-by-another-application"></a>La réplication DFS réplique-t-elle les fichiers qui sont en cours d’utilisation par une autre application ?

Si une application ouvre un fichier et crée un verrou de fichier sur celui-ci (ce qui l’empêche d’être utilisé par d’autres applications pendant qu’il est ouvert), la réplication DFS ne réplique pas le fichier tant qu’il n’est pas fermé. Si l’application ouvre le fichier avec un accès en lecture-partage, le fichier peut toujours être répliqué.

### <a name="does-dfs-replication-replicate-ntfs-file-permissions-alternate-data-streams-hard-links-and-reparse-points"></a>La réplication DFS réplique-t-elle les autorisations de fichiers NTFS, d’autres flux de données, les liens physiques et les points d’analyse ?

  - La réplication DFS réplique les autorisations de fichiers NTFS et d’autres flux de données.  
      
  - Microsoft ne prend pas en charge la création de liens physiques NTFS vers ou à partir de fichiers dans un dossier répliqué. Cette opération peut entraîner des problèmes de réplication avec les fichiers concernés. Les fichiers de liens physiques sont ignorés par la réplication DFS et ne sont pas répliqués. Les points de jonction ne sont pas non plus répliqués, et la réplication DFS journalise l’événement 4406 pour chaque point de jonction qu’elle rencontre.  
      
  - Les seuls points d’analyse répliqués par la réplication DFS sont ceux qui utilisent la balise IO\_REPARSE\_TAG\_SYMLINK. Toutefois, la réplication DFS ne garantit pas que la cible d’un lien symbolique est également répliquée. Pour plus d’informations, consultez le [blog Ask the Directory Services Team](https://blogs.technet.com/b/askds/archive/2011/09/30/friday-mail-sack-super-slo-mo-edition.aspx).  
      
  - Les fichiers avec la balise d’analyse IO\_REPARSE\_TAG\_DEDUP, IO\_REPARSE\_TAG\_SIS ou IO\_REPARSE\_TAG\_HSM sont répliqués comme des fichiers normaux. La balise d’analyse et les tampons de données d’analyse ne sont pas répliqués sur d’autres serveurs, car le point d’analyse fonctionne uniquement sur le système local. Par conséquent, la réplication DFS peut répliquer des dossiers sur des volumes qui utilisent la déduplication des données dans Windows Server 2012 ou le stockage d’instance simple (SIS, Single Instance Storage). Toutefois, les informations de déduplication des données sont tenues à jour séparément par chaque serveur sur lequel le service de rôle est activé.  
      

### <a name="does-dfs-replication-replicate-timestamp-changes-if-no-other-changes-are-made-to-the-file"></a>La réplication DFS réplique-t-elle les changements d’horodatage si aucun autre changement n’est apporté au fichier ?

Non, la réplication DFS ne réplique pas les fichiers pour lesquels le seul changement concerne l’horodatage. De plus, l’horodatage modifié n’est pas répliqué sur d’autres membres du groupe de réplication, sauf si d’autres changements sont apportés au fichier.

### <a name="does-dfs-replication-replicate-updated-permissions-on-a-file-or-folder"></a>La réplication DFS réplique-t-elle les autorisations mises à jour sur un fichier ou un dossier ?

Oui. La réplication DFS réplique les changements d’autorisation pour les fichiers et les dossiers. Seule la partie du fichier associée à la liste de contrôle d’accès (ACL) est répliquée, même si la réplication DFS doit toujours lire la totalité du fichier dans la zone de copie intermédiaire.


> [!NOTE]
> Changer des listes de contrôle d’accès sur un grand nombre de fichiers peut avoir un impact sur les performances de la réplication. Toutefois, lors de l’utilisation de la compression RDC, la quantité de données transférées est proportionnelle à la taille des listes de contrôle d’accès, et non à la taille du fichier entier. Le volume de trafic du disque reste proportionnel à la taille des fichiers, car ceux-ci doivent être lus vers et depuis le dossier intermédiaire. 
<br>


### <a name="does-dfs-replication-support-merging-text-files-in-the-event-of-a-conflict"></a>La réplication DFS prend-elle en charge la fusion de fichiers texte en cas de conflit ?

La réplication DFS ne fusionne pas les fichiers en cas de conflit. Toutefois, elle tente de conserver l’ancienne version du fichier dans le dossier masqué DfsrPrivate\\ConflictandDeleted sur l’ordinateur sur lequel le conflit a été détecté.

### <a name="does-dfs-replication-use-encryption-when-transmitting-data"></a>La réplication DFS utilise-t-elle le chiffrement lors de la transmission de données ?

Oui. La réplication DFS utilise des connexions RPC (appel de procédure distante) avec chiffrement.

### <a name="is-it-possible-to-disable-the-use-of-encrypted-rpc"></a>Est-il possible de désactiver l’utilisation d’une communication RPC chiffrée ?

Non. Le service de réplication DFS utilise des appels de procédure distante (RPC) sur TCP pour répliquer les données. Pour sécuriser les transferts de données sur Internet, le service de réplication DFS est conçu pour toujours utiliser la constante au niveau de l’authentification `RPC_C_AUTHN_LEVEL_PKT_PRIVACY`. Cela permet de garantir que la communication RPC sur Internet est toujours chiffrée. Par conséquent, il n’est pas possible de désactiver l’utilisation de la communication RPC chiffré par le service de réplication DFS.

Pour plus d’informations, consultez les sites web Microsoft suivants :

  - [Informations techniques de référence sur RPC](https://go.microsoft.com/fwlink/?linkid=182278)  
      
  - [À propos de la compression différentielle à distance](https://go.microsoft.com/fwlink/?linkid=182279)  
      
  - [Constantes au niveau de l’authentification](https://go.microsoft.com/fwlink/?linkid=182280)  
      

### <a name="how-are-simultaneous-replications-handled"></a>Comment les réplications simultanées sont-elles gérées ?

Il y a un gestionnaire de mises à jour par dossier répliqué. Les gestionnaires de mises à jour fonctionnent indépendamment les uns des autres.

Par défaut, un maximum de 16 (quatre dans Windows Server 2003 R2) téléchargements simultanés sont partagés entre toutes les connexions et tous les groupes de réplication. Étant donné que les connexions et les mises à jour de groupe de réplication ne sont pas sérialisées, il n’existe aucun ordre spécifique dans lequel les mises à jour sont reçues. Si deux planifications sont ouvertes, les mises à jour sont généralement reçues et installées à partir des deux connexions en même temps.

### <a name="how-do-i-force-replication-or-polling"></a>Comment forcer la réplication ou l’interrogation ?

Vous pouvez forcer la réplication immédiatement à l’aide de la Gestion DFS, comme décrit dans [Modifier des planifications de réplication](https://technet.microsoft.com/library/Cc732278). Vous pouvez également forcer la réplication à l’aide de l’applet de commande `Sync-DfsReplicationGroup` incluse dans le module PowerShell DFSR qui a été introduit avec Windows Server 2012 R2, ou de la commande **Dfsrdiag SyncNow**. Vous pouvez forcer l’interrogation à l’aide de l’applet de commande `Update-DfsrConfigurationFromAD` ou de la commande **Dfsrdiag PollAD**.

### <a name="is-it-possible-to-configure-a-quiet-time-between-replications-for-files-that-change-frequently"></a>Est-il possible de configurer une période d’inactivité entre les réplications pour les fichiers qui changent fréquemment ?

Non. Si la planification est ouverte, la réplication DFS réplique les changements au fur et à mesure de leur notification. Il n’existe aucun moyen de configurer une période d’inactivité pour des fichiers.

### <a name="is-it-possible-to-configure-one-way-replication-with-dfs-replication"></a>Est-il possible de configurer la réplication unidirectionnelle avec la réplication DFS ?

Oui. Si vous utilisez Windows Server 2012 ou Windows Server 2008 R2, vous pouvez créer un dossier répliqué en lecture seule qui réplique du contenu par le biais d’une connexion unidirectionnelle. Pour plus d’informations, consultez [Rendre un dossier répliqué accessible en lecture seule sur un membre spécifique](https://go.microsoft.com/fwlink/?linkid=156740) (https://go.microsoft.com/fwlink/?LinkId=156740).

Nous ne prenons pas en charge la création d’une connexion de réplication unidirectionnelle avec la réplication DFS dans Windows Server 2008 ou Windows Server 2003 R2. Cette opération peut entraîner de nombreux problèmes, notamment des erreurs de topologie de contrôle d’intégrité, des problèmes de copie préliminaire et des problèmes liés à la base de données de réplication DFS.

Si vous utilisez Windows Server 2008 ou Windows Server 2003 R2, vous pouvez simuler une connexion unidirectionnelle en effectuant les actions suivantes :

  - Formez les administrateurs à apporter des changements uniquement sur le ou les serveurs que vous souhaitez désigner comme serveurs principaux. Laissez ensuite le système répliquer les changements sur les serveurs de destination.  
      
  - Configurez les autorisations de partage sur les serveurs de destination afin que les utilisateurs finaux ne disposent pas d’autorisations en écriture. Si aucun changement n’est autorisé sur les serveurs de succursale, il n’y a rien à répliquer, en simulant une connexion unidirectionnelle et en maintenant l’utilisation du réseau étendu (WAN) à un bas niveau.  
      

### <a name="is-there-a-way-to-force-a-complete-replication-of-all-files-including-unchanged-files"></a>Existe-t-il un moyen de forcer une réplication complète de tous les fichiers, dont les fichiers non modifiés ?

Non. Si la réplication DFS considère que les fichiers sont identiques, elle ne les réplique pas. Si les fichiers modifiés n’ont pas été répliqués, la réplication DFS les réplique automatiquement quand elle est configurée pour le faire. Pour remplacer la planification configurée, utilisez la méthode WMI **ForceReplicate()** . Toutefois, il s’agit uniquement d’un remplacement de planification, et elle ne force pas la réplication de fichiers identiques ou inchangés.

### <a name="what-happens-if-the-primary-member-suffers-a-database-loss-during-initial-replication"></a>Que se passe-t-il si le membre principal subit une perte de base de données lors de la réplication initiale ?

Pendant la réplication initiale, les fichiers du membre principal sont toujours prioritaires dans la résolution des conflits qui se produit si des membres de réception possèdent des versions de fichiers différentes sur le membre principal. La désignation de membre principal, qui est stockée dans Active Directory Domain Services, est effacée une fois que le membre principal est prêt à être répliqué, mais avant la réplication de tous les membres du groupe de réplication.

Si la réplication initiale échoue ou si le service de réplication DFS redémarre pendant la réplication, le membre principal voit la désignation du membre principal dans la base de données de réplication DFS locale et retente la réplication initiale. Si la base de données de réplication DFS du membre principal est perdue après l’effacement de la désignation principale dans Active Directory Domain Services, mais avant que tous les membres du groupe de réplication ne terminent la réplication initiale, aucun membre du groupe de réplication ne parvient pas à répliquer le dossier, car aucun serveur n’est désigné comme membre principal. Si cela se produit, utilisez la commande **Dfsradmin membership /set /isprimary:true** sur le serveur du membre principal pour restaurer manuellement la désignation du membre principal.

Pour plus d’informations sur la réplication initiale, consultez [Créer un groupe de réplication](https://technet.microsoft.com/library/cc725893).


> [!WARNING]
> La désignation du membre principal est utilisée uniquement pendant le processus de réplication initiale. Si vous utilisez la commande <STRONG>Dfsradmin</STRONG> afin de spécifier un membre principal pour un dossier répliqué une fois la réplication terminée, la réplication DFS ne désigne pas le serveur comme membre principal dans Active Directory Domain Services. Toutefois, si la base de données de réplication DFS sur le serveur subit par la suite une altération irréversible ou une perte de données, le serveur tente d’effectuer une réplication initiale en tant que membre principal au lieu de récupérer ses données à partir d’un autre membre du groupe de réplication. En fait, le serveur devient un serveur principal non autorisé, ce qui peut entraîner des conflits. Par conséquent, spécifiez le membre principal manuellement uniquement si vous êtes certain que la réplication initiale a échoué de manière irrémédiable. 
<br>


### <a name="what-happens-if-the-replication-schedule-closes-while-a-file-is-being-replicated"></a>Que se passe-t-il si la planification de réplication se ferme pendant qu’un fichier est en cours de réplication ?

Si la compression différentielle à distance (RDC) est activée sur la connexion, la réplication entrante d’un fichier d’une taille supérieure à 64 Ko dont la réplication a commencé juste avant la fermeture de la planification (ou le passage en mode **Pas de bande passante**) se poursuit quand la planification s’ouvre (ou passe dans un mode autre que **Pas de bande passante**). La réplication se poursuit à partir de l’état dans lequel où elle se trouvait quand la réplication s’est arrêtée.

Si la compression RDC est désactivée, la réplication DFS redémarre complètement le transfert de fichiers. Cela peut retarder le moment où le fichier est disponible sur le membre de réception.

### <a name="what-happens-when-two-users-simultaneously-update-the-same-file-on-different-servers"></a>Que se passe-t-il quand deux utilisateurs mettent à jour simultanément le même fichier sur des serveurs différents ?

Quand la réplication DFS détecte un conflit, elle utilise la version du fichier qui a été enregistrée en dernier. Elle déplace l’autre fichier dans le dossier DfsrPrivate\\ConflictandDeleted (sous le chemin local du dossier répliqué sur l’ordinateur qui a résolu le conflit). Il y reste jusqu’au nettoyage du dossier des fichiers en conflit et supprimés, qui se produit quand ce dernier dépasse la taille configurée ou quand la réplication DFS rencontre une erreur d’espace disque insuffisant. Le dossier des fichiers en conflit et supprimés n’est pas répliqué, et cette méthode de résolution des conflits évite le problème des répertoires modifiés qui pouvait se produire dans le service FRS.

Quand un conflit se produit, la réplication DFS journalise un événement d’information dans le journal des événements de la réplication DFS. Cet événement ne nécessite pas d’action de la part de l’utilisateur pour les raisons suivantes :

  - Il n’est pas visible par les utilisateurs (il est visible uniquement par les administrateurs du serveur).  
      
  - La réplication DFS traite le dossier des fichiers en conflit et supprimés comme un cache. Quand un seuil de quota est atteint, elle nettoie certains de ces fichiers. Rien ne garantit que les fichiers en conflit seront enregistrés.  
      
  - Le conflit peut se trouver sur un serveur autre que l’origine du conflit.  
      

## <a name="staging"></a>Création intermédiaire

### <a name="does-dfs-replication-continue-staging-files-when-replication-is-disabled-by-a-schedule-or-bandwidth-throttling-quota-or-when-a-connection-is-manually-disabled"></a>La réplication DFS effectue-t-elle une copie intermédiaire des fichiers quand la réplication est désactivée par une planification ou un quota de limitation de bande passante, ou quand une connexion est désactivée manuellement ?

Non. La réplication DFS ne continue pas à créer des copies intermédiaires des fichiers en dehors des temps de réplication planifiés, si le quota de limitation de la bande passante a été dépassé ou quand les connexions sont désactivées.

### <a name="does-dfs-replication-prevent-other-applications-from-accessing-a-file-during-staging"></a>La réplication DFS empêche-t-elle d’autres applications d’accéder à un fichier pendant sa copie intermédiaire ?

Non. La réplication DFS ouvre les fichiers de façon à ne pas empêcher les utilisateurs ou les applications d’ouvrir des fichiers dans le dossier de réplication. Cette méthode est appelée « verrouillage opportuniste ».

### <a name="is-it-possible-to-change-the-location-of-the-staging-folder-with-the-dfs-management-tool"></a>Est-il possible de changer l’emplacement du dossier intermédiaire avec l’outil Gestion DFS ?

Oui. L’emplacement du dossier intermédiaire est configuré sous l’onglet **Avancé** de la boîte de dialogue **Propriétés** pour chaque membre d’un groupe de réplication.

### <a name="when-are-files-staged"></a>Quand une copie intermédiaire des fichiers est-elle créée ?

Une copie intermédiaire des fichiers est créée sur le membre d’envoi quand le membre de réception demande le fichier (à moins que la taille du fichier soit inférieure ou égale à 64 Ko) comme indiqué dans le tableau suivant. Si la compression différentielle à distance (RDC) est désactivée sur la connexion, une copie temporaire du fichier est créée, à moins que sa taille soir inférieure ou égale à 256 Ko. Une copie temporaire des fichiers est également créée sur le membre de réception au moment de leur transfert si leur taille est inférieure à 64 Ko, même si vous pouvez configurer ce paramètre avec une valeur comprise entre 16 Ko et 1 Mo. Si la planification est fermée, aucune copie intermédiaire des fichiers n’est créée.

### <a name="the-minimum-file-sizes-for-staging-files"></a>Taille minimale des fichiers pour les fichiers intermédiaires

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th> </th>
<th>RDC activée</th>
<th>RDC désactivée</th>
</tr>
</thead>
<tbody>
<tr class="even">
<td><p>Membre d’envoi</p></td>
<td><p>64 Ko</p></td>
<td><p>256 Ko</p></td>
</tr>
<tr class="odd">
<td><p>Membre de réception</p></td>
<td><p>64 Ko par défaut</p></td>
<td><p>64 Ko par défaut</p></td>
</tr>
</tbody>
</table>

### <a name="what-happens-if-a-file-is-changed-after-it-is-staged-but-before-it-is-completely-transmitted-to-the-remote-site"></a>Que se passe-t-il si un fichier est modifié après la création d’une copie intermédiaire mais avant qu’il soit complètement transmis au site distant ?

Si une partie du fichier est déjà en cours de transmission, la réplication DFS continue la transmission. Si le fichier est modifié avant que la réplication DFS ne commence à transmettre le fichier, la version la plus récente de ce dernier est envoyée.

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
<th>Raison</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>15 novembre 2018</p></td>
<td><p>Mise à jour pour Windows Server 2019.</p></td>
<td><p>Nouveau système d’exploitation.</p></td>
</tr>
<tr class="even">
<td><p>9 octobre 2013</p></td>
<td><p>Mise à jour de la section « Quelles sont les limites de la réplication DFS prises en charge ? » avec les résultats des tests effectués sur Windows Server 2012 R2.</p></td>
<td><p>Mises à jour pour la dernière version de Windows Server</p></td>
</tr>
<tr class="odd">
<td><p>30 janvier 2013</p></td>
<td><p>Ajout de l’entrée « La réplication DFS effectue-t-elle une copie intermédiaire des fichiers quand la réplication est désactivée par une planification ou un quota de limitation de bande passante, ou quand une connexion est désactivée manuellement ? ».</p></td>
<td><p>Questions des clients</p></td>
</tr>
<tr class="even">
<td><p>31 octobre 2012</p></td>
<td><p>Modification de l’entrée « Quelles sont les limites de la réplication DFS prises en charge ? » afin d’augmenter le nombre de fichiers répliqués testés sur un volume.</p></td>
<td><p>Feedback des clients</p></td>
</tr>
<tr class="odd">
<td><p>15 août 2012</p></td>
<td><p>Modification de l’entrée « La réplication DFS réplique-t-elle les autorisations de fichiers NTFS, d’autres flux de données, les liens physiques et les points d’analyse ? » pour mieux expliquer la façon dont la réplication DFS gère les liens physiques et les points d’analyse.</p></td>
<td><p>Commentaires des services de support client</p></td>
</tr>
<tr class="even">
<td><p>13 juin 2012</p></td>
<td><p>Modification de l’entrée « La réplication DFS fonctionne-t-elle sur les volumes ReFS ou FAT ? » pour ajouter des informations complémentaires sur ReFS.</p></td>
<td><p>Feedback des clients</p></td>
</tr>
<tr class="odd">
<td><p>25 avril 2012</p></td>
<td><p>Modification de l’entrée « La réplication DFS réplique-t-elle les autorisations de fichiers NTFS, d’autres flux de données, les liens physiques et les points d’analyse ? » pour clarifier la façon dont la réplication DFS gère les liens physiques.</p></td>
<td><p>Réduire la confusion potentielle</p></td>
</tr>
<tr class="even">
<td><p>30 mars 2011</p></td>
<td><p>Modification de l’entrée « La réplication DFS peut-elle répliquer des fichiers de base de données Microsoft Office Access ou Outlook.pst ? » pour corriger l’impact potentiel de l’utilisation de la réplication DFS avec des fichiers .pst et Access.</p>
<p>Ajout de la section « Comment puis-je améliorer les performances de la réplication ? ».</p></td>
<td><p>Questions des clients sur l’entrée précédente, qui indiquait à tors que la réplication de fichiers .pst ou Access pouvait corrompre la base de données de réplication DFS.</p></td>
</tr>
<tr class="odd">
<td><p>26 janvier 2011</p></td>
<td><p>Ajout de la section « Comment est-il possible de récupérer des fichiers à partir des dossiers ConflictAndDeleted ou PreExisting ? ».</p></td>
<td><p>Feedback des clients</p></td>
</tr>
<tr class="even">
<td><p>20 octobre 2010</p></td>
<td><p>Ajout de la section « Comment puis-je mettre à niveau ou remplacer un membre de la réplication DFS ? ».</p></td>
<td><p>Feedback des clients</p></td>
</tr>
</tbody>
</table>

