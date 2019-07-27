---
ms.assetid: d44d4430-41e2-483a-9685-92610cdef32a
title: Fonctionnement des plug-ins de mise à jour adaptée aux clusters
ms.topic: article
ms.prod: windows-server-threshold
manager: dongill
ms.author: jgerend
author: JasonGerend
ms.date: 04/28/2017
ms.technology: storage-failover-clustering
description: Comment utiliser les plug-ins pour coordonner les mises à jour lors de l’utilisation de la mise à jour adaptée aux clusters dans Windows Server pour installer des mises à jour sur un cluster.
ms.openlocfilehash: bd31a6056376b04fcb5a4a941b81a363548a2209
ms.sourcegitcommit: 6f968368c12b9dd699c197afb3a3d13c2211f85b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68544511"
---
# <a name="how-cluster-aware-updating-plug-ins-work"></a>Fonctionnement des plug-ins de mise à jour adaptée aux clusters

>S’applique à : Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

[Mise à jour adaptée aux clusters](cluster-aware-updating.md) (La mise à jour adaptée aux clusters utilise des plug-ins pour coordonner l’installation des mises à jour sur les nœuds d’un cluster de basculement. Cette rubrique fournit des informations sur l’utilisation\-des plug\--ins intégrés de la\-mise à jour adaptée aux clusters ou autres plug-ins que vous installez pour la mise à jour adaptée aux

## <a name="BKMK_INSTALL"></a>Installer un plug\--in  
Un plug\--in autre que les plug\--ins par défaut installés avec \(la mise à jour adaptée aux clusters **Microsoft. WindowsUpdatePlugin** et **Microsoft. HotfixPlugin** \) doit être installé séparément. Si la mise à jour adaptée\-aux clusters est utilisée en\-mode de mise à jour automatique, le plug-in doit être installé sur tous les nœuds du cluster. Si la mise à jour adaptée\-aux clusters est utilisée en\-mode de mise à jour à distance, la connexion doit être installée sur l’ordinateur coordinateur de mise à jour distante. Un plug\--in que vous installez peut avoir des conditions d’installation supplémentaires sur chaque nœud.  
  
Pour installer un plug\--in, suivez les instructions de l'\-éditeur de plug-in. Pour inscrire manuellement un plug\--in à l’aide de la mise à jour adaptée aux clusters, exécutez\-l’applet de commande [Register-CauPlugin](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/register-cauplugin) sur chaque ordinateur sur lequel le plug-in est installé.  
  
## <a name="specify-a-plug-in-and-plug-in-arguments"></a>Spécifier des arguments de plug-in et de plug-\-in\-  
  
### <a name="specify-a-cau-plug-in"></a>Spécifier une connexion\-de la mise à jour adaptée aux clusters

Dans l’interface utilisateur de la mise à jour\-adaptée aux clusters\-, sélectionnez un plug-\-in dans la liste déroulante des plug-ins disponibles quand vous utilisez la mise à jour adaptée aux clusters pour effectuer les actions suivantes:  
  
-   Appliquer des mises à jour au cluster  
  
-   Obtenir un aperçu des mises à jour du cluster  
  
-   Configurer les options\-de mise à jour automatique du cluster  
  
Par défaut, la mise à jour\-adaptée aux clusters sélectionne le plug-in **Microsoft. WindowsUpdatePlugin**. Toutefois, vous pouvez spécifier n’importe\-quel plug-in installé et inscrit auprès de la mise à jour adaptée aux clusters.

> [!TIP]  
> Dans l’interface utilisateur de la mise à jour adaptée aux clusters, vous ne pouvez spécifier qu’un seul plug-in\-pour la mise à jour adaptée aux clusters ou pour appliquer les mises à jour lors d’une exécution de mise à jour. À l’aide des applets de commande PowerShell de la mise à jour adaptée\-aux clusters, vous pouvez spécifier un ou plusieurs plug-ins. Si vous devez installer plusieurs types de mises à jour sur le cluster, il est généralement plus efficace de spécifier plusieurs plug-\-ins dans une exécution de mise à jour, plutôt que d’utiliser une exécution de mise à\-jour distincte pour chaque plug-in. Par exemple, les redémarrages de nœuds seront généralement moins nombreux.

À l’aide des applets de commande PowerShell de la mise à jour adaptée aux clusters qui sont répertoriées\-dans le tableau suivant, vous pouvez spécifier un ou plusieurs plug-ins pour une exécution ou une analyse de mise à jour en passant le paramètre **– CauPluginName** . Vous pouvez spécifier plusieurs noms\-de plug-in en les séparant par des virgules. Si vous spécifiez plusieurs\-plug-ins, vous pouvez également contrôler la\-façon dont les plug-ins s’influencent les uns les autres pendant une exécution de mise à jour en spécifiant les  **\-RunPluginsSerially**,  **\-StopOnPluginFailure**, paramètres et **– SeparateReboots** . Pour plus d’informations sur l’utilisation\-de plusieurs plug-ins, utilisez les liens fournis à la documentation de l’applet de commande dans le tableau suivant.  
  
|Applet de commande|Description|  
|----------|---------------|  
|[Add-CauClusterRole](https://docs.microsoft.com/en-us/powershell/module/clusterawareupdating/add-cauclusterrole)|Ajoute le rôle en cluster de la mise à jour\-adaptée aux clusters qui fournit la fonctionnalité de mise à jour automatique au cluster spécifié.|  
|[Invoke-CauRun](https://docs.microsoft.com/en-us/powershell/module/clusterawareupdating/invoke-caurun)|Effectue une analyse des nœuds de cluster à la recherche des mises à jour applicables et installe ces mises à jour via une Exécution de mise à jour sur le cluster spécifié.|  
|[Invoke-CauScan](https://docs.microsoft.com/en-us/powershell/module/clusterawareupdating/invoke-causcan)|Effectue une analyse des nœuds de cluster à la recherche des mises à jour applicables et retourne une liste de l'ensemble initial des mises à jour qui peuvent être appliquées à chaque nœud du cluster spécifié.|  
|[Set-CauClusterRole](https://docs.microsoft.com/en-us/powershell/module/clusterawareupdating/set-cauclusterrole)|Définit les propriétés de configuration du rôle en cluster de la Mise à jour adaptée aux clusters sur le cluster spécifié.|  
  
Si vous ne spécifiez pas de paramètre\-de connexion de la mise à jour adaptée aux clusters à l'\-aide de ces applets de commande, la valeur par défaut est le plug-in **Microsoft. WindowsUpdatePlugin**.  
  
### <a name="specify-cau-plug-in-arguments"></a>Spécifier les arguments\-de la mise à jour adaptée aux clusters  
Quand vous configurez les options d’exécution de mise à jour, vous pouvez spécifier un \(ou\) plusieurs arguments de paires\- *\=nom/valeur* pour le plug-in sélectionné à utiliser. Par exemple, dans l'interface utilisateur de la Mise à jour adaptée aux clusters, vous pouvez spécifier plusieurs arguments comme suit :  
  
**Nom1\=valeur1; Nom2\=valeur2; Nom3\=valeur3**  
  
Ces *paires\=nom-valeur* doivent être significatives pour\-le plug-in que vous spécifiez. Pour certains plug\--ins, les arguments sont facultatifs.  
  
La syntaxe des arguments du plug\--in de la mise à jour adaptée aux clusters respecte les règles générales suivantes:  
  
-   Les *paires\=nom-valeur* multiples sont séparées par des points-virgules.  
  
-   Une valeur qui contient des espaces est entourée par des guillemets, par exemple : **Nom1\=«valeur avec espaces»** .  
  
-   La syntaxe exacte de *value* dépend du plug-\-in.  
  
Pour spécifier des\-arguments de plug-in à l’aide des applets de commande PowerShell de la mise à jour adaptée aux clusters qui prennent en charge le paramètre **– CauPluginParameters** , transmettez un paramètre de la forme:  
  
**\-CauPluginArguments @ {nom1\=value1; Nom2\=valeur2; Nom3\=valeur3}**  
  
Vous pouvez également utiliser une table de hachage PowerShell prédéfinie. Pour spécifier des\-arguments de plug-in pour plusieurs\-plug-ins, transmettez plusieurs tables de hachage d’arguments, séparées par des virgules. Transmettez les\-arguments du plug-in\-dans le plug-in dans l’ordre spécifié dans **CauPluginName**.  
  
### <a name="specify-optional-plug-in-arguments"></a>Spécifier des arguments\-de plug-in facultatifs  
Les plug\--ins \(installés par la mise à jour adaptée aux clusters **Microsoft. WindowsUpdatePlugin** et **Microsoft. HotfixPlugin** \) fournissent des options supplémentaires que vous pouvez sélectionner. Dans l’interface utilisateur de la mise à jour adaptée aux clusters, celles-ci apparaissent dans une page **options supplémentaires** après avoir configuré les options d’exécution de mise à jour pour le plug-in\-. Si vous utilisez les applets de commande PowerShell de la mise à jour adaptée aux clusters, ces options sont configurées en tant qu’arguments plug-in\-facultatifs. Pour plus d’informations, voir [Utiliser Microsoft.WindowsUpdatePlugin](#BKMK_WUP) et [Utiliser Microsoft.HotfixPlugin](#BKMK_HFP) plus loin dans cette rubrique.  
  
## <a name="manage-plug-ins-using-windows-powershell-cmdlets"></a>Gérer les\-plug-ins à l’aide des applets de commande Windows PowerShell  
  
|Applet de commande|Description|  
|----------|---------------|  
|[CauPlugin](https://docs.microsoft.com/en-us/powershell/module/clusterawareupdating/get-cauplugin)|Récupère des informations sur un ou plusieurs plug-\-ins de mise à jour logicielle inscrits sur l’ordinateur local.|  
|[Register-CauPlugin]((https://docs.microsoft.com/en-us/powershell/module/clusterawareupdating/register-cauplugin))|Inscrit un plug-in\-de mise à jour logicielle de la mise à jour adaptée aux clusters sur l’ordinateur local.|  
|[Unregister-CauPlugin](https://docs.microsoft.com/en-us/powershell/module/clusterawareupdating/unregister-cauplugin)|Supprime un plug-in\-de mise à jour logicielle de la liste des plug-\-ins qui peuvent être utilisés par la mise à jour adaptée aux clusters. **Remarque :** Les plug\--ins installés avec la mise \(à jour adaptée aux clusters **Microsoft. WindowsUpdatePlugin** et **Microsoft. HotfixPlugin** \) ne peuvent pas être désinscrits.|  
  
## <a name="BKMK_WUP"></a>Utilisation de Microsoft. WindowsUpdatePlugin  

Le plug-\-in par défaut pour la mise à jour adaptée aux clusters, **Microsoft. WindowsUpdatePlugin**, effectue les actions suivantes:
- Communique avec l'Agent de mise à jour automatique Windows Update sur chaque nœud de cluster de basculement pour appliquer les mises à jour nécessaires pour les produits Microsoft qui sont en cours d'exécution sur chaque nœud.
- Installe les mises à jour de cluster directement à partir de Windows Update ou Microsoft Update,\-ou à \(partir\) d’un serveur WSUS local Windows Server Update Services.
- Installe uniquement les mises à jour GDR \(\) (General Distribution Release) sélectionnées. Par défaut, le plug\--in applique uniquement les mises à jour logicielles importantes. Aucune configuration n'est demandée. La configuration par défaut télécharge et installe les mises à jour importantes relatives aux correctifs logiciels grand public sur chaque nœud. 

> [!NOTE]
> Pour appliquer d’autres mises à jour que les mises à jour logicielles importantes sélectionnées par défaut \(par des mises à jour de pilotes\), vous pouvez configurer un paramètre de\-plug-in facultatif. Pour plus d'informations, voir [Configurer la chaîne de requête de l'Agent de mise à jour automatique Windows Update](#BKMK_QUERY).

### <a name="requirements"></a>Configuration requise

- S’il est utilisé \(\) , le cluster de basculement et l’ordinateur du coordinateur de mise à jour distante doivent remplir les conditions requises pour la mise à jour adaptée aux clusters et la configuration requise pour la gestion à distance indiquée dans [Configuration requise et meilleures pratiques](cluster-aware-updating-requirements.md)
- Consultez les [recommandations relatives à l'application des mises à jour Microsoft](cluster-aware-updating-requirements.md#BKMK_BP_WUA), puis effectuez les changements nécessaires dans votre configuration de la fonctionnalité Microsoft Update pour les nœuds de cluster de basculement.
- Pour de meilleurs résultats, nous vous recommandons d’exécuter la mise \(à\) jour adaptée aux clusters Best Practices Analyzer BPA pour vous assurer que le cluster et l’environnement de mise à jour sont configurés correctement pour appliquer les mises à jour avec la mise à jour adaptée aux clusters Pour plus d'informations, voir [Tester l'état de préparation aux mises à jour par la Mise à jour adaptée aux clusters](cluster-aware-updating-requirements.md#BKMK_BPA).

> [!NOTE]
> Les mises à jour qui demandent l'acceptation des termes du contrat de licence Microsoft ou qui imposent une interaction de l'utilisateur sont exclues. Elles doivent être installées manuellement.

### <a name="additional-options"></a>Options supplémentaires

Si vous le souhaitez, vous pouvez spécifier les\-arguments de plug-in suivants pour augmenter ou restreindre l’ensemble des mises à jour appliquées par\-le plug-in:
- Pour configurer le plug\--in pour appliquer les mises à jour recommandées en plus des mises à jour importantes sur chaque nœud, dans l’interface utilisateur de la mise à jour adaptée aux clusters, dans la page **options supplémentaires** , sélectionnez les **mises à jour recommandées de la même façon que pour recevoir les mises à jour importantes** case à cocher.
<br>Vous pouvez également configurer l’argument de plug-in\-«true» « **IncludeRecommendedUpdates»\=** .
- Pour configurer le plug\--in afin de filtrer les types de mises à jour GDR appliquées à chaque nœud du cluster, spécifiez une chaîne de requête de l’agent\-de Windows Update à l’aide d’un argument de plug-in **QueryString** . Pour plus d'informations, voir [Configurer la chaîne de requête de l'Agent de mise à jour automatique Windows Update](#BKMK_QUERY).

### <a name="BKMK_QUERY"></a>Configuration de la chaîne de requête de l’agent Windows Update  
Vous pouvez configurer un argument\-de plug-in pour le\-plug-in par défaut, **Microsoft. WindowsUpdatePlugin**, qui se \(compose\) d’une chaîne de requête agent WUA Windows Update. Cette instruction utilise l'API WUA pour identifier un ou plusieurs groupes de mises à jour Microsoft à appliquer à chaque nœud, en fonction de critères de sélection spécifiques. Vous pouvez combiner plusieurs critères à l'aide d'une logique ET ou d'une logique OU. La chaîne de requête WUA est spécifiée dans un\-argument de plug-in comme suit:  
  
**QueryString\="Critère1\=value1 et\/ou Criterion2\=value2 et\/ou..."**  
  
Par exemple, **Microsoft.WindowsUpdatePlugin** sélectionne automatiquement les mises à jour importantes à l’aide d’un argument **QueryString** par défaut construit à l’aide des critères **IsInstalled**, **Type**, **IsHidden**et **IsAssigned** :  
  
**QueryString\=«IsInstalled\=0 and type\=«Software» et IsHidden\=0 and IsAssigned\=1»**  
  
Si vous spécifiez un argument **QueryString** , il est utilisé à la place de la **chaîne** de connexion par défaut qui\-est configurée pour le plug-in.  
  
#### <a name="example-1"></a>Exemple 1
  
Pour configurer un **argument QueryString** qui installe une mise à jour spécifique identifiée par l’ID *f6ce46c1\-\-971C\-\-43f9 a2aa 783df125f003*:  
  
**QueryString\="UpdateId\=" f6ce46c1\-971C\-43f9a2aa783df125f003\-'etisInstalled\=0"\-**  
  
> [!NOTE]  
> L’exemple précédent est valide pour appliquer des mises à jour à l’aide\-de l’Assistant Mise à jour adaptée aux clusters. Si vous souhaitez installer une mise à jour spécifique en configurant\-les options de mise à jour automatique avec l’interface utilisateur de la mise à jour adaptée aux clusters ou en utilisant l’applet de commande **Add\-CauClusterRole** ou **Set\-CauClusterRole**PowerShell, vous devez mettre en forme Valeur UpdateID avec deux guillemets simples\-:  
>   
> **QueryString\="UpdateId\=' 'f6ce46c1\-971C\-43f9\-\=a2aa783df125f003\-' 'et IsInstalled 0"**  
  
#### <a name="example-2"></a>Exemple 2
  
Pour configurer un argument **QueryString** qui installe uniquement les pilotes :  
  
**QueryString\="IsInstalled\=0 and type\='Driver’and IsHidden\=0"**  
  
Pour plus d’informations sur les chaînes de requête pour\-le plug-in par défaut, **Microsoft. WindowsUpdatePlugin**, les critères \(de recherche tels que **isInstalled**\)et la syntaxe que vous pouvez inclure dans la requête les chaînes, consultez la section relative aux critères de recherche dans les informations de référence sur l' [API Windows Update Agent (WUA)](https://go.microsoft.com/fwlink/p/?LinkId=223304).  
  
## <a name="BKMK_HFP"></a>Utilisez Microsoft. HotfixPlugin  
Le plug\--in **Microsoft. HotfixPlugin** peut être utilisé pour appliquer les mises à \(jour\) \(LDR de la version de distribution limitée Microsoft, également appelées\) correctifs logiciels, autrefois appelées QFE. Téléchargez indépendamment pour résoudre des problèmes logiciels Microsoft spécifiques. Le plug-in installe les mises à jour à partir d’un dossier racine sur un partage de fichiers SMB et peut également être\-personnalisé pour appliquer des mises à jour de pilotes, de microprogrammes et de BIOS non Microsoft.

> [!NOTE]
> Les correctifs logiciels sont parfois disponibles en téléchargement à partir de Microsoft dans les Articles de la base de connaissances, mais ils\-sont également fournis aux clients en fonction des besoins.

### <a name="requirements"></a>Configuration requise

- S’il est utilisé \(\) , le cluster de basculement et l’ordinateur du coordinateur de mise à jour distante doivent remplir les conditions requises pour la mise à jour adaptée aux clusters et la configuration requise pour la gestion à distance indiquée dans [Configuration requise et meilleures pratiques](cluster-aware-updating-requirements.md)
- Consultez [Recommandations pour l'utilisation de Microsoft.HotfixPlugin](cluster-aware-updating-requirements.md#BKMK_BP_HF).
- Pour de meilleurs résultats, nous vous recommandons d’exécuter la mise \(à\) jour adaptée aux clusters Best Practices Analyzer le modèle BPA pour vous assurer que le cluster et l’environnement de mise à jour sont configurés correctement pour appliquer les mises à jour à l’aide de la mise à jour Pour plus d'informations, voir [Tester l'état de préparation aux mises à jour par la Mise à jour adaptée aux clusters](cluster-aware-updating-requirements.md#BKMK_BPA).
- Obtenir les mises à jour à partir du serveur de publication, les copier ou les extraire vers un dossier \(\) racine\) de correctifs du partage \(de fichiers SMB de serveur SMB qui prend en charge au moins SMB 2,0 et qui est accessible par l’ensemble du cluster les nœuds et l’ordinateur \(coordinateur\-de mise à jour distante si\)la mise à jour adaptée aux clusters est utilisée. Pour plus d'informations, voir [Configurer la structure d'un dossier racine de correctifs logiciels](#BKMK_HF_ROOT) plus loin dans cette rubrique. 

    > [!NOTE]
    > Par défaut, ce plug\--in installe uniquement les correctifs logiciels avec les extensions de nom de fichier suivantes:. msu,. msi et. msp.

- Copiez \(le fichier DefaultHotfixConfig. XML fourni dans le dossier **% systemroot%\\system32\\WindowsPowerShell\\v 1.0\\modules\\ClusterAwareUpdating** sur un ordinateur sur lequel les outils de la\) mise à jour adaptée aux clusters sont installés dans le dossier racine de correctifs logiciels que vous avez créé et sous lequel vous avez extrait les correctifs logiciels. Par exemple, copiez le fichier de configuration dans  *\\ \\la\\racine\\des\\correctifs logiciels MyFileServer*. 

    > [!NOTE]
    > Pour installer la plupart des correctifs logiciels fournis par Microsoft, ainsi que les autres mises à jour, vous pouvez utiliser le fichier de configuration de correctif logiciel par défaut en l'état. Si votre scénario l'impose, vous pouvez personnaliser le fichier de configuration dans le cadre d'une tâche avancée. Le fichier de configuration peut inclure des règles personnalisées, par exemple pour gérer les fichiers de correctifs logiciels qui ont des extensions spécifiques ou pour définir des comportements pour des conditions de sortie spécifiques. Pour plus d'informations, voir [Personnaliser le fichier de configuration de correctif logiciel](#BKMK_CONFIG_FILE) plus loin dans cette rubrique.

### <a name="configuration"></a>Configuration

Configurez les paramètres suivants. Pour plus d'informations, voir les liens vers les sections présentes plus loin dans cette rubrique.
- Chemin d'accès du dossier racine de correctifs logiciels partagé, qui contient les mises à jour à appliquer, ainsi que le fichier de configuration de correctifs logiciels. Vous pouvez taper ce chemin d’accès dans l’interface utilisateur de la mise à jour adaptée\-aux clusters ou configurer le **chemin d'\=accès HotfixRootFolderPath\<>** argument de connexion PowerShell. 

   > [!NOTE]
   > Vous pouvez spécifier le dossier racine de correctifs logiciels en tant que chemin d’accès au dossier local ou en tant que chemin d’accès UNC sous la forme  *\\ \\\\ServerName\\share RootFolderName*. Un chemin\-d’accès d’espace de noms DFS autonome ou basé sur un domaine peut être utilisé. Toutefois, les fonctionnalités\-de plug-in qui vérifient les autorisations d’accès dans le fichier de configuration de correctif logiciel sont incompatibles avec un chemin d’accès d’espace de noms DFS. par conséquent, si vous en configurez un, vous devez désactiver les autorisations vérifier l’accès à l’aide de l’interface utilisateur de la mise à jour adaptée aux clusters Argument de plug-in\- **«\=true» DisableAclChecks** .
- Paramètres sur le serveur qui héberge le dossier racine de correctifs logiciels pour vérifier les autorisations appropriées pour accéder au dossier et garantir l’intégrité des données accessibles à partir du \(dossier partagé SMB signature SMB\)ou chiffrement SMB. Pour plus d'informations, voir [Restreindre l'accès au dossier racine de correctifs logiciels](#BKMK_ACL).

### <a name="additional-options"></a>Options supplémentaires

- Éventuellement, configurez le\-plug-in afin que le chiffrement SMB soit appliqué lors de l’accès aux données à partir du partage de fichiers de correctifs logiciels. Dans l’interface utilisateur de la mise à jour adaptée aux clusters, dans la page **options supplémentaires** , sélectionnez l’option **exiger le chiffrement SMB pour accéder au dossier racine de correctifs logiciels** ou configurez l’argument de plug-in\-PowerShell **«true» RequireSMBEncryption\=** . 
  > [!IMPORTANT]
  > Vous devez effectuer des étapes de configuration supplémentaires sur le serveur SMB pour garantir l'intégrité des données via une signature SMB ou un chiffrement SMB. Pour plus d’informations, voir l’Étape 4 dans [Restreindre l'accès au dossier racine de correctifs logiciels](#BKMK_ACL). Si vous sélectionnez l'option permettant d'appliquer le chiffrement SMB, et si le dossier racine de correctifs logiciels n'est pas configuré pour être accessible via le chiffrement SMB, l'Exécution de mise à jour échoue.
- Éventuellement, désactivez les vérifications par défaut de l'adéquation des autorisations pour le dossier racine de correctifs logiciels et le fichier de configuration de correctif logiciel. Dans l’interface utilisateur de la mise à jour adaptée aux clusters, sélectionnez **désactiver la vérification de l’accès administrateur au dossier racine de correctifs logiciels et au fichier de configuration**, ou configurez l’argument de plug-in **\=** \-«true» DisableAclChecks.
- Éventuellement, configurez l’argument **HotfixInstallerTimeoutMinutes\= <Integer>**  pour spécifier la durée pendant laquelle\-le plug-in attend que le processus d’installation du correctif soit retourné. \(La valeur par défaut est 30 minutes.\) Par exemple, pour spécifier un délai d’expiration de deux heures, **définissez\=HotfixInstallerTimeoutMinutes 120**.
- Éventuellement, configurez l’argument\- **HotfixConfigFileName \= <name>**  plug in pour spécifier un nom pour le fichier de configuration de correctif logiciel qui se trouve dans le dossier racine de correctifs logiciels. Si aucune indication n'est spécifiée, le nom par défaut DefaultHotfixConfig.xml est utilisé.
  
### <a name="BKMK_HF_ROOT"></a>Configurer une structure de dossier racine de correctifs logiciels

Pour que le correctif\-logiciel soit opérationnel, les correctifs logiciels doivent être stockés dans\-une structure bien définie dans un dossier \(\)racine de correctifs de partage de fichiers SMB, et\-vous devez configurer la connexion de correctif logiciel avec le chemin d’accès au dossier racine de correctifs logiciels à l’aide de l’interface utilisateur de la mise à jour adaptée aux clusters ou des cmdlets PowerShell Ce chemin d’accès est passé au\-plug-in en tant qu’argument **HotfixRootFolderPath** . Vous disposez de plusieurs structures au choix pour le dossier racine de correctifs logiciels, en fonction de vos besoins de mise à jour, comme le montrent les exemples suivants. Les fichiers ou dossiers qui n'adhèrent pas à la structure sont ignorés.  
  
#### <a name="example-1---folder-structure-used-to-apply-hotfixes-to-all-cluster-nodes"></a>Exemple 1: structure de dossiers utilisée pour appliquer des correctifs logiciels à tous les nœuds de cluster
  
Pour spécifier que les correctifs s’appliquent à tous les nœuds de cluster, copiez-les dans un dossier nommé **CAUHotfix\_** sous le dossier racine de correctifs logiciels. Dans cet exemple, l'  argument de\-plug-in HotfixRootFolderPath est défini  *\\\\sur \\MyFileServer\\\\hotfixs root*. Le **dossier\_CAUHotfix All** contient trois mises à jour avec les extensions. msu,. msi et. msp, qui seront appliquées à tous les nœuds du cluster. Les noms de fichiers des mises à jour sont uniquement utilisés à des fins d'illustration.  
  
> [!NOTE]  
> Dans cet exemple et les exemples suivants, le fichier de configuration de correctif logiciel ayant le nom par défaut DefaultHotfixConfig.xml est représenté à son emplacement approprié dans le dossier racine de correctifs logiciels.  
  
```
\\MyFileServer\Hotfixes\Root\   
   DefaultHotfixConfig.xml  
   CAUHotfix_All\   
      Update1.msu   
      Update2.msi   
      Update3.msp  
      ...  
```  
  
#### <a name="example-2---folder-structure-used-to-apply-certain-updates-only-to-a-specific-node"></a>Exemple 2: structure de dossiers utilisée pour appliquer uniquement certaines mises à jour à un nœud spécifique
  
Pour spécifier les correctifs logiciels qui s'appliquent uniquement à un nœud spécifique, utilisez un sous-dossier sous le dossier racine de correctifs logiciels ayant le nom du nœud. Utilisez le nom NetBIOS du nœud de cluster, par exemple *ContosoNode1*. Déplacez ensuite les mises à jour qui s'appliquent uniquement à ce nœud vers ce sous-dossier. Dans l’exemple suivant, l'  argument de\-plug-in HotfixRootFolderPath est défini  *\\\\sur \\MyFileServer\\\\hotfixs root*. Les mises à jour dans le dossier **\_CAUHotfix All** s’appliquent à tous les nœuds de cluster, et la *mise à jour spécifique\_\_à node1. msu* est appliquée uniquement à *ContosoNode1*.  
  
```
\\MyFileServer\Hotfixes\Root\   
   DefaultHotfixConfig.xml  
   CAUHotfix_All\   
      Update1.msu   
      Update2.msi   
      Update3.msp  
      ...  
   ContosoNode1\   
      Node1_Specific_Update.msu   
      ...  
```  
  
#### <a name="example-3---folder-structure-used-to-apply-updates-other-than-msu-msi-and-msp-files"></a>Exemple 3-structure de dossiers utilisée pour appliquer des mises à jour autres que des fichiers. msu,. msi et. msp
  
Par défaut, **Microsoft.HotfixPlugin** applique uniquement les mises à jour ayant l'extension .msu, .msi ou .msp. Toutefois, certaines mises à jour peuvent avoir d'autres extensions et demander des commandes d'installation distinctes. Par exemple, vous pouvez être amené à appliquer une mise à jour de microprogramme avec l'extension .exe à un nœud dans un cluster. Vous pouvez configurer le dossier racine de correctifs logiciels avec un sous-dossier qui indique\-un type de mise à jour non par défaut spécifique à installer. Vous devez également configurer une règle d'installation de dossier correspondante qui spécifie la commande d'installation de l'élément `<FolderRules>` dans le fichier XML de configuration de correctif logiciel.  
  
Dans l’exemple suivant, l'  argument de\-plug-in HotfixRootFolderPath est défini  *\\\\sur \\MyFileServer\\\\hotfixs root*. Plusieurs mises à jour sont appliquées à l'ensemble des nœuds de cluster. En outre, la mise à jour de microprogramme *SpecialHotfix1.exe* est appliquée à *ContosoNode1* à l'aide de *FolderRule1*. Pour plus d’informations sur la configuration de *FolderRule1* dans le fichier de configuration de correctif logiciel, voir [Personnaliser le fichier de configuration de correctif logiciel](#BKMK_CONFIG_FILE) plus loin dans cette rubrique.  
  
```
\\MyFileServer\Hotfixes\Root\   
   DefaultHotfixConfig.xml  
   CAUHotfix_All\   
      Update1.msu   
      Update2.msi   
      Update3.msp  
      ...  
  
   ContosoNode1\   
      FolderRule1\  
          SpecialHotfix1.exe  
      ...  
```

### <a name="BKMK_CONFIG_FILE"></a>Personnaliser le fichier de configuration de correctif logiciel  
Le fichier de configuration de correctif logiciel contrôle la façon dont **Microsoft.HotfixPlugin** installe les types de fichier de correctif logiciel spécifiques dans un cluster de basculement. Le schéma XML du fichier de configuration est défini dans HotfixConfigSchema.xsd, qui se trouve dans le dossier suivant d'un ordinateur où sont installés les outils de la Mise à jour adaptée aux clusters :  
  
**dossier ClusterAwareUpdating modules\\\\\\\\%systemroot%\\system32 WindowsPowerShell v 1.0**  
  
Pour personnaliser le fichier de configuration de correctif logiciel, copiez l'exemple de fichier de configuration DefaultHotfixConfig.xml depuis cet emplacement vers le dossier racine de correctifs logiciels, puis effectuez les changements appropriés pour votre scénario.  
  
> [!IMPORTANT]  
> Pour appliquer la plupart des correctifs logiciels fournis par Microsoft, ainsi que les autres mises à jour, vous pouvez utiliser le fichier de configuration de correctif logiciel par défaut en l'état. La personnalisation du fichier de configuration de correctif logiciel est une tâche effectuée uniquement dans les scénarios d'usage avancés.  
  
Par défaut, le fichier XML de configuration de correctif logiciel définit les règles d'installation et les conditions de sortie des deux catégories suivantes de correctifs logiciels :  
  
-   Fichiers de correctifs logiciels avec les\-extensions que le plug- \(in peut installer par défaut. msu,. msi\)et. msp.  
  
    Ils sont définis en tant qu'éléments `<ExtensionRules>` dans l'élément `<DefaultRules>`. Il existe un élément `<Extension>` pour chacun des types de fichier pris en charge par défaut. La structure XML générale est la suivante :  
  
    ```xml  
    <DefaultRules>  
        <ExtensionRules>  
          <Extension name="MSI">  
            <!-- Template and ExitConditions elements for installation of .msi files follow -->  
             ...  
          </Extension>  
          <Extension name="MSU">  
            <!-- Template and ExitConditions elements for installation of .msu files follow -->  
             ...  
          </Extension>  
          <Extension name="MSP">  
            <!-- Template and ExitConditions elements for installation of .msp files follow -->  
             ...  
          </Extension>  
             ...  
       </ExtensionRules>  
    </DefaultRules>  
    ```  
  
    Si vous devez appliquer certains types de mise à jour à l'ensemble des nœuds de cluster de votre environnement, vous pouvez définir des éléments `<Extension>` supplémentaires.  
  
-   Correctifs logiciels ou autres fichiers de mise à jour qui ne sont pas des fichiers. msi,. msu ou.\-MSP, par exemple des pilotes non Microsoft, des microprogrammes et des mises à jour du BIOS.  
  
    Chaque type\-de fichier autre `<Folder>` que celui par défaut est configuré en `<FolderRules>` tant qu’élément dans l’élément. L'attribut name de l'élément `<Folder>` doit être identique au nom d'un dossier dans le dossier racine de correctifs logiciels, qui contient les mises à jour du type correspondant. Le dossier peut se trouver dans le dossier **CAUHotfix\_All** ou dans un\-dossier spécifique à un nœud. Par exemple, si *FolderRule1* est configuré dans le dossier racine de correctifs logiciels, configurez l'élément suivant du fichier XML pour définir le modèle d'installation et les conditions de sortie des mises à jour dans ce dossier :  
  
    ```xml  
    <FolderRules>  
          <Folder name="FolderRule1">  
            <!-- Template and ExitConditions elements for installation of updates in FolderRule1 follow -->  
             ...  
          </Folder>  
          ...  
    </FolderRules>  
    ```  
  
Les tableaux suivants décrivent les attributs `<Template>` et les sous-éléments `<ExitConditions>` possibles.  
  
|Attribut `<Template>`|Description|  
|--------------------------|---------------|  
|`path`|Chemin d'accès complet du programme d'installation pour le type de fichier défini dans l'attribut `<Extension name>` .<br /><br />Pour spécifier le chemin d’accès d’un fichier de mise à jour dans la structure du dossier racine de correctifs logiciels, utilisez `$update$`.|  
|`parameters`|Chaîne de paramètres obligatoires et facultatifs pour le programme spécifié dans `path`.<br /><br />Pour spécifier un paramètre qui représente le chemin d’accès d’un fichier de mise à jour dans la structure du dossier racine de correctifs logiciels, utilisez `$update$`.|  
  
|Sous-élément `<ExitConditions>`|Description|  
|---------------------------------|---------------|  
|`<Success>`|Définit un ou plusieurs codes de sortie qui indiquent que la mise à jour spécifiée a réussi. Il s'agit d'un sous-élément obligatoire.|  
|`<Success_RebootRequired>`|Définit éventuellement un ou plusieurs codes de sortie qui indiquent que la mise à jour spécifiée a réussi et que le nœud doit redémarrer. <br>**Remarque :** L’élément `<Folder>` peut éventuellement contenir l’attribut `alwaysReboot`. Si cet attribut est défini, il indique que, si un correctif installé par cette règle retourne l'un des codes de sortie défini dans `<Success>`, il est interprété en tant que condition de sortie `<Success_RebootRequired>` .|  
|`<Fail_RebootRequired>`|Définit éventuellement un ou plusieurs codes de sortie qui indiquent que la mise à jour spécifiée a échoué et que le nœud doit redémarrer.|  
|`<AlreadyInstalled>`|Définit éventuellement un ou plusieurs codes de sortie qui indiquent que la mise à jour spécifiée n'a pas été appliquée, car elle est déjà installée.|  
|`<NotApplicable>`|Définit éventuellement un ou plusieurs codes de sortie qui indiquent que la mise à jour spécifiée n'a pas été appliquée, car elle ne s'applique pas au nœud de cluster.|  
  
> [!IMPORTANT]  
> Tout code de sortie qui n'est pas explicitement défini dans `<ExitConditions>` est interprété comme un échec de la mise à jour. Par ailleurs, le nœud ne redémarre pas.  
  
### <a name="BKMK_ACL"></a>Restreindre l’accès au dossier racine de correctifs logiciels  
Vous devez suivre plusieurs étapes pour configurer le serveur de fichiers SMB et le partage de fichiers. Cela vous permet de sécuriser les fichiers du dossier racine de correctifs logiciels et le fichier de configuration de correctif logiciel en autorisant uniquement leur accès dans le cadre de **Microsoft.HotfixPlugin**. Ces étapes activent plusieurs fonctionnalités qui permettent d'éviter une éventuelle falsification des fichiers de correctifs logiciels et la compromission du cluster de basculement.  
  
Les étapes générales sont les suivantes :  
  
1.  Identifier le compte d’utilisateur utilisé pour les exécutions de mise à jour à\-l’aide du plug-in  
  
2.  Configurer ce compte d'utilisateur dans les groupes nécessaires sur un serveur de fichiers SMB  
  
3.  Configurer les autorisations d'accès au dossier racine de correctifs logiciels  
  
4.  Configurer les paramètres relatifs à l'intégrité des données SMB  
  
5.  Activer une règle de Pare-feu Windows sur le serveur SMB  
  
#### <a name="step-1-identify-the-user-account-that-is-used-for-updating-runs-by-using-the-hotfix-plug-in"></a>Étape 1. Identifiez le compte d’utilisateur utilisé pour les exécutions de mise à jour à\-l’aide du plug-in de correctif logiciel
  
Le compte utilisé dans la mise à jour adaptée aux clusters pour vérifier les paramètres de sécurité lors d’une exécution de mise à jour à l’aide de **Microsoft. HotfixPlugin** varie\-selon que la mise à jour adaptée aux clusters est utilisée en mode de mise à jour à distance\-ou en mode de mise à jour automatique:  
  
-   **Mode\-de mise à jour à distance** compte qui dispose de privilèges d’administrateur sur le cluster pour afficher un aperçu et appliquer les mises à jour.  
  
-   **Mode\-de mise à jour automatique** nom de l’objet ordinateur virtuel configuré dans Active Directory pour le rôle en cluster de la mise à jour adaptée aux clusters. Il s'agit soit du nom d'un objet ordinateur virtuel préconfiguré dans Active Directory pour le rôle en cluster de la Mise à jour adaptée aux clusters, soit du nom généré par la Mise à jour adaptée aux clusters pour le rôle en cluster. Pour obtenir le nom s’il est généré par la mise à jour adaptée aux clusters, exécutez l’applet de commande PowerShell **obtenir\-CauClusterRole** . Dans la sortie, **ResourceGroupName** est le nom du compte de l’objet ordinateur virtuel généré.  
  
#### <a name="step-2-configure-this-user-account-in-the-necessary-groups-on-an-smb-file-server"></a>Étape 2. Configurer ce compte d'utilisateur dans les groupes nécessaires sur un serveur de fichiers SMB
  
> [!IMPORTANT]  
> Vous devez ajouter le compte utilisé pour les Exécutions de mise à jour en tant que compte d'administrateur local sur le serveur SMB. Si cela n'est pas autorisé en raison des stratégies de sécurité de votre organisation, configurez ce compte avec les autorisations nécessaires sur le serveur SMB à l'aide de la procédure suivante.  
  
##### <a name="to-configure-a-user-account-on-the-smb-server"></a>Pour configurer un compte d'utilisateur sur le serveur SMB  
  
1.  Ajoutez le compte utilisé pour les Exécutions de mise à jour au groupe Utilisateurs du modèle COM distribué et à l'un des groupes suivants : Utilisateurs avec pouvoir, Opérateurs de serveur ou Opérateurs d'impression.  
  
2.  Pour activer les autorisations WMI nécessaires pour le compte, démarrez la console de gestion WMI sur le serveur SMB. Démarrez PowerShell, puis tapez la commande suivante:  
  
    ```  
    wmimgmt.msc  
    ```  
  
3.  Dans l’arborescence de la console\-, cliquez avec le bouton droit sur **contrôle \(WMI local\)** , puis cliquez sur **Propriétés**.  
  
4.  Cliquez sur **Sécurité**, puis développez **Racine**.  
  
5.  Cliquez sur **CIMV2**, puis sur **Sécurité**.  
  
6.  Ajoutez le compte utilisé pour les Exécutions de mise à jour à la liste **Noms de groupes ou d'utilisateurs** .  
  
7.  Accordez les autorisations **Méthodes d'exécution** et **Appel à distance autorisé** au compte utilisé pour les Exécutions de mise à jour.  
  
#### <a name="step-3-configure-permissions-to-access-the-hotfix-root-folder"></a>Étape 3. Configurer les autorisations d'accès au dossier racine de correctifs logiciels
  
Par défaut, lorsque vous tentez d’appliquer des mises à jour, le\-correctif logiciel de vérifie la configuration des autorisations du système de fichiers NTFS pour accéder au dossier racine de correctifs logiciels. Si les autorisations d’accès au dossier ne sont pas configurées correctement, une exécution de mise\-à jour à l’aide du plug-in de correctif logiciel peut échouer.  
  
Si vous utilisez la configuration par défaut du plug-\-in de correctif logiciel dans, assurez-vous que les autorisations d’accès au dossier remplissent les conditions suivantes.  
  
-   Le groupe Utilisateurs dispose d'une autorisation d'accès en lecture.  
  
-   Si le plug\--in applique des mises à jour avec l’extension. exe, le groupe utilisateurs dispose de l’autorisation EXECUTE.  
  
-   Seuls certains principaux de sécurité sont autorisés \(, mais n’ont\) pas besoin d’une autorisation d’écriture ou de modification. Les principaux autorisés sont le groupe Administrateurs local, ainsi que les comptes SYSTEME, CREATEUR PROPRIETAIRE et TrustedInstaller. Les autres comptes ou groupes ne sont pas autorisés à écrire ou changer les autorisations du dossier racine de correctifs logiciels.  
  
Si vous le souhaitez, vous pouvez désactiver les vérifications précédentes que\-le plug-in effectue par défaut. Vous pouvez le faire de deux façons :  
  
-   Si vous utilisez les applets de commande de la mise à jour adaptée aux clusters, configurez l’argument **DisableAclChecks\=«true»** dans le paramètre **CauPluginArguments** pour le plug-in\-de correctif logiciel.  
  
-   Si vous utilisez l'interface de la Mise à jour adaptée aux clusters, sélectionnez l'option **Désactiver la vérification des droits d'accès d'administrateur au dossier racine de correctif logiciel et au fichier de configuration** dans la page **Options de mise à jour supplémentaires** de l'Assistant qui permet de configurer les options d'Exécution de mise à jour.  
  
Cependant, dans de nombreux environnements, nous vous recommandons d'utiliser la configuration par défaut pour appliquer ces vérifications.  
  
#### <a name="step-4-configure-settings-for-smb-data-integrity"></a>Étape 4. Configurer les paramètres relatifs à l'intégrité des données SMB
  
Pour vérifier l’intégrité des données dans les connexions entre les nœuds de cluster et le partage de fichiers SMB\-, vous devez activer les paramètres sur le partage de fichiers SMB pour la signature SMB ou le chiffrement SMB. Le chiffrement SMB, qui offre une sécurité accrue et de meilleures performances dans de nombreux environnements, est pris en charge à partir de Windows Server 2012. Vous pouvez activer l'un ou l'autre de ces paramètres, ou les deux à la fois, comme suit :  
  
-   Pour activer la signature SMB, voir la procédure décrite dans l’[article 887429](https://support.microsoft.com/kb/887429) de la Base de connaissances Microsoft.  
  
-   Pour activer le chiffrement SMB pour le dossier partagé SMB, exécutez l’applet de commande PowerShell suivante sur le serveur SMB:  
  
    ```PowerShell  
    Set-SmbShare <ShareName> -EncryptData $true  
    ```  
  
    Où <*ShareName*> est le nom du dossier partagé SMB.  
  
Éventuellement, pour appliquer l’utilisation du chiffrement SMB dans les connexions au serveur SMB, sélectionnez l’option **exiger le chiffrement SMB pour accéder au dossier racine de correctifs logiciels** dans l’interface utilisateur de la mise à jour adaptée aux clusters, ou configurez le plug-in **«true» RequireSMBEncryption\=** \-dans argument en utilisant les applets de commande PowerShell de la mise à jour adaptée aux clusters.  
  
> [!IMPORTANT]  
> Si vous sélectionnez l'option permettant d'appliquer le chiffrement SMB, et si le dossier racine de correctifs logiciels n'est pas configuré pour les connexions qui utilisent le chiffrement SMB, l'Exécution de mise à jour échoue.  
  
#### <a name="step-5-enable-a-windows-firewall-rule-on-the-smb-server"></a>Étape 5. Activer une règle de Pare-feu Windows sur le serveur SMB
  
Vous devez activer la **gestion \(à distance du serveur\-de\) fichiers SMB dans** la règle du pare-feu Windows sur le serveur de fichiers SMB. Cette option est activée par défaut dans Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012.  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Vue d’ensemble de la mise à jour adaptée aux clusters](cluster-aware-updating.md)
  
-   [Applets de commande Windows PowerShell pour la mise à jour adaptée aux clusters](https://docs.microsoft.com/en-us/powershell/module/clusterawareupdating)  
  
-   [Référence du plug-in de mise à jour adaptée aux clusters](https://msdn.microsoft.com/library/hh418084.aspx)  
  
