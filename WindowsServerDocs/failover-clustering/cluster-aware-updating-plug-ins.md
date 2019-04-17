---
ms.assetid: d44d4430-41e2-483a-9685-92610cdef32a
title: "Fonctionnement des plug-ins de mise à jour adaptée aux clusters"
ms.topic: article
ms.prod: windows-server-threshold
manager: dongill
ms.author: jgerend
author: JasonGerend
ms.date: 4/28/2017
ms.technology: storage-failover-clustering
description: "Comment utiliser des plug-ins pour coordonner les mises à jour lorsque vous utilisez la mise à jour adaptée aux clusters dans Windows Server pour installer les mises à jour sur un cluster."
ms.openlocfilehash: eb606dfbe6596ecabd35e6ac36624fab2b4436b9
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# <a name="how-cluster-aware-updating-plug-ins-work"></a>Fonctionnement des plug-ins de mise à jour adaptée aux clusters

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016, Windows Server2012R2, Windows Server2012

[Mise à jour adaptée aux clusters](cluster-aware-updating.md) (adaptée aux clusters) utilise des plug-ins pour coordonner l’installation des mises à jour sur les nœuds d’un cluster de basculement. Cette rubrique fournit des informations sur l’utilisation des intégrée adaptée aux clusters un plug-ins ou autres un plug-ins que vous installez pour adaptée aux clusters.

## <a name="BKMK_INSTALL"></a>Installer un plug-in  
Un plug-in autre que l’installation d’un plug-ins par défaut qui sont installés avec adaptée aux clusters \ (**Microsoft.WindowsUpdatePlugin** et **Microsoft.HotfixPlugin**\) doit être installé séparément. Si adaptée aux clusters est utilisée en mode de mise à jour intégrée, un plug-in doit être installé sur tous les nœuds de cluster. Si adaptée aux clusters est utilisée en mode de mise à jour remote\, un plug-in doit être installé sur l’ordinateur coordinateur de mise à jour à distance. Un plug-in que vous installez peut avoir des exigences d’installation supplémentaires sur chaque nœud.  
  
Pour installer un plug-in, suivez les instructions de l’éditeur d’un plug-in. Pour inscrire manuellement un plug-in adaptée aux clusters, exécutez le [Register-CauPlugin](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/register-cauplugin) applet de commande sur chaque ordinateur sur lequel un plug-in est installé.  
  
## <a name="specify-a-plug-in-and-plug-in-arguments"></a>Spécifiez des arguments d’un plug-in et un plug-in  
  
### <a name="specify-a-cau-plug-in"></a>Spécifiez une adaptée aux clusters un plug-in

Dans l’UI CAU, vous sélectionnez un plug-in à partir d’une liste déroulantes un plug-ins disponibles quand vous utilisez adaptée aux clusters pour effectuer les actions suivantes:  
  
-   Appliquer des mises à jour au cluster  
  
-   Aperçu des mises à jour pour le cluster  
  
-   Configurer les options de mise à jour intégrée de cluster  
  
Par défaut, adaptée aux clusters sélectionne un plug-in **Microsoft.WindowsUpdatePlugin**. Toutefois, vous pouvez spécifier les plug-in qui est installé et inscrit avec adaptée aux clusters.

> [!TIP]  
> Dans l’UI CAU, vous ne pouvez spécifier qu’un seul un plug-in pour adaptée aux clusters à utiliser pour afficher un aperçu ou appliquer des mises à jour durant une exécution de mise à jour. En utilisant les applets de commande PowerShell adaptée aux clusters, vous pouvez spécifier un ou plusieurs un plug-ins. Si vous devez installer plusieurs types de mises à jour sur le cluster, il est généralement plus efficace de spécifier plusieurs un plug-ins dans une exécution de mise à jour, plutôt qu’utiliser une mise à jour exécution distincte pour chaque un plug-in. Par exemple, moins de redémarrages de nœuds seront produisent généralement.

En utilisant les applets de commande PowerShell adaptée aux clusters sont répertoriés dans le tableau suivant, vous pouvez spécifier un ou plusieurs un plug-ins pour une exécution de mise à jour ou une analyse en passant le **– CauPluginName** paramètre. Vous pouvez spécifier plusieurs noms d’un plug-in en les séparant par des virgules. Si vous spécifiez plusieurs un plug-ins, vous pouvez également contrôler comment les plug-ins s’influencent mutuellement durant une exécution de mise à jour en spécifiant le **\-RunPluginsSerially**, **\-StopOnPluginFailure**, et **– SeparateReboots** paramètres. Pour plus d’informations sur l’utilisation de plusieurs un plug-ins, utilisez les liens vers la documentation de l’applet de commande dans le tableau suivant.  
  
|Applet de commande|Description|  
|----------|---------------|  
|[Add-CauClusterRole](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/add-cauclusterrole)|Ajoute le rôle en cluster adaptée aux clusters qui fournit la fonctionnalité de mise à jour intégrée pour le cluster spécifié.|  
|[Invoke-CauRun](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/invoke-caurun)|Effectue une analyse des nœuds de cluster pour les mises à jour applicables et installe ces mises à jour par le biais d’une exécution de mise à jour sur le cluster spécifié.|  
|[Invoke-CauScan](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/invoke-causcan)|Effectue une analyse des nœuds de cluster pour les mises à jour applicables et retourne une liste de l’ensemble initial de mises à jour qui peuvent être appliquées à chaque nœud du cluster spécifié.|  
|[Set-CauClusterRole](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/set-cauclusterrole)|Définit les propriétés de configuration pour le rôle en cluster adaptée aux clusters sur le cluster spécifié.|  
  
Si vous ne spécifiez pas un paramètre un plug-in adaptée à l’aide de ces applets de commande, la valeur par défaut est un plug-in **Microsoft.WindowsUpdatePlugin**.  
  
### <a name="specify-cau-plug-in-arguments"></a>Spécifier des arguments un plug-in d’adaptée aux clusters  
Lorsque vous configurez les options d’exécution de mise à jour, vous pouvez spécifier un ou plusieurs *nom\ = valeur* paires \(arguments\) pour le texte sélectionné un plug-in à utiliser. Par exemple, dans l’UI CAU, vous pouvez spécifier plusieurs arguments comme suit:  
  
**Name1\ = valeur1; name2\ = valeur2. Name3\ = valeur3**  
  
Ces *nom\ = valeur* paires doivent avoir un sens pour un plug-in que vous spécifiez. Pour certains un plug-ins, les arguments sont facultatifs.  
  
La syntaxe des arguments un plug-in adaptée aux clusters respecte les règles générales:  
  
-   Plusieurs *nom\ = valeur* paires sont séparées par des points-virgules.  
  
-   Une valeur qui contient des espaces est entourée par des guillemets, par exemple: **Name1\ = «Value with Spaces»**.  
  
-   La syntaxe exacte de *valeur* dépend de l’installation d’un plug-in.  
  
Pour spécifier des arguments un plug-in à l’aide des applets de commande PowerShell adaptée aux clusters qui prennent en charge le **– CauPluginParameters** paramètre, passez un paramètre sous la forme:  
  
**\-CauPluginArguments @{Name1\ = valeur1; name2\ = valeur2. Name3\ = valeur3}**  
  
Vous pouvez également utiliser une table de hachage PowerShell prédéfinie. Pour spécifier des arguments un plug-in pour plusieurs un plug-in, passez plusieurs tables de hachage d’arguments, séparés par des virgules. Passez les arguments un plug-in dans l’ordre d’un plug-in est spécifié dans **CauPluginName**.  
  
### <a name="specify-optional-plug-in-arguments"></a>Spécifier des arguments d’un plug-in facultatifs  
L’installation d’un plug-ins adaptée \ (**Microsoft.WindowsUpdatePlugin** et **Microsoft.HotfixPlugin**\) offrent des options supplémentaires que vous pouvez sélectionner. Dans l’UI CAU, ceux-ci s’affichent sur une **des Options supplémentaires** page après avoir configuré les options d’exécution de mise à jour pour un plug-in. Si vous utilisez les applets de commande PowerShell adaptée aux clusters, ces options sont configurées en tant qu’arguments un plug-in facultatifs. Pour plus d’informations, voir [utiliser Microsoft.WindowsUpdatePlugin](#BKMK_WUP) et [utiliser Microsoft.HotfixPlugin](#BKMK_HFP) plus loin dans cette rubrique.  
  
## <a name="manage-plug-ins-using-windows-powershell-cmdlets"></a>Gérer un plug-ins à l’aide des applets de commande Windows PowerShell  
  
|Applet de commande|Description|  
|----------|---------------|  
|[Get-CauPlugin](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/get-cauplugin)|Récupère des informations sur un ou plusieurs logiciels mise à jour d’un plug-ins qui sont inscrites sur l’ordinateur local.|  
|[Register-CauPlugin]((https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/register-cauplugin))|Inscrit un logiciel adaptée aux clusters mise à jour d’un plug-in sur l’ordinateur local.|  
|[Unregister-CauPlugin](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/unregister-cauplugin)|Supprime un logiciel de mise à jour d’un plug-in de la liste d’un plug-ins qui peuvent être utilisés par adaptée aux clusters. **Remarque:** l’installation d’un plug-ins qui sont installés avec adaptée aux clusters \ (**Microsoft.WindowsUpdatePlugin** et **Microsoft.HotfixPlugin**\) ne peut pas être désinscrits.|  
  
## <a name="BKMK_WUP"></a>À l’aide de Microsoft.WindowsUpdatePlugin  

La valeur par défaut, un plug-in pour adaptée aux clusters, **Microsoft.WindowsUpdatePlugin**, effectue les actions suivantes:
- Communique avec l’Agent Windows Update sur chaque nœud de cluster de basculement pour appliquer des mises à jour qui sont nécessaires pour les produits Microsoft qui sont exécutent sur chaque nœud.
- Installe les mises à jour de cluster directement à partir de Windows Update ou MicrosoftUpdate, ou à partir d’un serveur de WindowsServerUpdateServices \(WSUS\) dans\ local.
- Installe uniquement sélectionné, distribution générale libérer \(GDR\) les mises à jour. Par défaut, un plug-in applique uniquement les mises à jour logicielles importantes. Aucune configuration n’est requise. La configuration par défaut télécharge et installe les mises à jour GDR importantes sur chaque nœud. 

> [!NOTE]
> Pour appliquer des mises à jour autres que les mises à jour logicielles importantes qui sont sélectionnées par défaut \ (par exemple, pilote updates\), vous pouvez configurer un paramètre facultatif un plug-in. Pour plus d’informations, voir [configurer la chaîne de requête de l’Agent Windows Update](#BKMK_QUERY).

### <a name="requirements"></a>Configuration requise

- Le cluster de basculement et à distance coordinateur de mise à jour ordinateur \(if used\) doivent respecter la configuration requise pour adaptée aux clusters et la configuration requise pour la gestion à distance répertoriés dans [configuration requise et meilleures pratiques adaptée aux clusters](cluster-aware-updating-requirements.md).
- Passez en revue [recommandations pour l’application des mises à jour Microsoft](cluster-aware-updating-requirements.md#BKMK_BP_WUA), puis apportez les modifications nécessaires dans votre configuration de MicrosoftUpdate pour les nœuds de cluster de basculement.
- Pour de meilleurs résultats, nous vous recommandons d’exécuter le \(BPA\) adaptée aux clusters Best Practices Analyzer pour vous assurer que l’environnement de cluster et la mise à jour sont correctement configurés pour appliquer des mises à jour à l’aide adaptée aux clusters. Pour plus d’informations, voir [Test CAU updating readiness](cluster-aware-updating-requirements.md#BKMK_BPA).

> [!NOTE]
> Mises à jour qui demandent l’acceptation des termes du contrat de licence Microsoft ou nécessitent l’intervention de l’utilisateur sont exclus, et ils doivent être installés manuellement.

### <a name="additional-options"></a>Options supplémentaires

Si vous le souhaitez, vous pouvez spécifier les arguments pour augmenter ou restreindre l’ensemble des mises à jour appliquées par un plug-in un plug-in suivants:
- Pour configurer le plug-in pour appliquer des mises à jour recommandées en plus des mises à jour importantes sur chaque nœud, dans l’UI CAU, sur le **Options supplémentaires** page, sélectionnez le **la même manière que vous recevez des mises à jour importantes de mises à jour recommandées** case à cocher.
<br>Vous pouvez également configurer le **'IncludeRecommendedUpdates' \ = 'True'** argument un plug-in.
- Pour configurer le plug-in et filtrer les types de mises à jour de correctif logiciel grand public qui sont appliqués à chaque nœud du cluster, spécifiez une chaîne de requête de l’Agent Windows Update à l’aide un **QueryString** argument un plug-in. Pour plus d’informations, voir [configurer la chaîne de requête de l’Agent Windows Update](#BKMK_QUERY).

### <a name="BKMK_QUERY"></a>Configurer la chaîne de requête de l’Agent Windows Update  
Vous pouvez configurer un argument un plug-in pour la valeur par défaut, un plug-in, **Microsoft.WindowsUpdatePlugin**, qui se compose d’une chaîne de requête \(WUA\) l’Agent Windows Update. Cette instruction utilise l’API WUA pour identifier un ou plusieurs groupes de mises à jour Microsoft à appliquer à chaque nœud, en fonction des critères de sélection. Vous pouvez combiner plusieurs critères à l’aide d’une logique et ou ou logique. La chaîne de requête WUA est spécifiée dans un argument un plug-in comme suit:  
  
**QueryString\ = «Criterion1\ = valeur1 and\ / ou Criterion2\ = valeur2 and\ / ou...»**  
  
Par exemple, **Microsoft.WindowsUpdatePlugin** sélectionne automatiquement les mises à jour importantes à l’aide d’une valeur par défaut **QueryString** argument est construit à l’aide de la **IsInstalled**, **Type**, **IsHidden**, et **IsAssigned** critères:  
  
**QueryString\ = «IsInstalled\ = 0 et Type\ = «Logiciel» et IsHidden\ = 0 et IsAssigned\ = 1»**  
  
Si vous spécifiez un **QueryString** argument, il est utilisé à la place de la valeur par défaut **QueryString** qui est configuré pour un plug-in.  
  
#### <a name="example-1"></a>Exemple 1
  
Pour configurer un **QueryString** argument qui installe une mise à jour spécifique identifiée par l’ID *f6ce46c1\-971c\-43f9\-a2aa\-783df125f003*:  
  
**QueryString\ = «UpdateID\ = 'f6ce46c1\-971c\-43f9\-a2aa\-783df125f003' et IsInstalled\ = 0»**  
  
> [!NOTE]  
> L’exemple précédent est valide pour l’application des mises à jour à l’aide de l’Assistant de mise à jour prenant en charge Cluster\. Si vous souhaitez installer une mise à jour spécifique en configurant les options de mise à jour intégrée avec la UI CAU ou en utilisant le **Add-CauClusterRole** ou **Set-CauClusterRole**applet de commande PowerShell, vous devez mettre en forme la valeur UpdateID entre deux guillemets simples single\:  
>   
> **QueryString\ = «UpdateID\ ='' f6ce46c1\-971c\-43f9\-a2aa\-783df125f003'' et IsInstalled\ = 0»**  
  
#### <a name="example-2"></a>Exemple 2
  
Pour configurer un **QueryString** argument qui installe uniquement les pilotes:  
  
**QueryString\ = «IsInstalled\ = 0 et Type\ = 'Pilote' et IsHidden\ = 0»**  
  
Pour plus d’informations sur les chaînes de requête pour la valeur par défaut, un plug-in, **Microsoft.WindowsUpdatePlugin**, les critères de recherche \ (tel que **IsInstalled**\), et la syntaxe que vous pouvez inclure dans les chaînes de requête, voir la section relative aux critères de recherche dans le [l’Agent Windows Update (WUA) API Reference](http://go.microsoft.com/fwlink/p/?LinkId=223304).  
  
## <a name="BKMK_HFP"></a>Utiliser Microsoft.HotfixPlugin  
Un plug-in **Microsoft.HotfixPlugin** peut être utilisé pour appliquer des mises à jour de Microsoft limitée distribution release \(LDR\) \ (également appelées correctifs logiciels et auparavant appelée QFEs\) que vous téléchargez indépendamment pour résoudre les problèmes logiciels Microsoft spécifiques. Le plug-in installe les mises à jour à partir d’un dossier racine sur un partage de fichiers SMB et peut également être personnalisé pour appliquer des mises à jour du BIOS, des microprogrammes et des pilotes autres que Microsoft.

> [!NOTE]
> Parfois, les correctifs sont disponibles en téléchargement à partir de Microsoft dans les articles de la Base de connaissances, mais ils sont également fournis aux clients en fonction des besoins d’as\.

### <a name="requirements"></a>Configuration requise

- Le cluster de basculement et à distance coordinateur de mise à jour ordinateur \(if used\) doivent respecter la configuration requise pour adaptée aux clusters et la configuration requise pour la gestion à distance répertoriés dans [configuration requise et meilleures pratiques adaptée aux clusters](cluster-aware-updating-requirements.md).
- Passez en revue [recommandations pour l’utilisation de Microsoft.HotfixPlugin](cluster-aware-updating-requirements.md#BKMK_BP_HF).
- Pour de meilleurs résultats, nous vous recommandons d’exécuter le modèle \(BPA\) adaptée aux clusters Best Practices Analyzer pour vous assurer que l’environnement de cluster et la mise à jour sont correctement configurés pour appliquer des mises à jour à l’aide adaptée aux clusters. Pour plus d’informations, voir [Test CAU updating readiness](cluster-aware-updating-requirements.md#BKMK_BPA).
- Obtenir les mises à jour de l’éditeur et les copier ou les extraire sur un partage de fichiers SMB \(SMB\) \ (Files\\\r\n\t\t\t\tDossier de racine de correctifs logiciels) qui prend en charge au moins SMB 2.0 et qui est accessible par tous les nœuds du cluster et l’ordinateur coordinateur de mise à jour à distance \ (si adaptée aux clusters est utilisée dans mode\ remote\ mise à jour). Pour plus d’informations, voir [configurer une structure de dossiers racine de correctif logiciel](#BKMK_HF_ROOT) plus loin dans cette rubrique. 

    > [!NOTE]
    > Par défaut, ce plug-in installe uniquement les correctifs logiciels ayant les extensions de nom de fichier suivantes: .msu, .msi et .msp.

- Copiez le fichier DefaultHotfixConfig.xml \ (qui est fourni dans le **%systemroot%\\System32\\WindowsPowerShell\\v1.0\\Modules\\ClusterAwareUpdating** dossier sur un ordinateur sur lequel les outils adaptée aux clusters sont installées\) vers le dossier racine de correctifs logiciels que vous avez créé et dans lequel vous avez extrait ces correctifs. Par exemple, copiez le fichier de configuration *\\\MyFileServer\\Hotfixes\\Root\\*. 

    > [!NOTE]
    > Pour installer la plupart des correctifs logiciels fournis par Microsoft et d’autres mises à jour, le fichier de configuration de correctif logiciel par défaut peut être utilisé sans modification. Si votre scénario l’exige, vous pouvez personnaliser le fichier de configuration comme une tâche avancée. Le fichier de configuration peut inclure des règles personnalisées, par exemple, pour gérer les fichiers de correctifs logiciels qui ont des extensions spécifiques ou pour définir des comportements pour des conditions de sortie spécifiques. Pour plus d’informations, voir [personnaliser le fichier de configuration de correctif logiciel](#BKMK_CONFIG_FILE) plus loin dans cette rubrique.

### <a name="configuration"></a>Configuration

Configurez les paramètres suivants. Pour plus d’informations, voir les liens vers les sections plus loin dans cette rubrique.
- Le chemin d’accès au dossier racine de correctifs logiciels partagé qui contient les mises à jour à appliquer et qui contient le fichier de configuration de correctif logiciel. Vous pouvez taper ce chemin d’accès dans l’UI CAU ou configurer le **HotfixRootFolderPath\ = \ < chemin d’accès >** argument un plug-in PowerShell. 

   > [!NOTE]
   > Vous pouvez spécifier le dossier racine de correctifs logiciels sous la forme d’un chemin d’accès du dossier local ou un chemin UNC sous la forme *\\\ServerName\\Share\\RootFolderName*. Un chemin d’accès basé sur les domain\ ou autonome Namespace DFS peut être utilisé. Toutefois, les fonctionnalités un plug-in qui vérifient les autorisations d’accès dans le fichier de configuration de correctif logiciel sont incompatibles avec un chemin d’accès DFS Namespace, afin de configurer ce dernier, vous devez désactiver la vérification des autorisations d’accès à l’aide de l’UI CAU ou en configurant le **DisableAclChecks\ = 'True'** argument un plug-in.
- Paramètres sur le serveur qui héberge le dossier racine de correctifs logiciels pour vérifier les autorisations appropriées pour accéder au dossier et garantir l’intégrité des données accédées à partir de SMB de dossier partagé \ (signature SMB ou Encryption\ SMB). Pour plus d’informations, voir [restreindre l’accès au dossier racine de correctif logiciel](#BKMK_ACL).

### <a name="additional-options"></a>Options supplémentaires

- Éventuellement, configurez un plug-in afin que le chiffrement SMB est appliqué lors de l’accès aux données à partir du partage de fichier de correctif logiciel. Dans l’UI CAU, sur le **Options supplémentaires** page, sélectionnez le **exiger le chiffrement SMB lors de l’accès du dossier racine de correctifs logiciels** ou configurez le **RequireSMBEncryption\ = 'True'** argument un plug-in PowerShell. 
  > [!IMPORTANT]
  > Vous devez effectuer les étapes de configuration supplémentaires sur le serveur SMB pour activer l’intégrité des données SMB avec la signature SMB ou chiffrement SMB. Pour plus d’informations, voir l’étape4dans [restreindre l’accès au dossier racine de correctif logiciel](#BKMK_ACL). Si vous sélectionnez l’option pour appliquer le chiffrement SMB et le dossier racine de correctifs logiciels n’est pas configuré pour l’accès à l’aide du chiffrement SMB, l’exécution de mise à jour échoue.
- Si vous le souhaitez, désactiver les vérifications par défaut pour les autorisations suffisantes pour le dossier racine de correctifs logiciels et le fichier de configuration de correctif logiciel. Dans l’UI CAU, sélectionnez **désactiver la vérification d’accès d’administrateur pour le fichier de configuration et le dossier racine de correctif logiciel**, ou configurez le **DisableAclChecks\ = 'True'** argument un plug-in.
- Éventuellement, configurez le **HotfixInstallerTimeoutMinutes\ =<Integer>** argument pour spécifier la durée d’attente pour le processus d’installation de correctifs logiciels renvoyer le correctif logiciel un plug-in. \ (La valeur par défaut est 30minutes. \) par exemple, pour spécifier un délai de deux heures, définissez **HotfixInstallerTimeoutMinutes\ = 120**.
- Éventuellement, configurez le **HotfixConfigFileName \ = <name>**un plug-in argument pour spécifier un nom pour le fichier de configuration de correctif logiciel qui se trouve dans le dossier racine de correctifs logiciels. Si non spécifié, le nom par défaut DefaultHotfixConfig.xml est utilisé.
  
### <a name="BKMK_HF_ROOT"></a>Configurer une structure de dossiers racine de correctif logiciel

Pour le correctif logiciel un plug-in pour travailler, correctifs logiciels doivent être stockés dans une structure est bien défini dans un partage de fichiers SMB \ (Files\\\r\n\t\t\t\tDossier racine de correctifs logiciels), et vous devez configurer le correctif logiciel un plug-in avec le chemin d’accès pour le dossier racine de correctifs logiciels à l’aide de l’UI CAU ou les applets de commande PowerShell adaptée aux clusters. Ce chemin d’accès est passé à le plug-in en tant que le **HotfixRootFolderPath** argument. Vous pouvez choisir de plusieurs structures pour le dossier racine de correctifs logiciels, en fonction des besoins de votre mise à jour, comme illustré dans les exemples suivants. Fichiers ou dossiers qui ne sont pas conformes à la structure sont ignorés.  
  
#### <a name="example-1---folder-structure-used-to-apply-hotfixes-to-all-cluster-nodes"></a>Exemple 1: structure de dossiers utilisée pour appliquer des correctifs logiciels à tous les nœuds de cluster
  
Pour spécifier que les correctifs logiciels s’appliquent à tous les nœuds de cluster, copiez-les dans un dossier nommé **CAUHotfix\_All** sous le dossier racine de correctifs logiciels. Dans cet exemple, le **HotfixRootFolderPath** argument un plug-in est défini sur *\\\MyFileServer\\Hotfixes\\Root\\*. Le **CAUHotfix\_All** dossier contient trois mises à jour avec les extensions .msu, .msi et .msp, qui seront appliqués à tous les nœuds de cluster. Les noms de fichiers de mise à jour sont uniquement à des fins d’illustration.  
  
> [!NOTE]  
> Dans cet exemple et les exemples suivants, le fichier de configuration de correctif logiciel avec son nom par défaut DefaultHotfixConfig.xml est indiqué dans son emplacement requis dans le dossier racine de correctifs logiciels.  
  
```
\\MyFileServer\Hotfixes\Root\   
   DefaultHotfixConfig.xml  
   CAUHotfix_All\   
      Update1.msu   
      Update2.msi   
      Update3.msp  
      ...  
```  
  
#### <a name="example-2---folder-structure-used-to-apply-certain-updates-only-to-a-specific-node"></a>Exemple 2: structure de dossiers utilisée pour appliquer certaines mises à jour uniquement à un nœud spécifique
  
Pour spécifier les correctifs logiciels qui s’appliquent uniquement à un nœud spécifique, utilisez un sous-dossier sous le dossier racine de correctifs logiciels portant le nom du nœud. Utiliser le nom NetBIOS du nœud du cluster, par exemple, *ContosoNode1*. Ensuite, déplacez les mises à jour qui s’appliquent uniquement à ce nœud vers ce sous-dossier. Dans l’exemple suivant, la **HotfixRootFolderPath** argument un plug-in est défini sur *\\\MyFileServer\\Hotfixes\\Root\\*. Mises à jour dans le **CAUHotfix\_All** dossier est appliqué à tous les nœuds de cluster, et *Node1\_Specific\_Update.msu* est appliqué uniquement à *ContosoNode1*.  
  
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
  
#### <a name="example-3---folder-structure-used-to-apply-updates-other-than-msu-msi-and-msp-files"></a>Exemple 3 - structure de dossiers utilisée pour appliquer des mises à jour autres que les fichiers .msu, .msi et .msp
  
Par défaut, **Microsoft.HotfixPlugin** s’applique uniquement les mises à jour avec l’extension .msu, .msi ou .msp. Toutefois, certaines mises à jour peuvent avoir d’autres extensions et requièrent des commandes d’installation distinctes. Par exemple, vous devrez peut-être appliquer une mise à jour de microprogramme avec l’extension .exe à un nœud dans un cluster. Vous pouvez configurer le dossier racine de correctifs logiciels avec un sous-dossier qui indique un spécifique, le type de mise à jour autre que celle par défaut doit être installé. Vous devez également configurer une règle d’installation de dossier correspondante qui spécifie la commande d’installation du `<FolderRules>`élément dans le fichier XML de configuration de correctif logiciel.  
  
Dans l’exemple suivant, la **HotfixRootFolderPath** argument un plug-in est défini sur *\\\MyFileServer\\Hotfixes\\Root\\*. Plusieurs mises à jour seront appliqués à tous les nœuds du cluster et une mise à jour du microprogramme *SpecialHotfix1.exe* seront appliqués au *ContosoNode1* à l’aide de *FolderRule1*. Pour plus d’informations sur la configuration *FolderRule1* dans le fichier de configuration de correctif logiciel, voir [personnaliser le fichier de configuration de correctif logiciel](#BKMK_CONFIG_FILE) plus loin dans cette rubrique.  
  
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
La configuration de correctif logiciel comment les contrôles de fichiers **Microsoft.HotfixPlugin** installe les types de fichier de correctif logiciel spécifiques dans un cluster de basculement. Le schéma XML du fichier de configuration est défini dans HotfixConfigSchema.xsd, qui se trouve dans le dossier suivant sur un ordinateur où sont installés les outils adaptée aux clusters:  
  
**Dossier %SystemRoot%\\System32\\WindowsPowerShell\\v1.0\\Modules\\ClusterAwareUpdating**  
  
Pour personnaliser le fichier de configuration de correctif logiciel, copiez l’exemple de fichier de configuration DefaultHotfixConfig.xml depuis cet emplacement pour le dossier racine de correctifs logiciels et effectuez les changements appropriés pour votre scénario.  
  
> [!IMPORTANT]  
> Pour appliquer la plupart des correctifs logiciels fournis par Microsoft et d’autres mises à jour, le fichier de configuration de correctif logiciel par défaut peut être utilisé sans modification. Personnalisation du fichier de configuration de correctif logiciel est une tâche uniquement dans les scénarios d’usage avancés.  
  
Par défaut, le fichier XML de configuration de correctif logiciel définit les règles d’installation et des conditions de sortie pour les deux catégories suivantes de correctifs logiciels:  
  
-   Fichiers de correctifs logiciels ayant les extensions que l’installation d’un plug-in peut installer par défaut \ (files\ .msu, .msi et .msp).  
  
    Ils sont définis en tant que `<ExtensionRules>`éléments dans le `<DefaultRules>`élément. Il existe un `<Extension>`élément pour chacun des types de fichier par défaut de la prise en charge. La structure XML générale est la suivante:  
  
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
  
    Si vous devez appliquer certains types de mise à jour pour tous les nœuds de cluster dans votre environnement, vous pouvez définir d’autres `<Extension>`éléments.  
  
-   Correctif logiciel ou autres fichiers de mise à jour qui ne sont pas .msi, .msu ou .msp fichiers, par exemple, BIOS, des microprogrammes et des pilotes autres que Microsoft met à jour.  
  
    Chaque type de fichier autre que celle par défaut est configuré comme un `<Folder>`élément dans le `<FolderRules>`élément. L’attribut de nom du `<Folder>`élément doit être identique au nom d’un dossier dans le dossier racine de correctifs logiciels qui contiendra les mises à jour du type correspondant. Le dossier peut se trouver dans le **CAUHotfix\_All** dossier ou dans un dossier spécifique node\. Par exemple, si *FolderRule1* est configuré dans le dossier racine de correctifs logiciels, configurez l’élément suivant dans le fichier XML pour définir un modèle d’installation et les conditions pour les mises à jour dans ce dossier de sortie:  
  
    ```xml  
    <FolderRules>  
          <Folder name="FolderRule1">  
            <!-- Template and ExitConditions elements for installation of updates in FolderRule1 follow -->  
             ...  
          </Folder>  
          ...  
    </FolderRules>  
    ```  
  
Les tableaux suivants décrivent les `<Template>`attributs et les éventuelles `<ExitConditions>`sous-éléments.  
  
|`<Template>` attribut|Description|  
|--------------------------|---------------|  
|`path`|Le chemin d’accès complet au programme d’installation pour le type de fichier qui est défini dans le `<Extension name>`attribut.<br /><br />Pour spécifier le chemin d’accès à un fichier de mise à jour dans la structure du dossier racine de correctif logiciel, utilisez `$update$`.|  
|`parameters`|Une chaîne de paramètres obligatoires et facultatifs pour le programme spécifié dans `path`.<br /><br />Pour spécifier un paramètre qui représente le chemin d’accès d’un fichier de mise à jour dans la structure du dossier racine de correctif logiciel, utilisez `$update$`.|  
  
|`<ExitConditions>` sous-élément|Description|  
|---------------------------------|---------------|  
|`<Success>`|Définit un ou plusieurs codes de sortie qui indiquent la mise à jour spécifiée a réussi. Il s’agit d’un sous-élément obligatoire.|  
|`<Success_RebootRequired>`|Définit éventuellement un ou plusieurs codes de sortie qui indiquent que la mise à jour spécifiée a réussi et le nœud doit redémarrer. <br>**Remarque:** le cas échéant, le `<Folder>`élément peut contenir les `alwaysReboot`attribut. Si cet attribut est défini, il indique que, si un correctif installé par cette règle retourne l’un des codes de sortie qui est définie dans `<Success>`, elle est interprétée comme un `<Success_RebootRequired>`condition de sortie.|  
|`<Fail_RebootRequired>`|Définit éventuellement un ou plusieurs codes de sortie qui indiquent que la mise à jour spécifiée a échoué et le nœud doit redémarrer.|  
|`<AlreadyInstalled>`|Définit éventuellement un ou plusieurs codes de sortie qui indiquent que la mise à jour spécifiée n’a pas été appliquée, car il est déjà installé.|  
|`<NotApplicable>`|Définit éventuellement un ou plusieurs codes de sortie qui indiquent que la mise à jour spécifiée n’a pas été appliquée, car il ne s’applique pas au nœud de cluster.|  
  
> [!IMPORTANT]  
> Un code de sortie qui n’est pas explicitement défini dans `<ExitConditions>`est interprétée comme un échec de la mise à jour, le nœud ne redémarre pas.  
  
### <a name="BKMK_ACL"></a>Restreindre l’accès au dossier racine de correctif logiciel  
Vous devez effectuer plusieurs étapes pour configurer le serveur et le fichier de partage de fichiers SMB pour sécuriser les fichiers du dossier racine de correctif logiciel et le fichier de configuration autorisant pour l’accès uniquement dans le contexte de **Microsoft.HotfixPlugin **. Ces étapes activent plusieurs fonctionnalités qui permettent d’empêcher la falsification possibles avec les fichiers de correctifs logiciels d’une façon qui peut compromettre le cluster de basculement.  
  
Les étapes générales sont les suivantes:  
  
1.  Identifier le compte d’utilisateur qui est utilisé pour les exécutions de mise à jour à l’aide de l’installation d’un plug-in  
  
2.  Configurez ce compte d’utilisateur dans les groupes nécessaires sur un serveur de fichiers SMB  
  
3.  Configurer les autorisations d’accès au dossier racine de correctif logiciel  
  
4.  Configurer les paramètres de l’intégrité des données SMB  
  
5.  Activer une règle de pare-feu Windows sur le serveur SMB  
  
#### <a name="step-1-identify-the-user-account-that-is-used-for-updating-runs-by-using-the-hotfix-plug-in"></a>Étape1. Identifier le compte d’utilisateur qui est utilisé pour les exécutions de mise à jour à l’aide du plug-in de correctif logiciel
  
Le compte qui est utilisé pour vérifier les paramètres de sécurité durant une exécution de mise à jour l’aide adaptée **Microsoft.HotfixPlugin** varie selon qu’adaptée aux clusters est utilisée en mode de mise à jour remote\ ou mise à jour intégrée, comme suit:  
  
-   **Mode de mise à jour Remote\** le compte qui dispose des privilèges d’administrateur sur le cluster pour obtenir un aperçu et appliquer des mises à jour.  
  
-   **Mode de mise à jour intégrée** le nom de l’objet ordinateur virtuel qui est configuré dans ActiveDirectory pour l’adaptée aux clusters rôle en cluster. Il s’agit du nom d’un objet ordinateur virtuel préconfiguré dans ActiveDirectory pour le rôle en cluster adaptée aux clusters ou le nom est généré par adaptée aux clusters pour le rôle en cluster. Pour obtenir le nom, s’il est généré par adaptée aux clusters, exécutez le **applet de commande Get-CauClusterRole** applet de commande PowerShell adaptée aux clusters. Dans la sortie, **ResourceGroupName** est le nom du compte d’objet ordinateur virtuel généré.  
  
#### <a name="step-2-configure-this-user-account-in-the-necessary-groups-on-an-smb-file-server"></a>Étape2. Configurez ce compte d’utilisateur dans les groupes nécessaires sur un serveur de fichiers SMB
  
> [!IMPORTANT]  
> Vous devez ajouter le compte qui est utilisé pour les exécutions de mise à jour comme un compte d’administrateur local sur le serveur SMB. Si cela n’est pas autorisé en raison des stratégies de sécurité de votre organisation, configurez ce compte avec les autorisations nécessaires sur le serveur SMB à l’aide de la procédure suivante.  
  
##### <a name="to-configure-a-user-account-on-the-smb-server"></a>Pour configurer un compte d’utilisateur sur le serveur SMB  
  
1.  Ajoutez le compte qui est utilisé pour les exécutions de mise à jour au groupe utilisateurs du modèle COM distribué et à un des groupes suivants: utilisateur avec pouvoir, opérateurs de serveur ou opérateurs d’impression.  
  
2.  Pour activer les autorisations WMI nécessaires pour le compte, démarrez la Console de gestion WMI sur le serveur SMB. Démarrez PowerShell et tapez la commande suivante:  
  
    ```  
    wmimgmt.msc  
    ```  
  
3.  Dans l’arborescence de la console, cliquez droit **contrôle WMI \(Local\)**, puis cliquez sur **propriétés **.  
  
4.  Cliquez sur **sécurité**, puis développez **racine **.  
  
5.  Cliquez sur **CIMV2**, puis cliquez sur **sécurité **.  
  
6.  Ajoutez le compte qui est utilisé pour les exécutions de mise à jour pour le **noms d’utilisateur ou groupe** liste.  
  
7.  Accordez le **exécuter les méthodes** et **activer à distance** autorisations pour le compte qui est utilisé pour les exécutions de mise à jour.  
  
#### <a name="step-3-configure-permissions-to-access-the-hotfix-root-folder"></a>Étape3. Configurer les autorisations d’accès au dossier racine de correctif logiciel
  
Par défaut, lorsque vous essayez d’appliquer des mises à jour, le correctif logiciel un plug-in vérifie la configuration des autorisations de système de fichiers NTFS pour l’accès au dossier racine de correctif logiciel. Si les autorisations d’accès de dossier ne sont pas configurées correctement, une exécution de mise à l’aide du plug-in de correctif logiciel risque d’échouer.  
  
Si vous utilisez la configuration par défaut du correctif un plug-in, assurez-vous que les autorisations d’accès de dossier répondent aux exigences suivantes.  
  
-   Le groupe utilisateurs dispose d’autorisation de lecture.  
  
-   Si un plug-in s’applique des mises à jour avec l’extension .exe, le groupe utilisateurs a l’autorisation d’exécution.  
  
-   Seuls certains principaux de sécurité sont autorisés \ (mais ne sont pas required\) à écrire ou changer les autorisations. Les principaux autorisés sont le local Administrateurs groupe, système, CREATEUR PROPRIETAIRE et TrustedInstaller. Autres comptes ou groupes ne sont pas autorisés à écrire ou changer les autorisations sur le dossier racine de correctifs logiciels.  
  
Si vous le souhaitez, vous pouvez désactiver les vérifications précédentes que l’installation d’un plug-in effectue par défaut. Vous pouvez le faire de deux manières:  
  
-   Si vous utilisez les applets de commande PowerShell adaptée aux clusters, configurez le **DisableAclChecks\ = 'True'** argument dans le **CauPluginArguments** paramètre pour le correctif logiciel un plug-in.  
  
-   Si vous utilisez le UI CAU, sélectionnez le **désactiver la vérification d’accès d’administrateur pour le fichier de configuration et le dossier racine de correctif logiciel** option sur le **les Options de mise à jour supplémentaires** page de l’Assistant qui permet de configurer les options d’exécution de mise à jour.  
  
Toutefois, en tant que meilleure pratique dans de nombreux environnements, nous vous recommandons d’utiliser la configuration par défaut pour appliquer ces vérifications.  
  
#### <a name="step-4-configure-settings-for-smb-data-integrity"></a>Étape4. Configurer les paramètres de l’intégrité des données SMB
  
Pour vérifier l’intégrité des données dans les connexions entre les nœuds de cluster et le partage de fichiers SMB, le correctif logiciel un plug-in nécessite l’activation de paramètres sur le partage de fichiers SMB pour la signature SMB ou chiffrement SMB. Le chiffrement SMB, qui offre une sécurité accrue et meilleures performances dans de nombreux environnements, est pris en charge à partir de Windows Server2012. Vous pouvez activer une ou les deux de ces paramètres, comme suit:  
  
-   Pour activer la signature SMB, voir la procédure décrite dans le [article 887429](http://support.microsoft.com/kb/887429) dans la Base de connaissances Microsoft.  
  
-   Pour activer le chiffrement SMB du dossier partagé SMB, exécutez l’applet de commande PowerShell suivante sur le serveur SMB:  
  
    ```PowerShell  
    Set-SmbShare <ShareName> -EncryptData $true  
    ```  
  
    Où <*nom_partage*> est le nom du dossier partagé SMB.  
  
Si vous le souhaitez, pour appliquer l’utilisation du chiffrement SMB dans les connexions au serveur SMB, sélectionnez le **exiger le chiffrement SMB lors de l’accès du dossier racine de correctifs logiciels** dans l’UI CAU ou configurez le **RequireSMBEncryption\ = 'True'** argument un plug-in à l’aide des applets de commande PowerShell adaptée aux clusters.  
  
> [!IMPORTANT]  
> Si vous sélectionnez l’option pour appliquer le chiffrement SMB et le dossier racine de correctifs logiciels n’est pas configuré pour les connexions qui utilisent le chiffrement SMB, l’exécution de mise à jour échoue.  
  
#### <a name="step-5-enable-a-windows-firewall-rule-on-the-smb-server"></a>Étape5. Activer une règle de pare-feu Windows sur le serveur SMB
  
Vous devez activer le **gestion à distance de serveur de fichiers \(SMB\-in\)** règle dans le pare-feu Windows sur le serveur de fichiers SMB. Ceci est activé par défaut dans Windows Server2016, Windows Server2012R2 et Windows Server2012.  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Vue d’ensemble de la mise à jour adaptée aux clusters](cluster-aware-updating.md)
  
-   [Cluster-Aware mise à jour Windows applets de commande PowerShell](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating)  
  
-   [Adaptée aux clusters référence plug-in de mise à jour](http://msdn.microsoft.com/library/hh418084.aspx)  
  
