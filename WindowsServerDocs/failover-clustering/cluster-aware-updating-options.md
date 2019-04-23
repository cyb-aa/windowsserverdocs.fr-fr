---
ms.assetid: 2f4b6641-0ec2-4b1c-85fb-a1f1d16685c8
title: Mise à jour adaptée aux clusters des options avancées et profils d’exécution de la mise à jour
ms.topic: article
ms.prod: windows-server-threshold
manager: dongill
ms.author: jgerend
author: JasonGerend
ms.date: 08/06/2018
description: Comment configurer les options avancées et la mise à jour des profils d’exécution pour la mise à jour (adaptée aux clusters)
ms.openlocfilehash: 5fac31ad35422e623b98caaabdd9eae183e2e5d8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830550"
---
# <a name="cluster-aware-updating-advanced-options-and-updating-run-profiles"></a>Mise à jour adaptée aux clusters des options avancées et profils d’exécution de la mise à jour

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette rubrique décrit les options d’exécution de la mise à jour qui peuvent être configurées pour un [la mise à jour adaptée aux clusters](cluster-aware-updating.md) (adaptée aux clusters) la mise à jour s’exécuter. Ces options avancées peuvent être configurées lorsque vous utilisez l’UI CAU ou les applets de commande PowerShell de Windows adaptée aux clusters pour appliquer les mises à jour ou pour configurer les options de mise à jour automatique.

La plupart des paramètres de configuration peuvent être enregistrés sous la forme d’un fichier XML appelé profil d’exécution de mise à jour et réutilisés pour des exécutions de mise à jour ultérieures. Les valeurs par défaut des options d’exécution de mise à jour qui sont fournies par la mise à jour adaptée aux clusters peuvent également être utilisées dans de nombreux environnements de clusters.

Pour plus d’informations sur d’autres options que vous pouvez spécifier pour chaque exécution de mise à jour et sur les profils d’exécution de mise à jour, voir les sections suivantes plus loin dans cette rubrique :

Options que vous spécifiez lorsque vous demandez une mise à jour exécuter utilisation mise à jour des profils Options d’exécution qui peuvent être définies dans un profil d’exécution de la mise à jour

Le tableau suivant répertorie les options que vous pouvez définir dans un profil d’exécution de mise à jour pour une mise à jour adaptée aux clusters. 

> [!NOTE] 
> Pour définir l’option PreUpdateScript ou PostUpdateScript, vérifiez que Windows PowerShell et .NET Framework 4.6 ou 4.5 sont installés et que la communication à distance PowerShell est activée sur chaque nœud du cluster. Pour plus d’informations, consultez Configurer les nœuds pour la gestion à distance dans [configuration requise et meilleures pratiques pour la mise à jour adaptée aux clusters](cluster-aware-updating-requirements.md).


|Option|Valeur par défaut|Détails|  
|------------|-------------------|-------------|  
|**StopAfter**|Durée illimitée|Délai, en minutes, au-delà duquel l’exécution de mise à jour sera arrêtée si elle n’est pas terminée. **Remarque :**  Si vous spécifiez une mise à jour préalable ou un script PowerShell de post-mise à jour, l’ensemble du processus d’exécution des scripts et des mises à jour doit être effectué dans le **StopAfter** limite de temps.|  
|**WarnAfter**|Par défaut, aucun avertissement ne s’affiche|Délai, en minutes, au-delà duquel un avertissement est affiché si l’exécution de mise à jour (y compris un script antérieur à la mise à jour et un script postérieur à la mise à jour, s’ils sont configurés) n’est pas terminée.|  
|**MaxRetriesPerNode**|3|Nombre maximal de fois où le processus de mise à jour (y compris un script antérieur à la mise à jour et un script postérieur à la mise à jour, s’ils sont configurés) sera relancé par nœud. La valeur maximale est 64.|  
|**MaxFailedNodes**|Pour la plupart des clusters, un entier à peu près égal au tiers du nombre de nœuds de cluster|Nombre maximal de nœuds sur lesquels la mise à jour peut échouer, en raison de l’échec des nœuds ou de l’arrêt de l’exécution du service de cluster. Si un nœud supplémentaire échoue, l’exécution de mise à jour est arrêtée.<br /><br /> La plage valide contient des valeurs inférieures de 0 à 1 au nombre de nœuds de cluster.|  
|**RequireAllNodesOnline**|Aucune|Indique que tous les nœuds doivent être en ligne et accessibles avant le début de la mise à jour.|  
|**RebootTimeoutMinutes**|15|Délai, en minutes, que la mise à jour adaptée aux clusters autorisera pour le redémarrage d’un nœud (si un redémarrage est nécessaire) et le démarrage de tous les services de démarrage automatique. Si le processus de redémarrage ne se termine dans ce délai, l’exécution de la mise à jour sur ce nœud est marquée comme ayant échoué.|  
|**PreUpdateScript**|Aucune|Le chemin d’accès et le nom d’un script PowerShell à exécuter sur chaque nœud avant le début de la mise à jour et avant le nœud est placé en mode maintenance. L’extension de nom de fichier doit être **.ps1**, et la longueur totale du chemin d’accès et de fichier du nom ne doit pas dépasser 260 caractères. Nous vous recommandons, à titre de meilleure pratique, de placer le script sur un disque dans le stockage du cluster ou sur un partage de fichiers réseau hautement disponible pour garantir qu’il est toujours accessible à tous les nœuds de cluster. Si le script est situé sur un partage de fichiers réseau, assurez-vous de configurer le partage avec l’autorisation de lecture pour le groupe Tout le monde et limitez l’accès en écriture pour empêcher toute falsification des fichiers par des utilisateurs non autorisés.<br /><br /> Si vous spécifiez un script antérieur à la mise à jour, assurez-vous que des paramètres tels que les limites de temps (par exemple, **StopAfter**) sont configurés pour permettre l’exécution correcte du script. Ces limites couvrent la totalité du processus d’exécution des scripts et d’installation des mises à jour, et pas seulement le processus d’installation des mises à jour.|  
|**PostUpdateScript**|Aucune|Le chemin d’accès et le nom d’un script PowerShell exécuter une fois la mise à jour terminée (après que le nœud a quitté le mode de maintenance). L’extension de nom de fichier doit être **.ps1** et la longueur totale du chemin d’accès et de fichier du nom ne doit pas dépasser 260 caractères. Nous vous recommandons, à titre de meilleure pratique, de placer le script sur un disque dans le stockage du cluster ou sur un partage de fichiers réseau hautement disponible pour garantir qu’il est toujours accessible à tous les nœuds de cluster. Si le script est situé sur un partage de fichiers réseau, assurez-vous de configurer le partage avec l’autorisation de lecture pour le groupe Tout le monde et limitez l’accès en écriture pour empêcher toute falsification des fichiers par des utilisateurs non autorisés.<br /><br /> Si vous spécifiez un script postérieur à la mise à jour, assurez-vous que des paramètres tels que les limites de temps (par exemple, **StopAfter**) sont configurés pour permettre l’exécution correcte du script. Ces limites couvrent la totalité du processus d’exécution des scripts et d’installation des mises à jour, et pas seulement le processus d’installation des mises à jour.|  
|**ConfigurationName**|Ce paramètre n’a d’effet que si vous exécutez des scripts.<br /><br /> Si vous spécifiez un script antérieur ou un script de mise à jour, mais vous ne spécifiez pas un **ConfigurationName**, la session par défaut de configuration pour PowerShell (Microsoft.PowerShell) est utilisée.|Spécifie la configuration de session PowerShell qui définit la session dans laquelle les scripts (spécifié par **PreUpdateScript** et **PostUpdateScript**) sont exécutés et peut limiter les commandes qui peuvent être exécutés.|  
|**CauPluginName**|**Microsoft.WindowsUpdatePlugin**|Plug-in que vous configurez avec la Mise à jour adaptée aux clusters à utiliser pour afficher un aperçu des mises à jour ou effectuer une exécution de mise à jour. Pour plus d’informations, consultez [comment mettre à jour adaptée aux clusters de plug-ins fonctionnent](cluster-aware-updating-plug-ins.md).|  
|**CauPluginArguments**|Aucune|Ensemble de paires de type *nom=valeur* (arguments) pour le plug-in de mise à jour à utiliser ; par exemple :<br /><br /> **Domain=Domain.local**<br /><br /> Ces paires *nom=valeur* doivent être significatives pour le plug-in que vous spécifiez dans **CauPluginName**.<br /><br /> Pour spécifier un argument à l’aide de l’interface utilisateur de la mise à jour adaptée aux clusters, tapez le *nom*, appuyez sur la touche Tab, puis tapez la *valeur* correspondante. Appuyez à nouveau sur la touche Tab pour fournir l’argument suivant. Chaque *nom* et chaque *valeur* sont automatiquement séparés par un signe égal (=). Plusieurs paires sont automatiquement séparées par des points-virgules.<br /><br /> Pour la valeur par défaut **Microsoft.WindowsUpdatePlugin** plug-in, aucun argument n’est nécessaire. Toutefois, vous pouvez spécifier un argument facultatif, par exemple pour spécifier une chaîne de requête standard émise par l’Agent de mise à jour automatique Windows Update pour filtrer l’ensemble des mises à jour appliquées par le plug-in. Pour un *nom*, utilisez **QueryString**et pour un *valeur*, placez la totalité de la requête entre guillemets.<br /><br /> Pour plus d’informations, consultez [comment mettre à jour adaptée aux clusters de plug-ins fonctionnent](cluster-aware-updating-plug-ins.md).|  
  
##  <a name="BKMK_runtime"></a> Options que vous spécifiez lorsque vous demandez une exécution de la mise à jour  
 Le tableau suivant répertorie les options (autres que celles présentes dans un profil d’exécution de mise à jour) que vous pouvez spécifier lorsque vous demandez une exécution de mise à jour. Pour plus d’informations sur les options que vous pouvez définir dans un profil d’exécution de mise à jour, voir le tableau précédent.  
  
|Option|Valeur par défaut|Détails|  
|------------|-------------------|-------------|  
|**ClusterName**|Aucune <br>**Remarque :**  Cette option doit être définie uniquement lorsque l’interface utilisateur de la mise à jour adaptée aux clusters n’est pas exécutée sur un nœud de cluster de basculement ou quand vous voulez faire référence à un cluster de basculement différent de l’endroit où l’interface utilisateur de la mise à jour adaptée aux clusters est exécutée.|Nom NetBIOS du cluster sur lequel effectuer l’exécution de mise à jour.|  
|**Informations d’identification**|Informations d’identification de compte en cours|Informations d’identification administratives du cluster cible sur lequel l’exécution de mise à jour sera effectuée. Vous disposez peut-être déjà les informations d’identification nécessaires si vous démarrez l’UI CAU (ou ouvrez une session PowerShell, si vous utilisez les applets de commande PowerShell d’adaptée aux clusters) à partir d’un compte disposant des droits d’administrateur et les autorisations sur le cluster.|  
|**NodeOrder**|Par défaut, la mise à jour adaptée aux clusters démarre avec le nœud possédant le plus petit nombre de rôles en cluster, puis progresse vers le nœud ayant le deuxième plus petit nombre de rôles, etc.|Noms des nœuds de cluster dans l’ordre où ils devraient être mis à jour (si possible).|  
  
##  <a name="BKMK_profile"></a> Utiliser des profils d’exécution de la mise à jour  
 Chaque exécution de mise à jour peut être associée à un profil d’exécution de mise à jour spécifique. La valeur par défaut de profil d’exécution de la mise à jour sont stockée dans le *%windir%\cluster* dossier. Si vous utilisez l’UI CAU dans la mise à jour à distance en mode, vous pouvez spécifier un profil d’exécution de la mise à jour au moment que vous appliquez des mises à jour, ou vous pouvez utiliser le profil d’exécution de la mise à jour par défaut. Si vous utilisez adaptée aux clusters en mode de mise à jour automatique, vous pouvez importer les paramètres à partir d’un profil d’exécution de la mise à jour spécifié lorsque vous configurez les options de mise à jour automatique. Dans les deux cas, vous pouvez remplacer les valeurs affichées pour les options d’exécution de mise à jour en fonction de vos besoins. Si vous le souhaitez, vous pouvez enregistrer les options d’exécution de mise à jour en tant que profil d’exécution de mise à jour sous le même nom de fichier ou sous un autre nom de fichier. La prochaine fois que vous appliquez des mises à jour ou configurez les options de mise à jour automatique, la mise à jour adaptée aux clusters sélectionne automatiquement le profil d’exécution de mise à jour précédemment sélectionné.  
  
 Vous pouvez modifier un profil d’exécution de la mise à jour existant ou créez-en un en sélectionnant **créer ou modifier le profil d’exécution de la mise à jour** dans l’UI CAU.

Voici quelques remarques importantes sur l’utilisation de la mise à jour des profils d’exécution :

* Un profil d’exécution de la mise à jour ne stocke pas les informations spécifiques aux clusters telles que des informations d’identification administratives. Si vous utilisez adaptée aux clusters en mode de mise à jour automatique, le profil d’exécution de la mise à jour n’également stocker les informations de planification de mise à jour automatique. Il est ainsi possible de partager un profil d’exécution de mise à jour entre tous les clusters de basculement d’une classe spécifiée.
* Si vous configurez les options de mise à jour automatique à l’aide d’un profil d’exécution de la mise à jour et modifiez ultérieurement le profil avec différentes valeurs pour les options d’exécution de la mise à jour, la configuration de mise à jour automatique ne change pas automatiquement. Pour appliquer les nouveaux paramètres d’exécution de mise à jour, vous devez à nouveau configurer les options de mise à jour automatique.
* L’éditeur de modèle exécuter malheureusement ne prend en charge les chemins de fichiers qui comportent des espaces, tel que *C:\Program Files*. Pour résoudre ce problème, stockez votre pre et publier des scripts de mise à jour dans un chemin d’accès qui ne comportent des espaces, ou utiliser PowerShell exclusivement pour gérer des profils d’exécution, placer des guillemets autour du chemin d’accès lors de l’exécution **Invoke-CauRun**.

### <a name="windows-powershell-equivalent-commands"></a>Commandes Windows PowerShell équivalentes
  
 Vous pouvez importer les paramètres à partir d’un profil d’exécution de la mise à jour lorsque vous exécutez le **Invoke-CauRun**, **Add-CauClusterRole**, ou **Set-CauClusterRole** applet de commande.  
  
 L’exemple suivant effectue une analyse et une exécution de mise à jour entière sur le cluster nommé *CONTOSO-FC1* en utilisant les options d’exécution de mise à jour qui sont spécifiées dans *C:\Windows\Cluster\DefaultParameters.xml*. Les valeurs par défaut sont utilisées pour les paramètres d’applet de commande restants.  
  
```powershell  
$MyRunProfile = Import-Clixml C:\Windows\Cluster\DefaultParameters.xml  
Invoke-CauRun –ClusterName CONTOSO-FC1 @MyRunProfile   
```  
  
 À l’aide d’un profil d’exécution de mise à jour, vous pouvez mettre à jour un cluster de basculement de façon répétée avec des paramètres cohérents pour la gestion des exceptions, des durées déterminées et d’autres paramètres de fonctionnement. Dans la mesure où ces paramètres sont en général spécifiques à une classe de clusters de basculement (par exemple, « Tous les clusters Microsoft SQL Server » ou « Mes clusters critiques »), vous pouvez nommer chaque profil d’exécution de mise à jour en fonction de la classe de clusters de basculement avec laquelle il sera utilisé. En outre, vous pouvez gérer le profil d’exécution de mise à jour sur un partage de fichiers qui est accessible à tous les clusters de basculement d’une classe spécifique dans votre département informatique.  
  
  
  
## <a name="see-also"></a>Voir aussi

-   [La mise à jour adaptée aux clusters](cluster-aware-updating.md)
  
-   [Applets de commande de mise à jour adaptée aux clusters dans Windows PowerShell](https://docs.microsoft.com/powershell/module/clusterawareupdating/?view=win10-ps)