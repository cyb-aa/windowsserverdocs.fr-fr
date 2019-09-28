---
title: 'Réplication DFS : Forum Aux Questions (FAQ)'
ms.date: 06/18/2014
ms.prod: windows-server
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 12410d619245153f759b54e7a8aff257888f04dc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386074"
---
# <a name="dfs-replication-frequently-asked-questions-faq"></a>Réplication DFS : Forum Aux Questions (FAQ)


Mise à jour : 30 avril 2019

S'applique à : Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Ce Forum aux questions répond aux questions relatives à la réplication système de fichiers DFS (DFS) (également appelée DFS-R ou DFSR) pour Windows Server.

Pour plus d'informations sur les espaces de noms DFS, consultez [Espaces de noms DFS : Forum aux questions](https://technet.microsoft.com/library/ee404780).

Pour plus d’informations sur les nouveautés de réplication DFS, consultez les rubriques suivantes :

  - [Vue d’ensemble des espaces de noms DFS et réplication DFS](https://technet.microsoft.com/library/jj127250) (dans Windows Server 2012)  
      
  - [Nouveautés de système de fichiers DFS](https://technet.microsoft.com/library/ee307957) rubrique dans [modifications des fonctionnalités de Windows Server 2008 à Windows Server 2008 R2](https://technet.microsoft.com/library/dd391932)  
      
  - [Système de fichiers DFS](https://technet.microsoft.com/library/cc753479) rubrique dans [modifications de la fonctionnalité de Windows Server 2003 avec SP1 vers Windows Server 2008](https://technet.microsoft.com/library/cc753208)  
      

Pour obtenir la liste des modifications récentes apportées à cette rubrique, consultez la section [Historique des modifications](#change-history) de cette rubrique.

      

## <a name="interoperability"></a>Interopérabilité

### <a name="can-dfs-replication-communicate-with-frs"></a>Peut-réplication DFS communiquer avec FRS ?

Non. Réplication DFS ne communique pas avec le service de réplication de fichiers (FRS). Réplication DFS et FRS peuvent s’exécuter sur le même serveur en même temps, mais ils ne doivent jamais être configurés pour répliquer les mêmes dossiers ou sous-dossiers, car cela peut entraîner une perte de données.

### <a name="can-dfs-replication-replace-frs-for-sysvol-replication"></a>Peut réplication DFS remplacer FRS pour la réplication SYSVOL

Oui, réplication DFS peut remplacer FRS pour la réplication SYSVOL sur des serveurs exécutant Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008. Les serveurs exécutant Windows Server 2003 R2 ne prennent pas en charge l’utilisation de réplication DFS pour répliquer le dossier SYSVOL.

Pour plus d’informations sur la réplication de SYSVOL à l’aide [de réplication DFS, consultez le Guide de migration de la réplication SYSVOL : FRS à réplication DFS](https://technet.microsoft.com/library/dd640019).

### <a name="can-i-upgrade-from-frs-to-dfs-replication-without-losing-configuration-settings"></a>Puis-je effectuer une mise à niveau de FRS vers réplication DFS sans perdre les paramètres de configuration ?

Oui. Pour migrer la réplication de FRS vers réplication DFS, consultez les documents suivants :

  - Pour migrer la réplication des dossiers autres que le dossier [SYSVOL, consultez le guide des opérations DFS : Migration de FRS vers réplication DFS](http://go.microsoft.com/fwlink/?linkid=192776) et [FRS2DFSR – un utilitaire de migration FRS vers DFSR](http://go.microsoft.com/fwlink/?linkid=195437) (http://go.microsoft.com/fwlink/?LinkID=195437).  
      
  - Pour migrer la réplication du dossier Sysvol vers réplication DFS [, consultez le Guide de migration de la réplication SYSVOL : FRS à réplication DFS](https://technet.microsoft.com/library/dd640019).  
      

### <a name="can-i-use-dfs-replication-in-a-mixed-windowsunix-environment"></a>Puis-je utiliser réplication DFS dans un environnement mixte Windows/UNIX ?

Oui. Même si réplication DFS prend en charge la réplication de contenu entre les serveurs exécutant Windows Server, les clients UNIX peuvent accéder aux partages de fichiers sur les serveurs Windows. Pour ce faire, installez les services pour NFS (Network File System) sur le serveur de réplication DFS.

Vous pouvez également utiliser la fonctionnalité client SMB/CIFS incluse dans de nombreux clients UNIX pour accéder directement aux partages de fichiers Windows, bien que cette fonctionnalité soit souvent limitée ou nécessite des modifications de l’environnement Windows (telles que la désactivation de la signature SMB à l’aide de Stratégie de groupe).

Réplication DFS interagit avec NFS sur un serveur exécutant un système d’exploitation Windows Server, mais vous ne pouvez pas répliquer un point de montage NFS.

### <a name="can-i-use-the-volume-shadow-copy-service-with-dfs-replication"></a>Puis-je utiliser la Service VSS avec réplication DFS ?

Oui. Réplication DFS est pris en charge sur les volumes Service VSS (VSS) et les instantanés précédents peuvent être restaurés avec le client des versions précédentes.

### <a name="can-i-use-windowsbackup-ntbackupexe-to-remotely-back-up-a-replicated-folder"></a>Puis-je utiliser la sauvegarde Windows (NTBackup. exe) pour sauvegarder à distance un dossier répliqué ?

Non, l’utilisation de la sauvegarde Windows (NTBackup. exe) sur un ordinateur exécutant Windows Server 2003 ou une version antérieure pour sauvegarder le contenu d’un dossier répliqué sur un ordinateur exécutant Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008 n’est pas prise en charge.

Pour sauvegarder des fichiers stockés dans un dossier répliqué, utilisez Sauvegarde Windows Server ou Microsoft® System Center Data Protection Manager. Pour plus d’informations sur les fonctionnalités de sauvegarde et de récupération de Windows Server 2008 R2 et Windows Server 2008, consultez [sauvegarde et récupération](https://technet.microsoft.com/library/Cc754097). Pour plus d’informations, consultez [System Center Data Protection Manager](http://go.microsoft.com/fwlink/?linkid=182261) (http://go.microsoft.com/fwlink/?LinkId=182261).

### <a name="do-file-system-policies-impact-dfs-replication"></a>Les stratégies de système de fichiers affectent-elles réplication DFS ?

Oui. Ne configurez pas les stratégies de système de fichiers sur les dossiers répliqués. La stratégie du système de fichiers réapplique les autorisations NTFS à chaque stratégie de groupe intervalle d’actualisation. Cela peut entraîner des violations de partage, car un fichier ouvert n’est pas répliqué tant que le fichier n’est pas fermé.

### <a name="does-dfs-replication-replicate-mailboxes-hosted-on-microsoft-exchange-server"></a>Réplication DFS répliquer les boîtes aux lettres hébergées sur Microsoft Exchange Server ?

Non. Réplication DFS ne peut pas être utilisé pour répliquer des boîtes aux lettres hébergées sur Microsoft Exchange Server.

### <a name="does-dfs-replication-support-file-screens-created-by-file-server-resource-manager"></a>Réplication DFS prend-il en charge les filtres de fichiers créés par Gestionnaire des ressources de serveur de fichiers ?

Oui. Toutefois, les paramètres de filtrage de fichiers du serveur de fichiers Gestionnaire des ressources (FSRM) doivent correspondre aux deux terminaisons de la réplication. En outre, réplication DFS dispose de son propre mécanisme de filtre pour les fichiers et les dossiers que vous pouvez utiliser pour exclure certains fichiers et types de fichiers de la réplication.

Voici les meilleures pratiques pour implémenter des filtres de fichiers ou des quotas :

  - Le dossier masqué DfsrPrivate ne doit pas être soumis à des quotas ou à des filtres de fichiers.  
      
  - Les fichiers filtrés ne doivent pas exister dans un dossier répliqué avant l’activation du filtrage.  
      
  - Aucun dossier ne peut dépasser le quota avant l’activation du quota.  
      
  - Vous devez utiliser des quotas réels avec précaution. Il est possible que les membres d’un groupe de réplication restent dans un quota avant la réplication, mais le dépassent lors de la réplication des fichiers. Par exemple, si un utilisateur copie un fichier de 10 mégaoctets (Mo) sur le serveur A (qui est ensuite à la limite inconditionnelle) et qu’un autre utilisateur copie un fichier de 5 Mo sur le serveur B, lorsque la réplication suivante se produit, les deux serveurs dépassent le quota de 5 mégaoctets. Cela peut entraîner une nouvelle tentative de réplication continue des fichiers par réplication DFS, provoquant des trous dans le vecteur de version et des problèmes de performances possibles.  
      

### <a name="is-dfs-replication-cluster-aware"></a>La prise en charge des clusters est-elle réplication DFS ?

Oui, réplication DFS dans Windows Server 2012 R2, Windows Server 2012 et Windows Server 2008 R2 offre la possibilité d’ajouter un cluster de basculement en tant que membre d’un groupe de réplication. Pour plus d’informations, consultez [Ajouter un cluster de basculement à un groupe de réplication](http://go.microsoft.com/fwlink/?linkid=155085) (http://go.microsoft.com/fwlink/?LinkId=155085). Le service réplication DFS sur les versions de Windows antérieures à Windows Server 2008 R2 n’est pas conçu pour se coordonner avec un cluster de basculement et le service ne bascule pas vers un autre nœud.


> [!NOTE]
> Réplication DFS ne prend pas en charge la réplication de fichiers sur des volumes partagés de cluster. 
<br>


### <a name="is-dfs-replication-compatible-with-data-deduplication"></a>Le réplication DFS est-il compatible avec la déduplication des données ?

Oui, réplication DFS peut répliquer des dossiers sur des volumes qui utilisent la déduplication des données dans Windows Server.

### <a name="is-dfs-replication-compatible-with-ris-and-wds"></a>Réplication DFS est-il compatible avec RIS et WDS ?

Oui. Réplication DFS réplique les volumes sur lesquels SIS (Single instance Storage) est activé. SIS est utilisé par les services d’installation à distance (RIS), les services de déploiement Windows (WDS) et Windows Storage Server.

### <a name="is-it-possible-to-use-dfs-replication-with-offline-files"></a>Est-il possible d’utiliser des réplication DFS avec Fichiers hors connexion ?

Vous pouvez utiliser réplication DFS et Fichiers hors connexion en toute sécurité dans les scénarios où il n’y a qu’un seul utilisateur à la fois qui écrit dans les fichiers. Cela est utile pour les utilisateurs qui se déplacent entre deux filiales et veulent pouvoir accéder à leurs fichiers dans l’une ou l’autre branche ou en mode hors connexion. Fichiers hors connexion met en cache les fichiers localement pour une utilisation hors connexion et réplication DFS réplique les données entre les succursales.

N’utilisez pas réplication DFS avec Fichiers hors connexion dans un environnement multi-utilisateur, car réplication DFS ne fournit pas de mécanisme de verrouillage distribué ou de fonction d’extraction de fichier. Si deux utilisateurs modifient le même fichier en même temps sur des serveurs différents, réplication DFS déplace l’ancien fichier vers le\\dossier DfsrPrivate ConflictandDeleted (situé sous le chemin d’accès local du dossier répliqué) lors de la prochaine réplication.

### <a name="what-antivirus-applications-are-compatible-with-dfs-replication"></a>Quelles sont les applications antivirus compatibles avec réplication DFS ?

Les applications antivirus peuvent entraîner une réplication excessive si leurs activités d’analyse modifient les fichiers dans un dossier répliqué. Pour plus d’informations, [tester l’interopérabilité des applications antivirus avec réplication DFS](http://go.microsoft.com/fwlink/?linkid=73990) (http://go.microsoft.com/fwlink/?LinkId=73990).

### <a name="what-are-the-benefits-of-using-dfs-replication-instead-of-windows-sharepoint-services"></a>Quels sont les avantages de l’utilisation de réplication DFS au lieu de Windows SharePoint Services ?

Windows® SharePoint® Services fournit une cohérence étroite sous la forme d’une fonctionnalité d’extraction de fichiers que réplication DFS ne fait pas. Si vous craignez que plusieurs personnes modifient le même fichier, nous vous recommandons d’utiliser Windows SharePoint Services. Windows SharePoint Services 2,0 avec Service Pack 2 est disponible dans le cadre de Windows Server 2003 R2. Windows SharePoint Services peut être téléchargé à partir du site Web de Microsoft. Il n’est pas inclus dans les versions plus récentes de Windows Server. Toutefois, si vous répliquez des données sur plusieurs sites et que les utilisateurs ne modifieront pas les mêmes fichiers en même temps, réplication DFS offre une plus grande bande passante et une gestion plus simple.

## <a name="limitations-and-requirements"></a>Limitations et exigences

### <a name="can-dfs-replication-replicate-between-branch-offices-without-a-vpn-connection"></a>Réplication DFS peut-il être répliqué entre des succursales sans connexion VPN ?

Oui : en supposant qu’il existe une liaison de réseau étendu (WAN) privée (pas Internet) connectant les succursales. Toutefois, vous devez ouvrir les ports appropriés dans les pare-feu externes. Réplication DFS utilise le mappeur de point de terminaison RPC (port 135) et un port éphémère affecté de façon aléatoire au-dessus de 1024. Vous pouvez utiliser l’outil en ligne de commande **Dfsrdiag** pour spécifier un port statique au lieu du port éphémère. Pour plus d’informations sur la spécification du mappeur de point de terminaison RPC, voir l' [article 154596](http://go.microsoft.com/fwlink/?linkid=73991) de la base de connaissances Microsoft (http://go.microsoft.com/fwlink/?LinkId=73991).

### <a name="can-dfs-replication-replicate-files-encrypted-with-the-encrypting-file-system"></a>Peut-réplication DFS répliquer des fichiers chiffrés avec le système de fichiers EFS ?

Non. Réplication DFS ne réplique pas les fichiers ou dossiers qui sont chiffrés à l’aide du système de fichiers EFS (EFS). Si un utilisateur chiffre un fichier qui a été précédemment répliqué, réplication DFS supprime le fichier de tous les autres membres du groupe de réplication. Cela garantit que la seule copie disponible du fichier est la version chiffrée sur le serveur.

### <a name="can-dfs-replication-replicate-outlook-pst-or-microsoft-office-access-database-files"></a>Peut-réplication DFS répliquer des fichiers de base de données Outlook. pst ou Microsoft Office Access ?

Réplication DFS pouvez répliquer en toute sécurité des fichiers de dossiers personnels Microsoft Outlook (. pst) et des fichiers Microsoft Access uniquement s’ils sont stockés à des fins d’archivage et ne sont pas accessibles sur le réseau à l’aide d’un client tel qu’Outlook ou Access (pour ouvrir le fichier. pst ou accéder à fichiers, copiez d’abord les fichiers sur un dispositif de stockage local). Les raisons sont les suivantes :

  - L’ouverture de fichiers. pst sur des connexions réseau peut entraîner une altération des données dans les fichiers. pst. Pour plus d’informations sur la raison pour laquelle les fichiers. pst ne peuvent pas être accessibles en toute sécurité à partir d’un réseau, http://go.microsoft.com/fwlink/?LinkId=125363) consultez l' [article 297019](http://go.microsoft.com/fwlink/?linkid=125363) de la base de connaissances Microsoft (.  
      
  - les fichiers. pst et Access ont tendance à rester ouverts pendant de longues périodes de temps tout en étant accessibles par un client tel qu’Outlook ou Office Access. Cela empêche réplication DFS de répliquer ces fichiers tant qu’ils ne sont pas fermés.  
      

### <a name="can-i-use-dfs-replication-in-a-workgroup"></a>Puis-je utiliser réplication DFS dans un groupe de travail ?

Non. Réplication DFS s’appuie sur Active Directory® Services de domaine pour la configuration. Il ne fonctionne que dans un domaine.

### <a name="can-more-than-one-folder-be-replicated-on-a-single-server"></a>Plusieurs dossiers peuvent-ils être répliqués sur un seul serveur ?

Oui. Réplication DFS pouvez répliquer de nombreux dossiers entre les serveurs. Assurez-vous que chacun des dossiers répliqués a un chemin racine unique et qu’ils ne se chevauchent pas. Par exemple, d :\\sales et d :\\Accounting peuvent être les chemins d’accès racine de deux\\dossiers répliqués, mais les rapports d\\:\\sales et d : sales ne peuvent pas être les chemins d’accès racine de deux dossiers répliqués.

### <a name="does-dfs-replication-require-dfs-namespaces"></a>Réplication DFS nécessite-t-il des espaces de noms DFS ?

Non. Les espaces de noms DFS et réplication DFS peuvent être utilisés séparément ou ensemble. En outre, les réplication DFS peuvent être utilisés pour répliquer des espaces de noms DFS autonomes, ce qui n’était pas possible avec le service de réplication de fichiers.

### <a name="does-dfs-replication-require-time-synchronization-between-servers"></a>Réplication DFS est-il nécessaire de synchroniser l’heure entre les serveurs ?

Non. Réplication DFS n’exige pas explicitement la synchronisation de l’heure entre les serveurs. Toutefois, réplication DFS exige que les horloges du serveur correspondent étroitement. Les horloges du serveur doivent être définies dans les cinq minutes les unes des autres (par défaut) pour que l’authentification Kerberos fonctionne correctement. Par exemple, réplication DFS utilise des horodatages pour déterminer quel fichier est prioritaire en cas de conflit. Les heures précises sont également importantes pour les garbage collection, les planifications et d’autres fonctionnalités.

### <a name="does-dfs-replication-support-replicating-an-entire-volume"></a>Réplication DFS prend-il en charge la réplication d’un volume entier ?

Oui. Toutefois, vous devez d’abord installer Windows Server 2003 Service Pack 2 ou le correctif logiciel. Pour plus d’informations, voir l' [article 920335](http://go.microsoft.com/fwlink/?linkid=76776) de la base http://go.microsoft.com/fwlink/?LinkId=76776) de connaissances Microsoft (. En outre, la réplication d’un volume entier peut provoquer les problèmes suivants :

  - Si le volume contient un fichier de pagination Windows, la réplication échoue et enregistre l’événement DFSR 4312 dans le journal des événements système.  
      
  - Réplication DFS définit les attributs système et masqués sur le dossier répliqué sur le ou les serveurs de destination. Cela est dû au fait que Windows applique les attributs système et masqués au dossier racine du volume par défaut. Si le chemin d’accès local du dossier répliqué sur le ou les serveurs de destination est également une racine de volume, aucune autre modification n’est apportée aux attributs du dossier.  
      
  - Lors de la réplication d’un volume qui contient le dossier système Windows, réplication DFS reconnaît le dossier% WINDIR% et ne le réplique pas. Toutefois, réplication DFS réplique les dossiers utilisés par les applications non-Microsoft, ce qui peut entraîner l’échec des applications sur le ou les serveurs de destination si les applications présentent des problèmes d’interopérabilité avec réplication DFS.  
      

### <a name="does-dfs-replication-support-rpc-over-http"></a>Réplication DFS prend-il en charge RPC sur HTTP ?

Non.

### <a name="does-dfs-replication-work-across-wireless-networks"></a>Réplication DFS fonctionne-t-il sur des réseaux sans fil ?

Oui. Réplication DFS est indépendant du type de connexion.

### <a name="does-dfs-replication-work-on-refs-or-fat-volumes"></a>Ne fonctionne-t-il réplication DFS sur des volumes ReFS ou FAT ?

Non. Réplication DFS prend en charge les volumes formatés avec le système de fichiers NTFS uniquement ; le système de fichiers résilient (ReFS) et le système de fichiers FAT ne sont pas pris en charge. Réplication DFS nécessite NTFS, car il utilise le journal des modifications NTFS et d’autres fonctionnalités du système de fichiers NTFS.

### <a name="does-dfs-replication-work-with-sparse-files"></a>Ne fonctionne-t-il réplication DFS avec les fichiers partiellement alloués ?

Oui. Vous pouvez répliquer des fichiers partiellement alloués. L’attribut **Sparse** est conservé sur le membre de réception.

### <a name="do-i-need-to-log-in-as-administrator-to-replicate-files"></a>Dois-je me connecter en tant qu’administrateur pour répliquer les fichiers ?

Non. Réplication DFS est un service qui s’exécute sous le compte système local. vous n’avez donc pas besoin de vous connecter en tant qu’administrateur pour effectuer la réplication. Toutefois, vous devez être un administrateur de domaine ou un administrateur local des serveurs de fichiers concernés pour apporter des modifications à la configuration de réplication DFS.

Pour plus d’informations, consultez « réplication DFS les exigences et la délégation de sécurité » dans la rubrique délégation de http://go.microsoft.com/fwlink/?LinkId=182294) [la capacité à gérer des réplication DFS](http://go.microsoft.com/fwlink/?linkid=182294) (.

### <a name="how-can-i-upgrade-or-replace-a-dfs-replication-member"></a>Comment puis-je mettre à niveau ou remplacer un membre réplication DFS ?

Pour mettre à niveau ou remplacer un membre réplication DFS, consultez ce billet de blog sur le blog de l’équipe Ask the Directory Services : [Remplacement du matériel ou du système d’exploitation du membre DFSR](http://blogs.technet.com/b/askds/archive/2010/09/10/series-wrap-up-and-downloads-replacing-dfsr-member-hardware-or-os.aspx).

### <a name="is-dfs-replication-suitable-for-replicating-roaming-profiles"></a>Est-réplication DFS approprié pour la réplication des profils itinérants ?

Oui. Certains scénarios sont pris en charge lors de la réplication des profils utilisateur itinérants. Pour plus d’informations sur les scénarios pris en charge, consultez la [déclaration de support de Microsoft concernant les données de profil utilisateur répliquées](http://go.microsoft.com/fwlink/?linkid=201282) (http://go.microsoft.com/fwlink/?LinkId=201282).

### <a name="is-there-a-file-character-limit-or-limit-to-the-folder-depth"></a>Existe-t-il une limite de caractères ou une limite à la profondeur des dossiers ?

Windows et réplication DFS prennent en charge des chemins de dossiers contenant jusqu’à 32000 caractères. Réplication DFS n’est pas limité aux chemins de dossiers de 260 caractères.

### <a name="must-members-of-a-replication-group-reside-in-the-same-domain"></a>Les membres d’un groupe de réplication doivent-ils se trouver dans le même domaine ?

Non. Les groupes de réplication peuvent s’étendre sur plusieurs domaines au sein d’une même forêt, mais pas dans différentes forêts.

### <a name="what-are-the-supported-limits-of-dfs-replication"></a>Quelles sont les limites de réplication DFS prises en charge ?

La liste suivante fournit un ensemble d’instructions d’évolutivité qui ont été testées par Microsoft sur Windows Server 2012 R2 :

  - Taille de tous les fichiers répliqués sur un serveur : 100 téraoctets.  
      
  - Nombre de fichiers répliqués sur un volume : 70 millions.  
      
  - Taille de fichier maximale : 250 gigaoctets.  
      


> [!IMPORTANT]
> Lors de la création de groupes de réplication avec un nombre ou une taille de fichiers volumineux, nous vous recommandons d’exporter un clone de base de données et d’utiliser des techniques de pré-amorçage pour réduire la durée de la réplication initiale. Pour plus d’informations, <A href="http://blogs.technet.com/b/filecab/archive/2013/08/21/dfs-replication-initial-sync-in-windows-server-2012-r2-attack-of-the-clones.aspx">voir réplication DFS la synchronisation initiale dans Windows Server 2012 R2 : Attaque des clones</A>. 
<br>


La liste suivante fournit un ensemble d’instructions d’évolutivité qui ont été testées par Microsoft sur Windows Server 2012, Windows Server 2008 R2 et Windows Server 2008 :

  - Taille de tous les fichiers répliqués sur un serveur : 10 téraoctets.  
      
  - Nombre de fichiers répliqués sur un volume : 11 millions.  
      
  - Taille de fichier maximale : 64 gigaoctets.  
      


> [!NOTE]
> Il n’y a plus de limite au nombre de groupes de réplication, de dossiers répliqués, de connexions ou de membres du groupe de réplication. 
<br>


Pour obtenir la liste des recommandations d’évolutivité qui ont été testées par Microsoft pour Windows Server 2003 R2, consultez recommandations en matière http://go.microsoft.com/fwlink/?LinkId=75043) d' [évolutivité réplication DFS](http://go.microsoft.com/fwlink/?linkid=75043) (.

### <a name="when-should-i-not-use-dfs-replication"></a>Quand dois-je ne pas utiliser réplication DFS ?

N’utilisez pas réplication DFS dans un environnement où plusieurs utilisateurs mettent à jour ou modifient simultanément les mêmes fichiers sur des serveurs différents. Cela peut entraîner l’réplication DFS de déplacer des copies conflictuelles des fichiers vers le dossier masqué\\DfsrPrivate ConflictandDeleted.

Lorsque plusieurs utilisateurs doivent modifier les mêmes fichiers simultanément sur des serveurs différents, utilisez la fonctionnalité d’extraction de fichiers de Windows SharePoint Services pour vous assurer qu’un seul utilisateur travaille sur un fichier. Windows SharePoint Services 2,0 avec Service Pack 2 est disponible dans le cadre de Windows Server 2003 R2. Windows SharePoint Services peut être téléchargé à partir du site Web de Microsoft. Il n’est pas inclus dans les versions plus récentes de Windows Server.

### <a name="why-is-a-schema-update-required-for-dfs-replication"></a>Pourquoi une mise à jour de schéma est-elle nécessaire pour réplication DFS ?

Réplication DFS utilise de nouveaux objets dans le contexte de nommage de domaine de Active Directory Domain Services pour stocker les informations de configuration. Ces objets sont créés lorsque vous mettez à jour le schéma de Active Directory Domain Services. Pour plus d’informations, consultez [vérifier les conditions requises pour réplication DFS](http://go.microsoft.com/fwlink/?linkid=182264) (http://go.microsoft.com/fwlink/?LinkId=182264).

## <a name="monitoring-and-management-tools"></a>Outils de surveillance et de gestion

### <a name="can-i-automate-the-health-report-to-receive-warnings"></a>Puis-je automatiser le rapport d’intégrité pour recevoir des avertissements ?

Oui. Il existe trois façons d’automatiser les rapports d’intégrité :

  - Utilisez le module Windows PowerShell DFSR inclus dans Windows Server 2012 R2 ou DfsrAdmin. exe conjointement aux tâches planifiées pour générer régulièrement des rapports d’intégrité. Pour plus d’informations, consultez automatisation des rapports d’intégrité http://go.microsoft.com/fwlink/?LinkId=74010) des [réplication DFS](http://go.microsoft.com/fwlink/?linkid=74010) (.  
      
  - Utilisez le pack d’administration réplication DFS pour System Center Operations Manager pour créer des alertes basées sur des conditions spécifiées.  
      
  - Utilisez le fournisseur WMI réplication DFS pour générer des scripts d’alertes.  
      

### <a name="can-i-use-microsoft-system-center-operations-manager-to-monitor-dfs-replication"></a>Puis-je utiliser Microsoft System Center Operations Manager pour analyser réplication DFS ?

Oui. Pour plus d’informations, consultez le [Pack d’administration réplication DFS pour System Center Operations Manager 2007](http://go.microsoft.com/fwlink/?linkid=182265) dans le centre de http://go.microsoft.com/fwlink/?LinkId=182265) téléchargement Microsoft (.

### <a name="does-dfs-replication-support-remote-management"></a>Réplication DFS prend-il en charge la gestion à distance ?

Oui. Réplication DFS prend en charge la gestion à distance à l’aide de la console de gestion DFS et de la commande **Ajouter un groupe de réplication** . Par exemple, sur le serveur A, vous pouvez vous connecter à un groupe de réplication défini dans la forêt avec les serveurs A et B comme membres.

La gestion DFS est fournie avec Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008 et Windows Server 2003 R2. Pour gérer les réplication DFS à partir d’autres versions de Windows, utilisez Bureau à distance ou le [Outils d’administration de serveur distant pour Windows 7](https://technet.microsoft.com/library/Ee449475).


> [!IMPORTANT]
> Pour afficher ou gérer les groupes de réplication qui contiennent des dossiers répliqués en lecture seule ou des membres qui sont des clusters de basculement, vous devez utiliser la version de la gestion DFS qui est incluse dans Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, <a href="http://go.microsoft.com/fwlink/p/?linkid=238560">à distance. Outils d’administration de serveur pour Windows 8</a>ou <a href="https://technet.microsoft.com/library/ee449475">Outils d’administration de serveur distant pour Windows 7</a>. 
<br>


### <a name="do-ultrasound-and-sonar-work-with-dfs-replication"></a>Ultrasound et sonar fonctionnent-ils avec réplication DFS ?

Non. Réplication DFS dispose de son propre ensemble d’outils de surveillance et de diagnostic. Ultrasound et sonar sont uniquement en capacité à surveiller le service FRS.

### <a name="how-can-files-be-recovered-from-the-conflictanddeleted-or-preexisting-folders"></a>Comment les fichiers peuvent-ils être récupérés à partir des dossiers ConflictAndDeleted ou PreExisting ?

Pour récupérer les fichiers perdus, restaurez les fichiers à partir du dossier du système de fichiers ou du dossier partagé à l’aide de l’historique des fichiers, de la commande **restaurer les versions précédentes** dans l’Explorateur de fichiers, ou en restaurant les fichiers à partir de la sauvegarde. Pour récupérer des fichiers directement à partir du dossier ConflictAndDeleted ou PreExisting `Get-DfsrPreservedFiles` , `Restore-DfsrPreservedFiles` utilisez les applets de commande Windows PowerShell (incluses avec le module DFSR dans Windows Server 2012 R2) ou l’exemple de script [RestoreDFSR](http://code.msdn.microsoft.com/restoredfsr) à partir de MSDN Galerie de code. Ce script est destiné uniquement à la récupération d’urgence et est fourni tel quel, sans garantie.

### <a name="is-there-a-way-to-know-the-state-of-replication"></a>Existe-t-il un moyen de connaître l’état de la réplication ?

Oui. Il existe plusieurs façons de surveiller la réplication :

  - Réplication DFS dispose d’un pack d’administration pour System Center Operations Manager qui assure une surveillance proactive.  
      
  - La gestion DFS dispose d’un rapport de diagnostic intégré pour le backlog de réplication, l’efficacité de la réplication et le nombre de fichiers et de dossiers dans un groupe de réplication donné.  
      
  - Le module Windows PowerShell DFSR dans Windows Server 2012 R2 contient des applets de commande pour démarrer les tests de propagation et écrire des rapports d’intégrité et de propagation. Pour plus d’informations, consultez [système de fichiers DFS des applets de commande de réplication dans Windows PowerShell](https://technet.microsoft.com/library/dn296601.aspx).  
      
  - Dfsrdiag. exe est un outil en ligne de commande qui peut générer un nombre de backlog ou déclencher un test de propagation. Les deux affichent l’état de réplication. La propagation vous indique si des fichiers sont en cours de réplication sur tous les nœuds. Backlog vous montre le nombre de fichiers qui doivent encore être répliqués avant que deux ordinateurs soient synchronisés. Le nombre de backlog est le nombre de mises à jour qu’un membre du groupe de réplication n’a pas traitées. Sur les ordinateurs exécutant Windows Server 2012 R2, Windows Server 2012 ou Windows Server 2008 R2, Dfsrdiag. exe peut également afficher les mises à jour que réplication DFS est en cours de réplication.  
      
  - Les scripts peuvent utiliser WMI pour collecter des informations sur les journaux de travaux en souffrance, manuellement ou via MOM.  
      

## <a name="performance"></a>Performances

### <a name="does-dfs-replication-support-dial-up-connections"></a>Réplication DFS prend-il en charge les connexions d’accès à distance ?

Bien que les réplication DFS fonctionnent à des vitesses d’accès à distance, elles peuvent être retardées s’il y a un grand nombre de modifications à répliquer. Si des modifications mineures sont apportées à des fichiers existants, réplication DFS avec la compression différentielle à distance (RDC) fournira des performances nettement supérieures à la copie directe du fichier.

### <a name="does-dfs-replication-perform-bandwidth-sensing"></a>Réplication DFS effectue-t-il la détection de bande passante ?

Non. Réplication DFS n’effectue pas de détection de bande passante. Vous pouvez configurer réplication DFS pour utiliser une quantité limitée de bande passante en fonction de la connexion (limitation de bande passante). Toutefois, réplication DFS ne réduit pas encore davantage l’utilisation de la bande passante si l’interface réseau est saturée, et réplication DFS peut saturer le lien pendant de courtes périodes. La limitation de la bande passante avec réplication DFS n’est pas complètement exacte, car réplication DFS limite la bande passante en limitant les appels RPC. Par conséquent, plusieurs mémoires tampons de niveaux inférieurs de la pile réseau (y compris RPC) peuvent interférer, provoquant des rafales de trafic réseau.

### <a name="does-dfs-replication-throttle-bandwidth-per-schedule-per-server-or-per-connection"></a>Réplication DFS limiter la bande passante par planification, par serveur ou par connexion ?

Si vous configurez la limitation de la bande passante lors de la spécification de la planification, toutes les connexions de ce groupe de réplication utiliseront ce paramètre pour la limitation de la bande passante. La limitation de bande passante peut également être définie en tant que paramètre au niveau de la connexion à l’aide de la gestion DFS.

### <a name="does-dfs-replication-use-active-directory-domain-services-to-calculate-site-links-and-connection-costs"></a>N’utilise-t-il réplication DFS Active Directory Domain Services pour calculer les liens de sites et les coûts de connexion ?

Non. Réplication DFS utilise la topologie définie par l’administrateur, qui est indépendante du coût de Active Directory Domain Services site.

### <a name="how-can-i-improve-replication-performance"></a>Comment améliorer les performances de la réplication ?

Pour en savoir plus sur les différentes méthodes de réglage des performances de réplication, consultez [réglage des performances de la réplication dans DFSR](http://blogs.technet.com/b/askds/archive/2010/03/31/tuning-replication-performance-in-dfsr-especially-on-win2008-r2.aspx) sur le blog de l' [équipe poser des services d’annuaire](http://blogs.technet.com/b/askds/).

### <a name="how-does-dfs-replication-avoid-saturating-a-connection"></a>Comment réplication DFS éviter de saturer une connexion ?

Dans réplication DFS vous définissez la bande passante maximale que vous souhaitez utiliser sur une connexion, et le service maintient ce niveau d’utilisation du réseau. Cela diffère de la Service de transfert intelligent en arrière-plan (BITS) et réplication DFS ne sature pas la connexion si vous la définissez de manière appropriée.

Néanmoins, la limitation de bande passante n’est pas de 100% précise et réplication DFS peut saturer le lien pendant de courtes périodes de temps. Cela est dû au fait que réplication DFS limite la bande passante en limitant les appels RPC. Étant donné que ce processus repose sur plusieurs mémoires tampons à des niveaux inférieurs de la pile réseau, y compris RPC, le trafic de réplication tend à voyager en rafales, ce qui peut parfois saturer les liaisons réseau.

Réplication DFS de Windows Server 2008 comprend plusieurs améliorations des performances, comme indiqué dans [système de fichiers DFS](https://technet.microsoft.com/library/Cc753479), une rubrique relative aux [modifications apportées aux fonctionnalités de Windows Server 2003 avec SP1 à Windows Server 2008](https://technet.microsoft.com/library/cc753208).

### <a name="how-does-dfs-replication-performance-compare-with-frs"></a>Comment réplication DFS les performances sont-elles comparées avec FRS ?

Réplication DFS est beaucoup plus rapide que le service FRS, en particulier lorsque des modifications mineures sont apportées à des fichiers volumineux et que la compression RDC est activée. Par exemple, avec RDC, une petite modification apportée à une présentation PowerPoint® de 2 Mo peut entraîner l’envoi d’un maximum de 60 kilo-octets (Ko) sur le réseau, soit une économie de 97% en octets transférés.

RDC n’est pas utilisé sur les fichiers inférieurs à 64 Ko et peut ne pas être utile sur les réseaux locaux à haut débit où la bande passante réseau n’est pas utilisée. RDC peut être désactivée en fonction de la connexion à l’aide de la gestion DFS.

### <a name="how-frequently-does-dfs-replication-replicate-data"></a>À quelle fréquence réplication DFS répliquer des données ?

Les données sont répliquées en fonction de la planification que vous définissez. Par exemple, vous pouvez définir la planification sur des intervalles de 15 minutes, sept jours par semaine. Pendant ces intervalles, la réplication est activée. La réplication démarre peu après la détection d’une modification de fichier (généralement en secondes).

La planification du groupe de réplication peut être définie sur l’heure UTC (Universal Time Coordinate) tandis que la planification de la connexion est définie sur l’heure locale du membre de réception. Prenez cela en compte lorsque le groupe de réplication s’étend sur plusieurs fuseaux horaires. L’heure locale correspond à l’heure du membre qui héberge la connexion entrante. La planification affichée de la connexion entrante et de la connexion sortante correspondante reflète les différences de fuseau horaire lorsque la planification est définie sur heure locale.

### <a name="how-much-of-my-servers-system-resources-will-dfs-replication-consume"></a>Quelle quantité de ressources système de mon serveur utilisera-t-il réplication DFS ?

Les ressources disque, mémoire et processeur utilisées par réplication DFS dépendent d’un certain nombre de facteurs, notamment le nombre et la taille des fichiers, le taux de modification, le nombre de membres du groupe de réplication et le nombre de dossiers répliqués. En outre, certaines ressources sont plus difficiles à estimer. Par exemple, la technologie ESE (Extensible Storage Engine) utilisée pour la base de données réplication DFS peut consommer un grand pourcentage de mémoire disponible, qu’elle libère à la demande. Les applications autres que réplication DFS peuvent être hébergées sur le même serveur en fonction de la configuration du serveur. Toutefois, lors de l’hébergement de plusieurs applications ou rôles de serveur sur un serveur unique, il est important que vous testiez cette configuration avant de l’implémenter dans un environnement de production.

### <a name="what-happens-if-a-wan-link-fails-during-replication"></a>Que se passe-t-il si une liaison WAN échoue lors de la réplication ?

Si la connexion est interrompue, réplication DFS tentera d’effectuer une réplication pendant que la planification est ouverte. Il y aura également des erreurs de connectivité signalées dans le journal des événements réplication DFS qui peuvent être collectées à l’aide de MOM (de manière proactive via des alertes) et le rapport d’intégrité des réplication DFS (de manière réactive, par exemple lorsqu’un administrateur l’exécute).

## <a name="remote-differential-compression-details"></a>Détails de la compression différentielle à distance

### <a name="are-changes-compressed-before-being-replicated"></a>Les modifications sont-elles compressées avant d’être répliquées ?

Oui. Les portions de fichiers modifiées sont compressées avant d’être envoyées pour tous les types de fichiers, à l’exception des suivants (qui sont déjà compressés) :. WMA,. wmv,. zip,. jpg,. mpg,. MPEG,. M1V,. MP2,. mp3,. MPa,. cab,. wav,. snd,. au,. ASF,. WM,. avi,. z,. gz,. tgz et. FRx. Les paramètres de compression pour ces types de fichiers ne sont pas configurables dans Windows Server 2003 R2.

### <a name="can-an-administrator-turn-off-rdc-or-change-the-threshold"></a>Un administrateur peut-il désactiver RDC ou modifier le seuil ?

Oui. Vous pouvez désactiver RDC via la page de propriétés d’une connexion donnée. La désactivation de RDC peut réduire l’utilisation de l’UC et la latence de réplication sur les liens de réseau local (LAN) rapides qui n’ont pas de contraintes de bande passante ou pour les groupes de réplication qui se composent principalement de fichiers inférieurs à 64 Ko. Si vous choisissez de désactiver RDC sur une connexion, testez l’efficacité de réplication avant et après la modification pour vérifier que vous avez amélioré les performances de réplication.

Vous pouvez modifier le seuil de taille de RDC à l’aide de la commande **Dfsradmin Connection Set** , du fournisseur WMI réplication DFS ou en modifiant manuellement le fichier XML de configuration.

### <a name="does-rdc-work-on-all-file-types"></a>RDC fonctionne-t-il sur tous les types de fichiers ?

Oui. RDC calcule les différences au niveau du bloc, quel que soit le type de données de fichier. Toutefois, RDC fonctionne plus efficacement sur certains types de fichiers tels que les documents Word, les fichiers PST et les images VHD.

### <a name="how-does-rdc-work-on-a-compressed-file"></a>Comment RDC fonctionne-t-il sur un fichier compressé ?

Réplication DFS utilise RDC, qui calcule les blocs du fichier qui ont changé et envoie uniquement ces blocs sur le réseau. Réplication DFS n’a pas besoin de connaître le contenu du fichier, mais uniquement les blocs qui ont changé.

### <a name="is-cross-file-rdc-enabled-when-upgrading-to-windows-server-enterprise-edition-or-datacenter-edition"></a>La connexion inter-fichiers est-elle activée lors de la mise à niveau vers Windows Server Enterprise Edition ou Datacenter Edition ?

Les éditions standard de Windows Server ne prennent pas en charge la RDC inter-fichiers. Toutefois, il est activé automatiquement lorsque vous effectuez une mise à niveau vers une édition qui prend en charge la gestion de RDC inter-fichiers, ou si un membre de la connexion de réplication exécute une édition prise en charge. Pour obtenir la liste des éditions qui prennent en charge la RDC inter-fichiers, consultez Quelles éditions du système d’exploitation Windows prennent en charge la RDC inter-fichiers ?

### <a name="is-rdc-true-block-level-replication"></a>La réplication au niveau du bloc RDC est-elle vraie ?

Non. RDC est un protocole à usage général permettant de compresser le transfert de fichiers. Réplication DFS utilise RDC sur les blocs au niveau du fichier, et non au niveau du bloc de disque. RDC divise un fichier en blocs. Pour chaque bloc dans un fichier, elle calcule une signature, qui est un petit nombre d’octets pouvant représenter le bloc plus grand. L’ensemble des signatures est transféré du serveur au client. Le client compare les signatures de serveur à ses propres signatures. Le client demande ensuite au serveur d’envoyer uniquement les données pour les signatures qui ne sont pas déjà sur le client.

### <a name="what-happens-if-i-rename-a-file"></a>Que se passe-t-il si je renomme un fichier ?

Réplication DFS renomme le fichier sur tous les autres membres du groupe de réplication lors de la réplication suivante. Les fichiers étant suivis à l’aide d’un ID unique, le fait de renommer un fichier et de déplacer le fichier au sein du réplica n’a aucun effet sur la capacité de réplication DFS à répliquer un fichier.

### <a name="what-is-cross-file-rdc"></a>Qu’est-ce que la RDC inter-fichiers ?

La fonctionnalité RDC entre fichiers permet réplication DFS d’utiliser la connexion RDC même si un fichier portant le même nom n’existe pas à la fin du client. Le processus RDC entre fichiers utilise une méthode heuristique pour déterminer les fichiers qui sont similaires au fichier qui doit être répliqué et utilise des blocs des fichiers similaires identiques au fichier de réplication pour réduire la quantité de données transférées sur le réseau étendu. La RDC inter-fichiers peut utiliser des blocs de cinq fichiers similaires au maximum dans ce processus.

Pour utiliser la connexion inter-fichiers (RDC), un membre de la connexion de réplication doit exécuter une édition de Windows qui prend en charge RDC inter-fichiers. Pour obtenir la liste des éditions qui prennent en charge la RDC inter-fichiers, consultez Quelles éditions du système d’exploitation Windows prennent en charge la RDC inter-fichiers ?

### <a name="what-is-rdc"></a>Qu’est-ce que RDC ?

La compression différentielle à distance (RDC) est un protocole client-serveur qui peut être utilisé pour mettre à jour efficacement des fichiers sur un réseau à bande passante limitée. RDC détecte les insertions, les suppressions et les réorganisations des données dans les fichiers, ce qui permet à réplication DFS de répliquer uniquement les modifications lors de la mise à jour des fichiers. La compression RDC est utilisée uniquement pour les fichiers de 64 Ko ou plus par défaut. RDC peut utiliser une version antérieure d’un fichier portant le même nom dans le dossier répliqué ou dans le dossier DfsrPrivate\\ConflictandDeleted (situé sous le chemin d’accès local du dossier répliqué).

### <a name="when-is-rdc-used-for-replication"></a>Quand RDC est-il utilisé pour la réplication ?

RDC est utilisé lorsque le fichier dépasse un seuil de taille minimale. Ce seuil de taille est de 64 Ko par défaut. Une fois qu’un fichier ayant dépassé ce seuil a été répliqué, les versions mises à jour du fichier utilisent toujours RDC, sauf si une grande partie du fichier est modifiée ou si RDC est désactivé.

### <a name="which-editions-of-the-windows-operating-system-support-cross-file-rdc"></a>Quelles éditions du système d’exploitation Windows prennent en charge RDC inter-fichiers ?

Pour utiliser la connexion inter-fichiers (RDC), un membre de la connexion de réplication doit exécuter une édition du système d’exploitation Windows qui prend en charge la connexion inter-fichiers RDC. Le tableau suivant répertorie les éditions du système d’exploitation Windows qui prennent en charge la RDC inter-fichiers.

### <a name="cross-file-rdc-availability-in-editions-of-the-windows-operating-system"></a>Disponibilité des RDC inter-fichiers dans les éditions du système d’exploitation Windows

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
<td><p>Windows Server 2012 R2</p></td>
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

\*Vous pouvez éventuellement désactiver la connexion RDC inter-fichiers sur Windows Server 2012 R2.

## <a name="replication-details"></a>Détails de la réplication

### <a name="can-i-change-the-path-for-a-replicated-folder-after-it-is-created"></a>Puis-je modifier le chemin d’accès d’un dossier répliqué après sa création ?

Non. Si vous avez besoin de modifier le chemin d’accès d’un dossier répliqué, vous devez le supprimer dans gestion du système de fichiers distribués DFS et le rajouter en tant que nouveau dossier répliqué. Réplication DFS utilise ensuite la compression différentielle à distance (RDC) pour effectuer une synchronisation qui détermine si les données sont identiques sur les membres d’envoi et de réception. Elle ne réplique pas à nouveau toutes les données du dossier.

### <a name="can-i-configure-which-file-attributes-are-replicated"></a>Puis-je configurer les attributs de fichier à répliquer ?

Non, vous ne pouvez pas configurer les attributs de fichier que réplication DFS réplique.

Pour obtenir la liste des valeurs d’attribut et leurs descriptions, consultez [attributs](http://go.microsoft.com/fwlink/?linkid=182268) de fichier http://go.microsoft.com/fwlink/?LinkId=182268) sur MSDN (.

Les valeurs d’attribut suivantes sont définies à l' `SetFileAttributes dwFileAttributes` aide de la fonction, et elles sont répliquées par réplication DFS. Les modifications apportées à ces valeurs d’attribut déclenchent la réplication des attributs. Le contenu du fichier n’est pas répliqué à moins que le contenu ne change également. Pour plus d’informations, consultez [fonction SetFileAttributes](http://go.microsoft.com/fwlink/?linkid=182269) dans la bibliothèque MSDN http://go.microsoft.com/fwlink/?LinkId=182269) (.

  - \_ATTRIBUT\_DE FICHIER MASQUÉ  
      
  - \_ATTRIBUT\_DE FICHIER READONLY  
      
  - SYSTÈME\_D'\_ATTRIBUTS DE FICHIER  
      
  - ATTRIBUT\_DE\_FICHIERNON\_CONTENUINDEXÉ\_  
      
  - \_ATTRIBUT\_DE FICHIER HORS CONNEXION  
      

Les valeurs d’attribut suivantes sont répliquées par réplication DFS, mais elles ne déclenchent pas de réplication.

  - ARCHIVE\_D'\_ATTRIBUT DE FICHIER  
      
  - \_ATTRIBUT\_DE FICHIER NORMAL  
      

Les valeurs d’attribut de fichier suivantes déclenchent également la réplication, bien qu’elles `SetFileAttributes` ne puissent pas être `GetFileAttributes` définies à l’aide de la fonction (utilisez la fonction pour afficher les valeurs d’attribut).

  - POINT\_D'\_ANALYSE\_D’ATTRIBUT DE FICHIER  
      

> [!NOTE]
> Réplication DFS ne réplique pas les valeurs d’attribut de point d’analyse, sauf si la balise d’analyse est IO_REPARSE_TAG_SYMLINK. Les fichiers avec les balises d’analyse IO_REPARSE_TAG_DEDUP, IO_REPARSE_TAG_SIS ou IO_REPARSE_TAG_HSM sont répliqués en tant que fichiers normaux. Toutefois, la balise d’analyse et les tampons de données de nouvelle analyse ne sont pas répliquées vers d’autres serveurs, car le point d’analyse fonctionne uniquement sur le système local. 
<br>

  - \_ATTRIBUT\_DE FICHIER COMPRESSÉ  
      
  - \_ATTRIBUT\_DE FICHIER CHIFFRÉ  
      

> [!NOTE]
> Réplication DFS ne réplique pas les fichiers qui sont chiffrés à l’aide du système de fichiers EFS (EFS). Réplication DFS réplique les fichiers qui sont chiffrés à l’aide de logiciels non-Microsoft, mais uniquement s’ils ne définissent pas la valeur de l’attribut FILE_ATTRIBUTE_ENCRYPTED sur le fichier. 
<br>

  - FICHIER\_PARTIELLEMENT\_ALLOUÉ\_AUX ATTRIBUTS DE FICHIER  
      
  - RÉPERTOIRE\_DES\_ATTRIBUTS DE FICHIER  
      

Réplication DFS ne réplique pas la valeur\_temporaire\_d’attribut de fichier.

### <a name="can-i-control-which-member-is-replicated"></a>Puis-je contrôler le membre à répliquer ?

Oui. Vous pouvez choisir une topologie lorsque vous créez un groupe de réplication. Ou vous pouvez sélectionner **aucune topologie** et configurer manuellement les connexions après la création du groupe de réplication.

### <a name="can-i-seed-a-replication-group-member-with-data-prior-to-the-initial-replication"></a>Puis-je amorcer un membre du groupe de réplication avec des données antérieures à la réplication initiale ?

Oui. Réplication DFS prend en charge la copie des fichiers vers un membre du groupe de réplication avant la réplication initiale. Cette « prédéfinition » peut réduire considérablement la quantité de données répliquées pendant la réplication initiale.

La réplication initiale n’a pas besoin de répliquer le contenu lorsque les fichiers diffèrent uniquement par des attributs réels ou des horodatages. Un attribut réel est un attribut qui peut être défini par la fonction `SetFileAttributes`Win32. Pour plus d’informations, consultez [fonction SetFileAttributes](http://go.microsoft.com/fwlink/?linkid=182269) dans la bibliothèque MSDN http://go.microsoft.com/fwlink/?LinkId=182269) (. Si deux fichiers diffèrent par d’autres attributs, tels que la compression, le contenu du fichier est répliqué.

Pour prédéfinir un membre du groupe de réplication, copiez les fichiers dans le dossier approprié sur le ou les serveurs de destination, créez le groupe de réplication, puis choisissez un membre principal. Choisissez le membre qui contient les fichiers les plus récents que vous souhaitez répliquer parce que le contenu du membre principal est considéré comme « faisant autorité ». Cela signifie que lors de la réplication initiale, les fichiers du membre principal remplaceront toujours les autres versions des fichiers sur les autres membres du groupe de réplication.

Pour plus d’informations sur la pré-amorçage et le clonage de la [base de données DFSR, voir réplication DFS la synchronisation initiale dans Windows Server 2012 R2 : Attaque des clones](http://blogs.technet.com/b/filecab/archive/2013/08/21/dfs-replication-initial-sync-in-windows-server-2012-r2-attack-of-the-clones.aspx).

Pour plus d’informations sur la réplication initiale, voir [créer un groupe de réplication](https://technet.microsoft.com/library/cc725893).

### <a name="does-dfs-replication-overcome-common-file-replication-service-issues"></a>Réplication DFS-il de surmonter les problèmes courants liés au service de réplication de fichiers ?

Oui. Réplication DFS configure trois problèmes courants liés à FRS :

  - Le journal est renvoyé à la ligne : Réplication DFS récupère à la volée les retours à la ligne du journal. Chaque fichier ou dossier existant est marqué comme journalWrap et vérifié par rapport au système de fichiers avant que la réplication soit réactivée. Pendant la récupération, ce volume n’est pas disponible pour la réplication dans les deux sens.  
      
  - Réplication excessive : Pour éviter une réplication excessive, réplication DFS utilise un système de crédits.  
      
  - Dossiers inter-courbes : Pour empêcher les noms de dossier morphed, réplication DFS stocke les données en conflit\\dans un dossier DfsrPrivate ConflictandDeleted masqué (situé sous le chemin d’accès local du dossier répliqué). Par exemple, la création simultanée de plusieurs dossiers avec des noms identiques sur des serveurs différents répliqués à l’aide du service FRS fait que le service FRS renomme le ou les dossiers les plus anciens. Réplication DFS déplace plutôt le ou les dossiers les plus anciens vers le dossier local en conflit et supprimé.  
      

### <a name="does-dfs-replication-replicate-files-in-chronological-order"></a>Réplication DFS répliquer les fichiers dans l’ordre chronologique ?

Non. Les fichiers peuvent être répliqués dans le désordre.

### <a name="does-dfs-replication-replicate-files-that-are-being-used-by-another-application"></a>Réplication DFS répliquer les fichiers utilisés par une autre application ?

Si une application ouvre un fichier et crée un verrou de fichier sur celui-ci (en l’empêchant d’être utilisé par d’autres applications pendant qu’il est ouvert), réplication DFS ne réplique pas le fichier tant qu’il n’est pas fermé. Si l’application ouvre le fichier avec l’accès en lecture-partage, le fichier peut toujours être répliqué.

### <a name="does-dfs-replication-replicate-ntfs-file-permissions-alternate-data-streams-hard-links-and-reparse-points"></a>Réplication DFS répliquer les autorisations de fichiers NTFS, les flux de données de remplacement, les liens physiques et les points d’analyse ?

  - Réplication DFS réplique les autorisations de fichiers NTFS et d’autres flux de données.  
      
  - Microsoft ne prend pas en charge la création de liens physiques NTFS vers ou à partir de fichiers dans un dossier répliqué. cela peut entraîner des problèmes de réplication avec les fichiers affectés. Les fichiers de liens physiques sont ignorés par réplication DFS et ne sont pas répliqués. Les points de jonction ne sont pas non plus répliqués, et réplication DFS enregistre l’événement 4406 pour chaque point de jonction qu’il rencontre.  
      
  - Les seuls points d’analyse répliqués par réplication DFS sont ceux qui utilisent la balise lien\_symbolique\_d’une balise d’analyse d’e/s\_; Toutefois, réplication DFS ne garantit pas que la cible d’un lien symbolique est également répliquée. Pour plus d’informations, consultez le [blog Ask the Directory Services Team blog](http://blogs.technet.com/b/askds/archive/2011/09/30/friday-mail-sack-super-slo-mo-edition.aspx).  
      
  - Les fichiers avec la\_déduplication\_de la balise\_\_d’analyse des\_e/s, la balise\_d’extraction d’e\_/s ou la\_balise d’analyse e/s\_sont répliquées en tant que fichiers normaux. La balise d’analyse et les tampons de données de nouvelle analyse ne sont pas répliquées vers d’autres serveurs, car le point d’analyse fonctionne uniquement sur le système local. Par conséquent, réplication DFS pouvez répliquer des dossiers sur des volumes qui utilisent la déduplication des données dans Windows Server 2012 ou SIS (Single instance Storage). Toutefois, les informations de la déduplication des données sont conservées séparément par chaque serveur sur lequel le service de rôle est activé.  
      

### <a name="does-dfs-replication-replicate-timestamp-changes-if-no-other-changes-are-made-to-the-file"></a>Réplication DFS répliquer les modifications d’horodatage si aucune autre modification n’est apportée au fichier ?

Non, réplication DFS ne réplique pas les fichiers pour lesquels la seule modification est une modification de l’horodateur. En outre, l’horodateur modifié n’est pas répliqué vers d’autres membres du groupe de réplication, sauf si d’autres modifications sont apportées au fichier.

### <a name="does-dfs-replication-replicate-updated-permissions-on-a-file-or-folder"></a>Réplication DFS répliquer les autorisations mises à jour sur un fichier ou un dossier ?

Oui. Réplication DFS réplique les modifications des autorisations pour les fichiers et les dossiers. Seule la partie du fichier associée à la liste d’Access Control (ACL) est répliquée, bien que réplication DFS doive toujours lire le fichier entier dans la zone de transit.


> [!NOTE]
> La modification des listes de contrôle d’accès sur un grand nombre de fichiers peut avoir un impact sur les performances de réplication. Toutefois, lors de l’utilisation de RDC, la quantité de données transférées est proportionnelle à la taille des listes de contrôle d’accès, et non à la taille du fichier entier. Le volume de trafic disque est toujours proportionnel à la taille des fichiers, car ceux-ci doivent être lus vers et depuis le dossier intermédiaire. 
<br>


### <a name="does-dfs-replication-support-merging-text-files-in-the-event-of-a-conflict"></a>Ne prend-réplication DFS en charge la fusion des fichiers texte en cas de conflit ?

Réplication DFS ne fusionne pas les fichiers en cas de conflit. Toutefois, il tente de conserver l’ancienne version du fichier dans le dossier DfsrPrivate\\masqué de l’ordinateur sur lequel le conflit a été détecté.

### <a name="does-dfs-replication-use-encryption-when-transmitting-data"></a>N’utilise-t-il réplication DFS le chiffrement lors de la transmission des données ?

Oui. Réplication DFS utilise des connexions d’appel de procédure distante (RPC) avec chiffrement.

### <a name="is-it-possible-to-disable-the-use-of-encrypted-rpc"></a>Est-il possible de désactiver l’utilisation d’un RPC chiffré ?

Non. Le service réplication DFS utilise des appels de procédure distante (RPC) sur TCP pour répliquer les données. Pour sécuriser les transferts de données sur Internet, le service réplication DFS est conçu pour toujours utiliser la constante `RPC_C_AUTHN_LEVEL_PKT_PRIVACY`de niveau authentification. Cela permet de s’assurer que la communication RPC sur Internet est toujours chiffrée. Par conséquent, il n’est pas possible de désactiver l’utilisation de RPC chiffré par le service réplication DFS.

Pour plus d’informations, consultez les sites Web Microsoft suivants :

  - [Référence technique RPC](http://go.microsoft.com/fwlink/?linkid=182278)  
      
  - [À propos de la compression différentielle à distance](http://go.microsoft.com/fwlink/?linkid=182279)  
      
  - [Constantes au niveau de l’authentification](http://go.microsoft.com/fwlink/?linkid=182280)  
      

### <a name="how-are-simultaneous-replications-handled"></a>Comment les réplications simultanées sont-elles gérées ?

Il existe un gestionnaire de mise à jour par dossier répliqué. Les gestionnaires de mise à jour fonctionnent indépendamment l’un de l’autre.

Par défaut, un maximum de 16 (quatre dans Windows Server 2003 R2) téléchargements simultanés est partagé entre toutes les connexions et tous les groupes de réplication. Étant donné que les connexions et les mises à jour du groupe de réplication ne sont pas sérialisées, il n’existe aucun ordre spécifique dans lequel les mises à jour sont reçues. Si deux planifications sont ouvertes, les mises à jour sont généralement reçues et installées à partir des deux connexions en même temps.

### <a name="how-do-i-force-replication-or-polling"></a>Comment faire forcer la réplication ou l’interrogation ?

Vous pouvez forcer la réplication immédiatement à l’aide de la gestion DFS, comme décrit dans [modifier les planifications de réplication](https://technet.microsoft.com/library/Cc732278). Vous pouvez également forcer la réplication à `Sync-DfsReplicationGroup` l’aide de la cmdlet, incluse dans le module PowerShell DFSR introduit avec Windows Server 2012 R2, ou la commande **Dfsrdiag SyncNow** . Vous pouvez forcer l’interrogation à l' `Update-DfsrConfigurationFromAD` aide de l’applet de commande ou de la commande **Dfsrdiag pollad** .

### <a name="is-it-possible-to-configure-a-quiet-time-between-replications-for-files-that-change-frequently"></a>Est-il possible de configurer une période de silence entre les réplications pour les fichiers qui changent fréquemment ?

Non. Si la planification est ouverte, réplication DFS répliquera les modifications au fur et à mesure de leur notification. Il n’existe aucun moyen de configurer une période de silence pour les fichiers.

### <a name="is-it-possible-to-configure-one-way-replication-with-dfs-replication"></a>Est-il possible de configurer la réplication unidirectionnelle avec réplication DFS ?

Oui. Si vous utilisez Windows Server 2012 ou Windows Server 2008 R2, vous pouvez créer un dossier répliqué en lecture seule qui réplique le contenu via une connexion unidirectionnelle. Pour plus d’informations, consultez [créer un dossier répliqué en lecture seule sur un membre particulier](http://go.microsoft.com/fwlink/?linkid=156740) (http://go.microsoft.com/fwlink/?LinkId=156740).

Nous ne prenons pas en charge la création d’une connexion de réplication unidirectionnelle avec réplication DFS dans Windows Server 2008 ou Windows Server 2003 R2. Cela peut entraîner de nombreux problèmes, notamment des erreurs de topologie de contrôle d’intégrité, des problèmes de préproduction et des problèmes liés à la base de données réplication DFS.

Si vous utilisez Windows Server 2008 ou Windows Server 2003 R2, vous pouvez simuler une connexion unidirectionnelle en effectuant les actions suivantes :

  - Former les administrateurs à apporter des modifications uniquement sur le ou les serveurs que vous souhaitez désigner comme serveurs principaux. Ensuite, laissez les modifications se répliquer sur les serveurs de destination.  
      
  - Configurez les autorisations de partage sur les serveurs de destination afin que les utilisateurs finaux ne disposent pas d’autorisations en écriture. Si aucune modification n’est autorisée sur les serveurs de succursale, il n’y a rien à répliquer, en simulant une connexion unidirectionnelle et en réduisant l’utilisation du réseau étendu.  
      

### <a name="is-there-a-way-to-force-a-complete-replication-of-all-files-including-unchanged-files"></a>Existe-t-il un moyen de forcer une réplication complète de tous les fichiers, y compris les fichiers non modifiés ?

Non. Si réplication DFS considère que les fichiers sont identiques, il ne les réplique pas. Si les fichiers modifiés n’ont pas été répliqués, réplication DFS les réplique automatiquement lorsqu’ils sont configurés pour le faire. Pour remplacer la planification configurée, utilisez la méthode WMI **ForceReplicate ()** . Toutefois, il s’agit uniquement d’une substitution de planification, qui ne force pas la réplication de fichiers identiques ou inchangés.

### <a name="what-happens-if-the-primary-member-suffers-a-database-loss-during-initial-replication"></a>Que se passe-t-il si le membre principal subit une perte de base de données lors de la réplication initiale ?

Pendant la réplication initiale, les fichiers du membre principal ont toujours la priorité dans la résolution des conflits qui se produit si les membres de réception ont des versions de fichiers différentes sur le membre principal. La désignation du membre principal est stockée dans Active Directory Domain Services, et la désignation est effacée une fois que le membre principal est prêt à être répliqué, mais avant la réplication de tous les membres du groupe de réplication.

Si la réplication initiale échoue ou si le service réplication DFS redémarre pendant la réplication, le membre principal voit la désignation du membre principal dans la base de données réplication DFS locale et retente la réplication initiale. Si la base de données réplication DFS du membre principal est perdue après l’effacement de la désignation principale dans Active Directory Domain Services, mais avant que tous les membres du groupe de réplication ne terminent la réplication initiale, tous les membres du groupe de réplication ne parviennent pas à Répliquez le dossier, car aucun serveur n’est désigné comme membre principal. Dans ce cas, utilisez la commande **Dfsradmin Membership/Set/IsPrimary : true** sur le serveur membre principal pour restaurer manuellement la désignation du membre principal.

Pour plus d’informations sur la réplication initiale, voir [créer un groupe de réplication](https://technet.microsoft.com/library/cc725893).


> [!WARNING]
> La désignation du membre principal est utilisée uniquement pendant le processus de réplication initiale. Si vous utilisez la commande <STRONG>Dfsradmin</STRONG> pour spécifier un membre principal pour un dossier répliqué une fois la réplication terminée, réplication DFS ne désigne pas le serveur comme membre principal dans Active Directory Domain Services. Toutefois, si la base de données réplication DFS sur le serveur subit par la suite une altération irréversible ou une perte de données, le serveur tente d’effectuer une réplication initiale en tant que membre principal au lieu de récupérer ses données à partir d’un autre membre de la réplication. Communauté. Fondamentalement, le serveur devient un serveur principal non autorisé, ce qui peut entraîner des conflits. Pour cette raison, spécifiez le membre principal manuellement uniquement si vous êtes certain que la réplication initiale a échoué irrémédiablement. 
<br>


### <a name="what-happens-if-the-replication-schedule-closes-while-a-file-is-being-replicated"></a>Que se passe-t-il si la planification de réplication se ferme pendant la réplication d’un fichier ?

Si la compression différentielle à distance (RDC) est activée sur la connexion, la réplication entrante d’un fichier d’une taille supérieure à 64 Ko qui a commencé la réplication juste avant la fermeture de la planification (ou **la modification sans bande passante**) se poursuit lorsque la planification s’ouvre (ou modifications apportées à autre chose qu' **aucune bande passante**). La réplication se poursuit à partir de l’État où elle se trouvait lorsque la réplication s’est arrêtée.

Si RDC est désactivé, réplication DFS redémarre complètement le transfert de fichiers. Cela peut retarder lorsque le fichier est disponible sur le membre de réception.

### <a name="what-happens-when-two-users-simultaneously-update-the-same-file-on-different-servers"></a>Que se passe-t-il lorsque deux utilisateurs mettent à jour simultanément le même fichier sur des serveurs différents ?

Lorsque réplication DFS détecte un conflit, il utilise la version du fichier qui a été enregistré en dernier. Il déplace l’autre fichier dans le dossier\\DfsrPrivate ConflictandDeleted (sous le chemin d’accès local du dossier répliqué sur l’ordinateur qui a résolu le conflit). Il reste jusqu’à ce que le nettoyage du dossier soit en conflit et supprimé, ce qui se produit lorsque le dossier des fichiers en conflit et supprimés dépasse la taille configurée ou réplication DFS rencontre une erreur d’espace disque insuffisant. Le dossier des fichiers en conflit et supprimés n’est pas répliqué, et cette méthode de résolution des conflits évite le problème des répertoires transplicas qui étaient possibles dans FRS.

En cas de conflit, réplication DFS consigne un événement d’information dans le journal des événements réplication DFS. Cet événement ne requiert pas d’action de l’utilisateur pour les raisons suivantes :

  - Elle n’est pas visible pour les utilisateurs (elle est visible uniquement par les administrateurs du serveur).  
      
  - Réplication DFS traite le dossier des fichiers en conflit et supprimés en tant que cache. Lorsqu’un seuil de quota est atteint, il nettoie certains de ces fichiers. Il n’y a aucune garantie que les fichiers en conflit seront enregistrés.  
      
  - Le conflit peut résider sur un serveur différent de l’origine du conflit.  
      

## <a name="staging"></a>Création intermédiaire

### <a name="does-dfs-replication-continue-staging-files-when-replication-is-disabled-by-a-schedule-or-bandwidth-throttling-quota-or-when-a-connection-is-manually-disabled"></a>Réplication DFS continue-t-il les fichiers intermédiaires lorsque la réplication est désactivée par un quota de limitation de bande passante ou de planification, ou lorsqu’une connexion est désactivée manuellement ?

Non. Réplication DFS ne continue pas à organiser les fichiers en dehors des temps de réplication planifiés, si le quota de limitation de bande passante a été dépassé ou lorsque les connexions sont désactivées.

### <a name="does-dfs-replication-prevent-other-applications-from-accessing-a-file-during-staging"></a>Réplication DFS empêche-t-il d’autres applications d’accéder à un fichier lors de la mise en lots ?

Non. Réplication DFS ouvre les fichiers d’une manière qui empêche les utilisateurs ou les applications d’ouvrir des fichiers dans le dossier réplication. Cette méthode est appelée « verrouillage opportuniste ».

### <a name="is-it-possible-to-change-the-location-of-the-staging-folder-with-the-dfs-management-tool"></a>Est-il possible de modifier l’emplacement du dossier intermédiaire avec l’outil de gestion DFS ?

Oui. L’emplacement du dossier intermédiaire est configuré sous l’onglet **avancé** de la boîte de dialogue **Propriétés** pour chaque membre d’un groupe de réplication.

### <a name="when-are-files-staged"></a>Quand les fichiers sont-ils intermédiaires ?

Les fichiers sont mis en place sur le membre émetteur lorsque le membre récepteur demande le fichier (à moins que le fichier ne soit de 64 Ko ou plus) comme indiqué dans le tableau suivant. Si la compression différentielle à distance (RDC) est désactivée sur la connexion, le fichier est intermédiaire à moins qu’il ne soit de 256 Ko ou plus. Les fichiers sont également mis en place sur le membre de réception lorsqu’ils sont transférés s’ils ont une taille inférieure à 64 Ko, même si vous pouvez configurer ce paramètre entre 16 Ko et 1 Mo. Si la planification est fermée, les fichiers ne sont pas intermédiaires.

### <a name="the-minimum-file-sizes-for-staging-files"></a>Tailles de fichier minimales pour les fichiers intermédiaires

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
<td><p>Membre expéditeur</p></td>
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

### <a name="what-happens-if-a-file-is-changed-after-it-is-staged-but-before-it-is-completely-transmitted-to-the-remote-site"></a>Que se passe-t-il si un fichier est modifié après avoir été mis en place mais avant d’être complètement transmis au site distant ?

Si une partie du fichier est déjà en cours de transmission, réplication DFS continue la transmission. Si le fichier est modifié avant que réplication DFS ne commence à transmettre le fichier, la version la plus récente du fichier est envoyée.

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
<td><p>Mise à jour des limites de réplication DFS prises en charge ? section avec les résultats des tests sur Windows Server 2012 R2.</p></td>
<td><p>Mises à jour de la dernière version de Windows Server</p></td>
</tr>
<tr class="odd">
<td><p>30 janvier, 2013</p></td>
<td><p>Vous avez ajouté le réplication DFS continuer les fichiers intermédiaires lorsque la réplication est désactivée par un quota de limitation de bande passante ou de planification, ou lorsqu’une connexion est désactivée manuellement ? mention.</p></td>
<td><p>Questions du client</p></td>
</tr>
<tr class="even">
<td><p>31 octobre 2012</p></td>
<td><p>Modification des limites de réplication DFS prises en charge ? entrée permettant d’augmenter le nombre testé de fichiers répliqués sur un volume.</p></td>
<td><p>Retour d'expérience du client</p></td>
</tr>
<tr class="odd">
<td><p>15 août 2012</p></td>
<td><p>Modifié le n’a-t-il réplication DFS répliquer les autorisations de fichiers NTFS, les flux de données alternatifs, les liens physiques et les points d’analyse ? entrée pour clarifier la façon dont réplication DFS gère les liens physiques et les points d’analyse.</p></td>
<td><p>Commentaires des services de support technique</p></td>
</tr>
<tr class="even">
<td><p>Le 13 juin 2012</p></td>
<td><p>Modifié le ne fonctionne-t-il réplication DFS sur des volumes ReFS ou FAT ? entrée pour ajouter une discussion de références.</p></td>
<td><p>Retour d'expérience du client</p></td>
</tr>
<tr class="odd">
<td><p>25 avril, 2012</p></td>
<td><p>Modifié le n’a-t-il réplication DFS répliquer les autorisations de fichiers NTFS, les flux de données alternatifs, les liens physiques et les points d’analyse ? entrée pour clarifier la façon dont réplication DFS gère les liens physiques.</p></td>
<td><p>Réduire la confusion potentielle</p></td>
</tr>
<tr class="even">
<td><p>Le 30 mars 2011</p></td>
<td><p>Modifié le peut réplication DFS répliquer les fichiers de base de données Outlook. pst ou Microsoft Office Access ? entrée pour corriger l’impact potentiel de l’utilisation de réplication DFS avec des fichiers. pst et Access.</p>
<p>Vous avez ajouté comment améliorer les performances de la réplication ?</p></td>
<td><p>Questions du client sur l’entrée précédente, ce qui indiquait incorrectement que la réplication de fichiers. pst ou Access pouvait corrompre la base de données réplication DFS.</p></td>
</tr>
<tr class="odd">
<td><p>26 janvier 2011</p></td>
<td><p>Ajout de la façon dont les fichiers peuvent être récupérés à partir des dossiers ConflictAndDeleted ou PreExisting ?</p></td>
<td><p>Retour d'expérience du client</p></td>
</tr>
<tr class="even">
<td><p>20 octobre, 2010</p></td>
<td><p>Vous avez ajouté Comment puis-je mettre à niveau ou remplacer un membre réplication DFS ?</p></td>
<td><p>Retour d'expérience du client</p></td>
</tr>
</tbody>
</table>

