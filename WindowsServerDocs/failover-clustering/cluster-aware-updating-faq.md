---
ms.assetid: 6416d125-bcaf-433d-971a-2f0283bca2c2
title: 'Mise à jour adaptée aux clusters : Forum aux questions'
ms.topic: article
ms.prod: windows-server
manager: lizross
ms.author: jgerend
author: JasonGerend
ms.date: 04/28/2017
description: Réponses aux questions fréquemment posées sur la mise à jour adaptée aux clusters dans Windows Server.
ms.openlocfilehash: ca81e952c0524af36ab6d241a205bd1cc971c74a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80828102"
---
# <a name="cluster-aware-updating-frequently-asked-questions"></a>Mise à jour adaptée aux clusters : Forum Aux Questions

> S’applique à : Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

La [mise à jour adaptée aux clusters \(la mise à jour](cluster-aware-updating.md) adaptée aux\) est une fonctionnalité qui coordonne les mises à jour logicielles sur tous les serveurs d’un cluster de basculement d’une manière qui n’affecte pas la disponibilité du service plus qu’un basculement planifié d’un nœud de cluster. Pour certaines applications avec des fonctionnalités de disponibilité continue \(comme hyper\-V avec la migration dynamique, ou un serveur de fichiers SMB 3. x avec basculement transparent SMB\), la mise à jour adaptée aux clusters peut coordonner la mise à jour automatisée des clusters sans aucun impact sur la disponibilité du service.

## <a name="does-cau-support-updating-storage-spaces-direct-clusters"></a>La mise à jour adaptée aux clusters espaces de stockage direct est-elle prise en charge ?  
Oui. La mise à jour adaptée aux clusters prend en charge la mise à jour des clusters [espaces de stockage direct](../storage/storage-spaces/storage-spaces-direct-overview.md) , quel que soit le type de déploiement : Hyper-convergé ou convergé. Plus précisément, l’orchestration de la mise à jour adaptée aux clusters garantit que la suspension de chaque nœud de cluster attend l’intégrité de l’espace de stockage en cluster sous-jacent.

## <a name="does-cau-work-with-windowsserver-2008r2-or-windows7"></a>La mise à jour adaptée aux clusters fonctionne-t-elle dans Windows Server 2008 R2 et Windows 7 ?  
Non. La mise à jour adaptée aux clusters coordonne l’opération de mise à jour des clusters uniquement à partir d’ordinateurs exécutant Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10, Windows 8.1 ou Windows 8. Le cluster de basculement mis à jour doit exécuter Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012.
  
## <a name="is-cau-limited-to-specific-clustered-applications"></a>La mise à jour adaptée aux applications en cluster est-elle limitée ?  
Non. Cette fonctionnalité peut être utilisée quel que soit le type de l’application en cluster. La mise à jour adaptée aux clusters est un cluster externe\-la mise à jour de la solution qui est superposée sur les API de clustering et les applets de commande PowerShell. Par conséquent, la mise à jour adaptée aux clusters peut coordonner les mises à jour des applications en cluster configurées dans un cluster de basculement Windows Server.  
  
> [!NOTE]  
> Actuellement, les charges de travail en cluster suivantes sont testées et certifiées pour la mise à jour adaptée aux clusters : SMB, hyper\-V, réplication DFS, espaces de noms DFS, iSCSI et NFS.  
  
## <a name="does-cau-support-updates-from-microsoft-update-and-windows-update"></a>La mise à jour adaptée aux clusters prend-elle en charge les mises à jour de Microsoft Update et de Windows Update ?  
Oui. Par défaut, la mise à jour adaptée aux clusters est configurée avec un plug\-dans qui utilise les API de l’utilitaire Windows Update Agent \(WUA\) sur les nœuds du cluster. L’infrastructure WUA peut être configurée pour pointer vers Microsoft Update et Windows Update ou pour Windows Server Update Services \(WSUS\) comme source de mises à jour.  
  
## <a name="does-cau-support-wsus-updates"></a>La mise à jour adaptée aux clusters prend-elle en charge les mises à jour des services WSUS ?  
Oui. Par défaut, la mise à jour adaptée aux clusters est configurée avec un plug\-dans qui utilise les API de l’utilitaire Windows Update Agent \(WUA\) sur les nœuds du cluster. L’infrastructure WUA peut être configurée pour pointer vers Microsoft Update et Windows Update ou vers un Windows Server Update Services local \(serveur WSUS\) comme source de mises à jour.  
  
## <a name="can-cau-apply-limited-distribution-release-updates"></a>La mise à jour adaptée aux clusters applique-t-elle les mises à jour LDR ?  
Oui. La version de distribution limitée \(les mises à jour LDR\), également appelées correctifs, ne sont pas publiées par Microsoft Update ou Windows Update. elles ne peuvent donc pas être téléchargées par l’agent Windows Update \(la\) de plug-in de la mise à jour adaptée aux clusters par défaut.\-  
  
Toutefois, la mise à jour adaptée aux clusters comprend un deuxième\-de connexion dans lequel vous pouvez choisir d’appliquer des mises à jour correctives. Ce correctif\-dans peut également être personnalisé pour appliquer des mises à jour du pilote, du microprogramme et du BIOS non\-Microsoft.  
  
## <a name="can-i-use-cau-to-apply-cumulative-updates"></a>Puis-je utiliser la mise à jour adaptée aux clusters pour appliquer des mises à jour cumulatives ?  
Oui. Si les mises à jour cumulatives sont des mises à jour de correctifs logiciels grand public ou LDR (Limited Distribution Release), elles peuvent être appliquées par la mise à jour adaptée aux clusters.  
  
## <a name="can-i-schedule-updates"></a>Puis-je planifier des mises à jour ?  
Oui. La mise à jour adaptée aux clusters prend en charge les modes de mise à jour suivants, qui permettent de planifier des mises à jour :  
  
**Mise à jour de l’auto\-** Permet au cluster de se mettre à jour lui-même en fonction d’un profil défini et d’une planification régulière, par exemple lors d’une fenêtre de maintenance mensuelle. Vous pouvez également démarrer une exécution de mise à jour de\-à la demande à tout moment. Pour activer le mode de mise à jour de la\-automatique, vous devez ajouter le rôle en cluster de la mise à jour adaptée aux clusters au cluster. La fonctionnalité de mise à jour de la mise à jour\-adaptée aux clusters se comporte comme toute autre charge de travail en cluster. elle peut fonctionner en toute transparence avec les basculements planifiés et non planifiés d’un ordinateur coordinateur de mise à jour.  
  
**Mise à jour des\-à distance** Vous permet de démarrer une exécution de mise à jour à tout moment à partir d’un ordinateur exécutant Windows ou Windows Server. Vous pouvez démarrer une exécution de mise à jour via la fenêtre de mise à jour adaptée aux clusters ou à l’aide de l’applet de commande **CauRun PowerShell Invoke\-** . La mise à jour\-distance est le mode de mise à jour par défaut pour la mise à jour adaptée aux clusters Vous pouvez utiliser le Planificateur de tâches pour exécuter l’applet de commande [Invoke-CauRun](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/invoke-caurun) au moment souhaité à partir d’un ordinateur distant qui ne fait pas partie d’un des nœuds du cluster.  
  
## <a name="can-i-schedule-updates-to-apply-during-a-backup"></a>Puis-je planifier des mises à jour à appliquer pendant une sauvegarde ?  
Oui. La mise à jour adaptée aux clusters n’impose aucune contrainte à cet égard. Toutefois, l’exécution de mises à jour logicielles sur un serveur \(avec les redémarrages potentiels associés\) alors qu’une sauvegarde du serveur est en cours n’est pas une pratique recommandée. Gardez à l’esprit que la mise à jour adaptée aux clusters s’appuie exclusivement sur les API de clustering pour déterminer quels basculements et restaurations de ressources effectuer ; elle n’a donc pas connaissance de l’état de la sauvegarde sur le serveur.  
  
## <a name="can-cau-work-with-configuration-manager"></a>La mise à jour adaptée aux clusters peut-elle fonctionner avec Configuration Manager  
La mise à jour adaptée aux clusters est un outil qui coordonne les mises à jour logicielles sur un nœud de cluster et Configuration Manager effectue également des mises à jour logicielles du serveur. Il est important de configurer ces outils de manière à ce qu’ils n’aient pas à se chevaucher les mêmes serveurs dans un déploiement de centre de développement, y compris l’utilisation de différents serveurs de Windows Server Update Services. Cela permet de s’assurer que l’objectif de l’utilisation de la mise à jour adaptée aux clusters n’est pas malencontreusement involontaire, car Configuration Manager\-la mise à jour pilotée n’incorpore pas la reconnaissance  
  
## <a name="do-i-need-administrative-credentials-to-run-cau"></a>Ai-je besoin d’informations d’identification d’administration pour exécuter la mise à jour adaptée aux clusters ?  
Oui. Pour exécuter les outils de mise à jour adaptée aux clusters, la mise à jour adaptée aux clusters nécessite des informations d’identification d’administration sur le serveur local, ou le privilège **Emprunter l’identité d’un client après l’authentification** sur le serveur local ou sur l’ordinateur client où doivent être exécutés les outils. Toutefois, pour coordonner les mises à jour logicielles sur les nœuds de cluster, la mise à jour adaptée aux clusters nécessite des informations d’identification d’administration de cluster sur chaque nœud. Bien que l’interface utilisateur de la mise à jour adaptée aux clusters puisse démarrer sans les informations d’identification, elle invite à entrer les informations d’identification d’administration de cluster lorsqu’elle se connecte à une instance de cluster pour afficher un aperçu ou appliquer des mises à jour.  
  
## <a name="can-i-script-cau"></a>Puis-je écrire la mise à jour adaptée ?  
Oui. La mise à jour adaptée aux clusters est fournie avec les applets de commande PowerShell qui offrent un ensemble complet d’options de script. Ce sont les mêmes applets de commande que celles appelées par l’interface utilisateur de la mise à jour adaptée aux clusters pour effectuer les tâches de mise à jour.  

## <a name="what-happens-to-active-clustered-roles"></a>Qu’advient-il des rôles en cluster actifs ?

Les rôles en cluster \(auparavant appelés applications et services\) qui sont actifs sur un nœud, basculent vers d’autres nœuds avant que la mise à jour logicielle puisse commencer. La mise à jour adaptée aux clusters orchestre ces basculements à l’aide d’un mode de maintenance qui met en pause et draine le nœud de tous les rôles en cluster actifs. Une fois les mises à jour logicielles terminées, la mise à jour adaptée aux clusters reprend l’exécution du nœud et restaure les rôles en cluster sur le nœud mis à jour. Vous avez ainsi l’assurance que la distribution des rôles en cluster sur les nœuds reste la même entre chaque exécution de mise à jour d’un cluster par la mise à jour adaptée aux clusters.  
  
## <a name="how-does-cau-select-target-nodes-for-clustered-roles"></a>Comment la mise à jour adaptée aux clusters cible-t-elle des nœuds cibles ?

La mise à jour adaptée aux clusters s’appuie sur les API de clustering pour coordonner les basculements. L’implémentation de l’API de clustering sélectionne les nœuds cibles en se basant sur les mesures internes et les heuristiques de placement intelligentes \(tels que les niveaux de charge de travail\) sur les nœuds cibles.  
  
## <a name="does-cau-load-balance-the-clustered-roles"></a>La mise à jour adaptée aux rôles en cluster est-elle équilibrée ?

La mise à jour adaptée aux clusters n’équilibre pas la charge des nœuds en cluster, mais elle tente de préserver la distribution des rôles en cluster. Après avoir mis à jour un nœud de cluster, la mise à jour adaptée aux clusters tente de restaurer les rôles en cluster préalablement hébergés sur ce nœud. La mise à jour adaptée aux clusters s’appuie sur les API de clustering pour restaurer les ressources au début du processus de mise en pause. Par conséquent, en l’absence de basculements non planifiés et de paramètres de propriétaire favoris, la distribution des rôles en cluster doit en principe rester identique.  
  
## <a name="how-does-cau-select-the-order-of-nodes-to-update"></a>Comment la mise à jour adaptée aux clusters détermine-t-elle l’ordre de mise à jour des nœuds ?  
Par défaut, la mise à jour adaptée aux clusters détermine l’ordre de mise à jour des nœuds selon le niveau d’activité. Les nœuds présentant le moins de rôles en cluster sont mis à jour en premier. Toutefois, un administrateur peut spécifier un ordre particulier pour la mise à jour des nœuds en spécifiant un paramètre pour l’exécution de mise à jour dans l’interface utilisateur de la mise à jour adaptée aux clusters ou à l’aide des applets de commande PowerShell.  
  
## <a name="what-happens-if-a-cluster-node-is-offline"></a>Que se passe-t-il si un nœud de cluster est hors connexion ?

L’administrateur qui lance une exécution de mise à jour peut spécifier le nombre maximal de nœuds hors connexion autorisés. L’exécution de mise à jour sur un cluster dont certains nœuds sont hors connexion peut alors se dérouler sans problème.  
  
## <a name="can-i-use-cau-to-update-only-a-single-node"></a>Puis-je utiliser la mise à jour adaptée aux clusters pour mettre à jour un seul nœud ?  
Non. La mise à jour adaptée aux clusters est un outil de mise à jour d’étendue de cluster\-qui vous permet de sélectionner uniquement les clusters à mettre à jour. Pour mettre à jour un seul nœud, utilisez d’autres outils de mise à jour du serveur à la place de la mise à jour adaptée aux clusters.  
  
## <a name="can-cau-report-updates-that-are-initiated-from-outside-cau"></a>La mise à jour adaptée aux mises à jour est-elle effectuée en dehors de la mise à jour adaptée aux clusters  
Non. Seules les exécutions de mise à jour qui ont été lancées à l’aide de la mise à jour adaptée aux clusters sont signalées. Toutefois, lorsqu’une exécution de mise à jour adaptée aux clusters ultérieures est lancée, les mises à jour qui ont été installées par le biais de méthodes non\-la mise à jour adaptée aux clusters sont prises en compte pour déterminer les mises à jour supplémentaires qui peuvent s’appliquer à chaque nœud du cluster.  
  
## <a name="can-cau-support-my-unique-it-process-needs"></a>La mise à jour adaptée aux clusters peut-elle prendre en charge mes besoins informatiques uniques ?  
Oui. La mise à jour adaptée aux clusters offre les options de flexibilité suivantes pour répondre à tous les besoins de l’entreprise en termes de processus informatiques :  
  
**Scripts** Une exécution de mise à jour peut spécifier un script PowerShell pre\-Update et une publication\-mettre à jour un script PowerShell. Le script de mise à jour préalable\-s’exécute sur chaque nœud du cluster avant que le nœud soit suspendu. Le script de mise à jour de publication\-s’exécute sur chaque nœud du cluster après l’installation des mises à jour du nœud.  
  
> [!NOTE]  
> .NET Framework 4,6 ou 4,5 et PowerShell doivent être installés sur chaque nœud de cluster sur lequel vous souhaitez exécuter les scripts de mise à jour préalable\-et de publication\-. Vous devez également activer la communication à distance PowerShell sur les nœuds du cluster. Pour plus d’informations sur la configuration système requise, consultez [Configuration requise et meilleures pratiques pour la mise à jour adaptée aux clusters](cluster-aware-updating-requirements.md).  
  
**Options d’exécution de mise à jour avancées** L’administrateur peut également spécifier à partir d’un grand nombre d’options d’exécution de mise à jour avancées, telles que le nombre maximal de tentatives de mise à jour du processus de mise à jour sur chaque nœud. Ces options peuvent être spécifiées à l’aide de l’interface utilisateur de la mise à jour adaptée aux clusters ou des applets de commande de PowerShell Ces paramètres de configuration peuvent être enregistrés dans un profil d’exécution de mise à jour et réutilisés pour des exécutions de mise à jour ultérieures.  
  
**\-de la connexion publique dans l’architecture** La mise à jour adaptée aux clusters inclut des fonctionnalités permettant de s’inscrire, d’annuler l’inscription et de sélectionner les plug-ins de\-. la mise à jour adaptée aux clusters est fournie avec deux plug-ins\-par défaut : l’un coordonne l’agent Windows Update\) \(les API le deuxième applique les correctifs logiciels qui sont copiés manuellement dans un partage de fichiers accessible aux nœuds du cluster. Si une entreprise a des besoins uniques qui ne peuvent pas être satisfaits avec ces deux plug-ins\-s, l’entreprise peut créer une nouvelle mise à jour de la mise à jour adaptée aux\-en fonction de la spécification de l’API publique. Pour plus d’informations, consultez la page [prise en charge de la mise à jour du\-de Cluster\-en référence](https://msdn.microsoft.com/library/hh418084(VS.85).aspx).  
  
Pour plus d’informations sur la configuration et la personnalisation de la configuration de la mise à jour adaptée aux clusters\-pour prendre en charge différents scénarios de mise à jour, consultez fonctionnement des [plug-ins\-](assetId:///847b571b-12b3-473c-953f-75a5a1f51333).  
  
## <a name="how-can-i-export-the-cau-preview-and-update-results"></a>Comment exporter les résultats de l’aperçu et de la mise à jour effectués par la mise à jour adaptée aux clusters ?  
La mise à jour adaptée aux clusters offre des options d’exportation via la commande\-interface de ligne et via l’interface utilisateur.  
  
**Options d’interface de ligne de commande\-:**  
  
-   Afficher un aperçu des résultats à l’aide de l’applet de commande PowerShell **Invoke\-CauScan | ConvertTo\-XML**. Sortie : XML  
  
-   Signaler les résultats à l’aide de l’applet de commande PowerShell **Invoke\-CauRun | ConvertTo\-XML**. Sortie : XML  
  
-   Résultats des rapports à l’aide de l’applet de commande PowerShell **obtenir\-CauReport | Exportez\-CauReport**. Sortie : HTML ou CSV.  
  
**Options d’interface utilisateur :**  
  
-   Utilisez l’écran **Afficher un aperçu des mises à jour** pour copier les résultats d’un rapport. Sortie : CSV  
  
-   Utilisez l’écran **Générer le rapport** pour copier les résultats d’un rapport. Sortie : CSV  
  
-   Utilisez l’écran **Générer le rapport** pour exporter les résultats d’un rapport. Sortie : HTML.  
  
## <a name="how-do-i-install-cau"></a>Comment installer la mise à jour adaptée aux clusters ?  
Une installation de la mise à jour adaptée aux clusters est intégrée de manière transparente à la fonctionnalité Clustering avec basculement. La mise à jour adaptée aux clusters est installée comme suit :  
  
-   Lorsque le clustering de basculement est installé sur un nœud de cluster, le fournisseur de\) de la mise à jour adaptée aux clusters \(Windows Management Instrumentation est automatiquement installé.  
  
-   Lorsque la fonctionnalité outils de clustering de basculement est installée sur un serveur ou un ordinateur client, l’interface utilisateur de mise à jour adaptée aux clusters et les applets de commande PowerShell sont installées automatiquement.
  
## <a name="does-cau-need-components-running-on-the-cluster-nodes-that-are-being-updated"></a>La mise à jour adaptée aux clusters nécessitent-elle des composants en cours d’exécution sur les nœuds de cluster  
La mise à jour adaptée aux clusters n’a pas besoin d’un service exécuté sur les nœuds du cluster Toutefois, la mise à jour adaptée aux clusters nécessite un composant logiciel \(le fournisseur WMI\) installé sur les nœuds du cluster. Ce composant est installé avec la fonctionnalité Clustering avec basculement.  
  
Pour activer le mode de mise à jour de la\-automatique, le rôle en cluster de la mise à jour adaptée aux clusters doit également être ajouté au cluster.  
  
## <a name="what-is-the-difference-between-using-cau-and-vmm"></a>Quelle est la différence entre l’utilisation de la mise à jour adaptée aux clusters et VMM ?  
  
-   System Center Virtual Machine Manager \(VMM\) est axé uniquement sur la mise à jour des clusters hyper\-V, alors que la mise à jour adaptée aux clusters peut mettre à jour tout type de cluster de basculement pris en charge, y compris les clusters hyper\-V.  
  
-   VMM nécessite une licence supplémentaire, alors que la mise à jour adaptée aux clusters est autorisée pour tous les serveurs Windows. Les fonctionnalités, les outils et l’interface utilisateur de la mise à jour adaptée aux clusters sont installés avec les composants Clustering avec basculement.  
  
-   Si vous possédez déjà une licence System Center, vous pouvez continuer à utiliser VMM pour mettre à jour les clusters hyper\-V, car il offre une expérience de gestion et de mise à jour logicielle intégrée.  
  
-   La mise à jour adaptée aux clusters est prise en charge uniquement sur les clusters qui exécutent Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012. VMM prend également en charge les clusters hyper\-V sur les ordinateurs exécutant Windows Server 2008 R2 et Windows Server 2008.  
  
## <a name="can-i-use-remote-updating-on-a-cluster-that-is-configured-for-self-updating"></a>Puis-je utiliser la mise à jour\-à distance sur un cluster qui est configuré pour la mise à jour automatique de\-?  
Oui. Un cluster de basculement dans une configuration de mise à jour\-automatique peut être mis à jour par le biais d’une mise à jour\-à distance sur\-demande, de la même façon que vous pouvez forcer une analyse Windows Update à tout moment sur votre ordinateur, même si Windows Update est configuré pour installer automatiquement les mises à jour. Au préalable, vous devez vous assurer qu’aucune exécution de mise à jour n’est en cours.  
  
## <a name="can-i-reuse-my-cluster-update-settings-across-clusters"></a>Puis-je utiliser les mêmes paramètres de mise à jour pour tous les clusters ?  
Oui. La mise à jour adaptée aux clusters comporte plusieurs options d’exécution de mise à jour qui vous permettent de définir le comportement de l’exécution de mise à jour pour un cluster. Vous pouvez enregistrer les options définies dans un profil d’exécution de mise à jour afin de les réutiliser pour d’autres clusters. Nous vous recommandons d’enregistrer et d’utiliser les mêmes paramètres sur tous les clusters de basculement qui nécessitent des mises à jour similaires. Par exemple, vous pouvez créer un « profil d’exécution de mise à jour de cluster d’SQL Server d’entreprise\-critique » pour tous les clusters Microsoft SQL Server qui prennent en charge les services\-critiques pour l’entreprise.  
  
## <a name="where-is-the-cau-plug-in-specification"></a>Où se trouve le plug-in de la mise à jour adaptée aux\-dans la spécification ?  
  
-   [Prise en charge de la mise à jour des\-du plug-in de cluster\-en référence](https://msdn.microsoft.com/library/hh418084(VS.85).aspx)  
  
-   [\-plug-in de mise à jour adaptée aux clusters dans l’exemple](https://code.msdn.microsoft.com/windowsdesktop/Cluster-Aware-Updating-6a8854c9)  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Vue d’ensemble de la mise à jour adaptée aux clusters\-  
  
