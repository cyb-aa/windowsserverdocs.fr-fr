---
ms.assetid: 6416d125-bcaf-433d-971a-2f0283bca2c2
title: "Mise à jour adaptée aux clusters: Forum aux Questions"
ms.topic: article
ms.prod: storage-failover-clustering
manager: dongill
ms.author: jgerend
author: JasonGerend
ms.date: 4/28/2017
description: "Réponses aux questions fréquemment posées sur la mise à jour adaptée aux clusters dans Windows Server."
ms.openlocfilehash: 8417ea8b6b76e16c3f4db3bac5062d90a8da3ff2
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# <a name="cluster-aware-updating-frequently-asked-questions"></a>Mise à jour adaptée aux clusters: Forum aux Questions

> S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016, Windows Server2012R2, Windows Server2012

[Mise à jour adaptée aux clusters](cluster-aware-updating.md) \(CAU\) est une fonctionnalité qui coordonne les mises à jour logicielles sur tous les serveurs dans un cluster de basculement d’une manière qui n’affecte pas la disponibilité du service plus qu’un basculement planifié d’un nœud de cluster. Pour certaines applications offrant des fonctionnalités de disponibilité continue \ tel que (Hyper\-V avec migration dynamique), ou un serveur de fichiers SMB 3.x avec Failover\ Transparent SMB, adaptée aux clusters peut coordonner la mise à jour d’automatisée des clusters sans aucun impact sur la disponibilité du service.

## <a name="does-cau-support-updating-storage-spaces-direct-clusters"></a>Adaptée aux clusters prend-elle en charge la mise à jour des clusters espaces de stockage Direct?  
Oui. Adaptée aux clusters prend en charge la mise à jour [espaces de stockage Direct](../storage/storage-spaces/storage-spaces-direct-overview.md) clusters, quelle que soit le type de déploiement: hyperconvergé ou convergé. Plus précisément, orchestration adaptée aux clusters permet de s’assurer que suspendre chaque nœud de cluster attend que l’espace de stockage en cluster sous-jacent être sain.

## <a name="does-cau-work-with-windows-server-2008-r2-or-windows-7"></a>Adaptée aux clusters fonctionne-t-elle avec Windows Server2008R2 ou Windows7?  
Non. Adaptée aux clusters coordonne la mise à jour de l’opération uniquement sur les ordinateurs exécutant Windows Server2016, Windows Server2012R2, Windows Server2012, Windows10, Windows8.1 ou Windows8 de cluster. Mise à jour le cluster de basculement doive exécuter Windows Server2016, Windows Server2012R2 ou Windows Server2012.
  
## <a name="is-cau-limited-to-specific-clustered-applications"></a>Adaptée aux clusters est limitée à des applications en cluster spécifiques?  
Non. Adaptée aux clusters est quel que soit le type de l’application en cluster. Adaptée aux clusters est une solution de mise à jour cluster\ externe située dans la couche supérieure des applets de commande PowerShell et les API de clustering. Par conséquent, adaptée aux clusters coordonne les mises à jour sur n’importe quelle application en cluster qui est configurée dans un cluster de basculement Windows Server.  
  
> [!NOTE]  
> Actuellement, les charges de travail en cluster suivantes ont été testées et certifiées pour adaptée aux clusters: SMB, Hyper\-V, la réplication DFS, espaces de noms DFS, iSCSI et NFS.  
  
## <a name="does-cau-support-updates-from-microsoft-update-and-windows-update"></a>Adaptée aux clusters prend-elle en charge les mises à jour à partir de MicrosoftUpdate et Windows Update?  
Oui. Par défaut, adaptée aux clusters est configurée avec un plug-in qui utilise l’API d’utilitaire \(WUA\) l’Agent Windows Update sur les nœuds de cluster. L’infrastructure WUA peut être configuré pour pointer vers MicrosoftUpdate et Windows Update ou WindowsServerUpdateServices \(WSUS\) comme source des mises à jour.  
  
## <a name="does-cau-support-wsus-updates"></a>Adaptée aux clusters prend-elle en charge les mises à jour WSUS?  
Oui. Par défaut, adaptée aux clusters est configurée avec un plug-in qui utilise l’API d’utilitaire \(WUA\) l’Agent Windows Update sur les nœuds de cluster. L’infrastructure WUA peut être configuré pour pointer vers MicrosoftUpdate et Windows Update ou à un serveur WindowsServerUpdateServices \(WSUS\) local comme source des mises à jour.  
  
## <a name="can-cau-apply-limited-distribution-release-updates"></a>Adaptée aux clusters peut appliquer des mises à jour de la version limitée de distribution?  
Oui. Distribution limitée version \(LDR\) mises à jour, également appelées correctifs logiciels, ne sont pas publiées par le biais de MicrosoftUpdate ou Windows Update, elles ne peuvent pas être téléchargées par l’Agent Windows Update \(WUA\) plug\-dans la mesure où adaptée aux clusters utilise par défaut.  
  
Toutefois, adaptée aux clusters inclut un deuxième un plug-in que vous pouvez sélectionner pour appliquer des mises à jour de correctif logiciel. Ce correctif un plug-in peut également être personnalisé pour appliquer des mises à jour du BIOS, des microprogrammes et des pilotes autres que Microsoft.  
  
## <a name="can-i-use-cau-to-apply-cumulative-updates"></a>Puis-je utiliser adaptée aux clusters pour appliquer des mises à jour cumulatives?  
Oui. Si les mises à jour cumulatives sont mises à jour de la version grand public ou mises à jour LDR, adaptée aux clusters peut appliquer.  
  
## <a name="can-i-schedule-updates"></a>Puis-je planifier des mises à jour?  
Oui. Adaptée aux clusters prend en charge les modes de mise à jour suivantes, qui permettent à planifier les mises à jour:  
  
**Mise à jour intégrée** permet au cluster se mettre à jour en fonction d’un profil défini et à intervalles réguliers, comme pendant une fenêtre de maintenance mensuelle. Vous pouvez également démarrer une exécution de mise à jour intégrée à la demande à tout moment. Pour activer le mode de mise à jour intégrée, vous devez ajouter le rôle en cluster adaptée aux clusters au cluster. La fonctionnalité intégrée mise à jour adaptée aux clusters effectue comme toute autre charge de travail en cluster, et il fonctionne de façon transparente avec les basculements planifiés et non d’un ordinateur coordinateur de mise à jour.  
  
**Mise à jour Remote\** vous permet de démarrer une exécution de mise à jour à tout moment à partir d’un ordinateur exécutant Windows ou Windows Server. Vous pouvez démarrer une mise à jour à exécuter par le biais de la fenêtre de la mise à jour adaptée aux clusters ou en utilisant le **Invoke-CauRun** applet de commande PowerShell. Mise à jour Remote\ est mise à jour adaptée aux clusters en mode par défaut. Vous pouvez utiliser le Planificateur de tâches pour exécuter le [Invoke-CauRun](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/invoke-caurun) applet de commande sur une planification souhaitée à partir d’un ordinateur distant qui n’est pas un des nœuds du cluster.  
  
## <a name="can-i-schedule-updates-to-apply-during-a-backup"></a>Puis-je planifier des mises à jour à appliquer lors d’une sauvegarde?  
Oui. Adaptée aux clusters n’impose aucune restriction à cet égard. Toutefois, effectuez les mises à jour logicielles sur un serveur \ (avec la restarts\ potentiels associés) pendant qu’une sauvegarde du serveur est en cours n’est pas une meilleure pratique informatique. N’oubliez pas qu’adaptée aux clusters s’appuie uniquement sur les API de clustering pour déterminer les ressources basculements et restaurations; adaptée aux clusters est donc sans se préoccuper de l’état de sauvegarde du serveur.  
  
## <a name="can-cau-work-with-system-center-configuration-manager"></a>Adaptée aux clusters utilisable avec SystemCenter ConfigurationManager?  
Adaptée aux clusters est un outil qui coordonne les mises à jour logicielles sur un nœud de cluster et ConfigurationManager effectue des mises à jour logicielles de serveur. Il est important de configurer ces outils afin qu’ils n’ont pas chevauchement de couverture des serveurs mêmes dans tout déploiement de centre de données, y compris à l’aide de différents serveurs WindowsServerUpdateServices. Cela garantit que l’objectif recherché en utilisant adaptée aux clusters n’est pas encontre par inadvertance, car pilotée par le Gestionnaire de Configuration fichiers\ mise à jour n’ajoute pas la reconnaissance des clusters.  
  
## <a name="do-i-need-administrative-credentials-to-run-cau"></a>Ai-je besoin d’informations d’identification d’administration pour exécuter adaptée aux clusters?  
Oui. Pour exécuter les outils adaptée aux clusters, elle nécessite des informations d’identification d’administration sur le serveur local, ou il doit le **emprunter l’identité d’un Client après l’authentification** sur le serveur local ou de l’ordinateur client sur lequel il est en cours d’exécution. Toutefois, pour coordonner les mises à jour logicielles sur les nœuds de cluster, adaptée aux clusters nécessite des informations d’identification d’administration de cluster sur chaque nœud. Bien que l’UI CAU peut démarrer sans les informations d’identification, il demande les informations d’identification d’administration de cluster lorsqu’il se connecte à une instance de cluster pour obtenir un aperçu ou appliquer des mises à jour.  
  
## <a name="can-i-script-cau"></a>Puis-je créer un script adaptée aux clusters?  
Oui. Adaptée aux clusters est fournie avec les applets de commande PowerShell qui offrent de nombreuses options de script. Il s’agit des mêmes applets de commande qui l’UI CAU appelle pour effectuer des actions adaptée aux clusters.  

## <a name="what-happens-to-active-clustered-roles"></a>Que se passe-t-il pour les rôles en cluster?

Rôles en cluster \ (anciennement appelé applications et services\) qui sont actifs sur un nœud, de basculer vers d’autres nœuds avant le démarrage des mises à jour logicielles. Adaptée aux clusters orchestre ces basculements à l’aide du mode de maintenance qui met en pause et draine le nœud de tous les rôles en cluster actifs. Lorsque les mises à jour logicielles sont terminées, adaptée aux clusters reprend l’exécution du nœud et les rôles en cluster restaurés automatiquement sur le nœud mis à jour. Cela garantit que la distribution des rôles en cluster par rapport aux nœuds reste la même entre l’exécution de mise à jour adaptée aux clusters d’un cluster.  
  
## <a name="how-does-cau-select-target-nodes-for-clustered-roles"></a>Comment adaptée aux clusters sélectionne les nœuds cibles pour les rôles en cluster?

Adaptée aux clusters s’appuie sur l’API de clustering pour coordonner les basculements. L’implémentation des API de clustering sélectionne les nœuds cibles en se basant sur les métriques internes et d’heuristiques de placement intelligent \ (par exemple, la charge de travail levels\) entre les nœuds cibles.  
  
## <a name="does-cau-load-balance-the-clustered-roles"></a>Est adaptée aux clusters équilibre la charge les rôles en cluster?

Adaptée aux clusters ne charge pas équilibrer les nœuds de cluster, mais elle essaie de conserver la distribution des rôles en cluster. Après avoir adaptée mise à jour d’un nœud de cluster, il tente échec précédent hébergé précédemment des rôles en cluster vers ce nœud. Adaptée aux clusters s’appuie sur l’API de clustering pour restaurer les ressources au début du processus de mise en pause. Par conséquent en l’absence de basculements non planifiés et les paramètres de propriétaires, la distribution des rôles en cluster doit restent inchangée.  
  
## <a name="how-does-cau-select-the-order-of-nodes-to-update"></a>Comment adaptée aux clusters ne sélectionne pas l’ordre des nœuds pour mettre à jour?  
Par défaut, adaptée aux clusters sélectionne l’ordre des nœuds pour mettre à jour en fonction du niveau d’activité. Les nœuds qui hébergent les rôles en cluster moins sont mis à jour tout d’abord. Toutefois, un administrateur peut spécifier un ordre particulier de mise à jour les nœuds en définissant un paramètre pour l’exécution de mise à jour dans l’UI CAU ou à l’aide des applets de commande PowerShell.  
  
## <a name="what-happens-if-a-cluster-node-is-offline"></a>Que se passe-t-il si un nœud de cluster est hors connexion?

L’administrateur qui lance une exécution de mise à jour peut spécifier le seuil acceptable pour le nombre de nœuds qui peuvent être en mode hors connexion. Par conséquent, une exécution de mise à jour peut s’effectuer sur un cluster même si tous les nœuds de cluster ne sont pas en ligne.  
  
## <a name="can-i-use-cau-to-update-only-a-single-node"></a>Puis-je utiliser adaptée aux clusters pour mettre à jour un seul nœud?  
Non. Adaptée aux clusters est un outil de mise à jour cluster\ dans son intégralité, afin qu’il autorise uniquement vous permet de sélectionner des clusters pour mettre à jour. Si vous souhaitez mettre à jour un seul nœud, vous pouvez utiliser le serveur existant de mise à jour des outils indépendamment adaptée aux clusters.  
  
## <a name="can-cau-report-updates-that-are-initiated-from-outside-cau"></a>Adaptée aux clusters peut signaler des mises à jour qui sont lancées à partir de la méthode externe?  
Non. Adaptée aux clusters peut uniquement rapport mise à jour s’exécute qui sont lancées à partir d’adaptée aux clusters. Toutefois, lorsqu’un ultérieures exécution adaptée aux clusters mise à jour est lancée, qui ont été installées par le biais de méthodes autres que les mises à jour sont considérés comme correctement pour déterminer les mises à jour supplémentaires qui peuvent être applicables à chaque nœud du cluster.  
  
## <a name="can-cau-support-my-unique-it-process-needs"></a>Peut mon processus informatiques a besoin de la prise en charge adaptée aux clusters?  
Oui. Adaptée aux clusters offre les options de flexibilité en fonction d’entreprise a besoin de processus informatiques des clients suivantes:  
  
**Scripts** une exécution de mise à jour peut spécifier un script PowerShell de pre\-mise à jour et un script PowerShell de post\-mise à jour. Le script de mise à jour pre\ s’exécute sur chaque nœud du cluster avant que le nœud est suspendu. Le script de mise à jour post\ s’exécute sur chaque nœud du cluster après l’installent des mises à jour de nœud.  
  
> [!NOTE]  
> .NETFramework 4.6 ou 4.5 et PowerShell doivent être installé sur chaque nœud de cluster sur lequel vous souhaitez exécuter les scripts pre\-mise à jour et post\-mise à jour. Vous devez également activer la communication à distance PowerShell sur les nœuds de cluster. Pour connaître la configuration requise, voir [configuration requise et meilleures pratiques pour la mise à jour adaptée aux clusters](cluster-aware-updating-requirements.md).  
  
**Options d’exécution de mise à jour avancées** l’administrateur peut spécifier en outre d’un grand nombre d’options d’exécution de mise à jour avancées telles que le nombre maximal de fois que le processus de mise à jour est nouvelles tentatives sur chaque nœud. Ces options peuvent être spécifiées à l’aide de l’UI CAU ou les applets de commande PowerShell adaptée aux clusters. Ces paramètres personnalisés peuvent être enregistrés dans un profil d’exécution de mise à jour et réutilisés pour des exécutions de mise à jour ultérieure.  
  
**Architecture publique un plug-in** adaptée aux clusters inclut des fonctionnalités pour vous inscrire, annuler l’inscription, et sélectionnez un plug-ins. adaptée est fourni avec deux un plug-ins par défaut: un coordonnant les API \(WUA\) l’Agent Windows Update sur chaque nœud de cluster; la seconde applique les correctifs logiciels qui sont copiés manuellement dans un partage de fichiers qui est accessible aux nœuds du cluster. Si une entreprise a des besoins, qui ne peuvent pas être satisfaites avec ces deux un plug-ins, l’entreprise peut créer une nouveau adaptée aux clusters un plug-in en fonction de la spécification d’API publique. Pour plus d’informations, voir [prenant en charge Cluster\ mise à jour de référence un plug-in](http://msdn.microsoft.com/library/hh418084(VS.85).aspx).  
  
Pour plus d’informations sur la configuration et la personnalisation adaptée aux clusters un plug-ins pour prendre en charge différents scénarios, consultez [fonctionnement un plug-ins](assetId:///847b571b-12b3-473c-953f-75a5a1f51333).  
  
## <a name="how-can-i-export-the-cau-preview-and-update-results"></a>Comment puis-je exporter l’aperçu adaptée aux clusters et mettre à jour les résultats?  
Adaptée aux clusters offre les options d’exportation via l’interface de ligne de commande et par le biais de l’interface utilisateur.  
  
**Options d’interface de ligne de commande:**  
  
-   Aperçu des résultats à l’aide de l’applet de commande PowerShell **Invoke-CauScan | ConvertTo\-Xml**. Sortie: XML  
  
-   Rapport de résultats à l’aide de l’applet de commande PowerShell **Invoke-CauRun | ConvertTo\-Xml**. Sortie: XML  
  
-   Rapport de résultats à l’aide de l’applet de commande PowerShell **applet de commande Get-CauReport | Export-CauReport**. Sortie: HTML ou CSV.  
  
**Options d’interface utilisateur:**  
  
-   Copier les résultats du rapport à partir de la **afficher un aperçu des mises à jour** écran. Sortie: CSV  
  
-   Copier les résultats du rapport à partir de la **générer un état** écran. Sortie: CSV  
  
-   Exporter les résultats du rapport à partir de la **générer un état** écran. Sortie: HTML  
  
## <a name="how-do-i-install-cau"></a>Comment installer adaptée aux clusters?  
Une installation adaptée aux clusters est intégrée en toute transparence de la fonctionnalité de Clustering de basculement. Adaptée aux clusters est installée comme suit:  
  
-   Lorsque le Clustering de basculement est installé sur un nœud de cluster, le fournisseur \(WMI\) adaptée aux clusters WindowsManagementInstrumentation est automatiquement installé.  
  
-   Lorsque la fonctionnalité Outils de clustering avec basculement est installée sur un ordinateur client ou serveur, les applets de commande PowerShell et l’interface utilisateur de mise à jour adaptée aux clusters sont automatiquement installés.
  
## <a name="does-cau-need-components-running-on-the-cluster-nodes-that-are-being-updated"></a>Adaptée aux clusters n’a besoin des composants en cours d’exécution sur les nœuds de cluster qui sont mis à jour?  
Adaptée aux clusters n’a pas besoin d’un service s’exécutant sur les nœuds de cluster. Toutefois, elle nécessite un composant logiciel \ (le provider\ WMI) est installé sur les nœuds de cluster. Ce composant est installé avec la fonctionnalité de Clustering de basculement.  
  
Pour activer le mode de mise à jour intégrée, le rôle en cluster adaptée aux clusters doit également être ajouté au cluster.  
  
## <a name="what-is-the-difference-between-using-cau-and-vmm"></a>Quelle est la différence entre l’utilisation adaptée aux clusters et VMM?  
  
-   SystemCenter Virtual Machine Manager \(VMM\) se concentre sur la mise à jour des clusters Hyper\-V uniquement, tandis qu’adaptée aux clusters peut mettre à jour n’importe quel type de cluster de basculement pris en charge, y compris les clusters Hyper\-V.  
  
-   VMM requiert des licences supplémentaires, tandis qu’adaptée aux clusters est concédé sous licence pour tous les Windows Server. Les fonctionnalités, outils et l’interface utilisateur adaptée aux clusters sont installés avec les composants clustering avec basculement.  
  
-   Si vous possédez déjà une licence de SystemCenter, vous pouvez continuer à utiliser VMM pour mettre à jour des clusters Hyper\-V, car il offre une gestion intégrée et une expérience de mise à jour logicielle.  
  
-   Adaptée aux clusters est prise en charge uniquement sur les clusters qui exécutent Windows Server2016, Windows Server2012R2 et Windows Server2012. VMM prend également en charge les clusters Hyper\-V sur les ordinateurs exécutant Windows Server2008R2 et Windows Server2008.  
  
## <a name="can-i-use-remote-updating-on-a-cluster-that-is-configured-for-self-updating"></a>Puis-je utiliser la mise à jour remote\ sur un cluster qui est configuré pour la mise à jour intégrée?  
Oui. Un cluster de basculement dans une configuration mise à jour intégrée peut être mis à jour par le biais de la mise à jour remote\ dans\ à la demande, tout comme vous pouvez forcer une analyse Windows Update à tout moment sur votre ordinateur, même si Windows Update est configuré pour installer automatiquement les mises à jour. Toutefois, vous devez vous assurer qu’une exécution de mise à jour n’est pas déjà en cours.  
  
## <a name="can-i-reuse-my-cluster-update-settings-across-clusters"></a>Puis-je réutiliser mes paramètres de mise à jour de cluster entre les clusters?  
Oui. Adaptée aux clusters prend en charge un nombre d’options d’exécution de mise à jour qui déterminent l’exécution de mise à jour de comportement lorsqu’il met à jour le cluster. Ces options peuvent être enregistrées en tant qu’un profil d’exécution de mise à jour, et ils peuvent être réutilisés pour un cluster. Nous vous recommandons d’enregistrer et réutiliser vos paramètres entre les clusters de basculement qui ont des besoins de mise à jour similaires. Par exemple, vous pouvez créer un «Business\ critiques SQLServer Cluster mise à jour profil» pour tous les clusters MicrosoftSQLServer qui prennent en charge des services essentiels business\.  
  
## <a name="where-is-the-cau-plug-in-specification"></a>Où se trouve la spécification d’un plug-in adaptée aux clusters?  
  
-   [Cluster\ prenant en charge la mise à jour d’un plug-in de référence](http://msdn.microsoft.com/library/hh418084(VS.85).aspx)  
  
-   [Exemple de mise à jour prenant en charge un plug-in de cluster](http://code.msdn.microsoft.com/windowsdesktop/Cluster-Aware-Updating-6a8854c9)  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Vue d’ensemble de la mise à jour prenant en charge Cluster\](cluster-aware-updating.md)  
  
