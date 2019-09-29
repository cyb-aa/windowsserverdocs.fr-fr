---
ms.assetid: 6416d125-bcaf-433d-971a-2f0283bca2c2
title: 'Mise à jour adaptée aux clusters : Forum aux questions'
ms.topic: article
ms.prod: windows-server
manager: dongill
ms.author: jgerend
author: JasonGerend
ms.date: 04/28/2017
description: Réponses aux questions fréquemment posées sur la mise à jour adaptée aux clusters dans Windows Server.
ms.openlocfilehash: a08366c7e64d9612d63e348d4cecdb4b2389737a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361349"
---
# <a name="cluster-aware-updating-frequently-asked-questions"></a>Mise à jour adaptée aux clusters : Questions fréquemment posées

> S’applique à : Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La [mise à jour adaptée aux clusters](cluster-aware-updating.md) \(CAU @ no__t-2 est une fonctionnalité qui coordonne les mises à jour logicielles sur tous les serveurs d’un cluster de basculement d’une manière qui n’a pas d’impact sur la disponibilité du service plus qu’un basculement planifié d’un nœud de cluster. Pour certaines applications avec des fonctionnalités de disponibilité continue \(such comme hyper @ no__t-1V avec la migration dynamique, ou un serveur de fichiers SMB 3. x avec basculement transparent SMB @ no__t-2, la mise à jour adaptée aux clusters peut coordonner la mise à jour automatisée des clusters sans aucun impact sur le service sa.

## <a name="does-cau-support-updating-storage-spaces-direct-clusters"></a>La mise à jour adaptée aux clusters espaces de stockage direct est-elle prise en charge ?  
Oui. La mise à jour adaptée aux clusters prend en charge la mise à jour des clusters [espaces de stockage direct](../storage/storage-spaces/storage-spaces-direct-overview.md) , quel que soit le type de déploiement : Hyper-convergé ou convergé. Plus précisément, l’orchestration de la mise à jour adaptée aux clusters garantit que la suspension de chaque nœud de cluster attend l’intégrité de l’espace de stockage en cluster sous-jacent.

## <a name="does-cau-work-with-windowsserver-2008r2-or-windows7"></a>La mise à jour adaptée aux clusters fonctionne-t-elle dans Windows Server 2008 R2 et Windows 7 ?  
Non. La mise à jour adaptée aux clusters coordonne l’opération de mise à jour des clusters uniquement à partir d’ordinateurs exécutant Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10, Windows 8.1 ou Windows 8. Le cluster de basculement mis à jour doit exécuter Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012.
  
## <a name="is-cau-limited-to-specific-clustered-applications"></a>La mise à jour adaptée aux applications en cluster est-elle limitée ?  
Non. Cette fonctionnalité peut être utilisée quel que soit le type de l’application en cluster. La mise à jour adaptée aux clusters est une solution de cluster externe @ no__t-0updating qui est superposée au-dessus des API de clustering et des applets de commande PowerShell. Par conséquent, la mise à jour adaptée aux clusters peut coordonner les mises à jour des applications en cluster configurées dans un cluster de basculement Windows Server.  
  
> [!NOTE]  
> Actuellement, les charges de travail en cluster suivantes sont testées et certifiées pour la mise à jour adaptée aux clusters : SMB, hyper @ no__t-0V, réplication DFS, espaces de noms DFS, iSCSI et NFS.  
  
## <a name="does-cau-support-updates-from-microsoft-update-and-windows-update"></a>La mise à jour adaptée aux clusters prend-elle en charge les mises à jour de Microsoft Update et de Windows Update ?  
Oui. Par défaut, la mise à jour adaptée aux clusters est configurée avec un plug @ no__t-0in qui utilise les API de l’agent de Windows Update \(WUA @ no__t-2 sur les nœuds du cluster. L’infrastructure WUA peut être configurée pour pointer vers Microsoft Update et Windows Update ou pour Windows Server Update Services \(WSUS @ no__t-1 comme source de mises à jour.  
  
## <a name="does-cau-support-wsus-updates"></a>La mise à jour adaptée aux clusters prend-elle en charge les mises à jour des services WSUS ?  
Oui. Par défaut, la mise à jour adaptée aux clusters est configurée avec un plug @ no__t-0in qui utilise les API de l’agent de Windows Update \(WUA @ no__t-2 sur les nœuds du cluster. L’infrastructure WUA peut être configurée pour pointer vers Microsoft Update et Windows Update ou vers un serveur local Windows Server Update Services \(WSUS @ no__t-1 en tant que source de mises à jour.  
  
## <a name="can-cau-apply-limited-distribution-release-updates"></a>La mise à jour adaptée aux clusters applique-t-elle les mises à jour LDR ?  
Oui. La version de distribution limitée \(LDR @ no__t-1 mises à jour, également appelées correctifs, ne sont pas publiées via Microsoft Update ou Windows Update. elles ne peuvent donc pas être téléchargées par l’agent Windows Update \(WUA @ no__t-3 plug @ no__t-fois WVGA que la mise à jour adaptée aux clusters utilise par défaut .  
  
Toutefois, la mise à jour adaptée aux clusters comprend un second plug @ no__t-0in que vous pouvez sélectionner pour appliquer les mises à jour correctives. Ce plug-in de correctif @ no__t-0in peut également être personnalisé pour appliquer des mises à jour du pilote, du microprogramme et du BIOS non-no__t-1Microsoft.  
  
## <a name="can-i-use-cau-to-apply-cumulative-updates"></a>Puis-je utiliser la mise à jour adaptée aux clusters pour appliquer des mises à jour cumulatives ?  
Oui. Si les mises à jour cumulatives sont des mises à jour de correctifs logiciels grand public ou LDR (Limited Distribution Release), elles peuvent être appliquées par la mise à jour adaptée aux clusters.  
  
## <a name="can-i-schedule-updates"></a>Puis-je planifier des mises à jour ?  
Oui. La mise à jour adaptée aux clusters prend en charge les modes de mise à jour suivants, qui permettent de planifier des mises à jour :  
  
**Self @ no__t-1updating** Permet au cluster de se mettre à jour lui-même en fonction d’un profil défini et d’une planification régulière, par exemple lors d’une fenêtre de maintenance mensuelle. Vous pouvez également démarrer une exécution à la demande no__t-0Updating à tout moment. Pour activer le mode Self @ no__t-0updating, vous devez ajouter le rôle en cluster de la mise à jour adaptée aux clusters au cluster. La fonctionnalité de mise à jour adaptée aux clusters no__t-0updating fonctionne comme toute autre charge de travail en cluster et peut fonctionner de manière transparente avec les basculements planifiés et non planifiés d’un ordinateur coordinateur de mise à jour.  
  
**À distance, @ no__t-1updating** Vous permet de démarrer une exécution de mise à jour à tout moment à partir d’un ordinateur exécutant Windows ou Windows Server. Vous pouvez démarrer une exécution de mise à jour par le biais de la fenêtre mise à jour adaptée aux clusters ou à l’aide de l’applet de commande PowerShell **@ no__t-1CauRun** . Remote @ no__t-0updating est le mode de mise à jour par défaut pour la mise à jour adaptée aux clusters. Vous pouvez utiliser le Planificateur de tâches pour exécuter l’applet de commande [Invoke-CauRun](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/invoke-caurun) au moment souhaité à partir d’un ordinateur distant qui ne fait pas partie d’un des nœuds du cluster.  
  
## <a name="can-i-schedule-updates-to-apply-during-a-backup"></a>Puis-je planifier des mises à jour à appliquer pendant une sauvegarde ?  
Oui. La mise à jour adaptée aux clusters n’impose aucune contrainte à cet égard. Toutefois, l’exécution de mises à jour logicielles sur un serveur \(With les redémarrages potentiels associés à @ no__t-1 alors qu’une sauvegarde du serveur est en cours n’est pas une pratique recommandée. Gardez à l’esprit que la mise à jour adaptée aux clusters s’appuie exclusivement sur les API de clustering pour déterminer quels basculements et restaurations de ressources effectuer ; elle n’a donc pas connaissance de l’état de la sauvegarde sur le serveur.  
  
## <a name="can-cau-work-with-system-center-configuration-manager"></a>La mise à jour adaptée aux clusters peut-elle fonctionner avec System Center Configuration Manager  
La mise à jour adaptée aux clusters est un outil qui coordonne les mises à jour logicielles sur un nœud de cluster et Configuration Manager effectue également des mises à jour logicielles du serveur. Il est important de configurer ces outils de manière à ce qu’ils n’aient pas à se chevaucher les mêmes serveurs dans un déploiement de centre de développement, y compris l’utilisation de différents serveurs de Windows Server Update Services. Cela permet de s’assurer que l’objectif de l’utilisation de la mise à jour adaptée aux clusters n’est pas involontairement supprimé, car Configuration Manager @ no__t-0driven ne prend pas en compte la prise en  
  
## <a name="do-i-need-administrative-credentials-to-run-cau"></a>Ai-je besoin d’informations d’identification d’administration pour exécuter la mise à jour adaptée aux clusters ?  
Oui. Pour exécuter les outils de mise à jour adaptée aux clusters, la mise à jour adaptée aux clusters nécessite des informations d’identification d’administration sur le serveur local, ou le privilège **Emprunter l’identité d’un client après l’authentification** sur le serveur local ou sur l’ordinateur client où doivent être exécutés les outils. Toutefois, pour coordonner les mises à jour logicielles sur les nœuds de cluster, la mise à jour adaptée aux clusters nécessite des informations d’identification d’administration de cluster sur chaque nœud. Bien que l’interface utilisateur de la mise à jour adaptée aux clusters puisse démarrer sans les informations d’identification, elle invite à entrer les informations d’identification d’administration de cluster lorsqu’elle se connecte à une instance de cluster pour afficher un aperçu ou appliquer des mises à jour.  
  
## <a name="can-i-script-cau"></a>Puis-je écrire la mise à jour adaptée ?  
Oui. La mise à jour adaptée aux clusters est fournie avec les applets de commande PowerShell qui offrent un ensemble complet d’options de script. Ce sont les mêmes applets de commande que celles appelées par l’interface utilisateur de la mise à jour adaptée aux clusters pour effectuer les tâches de mise à jour.  

## <a name="what-happens-to-active-clustered-roles"></a>Qu’advient-il des rôles en cluster actifs ?

Les rôles en cluster \(formerly appelés applications et services @ no__t-1 qui sont actifs sur un nœud, basculent vers d’autres nœuds avant que la mise à jour logicielle puisse commencer. La mise à jour adaptée aux clusters orchestre ces basculements à l’aide d’un mode de maintenance qui met en pause et draine le nœud de tous les rôles en cluster actifs. Une fois les mises à jour logicielles terminées, la mise à jour adaptée aux clusters reprend l’exécution du nœud et restaure les rôles en cluster sur le nœud mis à jour. Vous avez ainsi l’assurance que la distribution des rôles en cluster sur les nœuds reste la même entre chaque exécution de mise à jour d’un cluster par la mise à jour adaptée aux clusters.  
  
## <a name="how-does-cau-select-target-nodes-for-clustered-roles"></a>Comment la mise à jour adaptée aux clusters cible-t-elle des nœuds cibles ?

La mise à jour adaptée aux clusters s’appuie sur les API de clustering pour coordonner les basculements. L’implémentation de l’API de clustering sélectionne les nœuds cibles en se basant sur les mesures internes et les heuristiques de placement intelligent \(such en tant que niveaux de charge de travail @ no__t-1 sur les nœuds cibles.  
  
## <a name="does-cau-load-balance-the-clustered-roles"></a>La mise à jour adaptée aux rôles en cluster est-elle équilibrée ?

La mise à jour adaptée aux clusters n’équilibre pas la charge des nœuds en cluster, mais elle tente de préserver la distribution des rôles en cluster. Après avoir mis à jour un nœud de cluster, la mise à jour adaptée aux clusters tente de restaurer les rôles en cluster préalablement hébergés sur ce nœud. La mise à jour adaptée aux clusters s’appuie sur les API de clustering pour restaurer les ressources au début du processus de mise en pause. Par conséquent, en l’absence de basculements non planifiés et de paramètres de propriétaire favoris, la distribution des rôles en cluster doit en principe rester identique.  
  
## <a name="how-does-cau-select-the-order-of-nodes-to-update"></a>Comment la mise à jour adaptée aux clusters détermine-t-elle l’ordre de mise à jour des nœuds ?  
Par défaut, la mise à jour adaptée aux clusters détermine l’ordre de mise à jour des nœuds selon le niveau d’activité. Les nœuds présentant le moins de rôles en cluster sont mis à jour en premier. Toutefois, un administrateur peut spécifier un ordre particulier pour la mise à jour des nœuds en spécifiant un paramètre pour l’exécution de mise à jour dans l’interface utilisateur de la mise à jour adaptée aux clusters ou à l’aide des applets de commande PowerShell.  
  
## <a name="what-happens-if-a-cluster-node-is-offline"></a>Que se passe-t-il si un nœud de cluster est hors connexion ?

L’administrateur qui lance une exécution de mise à jour peut spécifier le nombre maximal de nœuds hors connexion autorisés. L’exécution de mise à jour sur un cluster dont certains nœuds sont hors connexion peut alors se dérouler sans problème.  
  
## <a name="can-i-use-cau-to-update-only-a-single-node"></a>Puis-je utiliser la mise à jour adaptée aux clusters pour mettre à jour un seul nœud ?  
Non. La mise à jour adaptée aux clusters est un outil de mise à jour de cluster @ no__t-0scoped, ce qui vous permet de sélectionner uniquement les clusters à mettre à jour. Pour mettre à jour un seul nœud, utilisez d’autres outils de mise à jour du serveur à la place de la mise à jour adaptée aux clusters.  
  
## <a name="can-cau-report-updates-that-are-initiated-from-outside-cau"></a>La mise à jour adaptée aux mises à jour est-elle effectuée en dehors de la mise à jour adaptée aux clusters  
Non. Seules les exécutions de mise à jour qui ont été lancées à l’aide de la mise à jour adaptée aux clusters sont signalées. Toutefois, lorsqu’une exécution de mise à jour adaptée aux clusters ultérieures est lancée, les mises à jour qui ont été installées par le biais de méthodes non-no__t-0CAU sont considérées comme pertinentes pour déterminer les mises à jour supplémentaires qui peuvent s’appliquer à chaque nœud de cluster.  
  
## <a name="can-cau-support-my-unique-it-process-needs"></a>La mise à jour adaptée aux clusters peut-elle prendre en charge mes besoins informatiques uniques ?  
Oui. La mise à jour adaptée aux clusters offre les options de flexibilité suivantes pour répondre à tous les besoins de l’entreprise en termes de processus informatiques :  
  
**Scripts** Une exécution de mise à jour peut spécifier un script PowerShell pre @ no__t-1Update et un script PowerShell postérieur à @ no__t-2update. Le script Pre @ no__t-0update s’exécute sur chaque nœud du cluster avant que le nœud soit suspendu. Le script de publication @ no__t-0update s’exécute sur chaque nœud du cluster après l’installation des mises à jour du nœud.  
  
> [!NOTE]  
> .NET Framework 4,6 ou 4,5 et PowerShell doivent être installés sur chaque nœud de cluster sur lequel vous souhaitez exécuter les scripts pre @ no__t-0update et postérieurs à @ no__t-1Update. Vous devez également activer la communication à distance PowerShell sur les nœuds du cluster. Pour plus d’informations sur la configuration système requise, consultez [Configuration requise et meilleures pratiques pour la mise à jour adaptée aux clusters](cluster-aware-updating-requirements.md).  
  
**Options d’exécution de mise à jour avancées** L’administrateur peut également spécifier à partir d’un grand nombre d’options d’exécution de mise à jour avancées, telles que le nombre maximal de tentatives de mise à jour du processus de mise à jour sur chaque nœud. Ces options peuvent être spécifiées à l’aide de l’interface utilisateur de la mise à jour adaptée aux clusters ou des applets de commande de PowerShell Ces paramètres de configuration peuvent être enregistrés dans un profil d’exécution de mise à jour et réutilisés pour des exécutions de mise à jour ultérieures.  
  
**Public plug @ no__t-1Dans architecture** La mise à jour adaptée aux clusters comprend des fonctionnalités permettant de s’inscrire, d’annuler l’inscription et de sélectionner plug @ no__t-2ins. La mise à jour adaptée aux clusters est fournie avec deux plug-no__t-0ins par défaut : l’un coordonne les API Windows Update Agent \(WUA @ no__t-2 sur chaque nœud de cluster ; le deuxième applique les correctifs logiciels qui sont copiés manuellement dans un partage de fichiers accessible aux nœuds du cluster. Si une entreprise a des besoins uniques qui ne peuvent pas être satisfaits avec ces deux plug-no__t-0ins, l’entreprise peut créer un nouveau plug @ no__t-1Dans en fonction de la spécification de l’API publique. Pour plus d’informations, voir [cluster @ no__t-1Aware Updating-no__t-2in Reference](https://msdn.microsoft.com/library/hh418084(VS.85).aspx).  
  
Pour plus d’informations sur la configuration et la personnalisation de la mise à jour adaptée aux clusters no__t-0ins afin de prendre en charge différents scénarios de mise à jour, consultez fonctionnement de [Plug @ no__t-2ins](assetId:///847b571b-12b3-473c-953f-75a5a1f51333).  
  
## <a name="how-can-i-export-the-cau-preview-and-update-results"></a>Comment exporter les résultats de l’aperçu et de la mise à jour effectués par la mise à jour adaptée aux clusters ?  
La mise à jour adaptée aux clusters offre des options d’exportation via l’interface de commande @ no__t-0line et via l’interface utilisateur.  
  
**Options de l’interface de commande @ no__t-1line :**  
  
-   Aperçu des résultats à l’aide de l’applet de commande PowerShell **Invoke @ no__t-1CauScan | ConvertTo @ no__t-2Xml**. Sortie : XML  
  
-   Résultats des rapports à l’aide de l’applet de commande PowerShell **Invoke @ no__t-1CauRun | ConvertTo @ no__t-2Xml**. Sortie : XML  
  
-   Résultats des rapports à l’aide de l’applet de commande PowerShell **@ no__t-1CauReport | Exportez @ no__t-2CauReport**. Sortie : HTML, CSV  
  
**Options d’interface utilisateur :**  
  
-   Utilisez l’écran **Afficher un aperçu des mises à jour** pour copier les résultats d’un rapport. Sortie : CSV  
  
-   Utilisez l’écran **Générer le rapport** pour copier les résultats d’un rapport. Sortie : CSV  
  
-   Utilisez l’écran **Générer le rapport** pour exporter les résultats d’un rapport. Sortie : HTML  
  
## <a name="how-do-i-install-cau"></a>Comment installer la mise à jour adaptée aux clusters ?  
Une installation de la mise à jour adaptée aux clusters est intégrée de manière transparente à la fonctionnalité Clustering avec basculement. La mise à jour adaptée aux clusters est installée comme suit :  
  
-   Lorsque le clustering de basculement est installé sur un nœud de cluster, le fournisseur de la mise à jour adaptée aux clusters Windows Management Instrumentation \(WMI @ no__t-1 est installé automatiquement.  
  
-   Lorsque la fonctionnalité outils de clustering de basculement est installée sur un serveur ou un ordinateur client, l’interface utilisateur de mise à jour adaptée aux clusters et les applets de commande PowerShell sont installées automatiquement.
  
## <a name="does-cau-need-components-running-on-the-cluster-nodes-that-are-being-updated"></a>La mise à jour adaptée aux clusters nécessitent-elle des composants en cours d’exécution sur les nœuds de cluster  
La mise à jour adaptée aux clusters n’a pas besoin d’un service exécuté sur les nœuds du cluster Toutefois, la mise à jour adaptée aux clusters nécessite un composant logiciel @no__t fournisseur WMI 0the @ no__t-1 installé sur les nœuds du cluster. Ce composant est installé avec la fonctionnalité Clustering avec basculement.  
  
Pour activer le mode Self @ no__t-0updating, le rôle en cluster de la mise à jour adaptée aux clusters doit également être ajouté au cluster.  
  
## <a name="what-is-the-difference-between-using-cau-and-vmm"></a>Quelle est la différence entre l’utilisation de la mise à jour adaptée aux clusters et VMM ?  
  
-   System Center Virtual Machine Manager \(VMM @ no__t-1 est axée sur la mise à jour des clusters hyper @ no__t-2V uniquement, tandis que la mise à jour adaptée aux clusters peut mettre à jour tout type de cluster de basculement pris en charge, y compris les clusters hyper @ no__t-3V.  
  
-   VMM nécessite une licence supplémentaire, alors que la mise à jour adaptée aux clusters est autorisée pour tous les serveurs Windows. Les fonctionnalités, les outils et l’interface utilisateur de la mise à jour adaptée aux clusters sont installés avec les composants Clustering avec basculement.  
  
-   Si vous possédez déjà une licence System Center, vous pouvez continuer à utiliser VMM pour mettre à jour les clusters hyper @ no__t-0V, car il offre une expérience de gestion et de mise à jour logicielle intégrée.  
  
-   La mise à jour adaptée aux clusters est prise en charge uniquement sur les clusters qui exécutent Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012. VMM prend également en charge les clusters hyper @ no__t-0V sur les ordinateurs exécutant Windows Server 2008 R2 et Windows Server 2008.  
  
## <a name="can-i-use-remote-updating-on-a-cluster-that-is-configured-for-self-updating"></a>Puis-je utiliser @ no__t-0updating à distance sur un cluster qui est configuré pour l’auto-no__t-1updating ?  
Oui. Un cluster de basculement dans une configuration Self @ no__t-0updating peut être mis à jour par le biais de l’accès à distance à @ no__t-1updating sur @ no__t-2demand, de la même façon que vous pouvez forcer une analyse de Windows Update à tout moment sur votre ordinateur, même si Windows Update est configuré pour installer les mises à jour systématiquement. Au préalable, vous devez vous assurer qu’aucune exécution de mise à jour n’est en cours.  
  
## <a name="can-i-reuse-my-cluster-update-settings-across-clusters"></a>Puis-je utiliser les mêmes paramètres de mise à jour pour tous les clusters ?  
Oui. La mise à jour adaptée aux clusters comporte plusieurs options d’exécution de mise à jour qui vous permettent de définir le comportement de l’exécution de mise à jour pour un cluster. Vous pouvez enregistrer les options définies dans un profil d’exécution de mise à jour afin de les réutiliser pour d’autres clusters. Nous vous recommandons d’enregistrer et d’utiliser les mêmes paramètres sur tous les clusters de basculement qui nécessitent des mises à jour similaires. Par exemple, vous pouvez créer un profil d’exécution de mise à jour de cluster « Business @ no__t-0Critical SQL Server » pour tous les clusters Microsoft SQL Server qui prennent en charge les services no__t-1critical métier.  
  
## <a name="where-is-the-cau-plug-in-specification"></a>Où se trouve la spécification de plug @ no__t-0in de la mise à jour adaptée aux clusters ?  
  
-   [Cluster @ no__t-1Aware mise à jour du plug @ no__t-référence 2in](https://msdn.microsoft.com/library/hh418084(VS.85).aspx)  
  
-   [Prise en charge de la mise à jour adaptée aux clusters, exemple @ no__t-1Dans](https://code.msdn.microsoft.com/windowsdesktop/Cluster-Aware-Updating-6a8854c9)  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Vue d’ensemble de la mise à jour du cluster @ no__t-1Aware](cluster-aware-updating.md)  
  
