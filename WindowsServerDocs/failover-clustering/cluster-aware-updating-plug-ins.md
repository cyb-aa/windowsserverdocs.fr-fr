---
ms.assetid: d44d4430-41e2-483a-9685-92610cdef32a
title: Fonctionnement des plug-ins de la mise à jour adaptée aux clusters
ms.topic: article
ms.prod: windows-server-threshold
manager: dongill
ms.author: jgerend
author: JasonGerend
ms.date: 04/28/2017
ms.technology: storage-failover-clustering
description: Comment utiliser des plug-ins pour coordonner les mises à jour lorsque vous utilisez la mise à jour adaptée aux clusters dans Windows Server pour installer les mises à jour sur un cluster.
ms.openlocfilehash: d09addb5e6787a8386d50570c0d27640646aa587
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854560"
---
# <a name="how-cluster-aware-updating-plug-ins-work"></a>Fonctionnement des plug-ins de la mise à jour adaptée aux clusters

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

[La mise à jour adaptée aux clusters](cluster-aware-updating.md) (adaptée aux clusters) utilise des plug-ins pour coordonner l’installation des mises à jour sur les nœuds d’un cluster de basculement. Cette rubrique fournit des informations sur l’utilisation intégrée\-dans plug d’adaptée aux clusters\-ins ou autres plug\-ins que vous installez pour adaptée aux clusters.

## <a name="BKMK_INSTALL"></a>Installer un plug-\-dans  
Un plug\-dans autre que la valeur par défaut, branchez\-ins installés avec adaptée aux clusters \( **Microsoft.WindowsUpdatePlugin** et **Microsoft.HotfixPlugin** \)doit être installé séparément. Si cette dernière est utilisée dans self\-mode, la prise de la mise à jour\-doit être installée sur tous les nœuds de cluster. Si adaptée aux clusters est utilisée dans à distance\-mode, la prise de la mise à jour\-doit être installée sur l’ordinateur coordinateur de mise à jour à distance. Un plug\-car vous installez peut avoir des exigences d’installation supplémentaires sur chaque nœud.  
  
Pour installer un plug-\-, suivez les instructions à partir de la fiche\-dans l’éditeur. Pour inscrire manuellement un plug\-avec adaptée aux clusters, exécutez le [Register-CauPlugin](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/register-cauplugin) applet de commande sur chaque ordinateur où le plug\-est installé.  
  
## <a name="specify-a-plug-in-and-plug-in-arguments"></a>Spécifiez un plug\-dans et branchez\-dans les arguments  
  
### <a name="specify-a-cau-plug-in"></a>Spécifiez un plug adaptée aux clusters\-dans

Dans l’UI CAU, vous sélectionnez un plug\-dans à partir d’un dépôt\-liste du plug-in disponible vers le bas\-ins lorsque vous utilisez adaptée aux clusters pour effectuer les actions suivantes :  
  
-   Appliquer des mises à jour au cluster  
  
-   Obtenir un aperçu des mises à jour du cluster  
  
-   Configuration de cluster self\-options mise à jour  
  
Par défaut, adaptée aux clusters sélectionne le plug\-dans **Microsoft.WindowsUpdatePlugin**. Toutefois, vous pouvez spécifier n’importe quel plug\-dans la mesure est installé et inscrit avec adaptée aux clusters.

> [!TIP]  
> Dans l’UI CAU, vous ne pouvez spécifier qu’un seul plug\-dans pour adaptée aux clusters à utiliser pour afficher un aperçu ou appliquer des mises à jour durant une exécution de la mise à jour. En utilisant les applets de commande PowerShell d’adaptée aux clusters, vous pouvez spécifier un ou plusieurs plug\-ins. Si vous devez installer plusieurs types de mises à jour sur le cluster, il est généralement plus efficace de spécifier plusieurs plug\-ins dans une exécution de la mise à jour, plutôt qu’à l’aide d’une exécution de la mise à jour distincte pour chaque plug\-dans. Par exemple, les redémarrages de nœuds seront généralement moins nombreux.

En utilisant les applets de commande PowerShell d’adaptée aux clusters qui sont répertoriées dans le tableau suivant, vous pouvez spécifier un ou plusieurs plug\-ins pour une exécution de la mise à jour ou une analyse en passant le **– CauPluginName** paramètre. Vous pouvez spécifier plusieurs plug\-dans les noms en les séparant par des virgules. Si vous spécifiez plusieurs plug\-ins, vous pouvez également contrôler comment le plug\-ins s’influencent mutuellement durant une exécution de la mise à jour en spécifiant le  **\-RunPluginsSerially**,  **\- StopOnPluginFailure**, et **– SeparateReboots** paramètres. Pour plus d’informations sur l’utilisation de plusieurs plug\-ins, utilisez les liens fournis à la documentation de l’applet de commande dans le tableau suivant.  
  
|Applet de commande|Description|  
|----------|---------------|  
|[Add-CauClusterRole](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/add-cauclusterrole)|Ajoute le rôle en cluster adaptée aux clusters qui fournit le libre-service\-mise à jour de la fonctionnalité vers le cluster spécifié.|  
|[Invoke-CauRun](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/invoke-caurun)|Effectue une analyse des nœuds de cluster à la recherche des mises à jour applicables et installe ces mises à jour via une Exécution de mise à jour sur le cluster spécifié.|  
|[Invoke-CauScan](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/invoke-causcan)|Effectue une analyse des nœuds de cluster à la recherche des mises à jour applicables et retourne une liste de l'ensemble initial des mises à jour qui peuvent être appliquées à chaque nœud du cluster spécifié.|  
|[Set-CauClusterRole](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/set-cauclusterrole)|Définit les propriétés de configuration du rôle en cluster de la Mise à jour adaptée aux clusters sur le cluster spécifié.|  
  
Si vous ne spécifiez pas un bouchon adaptée aux clusters\-dans le paramètre à l’aide de ces applets de commande, la valeur par défaut est le plug\-dans **Microsoft.WindowsUpdatePlugin**.  
  
### <a name="specify-cau-plug-in-arguments"></a>Spécifiez adaptée aux clusters Branchez\-dans les arguments  
Lorsque vous configurez les options d’exécution de la mise à jour, vous pouvez spécifier un ou plusieurs *nom\=valeur* paires \(arguments\) pour la connexion sélectionnée\-in à utiliser. Par exemple, dans l'interface utilisateur de la Mise à jour adaptée aux clusters, vous pouvez spécifier plusieurs arguments comme suit :  
  
**Name1\=Value1;Name2\=Value2;Name3\=Value3**  
  
Ces *nom\=valeur* paires doivent être significatives pour le plug\-dans la mesure où vous spécifiez. Pour certains plug\-ins les arguments sont facultatifs.  
  
La syntaxe du plug-in adaptée aux clusters\-dans les arguments respecte les règles générales :  
  
-   Plusieurs *nom\=valeur* paires sont séparées par des points-virgules.  
  
-   Une valeur qui contient des espaces est entourée par des guillemets, par exemple : **Nom1\=« Value with Spaces »**.  
  
-   La syntaxe exacte de *valeur* dépend de la fiche\-dans.  
  
Pour spécifier le plug\-dans les arguments à l’aide des applets de commande PowerShell d’adaptée aux clusters qui prennent en charge la **– CauPluginParameters** paramètre, passez un paramètre sous la forme :  
  
**\-CauPluginArguments @{Name1\=valeur1 ; Name2\=Value2 ; Nom3\=valeur3}**  
  
Vous pouvez également utiliser une table de hachage PowerShell prédéfinie. Pour spécifier le plug\-dans les arguments pour plusieurs plug\-, passez plusieurs tables de hachage d’arguments, séparés par des virgules. Passer le plug\-les arguments dans le plug\-afin que n’est spécifié dans **CauPluginName**.  
  
### <a name="specify-optional-plug-in-arguments"></a>Spécifiez plug facultatif\-dans les arguments  
Le plug\-ins adaptée \( **Microsoft.WindowsUpdatePlugin** et **Microsoft.HotfixPlugin** \) offrent des options supplémentaires que vous pouvez sélectionner. Dans l’UI CAU, ceux-ci s’affichent sur un **des Options supplémentaires** page après avoir configuré les options d’exécution de la mise à jour pour la connexion\-dans. Si vous utilisez les applets de commande PowerShell d’adaptée aux clusters, ces options sont configurées en tant que plug facultatif\-dans les arguments. Pour plus d’informations, voir [Utiliser Microsoft.WindowsUpdatePlugin](#BKMK_WUP) et [Utiliser Microsoft.HotfixPlugin](#BKMK_HFP) plus loin dans cette rubrique.  
  
## <a name="manage-plug-ins-using-windows-powershell-cmdlets"></a>Gérer les plug\-connexions à l’aide des applets de commande Windows PowerShell  
  
|Applet de commande|Description|  
|----------|---------------|  
|[Get-CauPlugin](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/get-cauplugin)|Récupère des informations sur un ou plusieurs logiciels, la mise à jour plug\-ins qui sont inscrits sur l’ordinateur local.|  
|[Register-CauPlugin]((https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/register-cauplugin))|Inscrit un logiciel adaptée aux clusters la mise à jour plug\-dans sur l’ordinateur local.|  
|[Unregister-CauPlugin](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/unregister-cauplugin)|Supprime un logiciel de mise à jour plug\-dans la liste du plug-in\-ins qui peuvent être utilisés par adaptée aux clusters. **Remarque :** Le plug\-ins installés avec adaptée aux clusters \( **Microsoft.WindowsUpdatePlugin** et **Microsoft.HotfixPlugin** \) ne peut pas être annulée.|  
  
## <a name="BKMK_WUP"></a>À l’aide de Microsoft.WindowsUpdatePlugin  

La prise par défaut\-dans pour adaptée aux clusters, **Microsoft.WindowsUpdatePlugin**, effectue les actions suivantes :
- Communique avec l'Agent de mise à jour automatique Windows Update sur chaque nœud de cluster de basculement pour appliquer les mises à jour nécessaires pour les produits Microsoft qui sont en cours d'exécution sur chaque nœud.
- Les installations de cluster mises à jour directement à partir de Windows Update ou Microsoft Update, ou d’un sur\-locale de Windows Server Update Services \(WSUS\) server.
- Installe uniquement sélectionné, le correctif logiciel grand public \(GDR\) mises à jour. Par défaut, le plug\-dans s’applique uniquement les mises à jour logicielles importantes. Aucune configuration n'est demandée. La configuration par défaut télécharge et installe les mises à jour importantes relatives aux correctifs logiciels grand public sur chaque nœud. 

> [!NOTE]
> Pour appliquer les mises à jour autre que les mises à jour logicielles importantes qui sont sélectionnées par défaut \(, par exemple, le pilote met à jour\), vous pouvez configurer un connecteur facultatif\-dans le paramètre. Pour plus d'informations, voir [Configurer la chaîne de requête de l'Agent de mise à jour automatique Windows Update](#BKMK_QUERY).

### <a name="requirements"></a>Configuration requise

- Le cluster de basculement et l’ordinateur coordinateur de mise à jour à distance \(si utilisé\) doit remplir les conditions pour adaptée aux clusters et la configuration requise pour la gestion à distance répertoriée dans [configuration requise et meilleures pratiques adaptée aux clusters ](cluster-aware-updating-requirements.md).
- Consultez les [recommandations relatives à l'application des mises à jour Microsoft](cluster-aware-updating-requirements.md#BKMK_BP_WUA), puis effectuez les changements nécessaires dans votre configuration de la fonctionnalité Microsoft Update pour les nœuds de cluster de basculement.
- Pour de meilleurs résultats, nous vous recommandons d’exécuter le Best Practices Analyzer adaptée aux clusters \(BPA\) pour vous assurer que l’environnement de cluster et de mise à jour sont correctement configurés pour appliquer des mises à jour à l’aide adaptée aux clusters. Pour plus d'informations, voir [Tester l'état de préparation aux mises à jour par la Mise à jour adaptée aux clusters](cluster-aware-updating-requirements.md#BKMK_BPA).

> [!NOTE]
> Les mises à jour qui demandent l'acceptation des termes du contrat de licence Microsoft ou qui imposent une interaction de l'utilisateur sont exclues. Elles doivent être installées manuellement.

### <a name="additional-options"></a>Options supplémentaires

Si vous le souhaitez, vous pouvez spécifier le plug suivant\-dans les arguments pour augmenter ou restreindre l’ensemble des mises à jour sont appliquées par le plug\-dans :
- Pour configurer le plug\-pour appliquer des mises à jour recommandées en plus des mises à jour importantes sur chaque nœud, dans l’UI CAU, sur le **des Options supplémentaires** page, sélectionnez le **donnent me recommandé met à jour de la même façon que Je reçois des mises à jour importantes** case à cocher.
<br>Vous pouvez également configurer le **'IncludeRecommendedUpdates'\='True'** Branchez\-dans l’argument.
- Pour configurer le plug\-dans pour filtrer les types de mises à jour GDR qui sont appliqués à chaque nœud du cluster, spécifiez une chaîne de requête de l’Agent Windows Update à l’aide un **QueryString** Branchez\-dans l’argument. Pour plus d'informations, voir [Configurer la chaîne de requête de l'Agent de mise à jour automatique Windows Update](#BKMK_QUERY).

### <a name="BKMK_QUERY"></a>Configurer la chaîne de requête de l’Agent Windows Update  
Vous pouvez configurer un bouchon\-dans l’argument pour la valeur par défaut Branchez\-in, **Microsoft.WindowsUpdatePlugin**, qui se compose d’un Agent de mise à jour Windows \(WUA\) chaîne de requête. Cette instruction utilise l'API WUA pour identifier un ou plusieurs groupes de mises à jour Microsoft à appliquer à chaque nœud, en fonction de critères de sélection spécifiques. Vous pouvez combiner plusieurs critères à l'aide d'une logique ET ou d'une logique OU. La chaîne de requête WUA est spécifiée dans un plug\-dans l’argument comme suit :  
  
**Chaîne de requête\=« Critère1\=Value1 et\/ou Criterion2\=Value2 et\/ou... »**  
  
Par exemple, **Microsoft.WindowsUpdatePlugin** sélectionne automatiquement les mises à jour importantes à l’aide d’un argument **QueryString** par défaut construit à l’aide des critères **IsInstalled**, **Type**, **IsHidden**et **IsAssigned** :  
  
**Chaîne de requête\=« IsInstalled\=0 et Type\=« Logiciel » et IsHidden\=0 et IsAssigned\=1 »**  
  
Si vous spécifiez un **QueryString** argument, il est utilisé à la place de la valeur par défaut **QueryString** qui est configuré pour la connexion\-dans.  
  
#### <a name="example-1"></a>Exemple 1
  
Pour configurer un **QueryString** argument qui installe une mise à jour spécifique identifiée par l’ID *f6ce46c1\-971c\-43f9\-a2aa\-783df125f003*:  
  
**QueryString\="UpdateID\='f6ce46c1\-971c\-43f9\-a2aa\-783df125f003' and IsInstalled\=0"**  
  
> [!NOTE]  
> L’exemple précédent est valide pour l’application des mises à jour à l’aide du Cluster\-Assistant prenant en charge de la mise à jour. Si vous souhaitez installer une mise à jour spécifique en configurant self\-options de mise à jour avec l’UI CAU ou à l’aide de la **ajouter\-CauClusterRole** ou **définir\-CauClusterRole**Applet de commande PowerShell, vous devez mettre en forme la valeur UpdateID unique deux\-caractères guillemet :  
>   
> **Chaîne de requête\=« UpdateID\='' f6ce46c1\-971c\-43f9\-a2aa\-783df125f003'' et IsInstalled\=« 0 »**  
  
#### <a name="example-2"></a>Exemple 2
  
Pour configurer un argument **QueryString** qui installe uniquement les pilotes :  
  
**Chaîne de requête\=« IsInstalled\=0 et Type\='Driver' et IsHidden\=0 »**  
  
Pour plus d’informations sur les chaînes de requête pour la valeur par défaut Branchez\-in, **Microsoft.WindowsUpdatePlugin**, les critères de recherche \(comme **IsInstalled**\), et la syntaxe que vous pouvez inclure dans les chaînes de requête, consultez la section relative aux critères de recherche dans les [référence de l’API Windows Update Agent (WUA)](https://go.microsoft.com/fwlink/p/?LinkId=223304).  
  
## <a name="BKMK_HFP"></a>Utiliser Microsoft.HotfixPlugin  
Le plug\-dans **Microsoft.HotfixPlugin** peut être utilisé pour appliquer le correctif logiciel de Microsoft limité \(LDR\) mises à jour \(également appelées correctifs logiciels et auparavant appelés QFE\) que vous téléchargez indépendamment pour résoudre des problèmes logiciels Microsoft spécifiques. Le plug-in installe des mises à jour à partir d’un dossier racine sur un partage de fichiers SMB et peut également être personnalisé pour appliquer non\-pilote Microsoft, des microprogrammes et des mises à jour du BIOS.

> [!NOTE]
> Parfois, les correctifs sont disponibles en téléchargement à partir de Microsoft dans les articles de la Base de connaissances, mais ils sont également fournis aux clients selon\-les besoins.

### <a name="requirements"></a>Configuration requise

- Le cluster de basculement et l’ordinateur coordinateur de mise à jour à distance \(si utilisé\) doit remplir les conditions pour adaptée aux clusters et la configuration requise pour la gestion à distance répertoriée dans [configuration requise et meilleures pratiques adaptée aux clusters ](cluster-aware-updating-requirements.md).
- Consultez [Recommandations pour l'utilisation de Microsoft.HotfixPlugin](cluster-aware-updating-requirements.md#BKMK_BP_HF).
- Pour de meilleurs résultats, nous vous recommandons d’exécuter le Best Practices Analyzer adaptée aux clusters \(BPA\) modèle pour vous assurer que l’environnement de cluster et de mise à jour sont correctement configurés pour appliquer des mises à jour à l’aide adaptée aux clusters. Pour plus d'informations, voir [Tester l'état de préparation aux mises à jour par la Mise à jour adaptée aux clusters](cluster-aware-updating-requirements.md#BKMK_BPA).
- Obtenir les mises à jour du serveur de publication et les copier ou extrayez-les dans un bloc de Message serveur \(SMB\) partage de fichiers \(dossier racine de correctifs logiciels\) qui prend en charge SMB 2.0 et qui est accessible par l’ensemble du cluster nœuds et l’ordinateur coordinateur de mise à jour à distance \(si adaptée aux clusters est utilisée dans à distance\-la mise à jour en mode\). Pour plus d'informations, voir [Configurer la structure d'un dossier racine de correctifs logiciels](#BKMK_HF_ROOT) plus loin dans cette rubrique. 

    > [!NOTE]
    > Par défaut, ce plug-\-dans installe uniquement les correctifs logiciels ayant les extensions de nom de fichier suivantes : .msu, .msi et .msp.

- Copiez le fichier DefaultHotfixConfig.xml \(qui est fourni dans le **%SystemRoot%\\System32\\WindowsPowerShell\\v1.0\\Modules\\ ClusterAwareUpdating** dossier sur un ordinateur où sont installés les outils adaptée aux clusters\) vers le dossier racine de correctifs logiciels que vous avez créé et dans lequel vous avez extrait les correctifs logiciels. Par exemple, copiez le fichier de configuration  *\\ \\MyFileServer\\correctifs\\racine\\*. 

    > [!NOTE]
    > Pour installer la plupart des correctifs logiciels fournis par Microsoft, ainsi que les autres mises à jour, vous pouvez utiliser le fichier de configuration de correctif logiciel par défaut en l'état. Si votre scénario l'impose, vous pouvez personnaliser le fichier de configuration dans le cadre d'une tâche avancée. Le fichier de configuration peut inclure des règles personnalisées, par exemple pour gérer les fichiers de correctifs logiciels qui ont des extensions spécifiques ou pour définir des comportements pour des conditions de sortie spécifiques. Pour plus d'informations, voir [Personnaliser le fichier de configuration de correctif logiciel](#BKMK_CONFIG_FILE) plus loin dans cette rubrique.

### <a name="configuration"></a>Configuration

Configurez les paramètres suivants. Pour plus d'informations, voir les liens vers les sections présentes plus loin dans cette rubrique.
- Chemin d'accès du dossier racine de correctifs logiciels partagé, qui contient les mises à jour à appliquer, ainsi que le fichier de configuration de correctifs logiciels. Vous pouvez taper ce chemin d’accès dans l’UI CAU ou configurer le **HotfixRootFolderPath\=\<chemin d’accès >** PowerShell plug\-dans l’argument. 

   > [!NOTE]
   > Vous pouvez spécifier le dossier racine de correctifs logiciels sous la forme d’un chemin d’accès du dossier local ou un chemin d’accès UNC au format  *\\ \\nom_serveur\\partage\\RootFolderName*. Un domaine\-basé ou chemin d’accès de DFS Namespace autonome peut être utilisé. Toutefois, le plug\-dans les fonctionnalités qui vérifient l’accès aux autorisations dans le fichier de configuration de correctif logiciel sont incompatibles avec un chemin d’accès DFS Namespace, afin de configurer ce dernier, vous devez désactiver la vérification des autorisations d’accès à l’aide de l’UI CAU ou en configurant le **DisableAclChecks\='True'** Branchez\-dans l’argument.
- Dossier partagé de paramètres sur le serveur qui héberge le dossier racine de correctifs logiciels pour vérifier les autorisations appropriées pour accéder au dossier et garantir l’intégrité des données accédées à partir de la SMB \(signature SMB ou chiffrement SMB\). Pour plus d'informations, voir [Restreindre l'accès au dossier racine de correctifs logiciels](#BKMK_ACL).

### <a name="additional-options"></a>Options supplémentaires

- Éventuellement, configurez le plug\-dans ainsi que le chiffrement SMB est appliqué lors de l’accès aux données à partir du partage de fichier de correctif. Dans l’UI CAU, sur le **des Options supplémentaires** page, sélectionnez le **exiger le chiffrement SMB lors de l’accès au dossier racine de correctif logiciel** ou configurez la **RequireSMBEncryption\= 'True'** PowerShell plug\-dans l’argument. 
  > [!IMPORTANT]
  > Vous devez effectuer des étapes de configuration supplémentaires sur le serveur SMB pour garantir l'intégrité des données via une signature SMB ou un chiffrement SMB. Pour plus d’informations, voir l’Étape 4 dans [Restreindre l'accès au dossier racine de correctifs logiciels](#BKMK_ACL). Si vous sélectionnez l'option permettant d'appliquer le chiffrement SMB, et si le dossier racine de correctifs logiciels n'est pas configuré pour être accessible via le chiffrement SMB, l'Exécution de mise à jour échoue.
- Éventuellement, désactivez les vérifications par défaut de l'adéquation des autorisations pour le dossier racine de correctifs logiciels et le fichier de configuration de correctif logiciel. Dans l’UI CAU, sélectionnez **désactiver la vérification d’accès d’administrateur pour le fichier de configuration et le dossier racine de correctif logiciel**, ou configurez la **DisableAclChecks\='True'** Branchez\-dans argument.
- Éventuellement, configurez le **HotfixInstallerTimeoutMinutes\= <Integer>**  branchez d’argument pour spécifier la durée pendant laquelle le correctif logiciel\-dans attend que le processus d’installation de correctif logiciel à retourner. \(La valeur par défaut est 30 minutes.\) Par exemple, pour spécifier un délai de deux heures, définissez **HotfixInstallerTimeoutMinutes\=120**.
- Éventuellement, configurez le **HotfixConfigFileName \= <name>**  Branchez\-dans l’argument pour spécifier un nom pour le fichier de configuration de correctif logiciel qui se trouve dans le dossier racine de correctifs logiciels. Si aucune indication n'est spécifiée, le nom par défaut DefaultHotfixConfig.xml est utilisé.
  
### <a name="BKMK_HF_ROOT"></a>Configurer une structure de dossiers de racine de correctif logiciel

Branchez le correctif logiciel\-pour travailler, correctifs logiciels doit être stocké dans une zone de configuration\-défini la structure dans un partage de fichiers SMB \(dossier racine de correctifs logiciels\), et vous devez configurer la prise de correctif logiciel\-avec le chemin d’accès à la dossier racine de correctifs logiciels à l’aide de l’UI CAU ou les applets de commande PowerShell d’adaptée aux clusters. Ce chemin d’accès est passé à la prise\-dans en tant que le **HotfixRootFolderPath** argument. Vous disposez de plusieurs structures au choix pour le dossier racine de correctifs logiciels, en fonction de vos besoins de mise à jour, comme le montrent les exemples suivants. Les fichiers ou dossiers qui n'adhèrent pas à la structure sont ignorés.  
  
#### <a name="example-1---folder-structure-used-to-apply-hotfixes-to-all-cluster-nodes"></a>Exemple 1 : structure de dossiers utilisée pour appliquer des correctifs logiciels à tous les nœuds de cluster
  
Pour spécifier que les correctifs logiciels s’appliquent à tous les nœuds de cluster, copiez-les dans un dossier nommé **CAUHotfix\_tous les** sous le dossier racine de correctif logiciel. Dans cet exemple, le **HotfixRootFolderPath** Branchez\-dans l’argument est défini sur *\\ \\MyFileServer\\correctifs\\racine\\*. Le **CAUHotfix\_tous les** dossier contient trois mises à jour des extensions .msu, .msi et .msp, qui seront appliqués à tous les nœuds de cluster. Les noms de fichiers des mises à jour sont uniquement utilisés à des fins d'illustration.  
  
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
  
#### <a name="example-2---folder-structure-used-to-apply-certain-updates-only-to-a-specific-node"></a>Exemple 2 - structure de dossiers utilisée pour appliquer certaines mises à jour uniquement à un nœud spécifique
  
Pour spécifier les correctifs logiciels qui s'appliquent uniquement à un nœud spécifique, utilisez un sous-dossier sous le dossier racine de correctifs logiciels ayant le nom du nœud. Utilisez le nom NetBIOS du nœud de cluster, par exemple *ContosoNode1*. Déplacez ensuite les mises à jour qui s'appliquent uniquement à ce nœud vers ce sous-dossier. Dans l’exemple suivant, le **HotfixRootFolderPath** Branchez\-dans l’argument est défini sur  *\\ \\MyFileServer\\correctifs\\racine\\*. Met à jour dans le **CAUHotfix\_tous les** dossier s’appliqueront à tous les nœuds de cluster, et *Node1\_spécifique\_Update.msu* s’appliqueront uniquement aux *ContosoNode1*.  
  
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
  
Par défaut, **Microsoft.HotfixPlugin** applique uniquement les mises à jour ayant l'extension .msu, .msi ou .msp. Toutefois, certaines mises à jour peuvent avoir d'autres extensions et demander des commandes d'installation distinctes. Par exemple, vous pouvez être amené à appliquer une mise à jour de microprogramme avec l'extension .exe à un nœud dans un cluster. Vous pouvez configurer le dossier racine de correctifs logiciels avec un sous-dossier qui indique un spécifique, non\-type de mise à jour par défaut doit être installé. Vous devez également configurer une règle d'installation de dossier correspondante qui spécifie la commande d'installation de l'élément `<FolderRules>` dans le fichier XML de configuration de correctif logiciel.  
  
Dans l’exemple suivant, le **HotfixRootFolderPath** Branchez\-dans l’argument est défini sur  *\\ \\MyFileServer\\correctifs\\racine\\*. Plusieurs mises à jour sont appliquées à l'ensemble des nœuds de cluster. En outre, la mise à jour de microprogramme *SpecialHotfix1.exe* est appliquée à *ContosoNode1* à l'aide de *FolderRule1*. Pour plus d’informations sur la configuration de *FolderRule1* dans le fichier de configuration de correctif logiciel, voir [Personnaliser le fichier de configuration de correctif logiciel](#BKMK_CONFIG_FILE) plus loin dans cette rubrique.  
  
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
  
**%systemroot%\\System32\\WindowsPowerShell\\v1.0\\Modules\\ClusterAwareUpdating folder**  
  
Pour personnaliser le fichier de configuration de correctif logiciel, copiez l'exemple de fichier de configuration DefaultHotfixConfig.xml depuis cet emplacement vers le dossier racine de correctifs logiciels, puis effectuez les changements appropriés pour votre scénario.  
  
> [!IMPORTANT]  
> Pour appliquer la plupart des correctifs logiciels fournis par Microsoft, ainsi que les autres mises à jour, vous pouvez utiliser le fichier de configuration de correctif logiciel par défaut en l'état. La personnalisation du fichier de configuration de correctif logiciel est une tâche effectuée uniquement dans les scénarios d'usage avancés.  
  
Par défaut, le fichier XML de configuration de correctif logiciel définit les règles d'installation et les conditions de sortie des deux catégories suivantes de correctifs logiciels :  
  
-   Fichiers de correctifs logiciels avec des extensions qui le plug\-dans peut installer par défaut \(fichiers .msu, .msi et .msp\).  
  
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
  
-   Correctif logiciel ou autre mettre à jour les fichiers qui ne sont pas .msi, .msu ou .msp, par exemple, non\-pilotes Microsoft, des microprogrammes et des mises à jour du BIOS.  
  
    Chaque non\-type de fichier par défaut est configuré comme un `<Folder>` élément dans le `<FolderRules>` élément. L'attribut name de l'élément `<Folder>` doit être identique au nom d'un dossier dans le dossier racine de correctifs logiciels, qui contient les mises à jour du type correspondant. Le dossier peut se trouver dans le **CAUHotfix\_tous les** dossier ou dans un nœud\-dossier spécifique. Par exemple, si *FolderRule1* est configuré dans le dossier racine de correctifs logiciels, configurez l'élément suivant du fichier XML pour définir le modèle d'installation et les conditions de sortie des mises à jour dans ce dossier :  
  
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
  
### <a name="BKMK_ACL"></a>Restreindre l’accès au dossier racine de correctif logiciel  
Vous devez suivre plusieurs étapes pour configurer le serveur de fichiers SMB et le partage de fichiers. Cela vous permet de sécuriser les fichiers du dossier racine de correctifs logiciels et le fichier de configuration de correctif logiciel en autorisant uniquement leur accès dans le cadre de **Microsoft.HotfixPlugin**. Ces étapes activent plusieurs fonctionnalités qui permettent d'éviter une éventuelle falsification des fichiers de correctifs logiciels et la compromission du cluster de basculement.  
  
Les étapes générales sont les suivantes :  
  
1.  Identifier le compte d’utilisateur qui est utilisé pour les exécutions de la mise à jour à l’aide du plug\-dans  
  
2.  Configurer ce compte d'utilisateur dans les groupes nécessaires sur un serveur de fichiers SMB  
  
3.  Configurer les autorisations d'accès au dossier racine de correctifs logiciels  
  
4.  Configurer les paramètres relatifs à l'intégrité des données SMB  
  
5.  Activer une règle de Pare-feu Windows sur le serveur SMB  
  
#### <a name="step-1-identify-the-user-account-that-is-used-for-updating-runs-by-using-the-hotfix-plug-in"></a>Étape 1. Identifier le compte d’utilisateur qui est utilisé pour les exécutions de la mise à jour à l’aide de la prise de correctif logiciel\-dans
  
Le compte qui est utilisé dans adaptée aux clusters pour vérifier les paramètres de sécurité lors de l’exécution de la mise à jour l’aide **Microsoft.HotfixPlugin** varie selon qu’adaptée aux clusters est utilisée dans à distance\-mise à jour du mode ou self\-mise à jour du mode, comme suit :  
  
-   **À distance\-la mise à jour en mode** le compte qui dispose des privilèges d’administrateur sur le cluster pour afficher un aperçu et appliquer des mises à jour.  
  
-   **Self\-la mise à jour en mode** rôle en cluster le nom de l’objet ordinateur virtuel qui est configuré dans Active Directory pour cette dernière. Il s'agit soit du nom d'un objet ordinateur virtuel préconfiguré dans Active Directory pour le rôle en cluster de la Mise à jour adaptée aux clusters, soit du nom généré par la Mise à jour adaptée aux clusters pour le rôle en cluster. Pour obtenir le nom s’il est généré par adaptée aux clusters, exécutez le **obtenir\-CauClusterRole** applet de commande PowerShell d’adaptée aux clusters. Dans la sortie, **ResourceGroupName** est le nom du compte de l’objet ordinateur virtuel généré.  
  
#### <a name="step-2-configure-this-user-account-in-the-necessary-groups-on-an-smb-file-server"></a>Étape 2. Configurer ce compte d'utilisateur dans les groupes nécessaires sur un serveur de fichiers SMB
  
> [!IMPORTANT]  
> Vous devez ajouter le compte utilisé pour les Exécutions de mise à jour en tant que compte d'administrateur local sur le serveur SMB. Si cela n'est pas autorisé en raison des stratégies de sécurité de votre organisation, configurez ce compte avec les autorisations nécessaires sur le serveur SMB à l'aide de la procédure suivante.  
  
##### <a name="to-configure-a-user-account-on-the-smb-server"></a>Pour configurer un compte d'utilisateur sur le serveur SMB  
  
1.  Ajoutez le compte utilisé pour les Exécutions de mise à jour au groupe Utilisateurs du modèle COM distribué et à l'un des groupes suivants : Utilisateurs avec pouvoir, Opérateurs de serveur ou Opérateurs d'impression.  
  
2.  Pour activer les autorisations WMI nécessaires pour le compte, démarrez la console de gestion WMI sur le serveur SMB. Démarrez PowerShell et tapez la commande suivante :  
  
    ```  
    wmimgmt.msc  
    ```  
  
3.  Dans l’arborescence de la console, avec le bouton droit\-cliquez sur **contrôle WMI \(Local\)**, puis cliquez sur **propriétés**.  
  
4.  Cliquez sur **Sécurité**, puis développez **Racine**.  
  
5.  Cliquez sur **CIMV2**, puis sur **Sécurité**.  
  
6.  Ajoutez le compte utilisé pour les Exécutions de mise à jour à la liste **Noms de groupes ou d'utilisateurs** .  
  
7.  Accordez les autorisations **Méthodes d'exécution** et **Appel à distance autorisé** au compte utilisé pour les Exécutions de mise à jour.  
  
#### <a name="step-3-configure-permissions-to-access-the-hotfix-root-folder"></a>Étape 3. Configurer les autorisations d'accès au dossier racine de correctifs logiciels
  
Par défaut, lorsque vous essayez d’appliquer des mises à jour, le correctif logiciel Branchez\-dans vérifie la configuration des autorisations du système de fichiers NTFS pour l’accès au dossier racine de correctif logiciel. Si les autorisations d’accès de dossier ne sont pas configurées correctement, une exécution de mise à l’aide de la prise de correctif logiciel\-dans risque d’échouer.  
  
Si vous utilisez la configuration par défaut du plug-in de correctif logiciel\-, assurez-vous que les autorisations d’accès de dossier respectent les exigences suivantes.  
  
-   Le groupe Utilisateurs dispose d'une autorisation d'accès en lecture.  
  
-   Si le plug\-permet d’appliquer les mises à jour avec l’extension .exe, le groupe d’utilisateurs a l’autorisation Execute.  
  
-   Seuls certains principaux de sécurité sont autorisés \(mais ne sont pas requises\) à écrire ou de modifier l’autorisation. Les principaux autorisés sont le groupe Administrateurs local, ainsi que les comptes SYSTEME, CREATEUR PROPRIETAIRE et TrustedInstaller. Les autres comptes ou groupes ne sont pas autorisés à écrire ou changer les autorisations du dossier racine de correctifs logiciels.  
  
Si vous le souhaitez, vous pouvez désactiver les vérifications précédentes que le plug\-dans effectue par défaut. Vous pouvez le faire de deux manières :  
  
-   Si vous utilisez les applets de commande PowerShell d’adaptée aux clusters, configurez le **DisableAclChecks\='True'** argument dans le **CauPluginArguments** paramètre pour la prise de correctif logiciel\-dans.  
  
-   Si vous utilisez l'interface de la Mise à jour adaptée aux clusters, sélectionnez l'option **Désactiver la vérification des droits d'accès d'administrateur au dossier racine de correctif logiciel et au fichier de configuration** dans la page **Options de mise à jour supplémentaires** de l'Assistant qui permet de configurer les options d'Exécution de mise à jour.  
  
Cependant, dans de nombreux environnements, nous vous recommandons d'utiliser la configuration par défaut pour appliquer ces vérifications.  
  
#### <a name="step-4-configure-settings-for-smb-data-integrity"></a>Étape 4. Configurer les paramètres relatifs à l'intégrité des données SMB
  
Pour vérifier l’intégrité des données dans les connexions entre les nœuds du cluster et le partage de fichiers SMB, branchez le correctif logiciel\-dans nécessite l’activation de paramètres sur le partage de fichiers SMB pour la signature SMB ou chiffrement SMB. Le chiffrement SMB, qui offre une sécurité renforcée et meilleures performances dans de nombreux environnements, est pris en charge dans Windows Server 2012. Vous pouvez activer l'un ou l'autre de ces paramètres, ou les deux à la fois, comme suit :  
  
-   Pour activer la signature SMB, voir la procédure décrite dans l’[article 887429](https://support.microsoft.com/kb/887429) de la Base de connaissances Microsoft.  
  
-   Pour activer le chiffrement SMB du dossier partagé SMB, exécutez l’applet de commande PowerShell suivante sur le serveur SMB :  
  
    ```PowerShell  
    Set-SmbShare <ShareName> -EncryptData $true  
    ```  
  
    Où <*ShareName*> est le nom du dossier partagé SMB.  
  
Si vous le souhaitez, pour appliquer l’utilisation du chiffrement SMB dans les connexions au serveur SMB, sélectionnez le **exiger le chiffrement SMB lors de l’accès au dossier racine de correctif logiciel** dans l’UI CAU ou configurez la **RequireSMBEncryption \='True'** Branchez\-dans l’argument à l’aide des applets de commande PowerShell d’adaptée aux clusters.  
  
> [!IMPORTANT]  
> Si vous sélectionnez l'option permettant d'appliquer le chiffrement SMB, et si le dossier racine de correctifs logiciels n'est pas configuré pour les connexions qui utilisent le chiffrement SMB, l'Exécution de mise à jour échoue.  
  
#### <a name="step-5-enable-a-windows-firewall-rule-on-the-smb-server"></a>Étape 5. Activer une règle de Pare-feu Windows sur le serveur SMB
  
Vous devez activer le **gestion à distance des serveurs de fichiers \(SMB\-dans\)**  règle dans le pare-feu Windows sur le serveur de fichiers SMB. Cette option est activée par défaut dans Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012.  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Vue d’ensemble de la mise à jour adaptée aux clusters](cluster-aware-updating.md)
  
-   [Adaptée aux clusters avec mise à jour Windows applets de commande PowerShell](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating)  
  
-   [Cluster prenant en charge la mise à jour des plug-in référence](https://msdn.microsoft.com/library/hh418084.aspx)  
  
