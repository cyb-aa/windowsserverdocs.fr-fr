---
ms.assetid: 341614c6-72c2-444f-8b92-d2663aab7070
title: Architecture des contrôleurs de domaine virtualisés
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: d69ccfd15004619f890c6f5c1cb630c62e16256b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889190"
---
# <a name="virtualized-domain-controller-architecture"></a>Architecture des contrôleurs de domaine virtualisés

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette rubrique décrit l'architecture du clonage et de la restauration sécurisée d'un contrôleur de domaine virtualisé. Elle illustre le processus de clonage et de restauration sécurisée à l'aide d'organigrammes, puis explique de manière détaillée chaque étape du processus.  
  
-   [Architecture de clonage de contrôleur de domaine virtualisés](../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_CloneArch)  
  
-   [Architecture de restauration sécurisée d’un contrôleur de domaine virtualisés](../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_SafeRestoreArch)  
  
## <a name="BKMK_CloneArch"></a>Architecture de clonage de contrôleur de domaine virtualisés  
  
### <a name="overview"></a>Vue d'ensemble  
Le clonage d'un contrôleur de domaine virtualisé repose sur la plateforme de l'hyperviseur pour exposer un identificateur appelé **ID de génération d'ordinateur virtuel** pour détecter la création d'un ordinateur virtuel. AD DS stocke d'abord la valeur de cet identifiant dans sa base de données (NTDS.DIT) durant la promotion du contrôleur de domaine. Quand l'ordinateur virtuel démarre, la valeur actuelle de l'ID de génération d'ordinateur virtuel de l'ordinateur virtuel est comparée à la valeur contenue dans la base de données. Si les deux valeurs sont différentes, le contrôleur de domaine réinitialise l'ID d'appel et supprime le pool RID, ce qui empêche ainsi la réutilisation de la valeur USN ou la création potentielle de principaux de sécurité dupliqués. Le contrôleur de domaine recherche ensuite un fichier DCCloneConfig.xml dans les emplacements décrits à l’étape 3 dans [Cloning Detailed Processing](../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_CloneProcessDetails). S'il trouve un fichier DCCloneConfig.xml, il en conclut qu'il est déployé en tant que clone. Il démarre donc le clonage pour s'approvisionner en tant que contrôleur de domaine supplémentaire en effectuant une nouvelle promotion à l'aide du contenu existant de NTDS.DIT et de SYSVOL, copié à partir du média source.  
  
Dans un environnement mixte où seuls certains hyperviseurs prennent en charge l'ID de génération d'ordinateur virtuel, il est possible qu'un média clone soit déployé involontairement sur un hyperviseur qui ne prend pas en charge l'ID de génération d'ordinateur virtuel. La présence du fichier DCCloneConfig.xml indique l'intention administrative de cloner un contrôleur de domaine. Ainsi, si un fichier DCCloneConfig.xml est détecté au démarrage, mais qu'aucun ID de génération d'ordinateur virtuel n'est fourni par l'hôte, le contrôleur de domaine clone démarre en mode de restauration des services d'annuaire (DSRM) pour éviter tout impact sur le reste de l'environnement. Le média clone peut ensuite être déplacé vers un hyperviseur qui prend en charge l'ID de génération d'ordinateur virtuel, ce qui permet de retenter le clonage.  
  
Si le média clone est déployé sur un hyperviseur qui prend en charge l'ID de génération d'ordinateur virtuel mais qu'aucun fichier DCCloneConfig.xml n'est fourni, quand le contrôleur de domaine détecte un changement d'ID de génération d'ordinateur virtuel entre son fichier DIT et celui du nouvel ordinateur virtuel, il déclenche des dispositifs de protection pour empêcher la réutilisation de la valeur USN et la duplication des SID. Cependant, le clonage n'est pas lancé. Ainsi, le contrôleur de domaine secondaire continue de s'exécuter sous la même identité que le contrôleur de domaine source. Ce contrôleur de domaine secondaire doit être supprimé du réseau le plus tôt possible pour éviter toute incohérence dans l'environnement. Pour plus d’informations sur la façon de récupérer ce contrôleur de domaine secondaire tout en veillant à ce que les mises à jour soient répliquées en sortie, voir l’article n° [2742970](https://support.microsoft.com/kb/2742970)dans la Base de connaissances Microsoft.  
  
### <a name="BKMK_CloneProcessDetails"></a>Processus détaillé du clonage  
Le schéma suivant illustre l'architecture d'une opération de clonage initiale et d'une opération de nouvelle tentative de clonage. Ces processus sont expliqués de manière détaillée plus loin dans cette rubrique.  
  
**Opération de clonage initiale**  
  
![Architecture de contrôleur de domaine virtualisé](media/Virtualized-Domain-Controller-Architecture/ADDS_VDC_InitialCloningProcess.png)  
  
**Réessayez l’opération de clonage**  
  
![Architecture de contrôleur de domaine virtualisé](media/Virtualized-Domain-Controller-Architecture/ADDS_VDC_CloningRetryProcess.png)  
  
Les étapes suivantes expliquent le processus de manière plus détaillée :  
  
1.  Un contrôleur de domaine d'ordinateur virtuel existant démarre sur un hyperviseur qui prend en charge l'ID de génération d'ordinateur virtuel.  
  
    1.  Cet ordinateur virtuel n'a aucune valeur d'ID de génération d'ordinateur virtuel déjà définie sur son objet ordinateur AD DS après la promotion.  
  
    2.  Même si sa valeur est Null, la prochaine création d'ordinateur signifie qu'il y a clonage, car aucun nouvel ID de génération d'ordinateur virtuel ne correspond.  
  
    3.  L'ID de génération d'ordinateur virtuel est défini après le prochain redémarrage du contrôleur de domaine et n'est pas répliqué.  
  
2.  L'ordinateur virtuel lit ensuite l'ID de génération d'ordinateur virtuel fourni par le pilote VMGenerationCounter. Il compare les deux ID de génération d'ordinateur virtuel.  
  
    1.  Si les ID correspondent, il ne s'agit pas d'un nouvel ordinateur virtuel et aucun clonage n'a lieu. S'il existe un fichier DCCloneConfig.xml, le contrôleur de domaine renomme le fichier avec un cachet de date et d'heure qui empêche le clonage. Le serveur continue de démarrer normalement. C'est ainsi que s'effectue chaque redémarrage de n'importe quel contrôleur de domaine virtuel dans Windows Server 2012.  
  
    2.  Si les deux ID ne correspondent pas, il s'agit d'un nouvel ordinateur virtuel qui contient un fichier NTDS.DIT d'un précédent contrôleur de domaine (ou il s'agit d'une capture instantanée restaurée). S'il existe un fichier DCCloneConfig.xml, le contrôleur de domaine poursuit les opérations de clonage. Sinon, il poursuit les opérations de restauration de capture instantanée. Consultez [Virtualized domain controller safe restore architecture](../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_SafeRestoreArch).  
  
    3.  Si l'hyperviseur ne fournit pas d'ID de génération d'ordinateur virtuel pour la comparaison, mais qu'il existe un fichier DCCloneConfig.xml, l'invité renomme le fichier, puis démarre en mode DSRM pour protéger le réseau contre une duplication du contrôleur de domaine. S'il n'existe aucun fichier dccloneconfig.xml, l'invité démarre normalement (avec le risque de duplication du contrôleur de domaine sur le réseau). Pour plus d’informations sur la récupération de ce contrôleur de domaine dupliqué, voir l’article n° [2742970](https://support.microsoft.com/kb/2742970) dans la Base de connaissances Microsoft.  
  
3.  Le service NTDS vérifie la valeur du nom de la valeur de Registre DWORD VDCisCloning (sous HKEY_Local_Machine\System\CurrentControlSet\Services\Ntds\Parameters).  
  
    1.  Si elle n'existe pas, il s'agit d'une première tentative de clonage de cet ordinateur virtuel. L'invité implémente les dispositifs de protection contre la duplication d'objet VDC en invalidant le pool RID local et en définissant un nouvel ID d'appel de réplication pour le contrôleur de domaine  
  
    2.  Si sa valeur est déjà 0x1, il s'agit d'une « nouvelle » tentative de clonage, après l'échec d'une précédente opération de clonage. Les mesures de sécurité contre la duplication d'objets VDC ne sont pas prises, car elles ont déjà dû être appliquées une fois auparavant. En outre, elles risquent d'altérer inutilement l'invité à plusieurs reprises.  
  
4.  Le nom de la valeur de Registre DWORD IsClone est écrit (sous Hkey_Local_Machine\System\CurrentControlSet\Services\NTDS\Parameters)  
  
5.  Le service NTDS change l'indicateur de démarrage de l'invité pour qu'il démarre désormais en mode de réparation des services d'annuaire.  
  
6.  Le service NTDS tente de lire le fichier DcCloneConfig.xml dans l'un des trois emplacements admis (répertoire de travail DSA, %windir%\NTDS ou média amovible accessible en lecture/écriture, dans l'ordre des lettres de lecteur, à la racine du lecteur).  
  
    1.  Si le fichier n'existe dans aucun emplacement valide, l'invité vérifie si l'adresse IP n'est pas dupliquée. Si l'adresse IP n'est pas dupliquée, le serveur démarre normalement. S'il existe une adresse IP dupliquée, l'ordinateur démarre en mode DSRM pour protéger le réseau contre une duplication du contrôleur de domaine.  
  
    2.  Si le fichier existe dans un emplacement valide, le service NTDS valide ses paramètres. Si le fichier est vide ou si des paramètres particuliers sont vides, le service NTDS configure automatiquement les valeurs de ces paramètres.  
  
    3.  Si le fichier DcCloneConfig.xml existe mais qu'il contient des entrées non valides ou qu'il est illisible, le clonage échoue et l'invité démarre en mode de restauration des services d'annuaire (DSRM).  
  
7.  L'invité désactive toutes les inscriptions automatiques DNS pour empêcher le détournement accidentel du nom et des adresses IP de l'ordinateur source.  
  
8.  L'invité arrête le service Netlogon pour éviter toute publication ou réponse relative aux demandes AD DS des clients sur le réseau.  
  
9. Le service NTDS vérifie qu'il n'y a aucun service ou programme installé qui ne fait pas partie des fichiers DefaultDCCloneAllowList.xml ou CustomDCCloneAllowList.xml  
  
    1.  S'il existe des services ou programmes installés qui ne figurent pas dans la liste d'exclusions/liste verte par défaut ou dans la liste d'exclusions/liste verte personnalisée, le clonage échoue et l'invité démarre en mode DSRM pour protéger le réseau contre une duplication du contrôleur de domaine.  
  
    2.  S'il n'y a aucune incompatibilité, le clonage se poursuit.  
  
10. Si l'adressage IP automatique est utilisé en raison de paramètres réseau vides dans le fichier DCCloneConfig.xml, l'invité active le protocole DHCP sur les cartes réseau pour obtenir un bail d'adresse IP, un routage réseau, ainsi que des informations de résolution de noms.  
  
11. L'invité localise et contacte le contrôleur de domaine qui exécute le rôle FSMO d'émulateur de contrôleur de domaine principal. Cela demande l'utilisation de DNS et du protocole DCLocator. Il établit une connexion RPC et appelle la méthode IDL_DRSAddCloneDC pour cloner l'objet ordinateur du contrôleur de domaine.  
  
    1.  Si l'objet ordinateur source de l'invité détient l'autorisation étendue d'en-tête de domaine « Autoriser un contrôleur de domaine à créer un clone de lui-même », le clonage se poursuit.  
  
    2.  Si l'objet ordinateur source de l'invité ne détient pas cette autorisation étendue, le clonage échoue et l'invité démarre en mode DSRM pour protéger le réseau contre une duplication du contrôleur de domaine.  
  
12. Le nom de l'objet ordinateur AD DS est défini pour correspondre au nom spécifié dans le fichier DCCloneConfig.xml, le cas échéant, ou généré automatiquement sur l'émulateur de contrôleur de domaine principal. Le service NTDS crée l'objet paramètre NTDS adéquat pour le site logique Active Directory approprié.  
  
    1.  S'il s'agit d'un clonage de contrôleur de domaine principal, l'invité renomme l'ordinateur local et redémarre. Après le redémarrage, il passe par l’étape 1-10 à nouveau, puis passe à l’étape 13.  
  
    2.  S'il s'agit du clonage d'un contrôleur de domaine réplica, aucun redémarrage n'a lieu à ce stade.  
  
13. L'invité fournit les paramètres de promotion au service Serveur de rôles DS, qui commence la promotion.  
  
14. Le service Serveur de rôles DS arrête tous les services liés aux services AD DS (NTDS, NTFRS/DFSR, KDC, DNS).  
  
15. L'invité force la synchronisation date/heure NT5DS (protocole Windows NTP) avec un autre contrôleur de domaine (dans une hiérarchie de service de temps Windows par défaut, cela revient à utiliser l'émulateur de contrôleur de domaine principal). L'invité contacte l'émulateur de contrôleur de domaine principal. Tous les tickets Kerberos existants sont vidés.  
  
16. L'invité configure l'exécution automatique des services DFSR/NTFRS. L’invité supprime tous les fichiers de base de données DFSR et NTFRS existants (par défaut : c:\windows\ntfrs et c:\system information\dfsr volume\\ *< database_GUID >*), afin de forcer la synchronisation ne faisant pas autorité de SYSVOL au prochain démarrage du service. L'invité ne supprime pas le contenu des fichiers de SYSVOL, pour prédéfinir SYSVOL quand la synchronisation démarrera plus tard.  
  
17. L'invité est renommé. Le service Serveur de rôles DS sur l'invité commence la configuration d'AD DS (promotion) en utilisant le fichier de base de données existant NTDS.DIT en tant que source, au lieu de la base de données de modèle incluse dans c:\windows\system32, comme le fait normalement une promotion.  
  
18. L'invité contacte le détenteur du rôle FSMO du maître RID pour obtenir une nouvelle allocation du pool RID.  
  
19. Le processus de promotion crée un ID d'appel et recrée l'objet Paramètres NTDS pour le contrôleur de domaine cloné (indépendamment du clonage, cela fait partie de la promotion de domaine en cas d'utilisation d'une base de données NTDS.DIT existante).  
  
20. Le service NTDS réplique les objets manquants, nouveaux ou qui ont une version supérieure, à partir d'un contrôleur de domaine partenaire. NTDS.DIT contient déjà des objets qui datent du moment où le contrôleur de domaine source était hors connexion. Dans la mesure du possible, ces objets sont utilisés pour réduire le trafic de réplication entrant. Les partitions du catalogue global sont remplies.  
  
21. Le service DFSR ou FRS démarre. Comme il n'y a aucune base de données, SYSVOL effectue une synchronisation ne faisant pas autorité des données entrantes à partir d'un partenaire de réplication. Ce processus réutilise les données préexistantes du dossier SYSVOL pour réduire le trafic de réplication sur le réseau.  
  
22. L'invité réactive l'inscription de client DNS une fois que l'ordinateur a un nom unique et qu'il est connecté au réseau.  
  
23. L'invité exécute les modules SYSPREP spécifiés par l'élément <SysprepInformation> du fichier DefaultDCCloneAllowList.xml pour supprimer les références au nom d'ordinateur et au SID précédents.  
  
24. La promotion du clonage est effectuée.  
  
    1.  L'invité supprime l'indicateur de démarrage en mode DSRM pour que le prochain redémarrage soit normal.  
  
    2.  L'invité renomme le fichier DCCloneConfig.xml en ajoutant un cachet de date/heure qui l'empêche d'être lu à nouveau au prochain démarrage.  
  
    3.  L'invité supprime le nom de la valeur de Registre DWORD VdcIsCloning sous HKEY_Local_Machine\System\CurrentControlSet\Services\NTDS\Parameters.  
  
    4.  L'invité affecte la valeur 0x1 au nom de la valeur de Registre DWORD « VdcCloningDone » sous HKEY_Local_Machine\System\CurrentControlSet\Services\NTDS\Parameters. Windows n'utilise pas cette valeur mais la fournit à la place en tant que marqueur pour des logiciels tiers.  
  
25. L'invité met à jour l'attribut msDS-GenerationID sur son propre objet contrôleur de domaine cloné pour correspondre à l'ID de génération d'ordinateur virtuel invité actuel.  
  
26. L'invité redémarre. Il est désormais un contrôleur de domaine de publication normal.  
  
## <a name="BKMK_SafeRestoreArch"></a>Architecture de restauration sécurisée d’un contrôleur de domaine virtualisés  
  
### <a name="overview"></a>Vue d'ensemble  
AD DS repose sur la plateforme de l'hyperviseur pour exposer un identificateur appelé **ID de génération d'ordinateur virtuel** pour détecter la restauration de capture instantanée d'un ordinateur virtuel. AD DS stocke d'abord la valeur de cet identifiant dans sa base de données (NTDS.DIT) durant la promotion du contrôleur de domaine. Quand un administrateur restaure l'ordinateur virtuel à partir d'une capture instantanée précédente, la valeur actuelle de l'ID de génération d'ordinateur virtuel de l'ordinateur virtuel est comparée à la valeur contenue dans la base de données. Si les deux valeurs sont différentes, le contrôleur de domaine réinitialise l'ID d'appel et supprime le pool RID, ce qui empêche ainsi la réutilisation de la valeur USN ou la création potentielle de principaux de sécurité dupliqués. Une restauration sécurisée peut se produire dans deux cas de figure :  
  
-   quand un contrôleur de domaine virtuel a démarré après la restauration d'une capture instantanée pendant qu'il était arrêté ;  
  
-   quand une capture instantanée est restaurée sur un contrôleur de domaine virtuel en cours d'exécution.  
  
    Si le contrôleur de domaine virtualisé dans la capture instantanée est dans un état interrompu et non à l'arrêt, vous devez redémarrer le service AD DS pour déclencher une nouvelle demande de pool RID. Vous pouvez redémarrer le service AD DS à l'aide du composant logiciel enfichable Services ou à l'aide de Windows PowerShell (Restart-Service NTDS -force).  
  
Les sections suivantes expliquent en détail la restauration sécurisée pour chaque scénario.  
  
### <a name="safe-restore-detailed-processing"></a>Processus détaillé de la restauration sécurisée  
L'organigramme suivant montre le déroulement de la restauration sécurisée quand un contrôleur de domaine virtuel a démarré après la restauration d'une capture instantanée pendant qu'il était arrêté.  
  
![Architecture de contrôleur de domaine virtualisé](media/Virtualized-Domain-Controller-Architecture/ADDS_VDC_VirtualizationSafeguardsDuringNormalBoot.png)  
  
1.  Quand l'ordinateur virtuel démarre après la restauration d'une capture instantanée, un nouvel ID de génération d'ordinateur virtuel lui est affecté par l'hôte hyperviseur en raison de la restauration de la capture instantanée.  
  
2.  Le nouvel ID de génération d'ordinateur virtuel de l'ordinateur virtuel est comparé à l'ID de génération d'ordinateur virtuel dans la base de données. Dans la mesure où les deux ID ne correspondent pas, des dispositifs de protection sont utilisés pour la virtualisation (voir l'étape 3 de la section précédente). Une fois la restauration appliquée, l'ID de génération d'ordinateur virtuel défini sur son objet ordinateur AD DS est mis à jour pour correspondre au nouvel ID fourni par l'hôte hyperviseur.  
  
3.  L'invité utilise des dispositifs de protection en :  
  
    1.  invalidant le pool RID local ;  
  
    2.  définissant un nouvel ID d'appel pour la base de données du contrôleur de domaine.  
  
> [!NOTE]  
> Cette partie de la restauration sécurisée se superpose au processus de clonage. Bien que ce processus concerne la restauration sécurisée d'un contrôleur de domaine virtuel après son démarrage à la suite d'une restauration de capture instantanée, les mêmes étapes ont lieu durant le processus de clonage.  
  
Le schéma suivant montre comment les dispositifs de protection de virtualisation empêchent la divergence induite par la restauration de la valeur USN quand une capture instantanée est restaurée sur un contrôleur de domaine virtuel en cours d'exécution.  
  
![Architecture de contrôleur de domaine virtualisé](media/Virtualized-Domain-Controller-Architecture/ADDS_VDC_VirtualizationSafeguardsDuringSnapShotRestore.png)  
  
> [!NOTE]  
> L'illustration précédente est simplifiée pour permettre l'explication des concepts.  
  
1.  À l'heure T1, l'administrateur de l'hyperviseur prend une capture instantanée du contrôleur de domaine DC1 virtuel. À ce moment-là, le contrôleur de domaine DC1 possède une valeur USN (**highestCommittedUsn** en pratique) égale à 100, InvocationId (représentée en tant qu'ID dans le schéma précédent) possède la valeur A (GUID en pratique). La valeur de savedVMGID est l'ID de génération d'ordinateur virtuel contenu dans le fichier DIT du contrôleur de domaine (stocké en fonction de l'objet ordinateur du contrôleur de domaine dans un attribut nommé **msDS-GenerationId**). La valeur de VMGID est la valeur actuelle de l'ID de génération d'ordinateur virtuel disponible à partir du pilote d'ordinateur virtuel. Cette valeur est fournie par l'hyperviseur.  
  
2.  Plus tard, à l'heure T2, 100 utilisateurs sont ajoutés à ce contrôleur de domaine (considérez les utilisateurs comme des exemples de mises à jour qui auraient pu être effectuées sur ce contrôleur de domaine entre les heures T1 et T2 ; ces mises à jour peuvent en fait être un mélange de créations d'utilisateurs, de créations de groupes, de mises à jour de mots de passe, de mises à jour d'attributs, etc.). Dans cet exemple, chaque mise à jour consomme une seule valeur USN (même si, en pratique, la création d'un utilisateur consomme plusieurs valeurs USN). Avant de valider ces mises à jour, le contrôleur de domaine DC1 vérifie si la valeur de l'ID de génération d'ordinateur virtuel dans sa base de données (savedVMGID) est la même que la valeur actuelle disponible à partir du pilote (VMGID). Les valeurs sont identiques, aucune restauration n'a eu lieu pour l'instant. Ainsi, les mises à jour sont validées et la valeur USN monte à 200, ce qui signifie que la prochaine mise à jour peut utiliser la valeur USN 201. Il n'y a aucun changement dans InvocationId, savedVMGID ou VMGID. Ces mises à jour sont répliquées vers le contrôleur de domaine DC2 au prochain cycle de réplication. DC2 met à jour limite supérieure (et **UptoDatenessVector**) représentée ici simplement comme DC1(A) @USN = 200. En d'autres termes, le contrôleur de domaine DC2 connaît l'existence de toutes les mises à jour du contrôleur de domaine DC1 dans le contexte de l'InvocationId A jusqu'à la valeur USN 200.  
  
3.  À l'heure T3, la capture instantanée prise à l'heure T1 est appliquée au contrôleur de domaine DC1. Le contrôleur de domaine DC1 est restauré. Ainsi, sa valeur USN repasse à 100, ce qui signifie qu'il peut associer aux prochaines mises à jour des valeurs USN commençant à partir de 101. Toutefois, à ce stade, la valeur de VMGID est différente sur les hyperviseurs qui prennent en charge l'ID de génération d'ordinateur virtuel.  
  
4.  Plus tard, quand le contrôleur de domaine DC1 effectue une mise à jour, il vérifie si la valeur de l'ID de génération d'ordinateur virtuel contenue dans sa base de données (savedVMGID) est la même que la valeur du pilote d'ordinateur virtuel (VMGID). Dans le cas présent, les valeurs ne sont pas les mêmes. Le contrôleur de domaine DC1 en déduit qu'il y a eu une restauration et déclenche les dispositifs de protection de virtualisation. En d'autres termes, il réinitialise son InvocationId (ID = B) et supprime le pool RID (non représenté sur le schéma précédent). Il enregistre la nouvelle valeur de VMGID dans sa base de données et valide ces mises à jour (USN 101 - 250) dans le contexte de la nouvelle B. InvocationId Au prochain cycle de réplication, DC2 ne sait rien de DC1 dans le contexte de l’InvocationId B, et demande à tous les éléments de DC1 associé InvocationID B. Par conséquent, les mises à jour exécutées sur DC1 à la suite de l’application de capture instantanée seront converger en toute sécurité. En outre, l'ensemble des mises à jour effectuées sur le contrôleur de domaine DC1 à l'heure T2 (qui ont été perdues sur le contrôleur de domaine DC1 après la restauration de la capture instantanée) sont à nouveau répliquées vers le contrôleur de domaine DC1 à la prochaine réplication planifiée, car elles avaient été répliquées vers le contrôleur de domaine DC2 (comme le montre la ligne pointillée en direction du contrôleur de domaine DC1).  
  
Une fois que l'invité utilise les dispositifs de protection de virtualisation, le service NTDS effectue une réplication ne faisant pas autorité des différences entre les objets Active Directory en entrée à partir d'un contrôleur de domaine partenaire. Le vecteur de mise à jour du service d'annuaire de destination est mis à jour en conséquence. L'invité synchronise ensuite SYSVOL :  
  
-   Si le service FRS est utilisé, l'invité arrête le service NTFRS et définit la valeur de Registre D2 BURFLAGS. Il démarre ensuite le service NTFRS, qui effectue une réplication ne faisant pas autorité en entrée, en réutilisant les données SYSVOL existantes inchangées quand cela est possible.  
  
-   Si vous utilisez DFSR, l’invité arrête le service DFSR et supprime les fichiers de base de données DFSR (emplacement par défaut : %systemroot%\system volume information\dfsr\\*<database GUID>*). Il démarre ensuite le service DFSR, qui effectue une réplication ne faisant pas autorité en entrée, en réutilisant les données SYSVOL existantes inchangées quand cela est possible.  
  
> [!NOTE]  
> -   Si l'hyperviseur ne fournit aucun ID de génération d'ordinateur virtuel de comparaison, il ne prend pas en charge les dispositifs de protection de virtualisation et l'invité fonctionne comme un contrôleur de domaine virtualisé qui exécute Windows Server 2008 R2 ou une version antérieure. L'invité implémente la protection par mise en quarantaine de la restauration USN en cas de tentative de démarrage d'une réplication avec des valeurs USN qui ne sont pas postérieures à la dernière valeur USN la plus élevée détectée par le contrôleur de domaine partenaire. Pour plus d’informations sur la protection par mise en quarantaine de la restauration USN, voir [USN et restauration USN](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv(WS.10).aspx)  
  


