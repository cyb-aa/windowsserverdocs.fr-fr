---
ms.assetid: 2f4b6641-0ec2-4b1c-85fb-a1f1d16685c8
title: "Prenant en charge éclat mise à jour des options avancées et profils d’exécution de mise à jour"
ms.topic: article
ms.prod: storage-failover-clustering
manager: dongill
ms.author: jgerend
author: JasonGerend
ms.date: 6/7/2017
description: "Comment configurer les options avancées et la mise à jour des profils d’exécution pour adaptée aux clusters mise à jour (adaptée aux clusters)"
ms.openlocfilehash: 5b6f035791a946ff96ff6a95a1f753ef505d54b4
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# <a name="cluster-aware-updating-advanced-options-and-updating-run-profiles"></a>Mise à jour adaptée aux clusters options avancées et profils d’exécution de mise à jour

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016, Windows Server2012R2, Windows Server2012

Cette rubrique décrit les options d’exécution de mise à jour qui peuvent être configurées pour un [mise à jour adaptée aux clusters](cluster-aware-updating.md) (adaptée aux clusters) mise à jour de s’exécuter. Ces options avancées peuvent être configurées lorsque vous utilisez le UI CAU ou les applets de commande Windows PowerShell d’adaptée aux clusters pour appliquer des mises à jour ou pour configurer les options de mise à jour automatique.

La plupart des paramètres de configuration peuvent être enregistrés comme un fichier XML appelé un profil d’exécution de mise à jour et réutilisés pour des exécutions de mise à jour ultérieure. Les valeurs par défaut pour les options d’exécution de mise à jour qui sont fournies par adaptée aux clusters peuvent également être utilisés dans de nombreux environnements de cluster.

Pour plus d’informations sur les options supplémentaires que vous pouvez spécifier pour chaque exécution de mise à jour et profils d’exécution de mise à jour, consultez les sections suivantes plus loin dans cette rubrique:

Options que vous spécifiez lorsque vous demandez une mise à jour exécuter utilisation mise à jour de profils Options d’exécution qui peuvent être définies dans un profil d’exécution de mise à jour

Le tableau suivant répertorie les options que vous pouvez définir dans un profil d’exécution de mise à jour adaptée aux clusters. 

> [!NOTE] 
> Pour définir l’option PreUpdateScript ou PostUpdateScript, vérifiez que Windows PowerShell et .NETFramework 4.6 ou 4.5 sont installés et que la communication à distance PowerShell est activée sur chaque nœud du cluster. Pour plus d’informations, voir Configurer les nœuds pour la gestion à distance dans [configuration requise et meilleures pratiques pour la mise à jour adaptée aux clusters ](cluster-aware-updating-requirements.md).


|Option|Valeur par défaut|Détails|  
|------------|-------------------|-------------|  
|**StopAfter**|Durée illimitée|Durée en minutes après lequel l’exécution de mise à jour sera arrêtée si elle n’est pas terminée. **Remarque:** si vous spécifiez une mise à jour avant ou un script PowerShell de mise à jour, l’ensemble du processus d’exécution des scripts et des mises à jour doit être effectué dans le **StopAfter** limite de temps.|  
|**WarnAfter**|Par défaut, aucun message d’avertissement s’affiche.|Durée en minutes, après laquelle un avertissement s’affiche si la mise à jour de s’exécuter (notamment un script antérieur et un script de mise à jour, s’ils sont configurés) n’est pas terminée.|  
|**MaxRetriesPerNode**|3|Nombre maximal de fois que le processus de mise à jour (notamment un script antérieur et un script de mise à jour, s’ils sont configurés) sera relancé par nœud. La valeur maximale est 64.|  
|**MaxFailedNodes**|Pour la plupart des clusters, un entier qui est d’environ un tiers du nombre de nœuds de cluster|Nombre maximal de nœuds sur quelle mise à jour peut échouer, soit parce que l’échec des nœuds ou l’arrêt du service de Cluster en cours d’exécution. Si un nœud supplémentaire échoue, l’exécution de mise à jour est arrêté.<br /><br /> La plage valide de valeurs est moins de 0 à 1 au nombre de nœuds de cluster.|  
|**RequireAllNodesOnline**|None|Spécifie que tous les nœuds doivent être en ligne et accessibles avant la mise à jour commence.|  
|**RebootTimeoutMinutes**|15|Durée en minutes adaptée aux clusters autorisera pour le redémarrage d’un nœud (si un redémarrage est nécessaire) et le démarrage de tous les services de démarrage automatique. Si le processus de redémarrage ne terminé pendant ce délai, l’exécution de mise à jour sur ce nœud est marquée comme ayant échoué.|  
|**PreUpdateScript**|None|Le chemin d’accès et le nom d’un script PowerShell à exécuter sur chaque nœud avant le début de la mise à jour, et avant le nœud est placé en mode de maintenance. L’extension de nom de fichier doit être **.ps1**, et la longueur totale du chemin d’accès plus fichier le nom ne doit pas excéder 260caractères. Comme meilleure pratique, le script doit se trouver sur un disque de stockage de cluster, ou à un partage de fichiers réseau hautement disponible, pour vous assurer qu’il est toujours accessible à tous les nœuds du cluster. Si le script se trouve sur un partage de fichiers réseau, assurez-vous de configurer le partage de fichiers pour l’autorisation de lecture pour tout le monde de groupe et limitez l’accès en écriture pour éviter la falsification des fichiers par des utilisateurs non autorisés.<br /><br /> Si vous spécifiez un script antérieur, assurez-vous que les limites de paramètres tels que le temps (par exemple, **StopAfter**) sont configurés pour autoriser le script s’exécute correctement. Ces limites couvrent la totalité du processus d’exécution des scripts et l’installation des mises à jour, pas seulement le processus d’installation des mises à jour.|  
|**PostUpdateScript**|None|Le chemin d’accès et le nom d’un script PowerShell exécuter une fois la mise à jour terminée (après que le nœud a quitté le mode de maintenance). L’extension de nom de fichier doit être **.ps1** et la longueur totale du chemin d’accès plus fichier le nom ne doit pas excéder 260caractères. Comme meilleure pratique, le script doit se trouver sur un disque de stockage de cluster, ou à un partage de fichiers réseau hautement disponible, pour vous assurer qu’il est toujours accessible à tous les nœuds du cluster. Si le script se trouve sur un partage de fichiers réseau, assurez-vous de configurer le partage de fichiers pour l’autorisation de lecture pour tout le monde de groupe et limitez l’accès en écriture pour éviter la falsification des fichiers par des utilisateurs non autorisés.<br /><br /> Si vous spécifiez un script de mise à jour, assurez-vous que les limites de paramètres tels que le temps (par exemple, **StopAfter**) sont configurés pour autoriser le script s’exécute correctement. Ces limites couvrent la totalité du processus d’exécution des scripts et l’installation des mises à jour, pas seulement le processus d’installation des mises à jour.|  
|**ConfigurationName**|Ce paramètre n’a d’effet que si vous exécutez des scripts.<br /><br /> Si vous spécifiez un script antérieur ou un script de mise à jour, mais vous ne spécifiez pas un **ConfigurationName**, la session par défaut de configuration pour PowerShell (Microsoft.PowerShell) est utilisée.|Spécifie la configuration de session PowerShell qui définit la session dans laquelle les scripts (spécifiés par **PreUpdateScript** et **PostUpdateScript**) sont exécutés et peut limiter les commandes qui peuvent être exécutées.|  
|**CauPluginName**|**Microsoft.WindowsUpdatePlugin**|Du plug-in que vous configurez la mise à jour adaptée aux clusters à utiliser pour afficher un aperçu des mises à jour ou effectuer une exécution de mise à jour. Pour plus d’informations, voir [plug-ins de la mise à jour adaptée aux clusters](cluster-aware-updating-plug-ins.md).|  
|**CauPluginArguments**|None|Un ensemble de *nom = valeur* paires (arguments) pour la mise à jour du plug-in à utiliser, par exemple:<br /><br /> **Domain=Domain.local**<br /><br /> Ces *nom = valeur* paires doivent être significatives pour le plug-in que vous spécifiez dans **CauPluginName**.<br /><br /> Pour spécifier un argument à l’aide de l’UI CAU, tapez le *nom*, appuyez sur la touche Tab et tapez le correspondant *valeur*. Appuyez sur la touche Tab pour fournir l’argument suivant. Chaque *nom* et *valeur* sont automatiquement séparés par un signe égal (=). Plusieurs paires sont automatiquement séparées par des points-virgules.<br /><br /> Pour la valeur par défaut **Microsoft.WindowsUpdatePlugin** plug-in, aucun argument n’est nécessaire. Toutefois, vous pouvez spécifier un argument facultatif, par exemple pour spécifier une chaîne de requête de l’Agent Windows Update standard pour filtrer l’ensemble des mises à jour appliquées par le plug-in. Pour un *nom*, utilisez **QueryString**et pour un *valeur*, placez la requête complète entre guillemets.<br /><br /> Pour plus d’informations, voir [plug-ins de la mise à jour adaptée aux clusters](cluster-aware-updating-plug-ins.md).|  
  
##  <a name="BKMK_runtime"></a>Options que vous spécifiez lorsque vous demandez une exécution de mise à jour  
 Le tableau suivant répertorie les options (autres que celles dans un profil d’exécution de mise à jour) que vous pouvez spécifier lorsque vous demandez une exécution de mise à jour. Pour plus d’informations sur les options que vous pouvez définir dans un profil d’exécution de mise à jour, voir le tableau précédent.  
  
|Option|Valeur par défaut|Détails|  
|------------|-------------------|-------------|  
|**ClusterName**|None <br>**Remarque:** cette option doit être définie seulement quand le UI CAU n’est pas exécutée sur un nœud de cluster de basculement, ou que vous souhaitez faire référence à un cluster de basculement différent à partir duquel l’UI CAU est exécutée.|Nom NetBIOS du cluster sur lequel effectuer l’exécution de mise à jour.|  
|**Informations d’identification**|Informations d’identification de compte en cours|Les informations d’identification d’administration pour la cible de cluster sur lequel l’exécution de mise à jour sera effectuée. Vous avez peut-être déjà les informations d’identification nécessaires si vous démarrez l’UI CAU (ou ouvrez une session PowerShell, si vous utilisez les applets de commande PowerShell adaptée aux clusters) à partir d’un compte qui dispose des autorisations et droits d’administrateur sur le cluster.|  
|**NodeOrder**|Par défaut, adaptée aux clusters démarre avec le nœud qui possède le plus petit nombre de rôles en cluster, puis progresse vers le nœud qui possède la deuxième plus petit nombre et ainsi de suite.|Noms des nœuds de cluster dans l’ordre qu’ils doivent être mis à jour (si possible).|  
  
##  <a name="BKMK_profile"></a>Utiliser des profils d’exécution de mise à jour  
 Chaque exécution de mise à jour peut être associé à un profil d’exécution de mise à jour spécifique. La valeur par défaut profil d’exécution de mise à jour sont stockée dans le *%windir%\cluster* dossier. Si vous utilisez le UI CAU dans la mise à jour à distance en mode, vous pouvez spécifier un profil d’exécution de mise à jour à l’heure que vous appliquez des mises à jour, ou vous pouvez utiliser le profil d’exécution de mise à jour par défaut. Si vous utilisez adaptée aux clusters en mode de mise à jour automatique, vous pouvez importer les paramètres à partir d’un profil d’exécution de mise à jour spécifié lorsque vous configurez les options de mise à jour automatique. Dans les deux cas, vous pouvez remplacer les valeurs affichées pour les options d’exécution de mise à jour selon vos besoins. Si vous le souhaitez, vous pouvez enregistrer les options d’exécution de mise à jour comme un profil d’exécution de mise à jour avec le même nom de fichier ou un autre nom de fichier. La prochaine fois que vous appliquez des mises à jour ou configurez automatiquement les options de mise à jour automatique, adaptée aux clusters sélectionne la mise à jour de profil d’exécution qui a été précédemment sélectionné.  
  
 Vous pouvez modifier un profil d’exécution de mise à jour existant ou créer un nouveau en sélectionnant **créer ou modifier le profil d’exécution de mise à jour** dans l’UI CAU.

Voici quelques remarques importantes sur l’utilisation de profils d’exécution de mise à jour:

* Un profil d’exécution de mise à jour ne stocke pas les informations spécifiques aux clusters telles que les informations d’identification d’administration. Si vous utilisez adaptée aux clusters en mode de mise à jour automatique, le profil d’exécution de mise à jour n’également stocker les informations de planification de mise à jour automatique. Cela rend possible de partager un profil d’exécution de mise à jour sur tous les clusters de basculement dans une classe spécifiée.
* Si vous configurez les options de mise à jour automatique à l’aide d’un profil d’exécution de mise à jour et que vous modifiez par la suite le profil avec différentes valeurs pour les options d’exécution de mise à jour, la configuration de mise à jour automatique ne change pas automatiquement. Pour appliquer les nouveaux paramètres de mise à jour, vous devez configurer les options de mise à jour automatique à nouveau.
* L’éditeur de profil exécuter Malheureusement non pris en charge les chemins de fichier qui comprennent des espaces, tel que *C:\Program Files*. Pour résoudre ce problème, stockez votre antérieur et valider les scripts de mise à jour dans un chemin d’accès qui n’inclure des espaces ou utiliser PowerShell exclusivement pour gérer les profils d’exécution, de placer le chemin d’accès entre guillemets lors de l’exécution **Invoke-CauRun**.

### <a name="windows-powershell-equivalent-commands"></a>Commandes Windows PowerShell équivalentes
  
 Vous pouvez importer les paramètres à partir d’un profil d’exécution de mise à jour lorsque vous exécutez le **Invoke-CauRun**, **Add-CauClusterRole**, ou **Set-CauClusterRole** applet de commande.  
  
 L’exemple suivant effectue une analyse et un complète mise à jour de s’exécuter sur le cluster nommé *CONTOSO-FC1*, en utilisant les options de mise à jour qui sont spécifiées dans *C:\Windows\Cluster\DefaultParameters.xml*. Valeurs par défaut sont utilisées pour les paramètres d’applet de commande restants.  
  
```powershell  
$MyRunProfile = Import-Clixml C:\Windows\Cluster\DefaultParameters.xml  
Invoke-CauRun –ClusterName CONTOSO-FC1 @MyRunProfile   
```  
  
 En utilisant un profil d’exécution de mise à jour, vous pouvez mettre à jour un cluster de basculement répétée avec des paramètres cohérents pour la gestion des exceptions, les limites de temps et d’autres paramètres opérationnels. Étant donné que ces paramètres sont généralement spécifiques à une classe de clusters de basculement, tels que «Tous les clusters MicrosoftSQLServer» ou «Mes clusters critiques», vous souhaiterez peut-être nommer chaque profil d’exécution de mise à jour en fonction de la classe de Clusters de basculement il sera utilisé avec. En outre, vous souhaiterez peut-être gérer le profil d’exécution de mise à jour sur un partage de fichiers qui est accessible à tous les clusters de basculement d’une classe spécifique dans votre organisation informatique.  
  
  
  
## <a name="see-also"></a>Voir aussi

-   [Mise à jour adaptée aux clusters](cluster-aware-updating.md)
  
-   [Applets de commande de mise à jour adaptée aux clusters dans Windows PowerShell](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating)