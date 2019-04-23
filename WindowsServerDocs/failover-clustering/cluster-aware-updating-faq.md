---
ms.assetid: 6416d125-bcaf-433d-971a-2f0283bca2c2
title: La mise à jour adaptée aux clusters - Forum aux Questions
ms.topic: article
ms.prod: windows-server-threshold
manager: dongill
ms.author: jgerend
author: JasonGerend
ms.date: 04/28/2017
description: Réponses aux questions fréquemment posées sur la mise à jour adaptée aux clusters dans Windows Server.
ms.openlocfilehash: f9009811093823554f16295cc1205f1b99ead93f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882520"
---
# <a name="cluster-aware-updating-frequently-asked-questions"></a>La mise à jour adaptée aux clusters : Forum Aux Questions

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

[La mise à jour adaptée aux clusters](cluster-aware-updating.md) \(adaptée aux clusters\) est une fonctionnalité qui coordonne le logiciel met à jour sur tous les serveurs dans un cluster de basculement d’une manière qui n’affecte pas la disponibilité du service plus qu’un basculement planifié sur un nœud de cluster. Pour certaines applications offrant des fonctionnalités de disponibilité continue \(telles que Hyper\-V avec migration dynamique ou un serveur de fichiers SMB 3.x avec basculement Transparent SMB\), adaptée aux clusters coordonne la mise à jour d’automatisée des clusters sans aucun impact sur la disponibilité du service.

## <a name="does-cau-support-updating-storage-spaces-direct-clusters"></a>Adaptée aux clusters prend-elle en charge des clusters d’espaces de stockage Direct avec mise à jour ?  
Oui. Adaptée aux clusters prend en charge la mise à jour [espaces de stockage Direct](../storage/storage-spaces/storage-spaces-direct-overview.md) clusters, quel que soit le type de déploiement : hyperconvergé ou convergé. Plus précisément, orchestration d’adaptée aux clusters permet de s’assurer que la suspension de chaque nœud de cluster attend de l’espace de stockage en cluster sous-jacent être sain.

## <a name="does-cau-work-with-windowsserver-2008r2-or-windows7"></a>La mise à jour adaptée aux clusters fonctionne-t-elle dans Windows Server 2008 R2 et Windows 7 ?  
Non. Adaptée aux clusters coordonne la mise à jour de l’opération uniquement sur les ordinateurs exécutant Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10, Windows 8.1 ou Windows 8 de cluster. Le cluster de basculement en cours de mise à jour doit exécuter Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012.
  
## <a name="is-cau-limited-to-specific-clustered-applications"></a>Adaptée aux clusters est limitée aux applications en cluster spécifiques ?  
Non. Cette fonctionnalité peut être utilisée quel que soit le type de l’application en cluster. Adaptée aux clusters est un cluster externe\-solution située dans la couche supérieure des API et les cmdlets PowerShell de clustering de la mise à jour. Par conséquent, adaptée aux clusters coordonne la mise à jour pour n’importe quelle application en cluster qui est configurée dans un cluster de basculement Windows Server.  
  
> [!NOTE]  
> Actuellement, les charges de travail en cluster suivantes sont testées et certifiées pour adaptée aux clusters : SMB, Hyper\-V, la réplication DFS, espaces de noms DFS, iSCSI et NFS.  
  
## <a name="does-cau-support-updates-from-microsoft-update-and-windows-update"></a>La mise à jour adaptée aux clusters prend-elle en charge les mises à jour de Microsoft Update et de Windows Update ?  
Oui. Par défaut, adaptée aux clusters est configurée avec un plug\-dans qui utilise l’Agent de mise à jour Windows \(WUA\) API de l’utilitaire sur les nœuds du cluster. L’infrastructure WUA peut être configuré pour pointer vers Microsoft Update et Windows Update ou Windows Server Update Services \(WSUS\) comme source des mises à jour.  
  
## <a name="does-cau-support-wsus-updates"></a>La mise à jour adaptée aux clusters prend-elle en charge les mises à jour des services WSUS ?  
Oui. Par défaut, adaptée aux clusters est configurée avec un plug\-dans qui utilise l’Agent de mise à jour Windows \(WUA\) API de l’utilitaire sur les nœuds du cluster. L’infrastructure WUA peut être configuré pour pointer vers Microsoft Update et Windows Update ou un serveur Windows Server Update Services local \(WSUS\) serveur comme source des mises à jour.  
  
## <a name="can-cau-apply-limited-distribution-release-updates"></a>La mise à jour adaptée aux clusters applique-t-elle les mises à jour LDR ?  
Oui. Limité de correctif logiciel \(LDR\) mises à jour, également appelées correctifs logiciels, ne sont pas publiées via Microsoft Update ou Windows Update, ils ne peuvent pas être téléchargées par l’Agent de mise à jour Windows \(WUA\) Branchez\-dans la mesure adaptée aux clusters utilise par défaut.  
  
Toutefois, adaptée aux clusters inclut un second plug\-dans la mesure où vous pouvez choisir d’appliquer des mises à jour de correctif logiciel. Ce plug-correctif logiciel\-dans peut également être personnalisée pour appliquer non\-pilote Microsoft, des microprogrammes et des mises à jour du BIOS.  
  
## <a name="can-i-use-cau-to-apply-cumulative-updates"></a>Puis-je utiliser la mise à jour adaptée aux clusters pour appliquer des mises à jour cumulatives ?  
Oui. Si les mises à jour cumulatives sont des mises à jour de correctifs logiciels grand public ou LDR (Limited Distribution Release), elles peuvent être appliquées par la mise à jour adaptée aux clusters.  
  
## <a name="can-i-schedule-updates"></a>Puis-je planifier les mises à jour ?  
Oui. La mise à jour adaptée aux clusters prend en charge les modes de mise à jour suivants, qui permettent de planifier des mises à jour :  
  
**Self\-la mise à jour** permet au cluster se mettre à jour selon une planification régulière, un profil défini comme pendant une fenêtre de maintenance mensuelle. Vous pouvez également démarrer un libre-service\-la mise à jour exécutés à la demande à tout moment. Pour activer auto\-la mise à jour en mode, vous devez ajouter le rôle en cluster adaptée aux clusters au cluster. Cette dernière self\-mise à jour de la fonctionnalité fonctionne comme toute autre charge de travail en cluster, et il peut fonctionner en toute transparence avec les basculements planifiés et non planifiés d’un ordinateur coordinateur de mise à jour.  
  
**À distance\-la mise à jour** afin de pouvoir démarrer une exécution de la mise à jour à tout moment à partir d’un ordinateur exécutant Windows ou Windows Server. Vous pouvez démarrer une mise à jour à exécuter via la fenêtre de la mise à jour adaptée aux clusters ou en utilisant le **Invoke\-CauRun** applet de commande PowerShell. À distance\-la mise à jour est la valeur par défaut, la mise à jour de mode pour adaptée aux clusters. Vous pouvez utiliser le Planificateur de tâches pour exécuter l’applet de commande [Invoke-CauRun](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/invoke-caurun) au moment souhaité à partir d’un ordinateur distant qui ne fait pas partie d’un des nœuds du cluster.  
  
## <a name="can-i-schedule-updates-to-apply-during-a-backup"></a>Puis-je planifier les mises à jour à appliquer lors d’une sauvegarde ?  
Oui. Adaptée aux clusters n’impose aucune restriction à cet égard. Toutefois, effectuez les mises à jour logicielles sur un serveur \(présentant le risque associé redémarre\) pendant qu’une sauvegarde du serveur est en cours n’est pas une meilleure pratique informatique. Gardez à l’esprit que la mise à jour adaptée aux clusters s’appuie exclusivement sur les API de clustering pour déterminer quels basculements et restaurations de ressources effectuer ; elle n’a donc pas connaissance de l’état de la sauvegarde sur le serveur.  
  
## <a name="can-cau-work-with-system-center-configuration-manager"></a>Adaptée aux clusters utilisable avec System Center Configuration Manager ?  
Adaptée aux clusters est un outil qui coordonne les mises à jour logicielles sur un nœud de cluster et Configuration Manager effectue des mises à jour logicielles de serveur. Il est important de configurer ces outils afin qu’ils n’ont pas effectuent sur les mêmes serveurs dans tout déploiement de centre de données, y compris à l’aide de différents serveurs Windows Server Update Services. Cela garantit que l’objectif derrière adaptée n’est pas par inadvertance invalidée, étant donné que Configuration Manager\-pilotés par la mise à jour n’ajoute pas la reconnaissance de cluster.  
  
## <a name="do-i-need-administrative-credentials-to-run-cau"></a>Ai-je besoin d’informations d’identification d’administration pour exécuter la mise à jour adaptée aux clusters ?  
Oui. Pour exécuter les outils de mise à jour adaptée aux clusters, la mise à jour adaptée aux clusters nécessite des informations d’identification d’administration sur le serveur local, ou le privilège **Emprunter l’identité d’un client après l’authentification** sur le serveur local ou sur l’ordinateur client où doivent être exécutés les outils. Toutefois, pour coordonner les mises à jour logicielles sur les nœuds de cluster, la mise à jour adaptée aux clusters nécessite des informations d’identification d’administration de cluster sur chaque nœud. Bien que l’UI CAU peut démarrer sans les informations d’identification, il vous invite à entrer les informations d’identification d’administration de cluster lorsqu’il se connecte à une instance de cluster pour avoir un aperçu ou appliquer des mises à jour.  
  
## <a name="can-i-script-cau"></a>Puis-je créer un script adaptée aux clusters ?  
Oui. Adaptée aux clusters est fourni avec les applets de commande PowerShell qui offrent un ensemble complet d’options de script. Ce sont les mêmes applets de commande que celles appelées par l’interface utilisateur de la mise à jour adaptée aux clusters pour effectuer les tâches de mise à jour.  

## <a name="what-happens-to-active-clustered-roles"></a>Que se passe-t-il pour les rôles en cluster actifs ?

Rôles en cluster \(auparavant appelé applications et services\) qui sont actifs sur un nœud, basculer vers d’autres nœuds avant la mise à jour logicielles puisse commencer. La mise à jour adaptée aux clusters orchestre ces basculements à l’aide d’un mode de maintenance qui met en pause et draine le nœud de tous les rôles en cluster actifs. Une fois les mises à jour logicielles terminées, la mise à jour adaptée aux clusters reprend l’exécution du nœud et restaure les rôles en cluster sur le nœud mis à jour. Vous avez ainsi l’assurance que la distribution des rôles en cluster sur les nœuds reste la même entre chaque exécution de mise à jour d’un cluster par la mise à jour adaptée aux clusters.  
  
## <a name="how-does-cau-select-target-nodes-for-clustered-roles"></a>Comment adaptée aux clusters sélectionner des nœuds cibles pour les rôles en cluster ?

La mise à jour adaptée aux clusters s’appuie sur les API de clustering pour coordonner les basculements. L’implémentation API de clustering sélectionne les nœuds cibles en vous appuyant sur les mesures internes et les heuristiques de placement intelligent \(telles que les niveaux de charge de travail\) entre les nœuds cible.  
  
## <a name="does-cau-load-balance-the-clustered-roles"></a>Adaptée aux clusters équilibrer la charge les rôles en cluster ?

Adaptée aux clusters ne charge équilibre les nœuds de cluster, mais il tente de préserver la distribution des rôles en cluster. Après avoir mis à jour un nœud de cluster, la mise à jour adaptée aux clusters tente de restaurer les rôles en cluster préalablement hébergés sur ce nœud. La mise à jour adaptée aux clusters s’appuie sur les API de clustering pour restaurer les ressources au début du processus de mise en pause. Par conséquent, en l’absence de basculements non planifiés et de paramètres de propriétaire favoris, la distribution des rôles en cluster doit en principe rester identique.  
  
## <a name="how-does-cau-select-the-order-of-nodes-to-update"></a>Comment la mise à jour adaptée aux clusters détermine-t-elle l’ordre de mise à jour des nœuds ?  
Par défaut, la mise à jour adaptée aux clusters détermine l’ordre de mise à jour des nœuds selon le niveau d’activité. Les nœuds présentant le moins de rôles en cluster sont mis à jour en premier. Toutefois, un administrateur peut spécifier un ordre particulier pour la mise à jour les nœuds en définissant un paramètre pour l’exécution de la mise à jour dans l’UI CAU ou à l’aide des applets de commande PowerShell.  
  
## <a name="what-happens-if-a-cluster-node-is-offline"></a>Que se passe-t-il si un nœud de cluster est hors connexion ?

L’administrateur qui lance une exécution de mise à jour peut spécifier le nombre maximal de nœuds hors connexion autorisés. L’exécution de mise à jour sur un cluster dont certains nœuds sont hors connexion peut alors se dérouler sans problème.  
  
## <a name="can-i-use-cau-to-update-only-a-single-node"></a>Puis-je utiliser adaptée aux clusters pour mettre à jour d’un seul nœud ?  
Non. Adaptée aux clusters est un cluster\-étendue de la mise à jour de l’outil, afin qu’il vous autorise uniquement à sélectionner des clusters pour mettre à jour. Pour mettre à jour un seul nœud, utilisez d’autres outils de mise à jour du serveur à la place de la mise à jour adaptée aux clusters.  
  
## <a name="can-cau-report-updates-that-are-initiated-from-outside-cau"></a>Adaptée aux clusters peut signaler des mises à jour sont lancées à partir de l’extérieur adaptée aux clusters ?  
Non. Seules les exécutions de mise à jour qui ont été lancées à l’aide de la mise à jour adaptée aux clusters sont signalées. Toutefois, lorsque un suivantes exécution adaptée aux clusters la mise à jour est lancée, mises à jour qui ont été installés via non\-méthodes sont considérés comme correctement pour déterminer les mises à jour supplémentaires qui peuvent s’appliquer à chaque nœud du cluster.  
  
## <a name="can-cau-support-my-unique-it-process-needs"></a>Pouvez mon processus informatique unique a besoin de la prise en charge adaptée aux clusters ?  
Oui. La mise à jour adaptée aux clusters offre les options de flexibilité suivantes pour répondre à tous les besoins de l’entreprise en termes de processus informatiques :  
  
**Scripts** une exécution de la mise à jour peut spécifier un pre\-mettre à jour le script PowerShell et un post\-mettre à jour le script PowerShell. La version antérieure de\-script de mise à jour s’exécute sur chaque nœud de cluster avant que le nœud est suspendu. Le billet\-script de mise à jour s’exécute sur chaque nœud de cluster après l’installent des mises à jour du nœud.  
  
> [!NOTE]  
> .NET framework 4.6 ou 4.5 et PowerShell doivent être installé sur chaque nœud de cluster sur lequel vous souhaitez exécuter la version antérieure de\-mettre à jour et valider\-mettre à jour les scripts. Vous devez également activer la communication à distance de PowerShell sur les nœuds du cluster. Pour connaître la configuration requise, consultez [configuration requise et meilleures pratiques pour la mise à jour adaptée aux clusters](cluster-aware-updating-requirements.md).  
  
**Options d’exécution de la mise à jour avancées** l’administrateur peut en outre spécifier à partir d’un éventail d’options d’exécution de la mise à jour avancées telles que le nombre maximal de fois que le processus de mise à jour est retenté sur chaque nœud. Ces options peuvent être spécifiées à l’aide de l’UI CAU ou les applets de commande PowerShell d’adaptée aux clusters. Ces paramètres de configuration peuvent être enregistrés dans un profil d’exécution de mise à jour et réutilisés pour des exécutions de mise à jour ultérieures.  
  
**Public plug\-dans architecture** adaptée aux clusters inclut les fonctionnalités conçues pour s’inscrire, annuler l’inscription, et sélectionnez Branchez\-ins. Adaptée aux clusters est livré avec la valeur par défaut deux plug\-ins : un coordonne l’Agent de mise à jour Windows \(WUA\) API sur chaque nœud de cluster ; la deuxième applique les correctifs logiciels qui sont copiés manuellement dans un partage de fichiers est accessible aux nœuds du cluster. Si une entreprise a des besoins uniques qui ne peuvent pas être satisfaites avec ces deux Branchez\-ins, l’entreprise peut créer un nouveau plug adaptée aux clusters\-dans conformément à la spécification d’API publique. Pour plus d’informations, consultez [Cluster\-Plug prenant en charge de la mise à jour\-dans référence](https://msdn.microsoft.com/library/hh418084(VS.85).aspx).  
  
Pour plus d’informations sur la configuration et de personnalisation adaptée aux clusters Branchez\-ins pour prendre en charge différents scénarios, de la mise à jour consultez [comment brancher\-ins fonctionnent](assetId:///847b571b-12b3-473c-953f-75a5a1f51333).  
  
## <a name="how-can-i-export-the-cau-preview-and-update-results"></a>Comment exporter les résultats de l’aperçu et de la mise à jour effectués par la mise à jour adaptée aux clusters ?  
Options via la commande d’exportation à jour adaptée aux offres\-interface de ligne et via l’interface utilisateur.  
  
**Commande\-options de l’interface de ligne :**  
  
-   Aperçu des résultats à l’aide de l’applet de commande PowerShell **Invoke\-CauScan | ConvertTo\-Xml**. Output: XML  
  
-   Rapport de résultats à l’aide de l’applet de commande PowerShell **Invoke\-CauRun | ConvertTo\-Xml**. Output: XML  
  
-   Rapport de résultats à l’aide de l’applet de commande PowerShell **obtenir\-CauReport | Exporter\-CauReport**. Output: HTML, CSV  
  
**Options de l’interface utilisateur :**  
  
-   Utilisez l’écran **Afficher un aperçu des mises à jour** pour copier les résultats d’un rapport. Output: Volume partagé de cluster  
  
-   Utilisez l’écran **Générer le rapport** pour copier les résultats d’un rapport. Output: Volume partagé de cluster  
  
-   Utilisez l’écran **Générer le rapport** pour exporter les résultats d’un rapport. Output: HTML  
  
## <a name="how-do-i-install-cau"></a>Comment installer la mise à jour adaptée aux clusters ?  
Une installation de la mise à jour adaptée aux clusters est intégrée de manière transparente à la fonctionnalité Clustering avec basculement. La mise à jour adaptée aux clusters est installée comme suit :  
  
-   Lorsque le clustering avec basculement est installée sur un nœud de cluster, l’Instrumentation de gestion adaptée aux clusters Windows \(WMI\) fournisseur est installé automatiquement.  
  
-   Lorsque la fonctionnalité Outils de clustering avec basculement est installée sur un ordinateur client ou serveur, les applets de commande PowerShell et de l’interface utilisateur de la mise à jour adaptée aux clusters sont automatiquement installés.
  
## <a name="does-cau-need-components-running-on-the-cluster-nodes-that-are-being-updated"></a>Adaptée aux clusters n’a besoin des composants qui s’exécutent sur les nœuds de cluster en cours de mise à jour ?  
Adaptée aux clusters n’a pas besoin d’un service en cours d’exécution sur les nœuds du cluster. Toutefois, elle nécessite un composant logiciel \(le fournisseur WMI\) installé sur les nœuds du cluster. Ce composant est installé avec la fonctionnalité Clustering avec basculement.  
  
Pour activer auto\-la mise à jour en mode, le rôle en cluster adaptée aux clusters doit également être ajouté au cluster.  
  
## <a name="what-is-the-difference-between-using-cau-and-vmm"></a>Quelle est la différence entre l’utilisation adaptée aux clusters et VMM ?  
  
-   System Center Virtual Machine Manager \(VMM\) se concentre sur la mise à jour Hyper uniquement\-V clusters, alors qu’adaptée aux clusters peut mettre à jour n’importe quel type de cluster de basculement prises en charge, y compris Hyper\-clusters V.  
  
-   VMM nécessite des licences supplémentaires, tandis qu’adaptée aux clusters est concédé sous licence pour tous les Windows Server. Les fonctionnalités, les outils et l’interface utilisateur de la mise à jour adaptée aux clusters sont installés avec les composants Clustering avec basculement.  
  
-   Si vous possédez déjà une licence de System Center, vous pouvez continuer à utiliser VMM pour mettre à jour Hyper\-V clusters, car il offre une gestion intégrée et l’expérience de mise à jour logicielle.  
  
-   Adaptée aux clusters est prise en charge uniquement sur les clusters qui exécutent Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012. VMM prend également en charge Hyper\-V clusters sur des ordinateurs exécutant Windows Server 2008 R2 et Windows Server 2008.  
  
## <a name="can-i-use-remote-updating-on-a-cluster-that-is-configured-for-self-updating"></a>Puis-je utiliser à distance\-la mise à jour sur un cluster qui est configuré pour self\-la mise à jour ?  
Oui. Un cluster de basculement dans un libre-service\-mise à jour de configuration peut être mis à jour à distance par le biais\-la mise à jour sur\-à la demande, tout comme vous pouvez forcer une analyse de mise à jour de Windows à tout moment sur votre ordinateur, même si la mise à jour de Windows est configuré pour installer les mises à jour automatiquement. Au préalable, vous devez vous assurer qu’aucune exécution de mise à jour n’est en cours.  
  
## <a name="can-i-reuse-my-cluster-update-settings-across-clusters"></a>Puis-je utiliser les mêmes paramètres de mise à jour pour tous les clusters ?  
Oui. La mise à jour adaptée aux clusters comporte plusieurs options d’exécution de mise à jour qui vous permettent de définir le comportement de l’exécution de mise à jour pour un cluster. Vous pouvez enregistrer les options définies dans un profil d’exécution de mise à jour afin de les réutiliser pour d’autres clusters. Nous vous recommandons d’enregistrer et d’utiliser les mêmes paramètres sur tous les clusters de basculement qui nécessitent des mises à jour similaires. Par exemple, vous pouvez créer une « Business\-profil d’exécution de mise à jour critique SQL Server Cluster » pour tous les clusters Microsoft SQL Server qui prennent en charge de business\-services critiques.  
  
## <a name="where-is-the-cau-plug-in-specification"></a>Où est le plug adaptée aux clusters\-dans spécification ?  
  
-   [Cluster\-prenant en charge la mise à jour Plug\-dans la référence](https://msdn.microsoft.com/library/hh418084(VS.85).aspx)  
  
-   [Cluster prenant en charge la mise à jour plug\-dans l’exemple](https://code.msdn.microsoft.com/windowsdesktop/Cluster-Aware-Updating-6a8854c9)  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Cluster\-vue d’ensemble de la mise à jour](cluster-aware-updating.md)  
  
