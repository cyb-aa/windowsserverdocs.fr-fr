---
ms.assetid: 341614c6-72c2-444f-8b92-d2663aab7070
title: "Architecture des contrôleurs de domaine virtualisés"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ac8b190df065547d82aa431761eb5c00c94a2ad6
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="virtualized-domain-controller-architecture"></a>Architecture des contrôleurs de domaine virtualisés

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Cette rubrique décrit l’architecture du clonage du contrôleur de domaine virtualisés et de restauration sécurisée. Il illustre le processus de clonage et la restauration sécurisée avec organigrammes et fournit une explication détaillée de chaque étape du processus.  
  
-   [Architecture de clonage de contrôleur de domaine virtualisés](../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_CloneArch)  
  
-   [Architecture de restauration sécurisée de contrôleur de domaine virtualisés](../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_SafeRestoreArch)  
  
## <a name="BKMK_CloneArch"></a>Architecture de clonage de contrôleur de domaine virtualisés  
  
### <a name="overview"></a>Vue d’ensemble  
Clonage de contrôleur de domaine virtualisé repose sur la plateforme d’hyperviseur pour exposer un identificateur appelé **ID de génération** pour détecter la création d’un ordinateur virtuel. Les services AD DS stocke d’abord la valeur de cet identifiant dans sa base de données (NTDS. DIT) pendant la promotion du contrôleur de domaine. Lorsque l’ordinateur virtuel démarre, la valeur actuelle de l’ID de génération d’ordinateur virtuel à partir de l’ordinateur virtuel est comparée à la valeur dans la base de données. Si les deux valeurs sont différentes, le contrôleur de domaine réinitialise l’ID d’appel et supprime le pool RID, ce qui empêche la réutilisation des numéros USN ou la création potentielle de principaux de sécurité dupliqués. La recherche d’un fichier DCCloneConfig.xml dans les emplacements ensuite le contrôleur de domaine appelé à l’étape 3 dans [Cloning Detailed Processing](../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_CloneProcessDetails). S’il trouve un fichier DCCloneConfig.xml, il en conclut qu’il est déployé en tant que clone, afin qu’il lance le clonage pour s’imposer en tant que contrôleur de domaine supplémentaire à nouveau la promotion à l’aide de la NTDS existant. Contenu du fichier DIT et SYSVOL copié à partir du média source.  
  
Dans un environnement mixte où seuls certains hyperviseurs prennent en charge les ID de génération et d’autres non, il est possible pour un média clone soit déployé involontairement sur un hyperviseur qui ne prend pas en charge les ID de génération. La présence du fichier DCCloneConfig.xml indique l’intention administrative de cloner un contrôleur de domaine. Par conséquent, si un fichier DCCloneConfig.xml est trouvé au cours de démarrage, mais qu’un ID de génération d’ordinateur virtuel n’est fourni par l’ordinateur hôte, le contrôleur de domaine clone est démarré dans Services restaurer Mode annuaire (DSRM) pour éviter tout impact sur le reste de l’environnement. Le média clone peut être déplacé vers un hyperviseur qui prend en charge les ID de génération et, le clonage peut être retentée.  
  
Si le média clone est déployé sur un hyperviseur qui prend en charge les ID de génération, mais un fichier DCCloneConfig.xml n’est pas fourni, comme le contrôleur de domaine détecte un changement d’ID de génération entre son fichier DIT et celui du nouvel ordinateur virtuel, il déclenche des dispositifs de protection pour empêcher la réutilisation des numéros USN et la duplication des SID. Toutefois, le clonage ne démarre pas, afin que le contrôleur de domaine secondaire continue à s’exécuter sous la même identité que le contrôleur de domaine source. Ce contrôleur de domaine secondaire doit être supprimé à partir du réseau au plus tôt possible pour éviter toute incohérence dans l’environnement. Pour plus d’informations sur la façon de récupérer ce contrôleur de domaine secondaire tout en garantissant que les mises à jour soient répliquées en sortie, voir Microsoft KB article [2742970](https://support.microsoft.com/kb/2742970).  
  
### <a name="BKMK_CloneProcessDetails"></a>Processus détaillé du clonage  
Le diagramme suivant illustre l’architecture pour une opération de clonage initiale et pour une opération de nouvelle tentative de clonage. Ces processus sont expliqués plus en détail plus loin dans cette rubrique.  
  
**Opération de clonage initiale**  
  
![Architecture du contrôleur de domaine virtualisé](media/Virtualized-Domain-Controller-Architecture/ADDS_VDC_InitialCloningProcess.png)  
  
**Opération de nouvelle tentative de clonage**  
  
![Architecture du contrôleur de domaine virtualisé](media/Virtualized-Domain-Controller-Architecture/ADDS_VDC_CloningRetryProcess.png)  
  
Les étapes suivantes expliquent le processus plus en détail:  
  
1.  Contrôleur de domaine virtuel existant démarre sur un hyperviseur qui prend en charge les ID de génération d’ordinateur virtuel.  
  
    1.  Cet ordinateur virtuel n’a aucune valeur ID de génération de machine virtuelle existant sur son objet ordinateur AD DS après la promotion.  
  
    2.  Même si elle est null, la prochaine création d’ordinateur signifie qu’il toujours clones, comme un nouvel ordinateur virtuel-ID de génération ne correspond pas.  
  
    3.  L’ID de génération d’ordinateur virtuel est défini après le prochain redémarrage du contrôleur de domaine et n’est pas répliqué.  
  
2.  L’ordinateur virtuel lit ensuite l’ID de génération d’ordinateur virtuel fourni par le pilote VMGenerationCounter. Il compare les deux ID de génération d’ordinateur virtuel.  
  
    1.  Si les ID correspondent, il ne s’agit pas d’un nouvel ordinateur virtuel et le clonage se poursuit pas. Si un fichier DCCloneConfig.xml existe, le contrôleur de domaine renomme le fichier avec un horodatage pour empêcher le clonage. Le serveur continue de démarrer normalement. Il s’agit de fonctionne de chaque redémarrage de n’importe quel contrôleur de domaine virtuel dans Windows Server 2012.  
  
    2.  Si les deux ID ne correspondent pas, il s’agit d’un nouvel ordinateur virtuel qui contient un NTDS. DIT un précédent contrôleur de domaine (ou il s’agit d’une capture instantanée restaurée). Si un fichier DCCloneConfig.xml existe, le contrôleur de domaine se poursuit avec les opérations de clonage. Si ce n’est pas le cas, il poursuit les opérations de restauration de capture instantanée. Voir [architecture de restauration sécurisée contrôleur de domaine virtualisés](../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_SafeRestoreArch).  
  
    3.  Si l’hyperviseur ne fournit pas un ID de génération d’ordinateur virtuel pour la comparaison, mais il existe un fichier DCCloneConfig.xml, l’invité renomme le fichier et puis démarre en mode DSRM pour protéger le réseau à partir d’un contrôleur de domaine en double. S’il n’existe aucun fichier dccloneconfig.xml, l’invité démarre normalement (avec le risque d’un contrôleur de domaine dupliqué sur le réseau). Pour plus d’informations sur la façon de récupérer ce contrôleur de domaine dupliqué, voir Microsoft l’article [2742970](https://support.microsoft.com/kb/2742970).  
  
3.  Le service NTDS vérifie la valeur du nom de valeur de Registre DWORD VDCisCloning (sous HKEY_Local_Machine\System\CurrentControlSet\Services\Ntds\Parameters).  
  
    1.  Si elle n’existe pas, il s’agit d’une première tentative de clonage de cet ordinateur virtuel. L’invité implémente les garanties de duplication d’objet VDC invalidant le pool RID local et la définition d’un ID d’appel de la réplication du contrôleur de domaine  
  
    2.  S’il est déjà défini sur 0 x 1, il s’agit une «nouvelle» tentative de clonage, où une précédente opération de clonage a échoué. Les mesures de sécurité de duplication objet VDC ne sont pas prises qu’ils avaient déjà exécutée une seule fois avant et altérer inutilement l’invité plusieurs fois.  
  
4.  Le nom de valeur de Registre DWORD IsClone est écrit (sous Hkey_Local_Machine\System\CurrentControlSet\Services\NTDS\Parameters)  
  
5.  Le service NTDS change l’indicateur de démarrage invité à démarrer en Mode de réparation DS pour des redémarrages supplémentaires.  
  
6.  Le service NTDS tente de lire le fichier DcCloneConfig.xml dans un des trois emplacements admis (répertoire de travail DSA, % windir%\NTDS ou média amovible en lecture/écriture, dans l’ordre de la lettre de lecteur, à la racine du lecteur).  
  
    1.  Si le fichier n’existe pas dans n’importe quel emplacement valide, l’invité vérifie si l’adresse IP pour la duplication. Si l’adresse IP n’est pas dupliquée, le serveur démarre normalement. S’il existe une adresse IP dupliquée, l’ordinateur démarre en mode DSRM pour protéger le réseau à partir d’un contrôleur de domaine en double.  
  
    2.  Si le fichier n’existe pas dans un emplacement valide, le service NTDS valide ses paramètres. Si le fichier est vide (ou des paramètres particuliers sont vides) NTDS configure automatiquement les valeurs de ces paramètres.  
  
    3.  Si le fichier DcCloneConfig.xml existe, mais il contient des entrées non valides ou qu’il est illisible, le clonage échoue et l’invité démarre en Mode restauration des Services d’annuaire (DSRM).  
  
7.  L’invité désactive toutes les inscriptions automatiques DNS pour empêcher le détournement des adresses IP et nom de l’ordinateur source.  
  
8.  L’invité arrête le service Netlogon pour éviter toute publication ou réponse de demandes réseau AD DS à partir de clients.  
  
9. Le service NTDS vérifie qu’il n’existe aucun service ou programme installé qui ne font pas partie du fichier DefaultDCCloneAllowList.xml ou CustomDCCloneAllowList.xml  
  
    1.  S’il existe des services ou programmes installés qui ne sont pas dans l’exclusion par défaut de liste verte ou liste verte de l’exclusion personnalisée, le clonage échoue et l’invité démarre en mode DSRM pour protéger le réseau à partir d’un contrôleur de domaine en double.  
  
    2.  S’il n’existe aucune incompatibilité, le clonage se poursuit.  
  
10. Si l’adressage IP automatique doit être utilisé en raison de paramètres de réseau DCCloneConfig.xml vides, l’invité active le protocole DHCP sur les cartes réseau d’obtenir une adresse IP bail d’adresse, le routage réseau et informations de résolution de nom.  
  
11. L’invité localise et contacte le contrôleur de domaine qui exécute l’émulateur de contrôleur de domaine principal rôle FSMO. Il utilise DNS et le protocole DCLocator. Il établit une connexion RPC et appelle la méthode IDL_DRSAddCloneDC pour cloner l’objet ordinateur du contrôleur de domaine.  
  
    1.  Si l’objet l’invité source ordinateur détient autorisation étendue de domaine principal de ««autoriser un contrôleur de domaine créer un clone de lui-même» puis le clonage se poursuit.  
  
    2.  Si l’objet ordinateur source de l’invité ne détient pas qu’une autorisation étendue, le clonage échoue et l’invité démarre en mode DSRM pour protéger le réseau à partir d’un contrôleur de domaine en double.  
  
12. Le nom de l’objet ordinateur AD DS est défini pour correspondre au nom spécifié dans le fichier DCCloneConfig.xml, le cas échéant, ou généré automatiquement sur l’émulateur. Le service NTDS crée l’objet paramètre NTDS adéquat pour le site logique Active Directory approprié.  
  
    1.  S’il s’agit d’un contrôleur de domaine principal le clonage, puis l’invité renomme l’ordinateur local et redémarre. Après le redémarrage, il passe par le biais de l’étape 1-10 à nouveau, puis passe à l’étape 13.  
  
    2.  S’il s’agit d’un contrôleur de domaine réplica le clonage, il existe aucun redémarrage à ce stade.  
  
13. L’invité fournit les paramètres de promotion au service serveur de rôles DS, qui commence la promotion.  
  
14. Le service de serveur de rôles DS arrête tous les services AD DS liés (NTDS, NTFRS/DFSR, KDC, DNS).  
  
15. L’invité force NT5DS synchronisation date / heure (Windows NTP) avec un autre contrôleur de domaine (dans une hiérarchie de Service de temps Windows par défaut, cela signifie à l’aide de l’émulateur). L’invité contacte l’émulateur. Tous les tickets Kerberos existants sont vidés.  
  
16. L’invité configure les services DFSR / NTFRS pour s’exécuter automatiquement. L’invité supprime tous les fichiers de base de données DFSR et NTFRS existants (par défaut: c:\windows\ntfrs et c:\system information\dfsr\\ volume*< database_GUID >*), pour forcer la synchronisation ne faisant pas autorité de SYSVOL au prochain démarrage du service. L’invité ne supprime pas le contenu du fichier de SYSVOL, pour prédéfinir SYSVOL quand la synchronisation démarre plus tard.  
  
17. L’invité est renommé. Le service de serveur de rôles DS sur l’invité commence la configuration d’AD DS (promotion), à l’aide de la NTDS existant. Fichier de base de données DIT une source, plutôt que la base de données de modèle incluse dans c:\windows\system32, comme une promotion normalement.  
  
18. L’invité contacte le détenteur du rôle FSMO du maître RID pour obtenir une nouvelle allocation du pool RID.  
  
19. Le processus de promotion crée un ID d’appel et recrée l’objet Paramètres NTDS pour le contrôleur de domaine cloné (indépendamment du clonage, cela fait partie de la promotion de domaine lorsque vous utilisez un NTDS existant. Base de données DIT).  
  
20. Le service NTDS réplique les objets qui sont manquants, nouveaux ou ont une version supérieure à partir d’un contrôleur de domaine partenaire. NTDS. DIT contient déjà des objets à partir du moment que le contrôleur de domaine source était hors connexion et celles sont utilisés en tant que possible afin de réduire le trafic de réplication entrant. Les partitions de catalogue global sont remplies.  
  
21. Le service DFSR ou FRS démarre et car il n’existe aucune base de données, SYSVOL ne faisant pas autorité synchronise entrant à partir d’un partenaire de réplication. Ce processus réutilise les données existantes dans le dossier SYSVOL, afin de réduire le trafic de réplication.  
  
22. L’invité réactive l’inscription du client DNS maintenant que l’ordinateur est unique nommé et en réseau.  
  
23. L’invité exécute les modules SYSPREP spécifiés par le fichier DefaultDCCloneAllowList.xml <SysprepInformation> élément pour supprimer les références au nom de l’ordinateur précédent et SID.  
  
24. La promotion du clonage est terminé.  
  
    1.  L’invité supprime l’indicateur de démarrage DSRM afin que le prochain redémarrage soit normal.  
  
    2.  L’invité renomme le fichier DCCloneConfig.xml avec un cachet de date-heure ajouté, afin qu’il n’est pas lu à nouveau au prochain démarrage.  
  
    3.  L’invité supprime le nom de valeur de Registre DWORD VdcIsCloning sous HKEY_Local_Machine\System\CurrentControlSet\Services\NTDS\Parameters.  
  
    4.  L’invité affecte DWORD «VdcCloningDone» nom de la valeur du Registre sous HKEY_Local_Machine\System\CurrentControlSet\Services\NTDS\Parameters 0 x 1. Windows n’utilise pas cette valeur, mais fournit plutôt en tant que marqueur pour des tiers.  
  
25. L’invité met à jour l’attribut msDS-GenerationID sur son propre objet contrôleur de domaine cloné pour correspondre à l’invité actuelle ID de génération d’ordinateur virtuel.  
  
26. L’invité redémarre. Il est désormais un contrôleur de domaine de publication normal.  
  
## <a name="BKMK_SafeRestoreArch"></a>Architecture de restauration sécurisée de contrôleur de domaine virtualisés  
  
### <a name="overview"></a>Vue d’ensemble  
Les services AD DS repose sur la plateforme d’hyperviseur pour exposer un identificateur appelé **ID de génération** pour détecter la restauration de capture instantanée d’un ordinateur virtuel. Les services AD DS stocke d’abord la valeur de cet identifiant dans sa base de données (NTDS. DIT) pendant la promotion du contrôleur de domaine. Lorsqu’un administrateur restaure l’ordinateur virtuel à partir d’une capture instantanée précédente, la valeur actuelle de l’ID de génération d’ordinateur virtuel à partir de l’ordinateur virtuel est comparée à la valeur dans la base de données. Si les deux valeurs sont différentes, le contrôleur de domaine réinitialise l’ID d’appel et supprime le pool RID, ce qui empêche la réutilisation des numéros USN ou la création potentielle de principaux de sécurité dupliqués. Il existe deux scénarios où la restauration sécurisée peut se produire:  
  
-   Quand un contrôleur de domaine virtuel est démarré après la restauration d’une capture instantanée pendant qu’il a été arrêté  
  
-   Quand une capture instantanée est restaurée sur un contrôleur de domaine virtuel en cours d’exécution  
  
    Si le contrôleur de domaine virtualisés dans la capture instantanée est dans un état suspendu plutôt que l’arrêt, vous devez redémarrer le service AD DS pour déclencher une nouvelle demande de pool RID. Vous pouvez redémarrer le service AD DS à l’aide du composant logiciel enfichable Services ou à l’aide de Windows PowerShell (Restart-Service NTDS-force).  
  
Les sections suivantes expliquent en détail pour chaque scénario la restauration sécurisée.  
  
### <a name="safe-restore-detailed-processing"></a>Détaillé de la restauration sécurisée  
L’organigramme suivant présente la restauration sécurisée comment se produit lorsqu’un contrôleur de domaine virtuel a démarré après la restauration d’une capture instantanée pendant qu’il a été arrêté.  
  
![Architecture du contrôleur de domaine virtualisé](media/Virtualized-Domain-Controller-Architecture/ADDS_VDC_VirtualizationSafeguardsDuringNormalBoot.png)  
  
1.  Lorsque l’ordinateur virtuel démarre après une restauration de capture instantanée, elle aura le nouvel ID de génération d’ordinateur virtuel fourni par l’hôte hyperviseur en raison de la restauration de capture instantanée.  
  
2.  Le nouvel ID de génération d’ordinateur virtuel à partir de l’ordinateur virtuel est comparé à l’ID de génération d’ordinateur virtuel dans la base de données. Étant donné que les deux ID ne correspondent pas, il utilise des dispositifs (voir l’étape 3 dans la section précédente). Une fois la restauration appliquée, l’ID de génération définie sur son objet ordinateur AD DS est mis à jour pour correspondre au nouvel ID fourni par l’hôte hyperviseur.  
  
3.  L’invité utilise des dispositifs de protection en:  
  
    1.  Invalidant le pool RID local.  
  
    2.  Définissant un nouvel ID d’appel pour la base de données du contrôleur de domaine.  
  
> [!NOTE]  
> Cette partie de la restauration sécurisée se superpose au processus de clonage. Bien que ce processus concerne la restauration sécurisée d’un contrôleur de domaine virtuel après son démarrage après une restauration de capture instantanée, les mêmes étapes se produisent pendant le processus de clonage.  
  
Le diagramme suivant montre comment les dispositifs de protection de virtualisation empêchent la divergence induite par la restauration USN quand une capture instantanée est restaurée sur un contrôleur de domaine virtuel en cours d’exécution.  
  
![Architecture du contrôleur de domaine virtualisé](media/Virtualized-Domain-Controller-Architecture/ADDS_VDC_VirtualizationSafeguardsDuringSnapShotRestore.png)  
  
> [!NOTE]  
> L’illustration précédente est simplifiée pour expliquer les concepts.  
  
1.  À l’heure T1, l’administrateur de l’hyperviseur prend un instantané de DC1 virtuel. DC1 à ce stade a la valeur USN (**highestCommittedUsn** en pratique) de la valeur 100, InvocationId (représentée en tant qu’ID dans le diagramme précédent) A (en pratique il serait GUID). La valeur de savedVMGID est l’ID de génération dans le fichier DIT du contrôleur de domaine (stocké sur l’objet ordinateur du contrôleur de domaine dans un attribut nommé **msDS-GenerationId**). Le VMGID est la valeur actuelle de l’ID de génération disponible à partir du pilote d’ordinateur virtuel. Cette valeur est fournie par l’hyperviseur.  
  
2.  À un moment ultérieur T2, 100 utilisateurs sont ajoutés à ce contrôleur de domaine (prendre en compte les utilisateurs en tant qu’un exemple de mises à jour qui pourrait ont été effectuées sur ce contrôleur de domaine entre temps T1 et T2; ces mises à jour peuvent être en fait un mélange de créations d’utilisateurs, les créations de groupe, mises à jour du mot de passe, les mises à jour de l’attribut, etc.). Dans cet exemple, chaque mise à jour consomme une seule valeur USN (même si dans la pratique, la création d’un utilisateur consomme plusieurs valeurs USN). Avant de valider ces mises à jour, DC1 vérifie si la valeur d’ID de génération dans sa base de données (savedVMGID) est identique à la valeur actuelle disponible à partir du pilote (VMGID). Ils sont identiques, aucune restauration n’a eu lieu encore, afin que les mises à jour sont validées et USN déplace jusqu'à 200, indiquant que la prochaine mise à jour peut utiliser la valeur USN 201. Il n’existe aucun changement dans InvocationId, savedVMGID ou VMGID. Ces mises à jour sont répliquées vers DC2 au prochain cycle de réplication. DC2 met à jour haut filigrane (et **UptoDatenessVector**) représentée ici simplement sous la forme DC1(A) @USN = 200. Autrement dit, DC2 connaît toutes les mises à jour à partir de DC1 dans le contexte de l’InvocationId A jusqu'à la valeur USN 200.  
  
3.  À l’heure T3, la capture instantanée prise à l’heure T1 est appliquée sur DC1. DC1 a été restauré. ainsi, sa valeur USN repasse à 100, indiquant qu’il pourrait utiliser USN de 101 à associer à des mises à jour ultérieures. Toutefois, à ce stade, la valeur de VMGID est différente sur les hyperviseurs qui prennent en charge les ID de génération.  
  
4.  Par la suite, lorsque DC1 effectue une mise à jour, il vérifie si la valeur d’ID de génération qu’il contient dans sa base de données (savedVMGID) est identique à la valeur du pilote d’ordinateur virtuel (VMGID). Dans ce cas, il n'est pas le même, afin de DC1 déduit en tant qu’une restauration et déclenche les dispositifs de protection; en d’autres termes, il réinitialise son InvocationId (ID = B) et supprime le pool RID (non illustré dans le diagramme précédent). Il enregistre la nouvelle valeur de VMGID dans sa base de données et valide ces mises à jour (USN de 101 - 250) dans le contexte de la nouvelle B. InvocationId Au prochain cycle de réplication, DC2 sait rien de DC1 dans le contexte de l’InvocationId B, il demande tout à partir de DC1 associé InvocationID B. Par conséquent, les mises à jour effectuées sur DC1 à la suite de l’application de la capture instantanée seront converger en toute sécurité. En outre, l’ensemble des mises à jour qui ont été exécutées sur DC1 à T2 (qui ont été perdus sur DC1 après la restauration de la capture instantanée) répliquait dans DC1 lors de la prochaine réplication planifiée, car ils avaient répliqués sur DC2 (comme indiqué par la ligne en pointillé sur DC1).  
  
Une fois que l’invité utilise des dispositifs, NTDS réplique les différences entre les objets d’Active Directory faisant à partir d’un contrôleur de domaine partenaire. Le vecteur de mise à la destination du service d’annuaire est mis à jour en conséquence. L’invité synchronise ensuite SYSVOL:  
  
-   Si vous utilisez FRS, l’invité arrête le service NTFRS et définit la valeur de Registre D2 BURFLAGS. Il démarre ensuite le service NTFRS, qui réplique faisant entrantes, en réutilisant données SYSVOL existantes inchangées quand cela est possible.  
  
-   Si vous utilisez DFSR, l’invité arrête le service DFSR et supprime les fichiers de base de données DFSR (emplacement par défaut: %systemroot%\system volume information\dfsr\\*<database GUID>*). Il démarre ensuite le service DFSR, qui réplique faisant entrantes, en réutilisant données SYSVOL existantes inchangées quand cela est possible.  
  
> [!NOTE]  
> -   Si l’hyperviseur ne fournit pas un ID de génération d’ordinateur virtuel pour la comparaison, l’hyperviseur ne prend pas en charge les dispositifs de protection et l’invité fonctionne comme un contrôleur de domaine virtualisé qui exécute Windows Server 2008 R2 ou version antérieure. L’invité implémente la protection de mise en quarantaine de restauration USN s’il existe une tentative de démarrage d’une réplication avec des valeurs USN qui n’est pas postérieures le dernier USN la plus élevée par le contrôleur de domaine partenaire. Pour plus d’informations sur la protection de mise en quarantaine de restauration USN, voir [USN et restauration USN](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv(WS.10).aspx)  
  


