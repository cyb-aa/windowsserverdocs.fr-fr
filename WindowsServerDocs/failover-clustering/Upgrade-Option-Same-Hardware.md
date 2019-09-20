---
title: Mise à niveau des clusters de basculement à l’aide du même matériel
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.date: 02/28/2019
description: Cet article décrit la mise à niveau d’un cluster de basculement à deux nœuds à l’aide du même matériel
ms.localizationpriority: medium
ms.openlocfilehash: aa9a31b1faa48a4eaf2a17bc8ecda690b4cf1f12
ms.sourcegitcommit: ccec91c1d32a978159f9b8bb5e39ead5805c26c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/19/2019
ms.locfileid: "71143787"
---
# <a name="upgrading-failover-clusters-on-the-same-hardware"></a>Mise à niveau de clusters de basculement sur le même matériel

> S’applique à : Windows Server 2019, Windows Server 2016

Un cluster de basculement est un groupe d'ordinateurs indépendants qui travaillent conjointement pour accroître la disponibilité des applications et des services. Les serveurs en cluster (appelés « nœuds ») sont connectés par des câbles physiques et par des logiciels. En cas de défaillance de l'un des nœuds, un autre prend sa place pour fournir le service (processus appelé « basculement »), garantissant ainsi des interruptions minimales de service pour les utilisateurs.

Ce guide décrit les étapes de la mise à niveau des nœuds de cluster vers Windows Server 2019 ou Windows Server 2016 à partir d’une version antérieure utilisant le même matériel.

## <a name="overview"></a>Vue d'ensemble

La mise à niveau du système d’exploitation sur un cluster de basculement existant est uniquement prise en charge lors du passage de Windows Server 2016 à Windows 2019.  Si le cluster de basculement exécute une version antérieure, telle que Windows Server 2012 R2 et versions antérieures, la mise à niveau alors que les services de cluster sont en cours d’exécution ne permet pas de joindre des nœuds ensemble.  Si vous utilisez le même matériel, vous pouvez prendre des mesures pour l’extraire vers la version la plus récente.  

Avant toute mise à niveau de votre cluster de basculement, consultez le contenu de la [mise à niveau de Windows Server](../upgrade/upgrade-overview.md).  Lorsque vous mettez à niveau un serveur Windows sur place, vous passez d’une version existante du système d’exploitation à une version plus récente tout en restant sur le même matériel. Windows Server peut être mis à niveau sur place au moins une, et parfois jusqu’à deux versions. Par exemple, Windows Server 2012 R2 et Windows Server 2016 peuvent être mis à niveau sur place vers Windows Server 2019.  Gardez également à l’esprit que l' [Assistant Migration de cluster](https://blogs.msdn.microsoft.com/clustering/2012/06/25/how-to-move-highly-available-clustered-vms-to-windows-server-2012-with-the-cluster-migration-wizard/) peut être utilisé, mais il n’est pris en charge que jusqu’à deux versions. Le graphique suivant montre les chemins de mise à niveau pour Windows Server. Les flèches vers le bas représentent le chemin de mise à niveau pris en charge allant des versions antérieures jusqu’à Windows Server 2019.

![Diagramme de mise à niveau sur place](media/In-Place-Upgrade/In-Place-Upgrade-1.png)

Les étapes suivantes sont un exemple de passage d’un serveur de cluster de basculement Windows Server 2012 à Windows Server 2019 utilisant le même matériel.  

Avant de commencer une mise à niveau, vérifiez qu’une sauvegarde en cours, y compris l’état du système, a été effectuée.  Assurez-vous également que tous les pilotes et microprogrammes ont été mis à jour avec les niveaux certifiés pour le système d’exploitation que vous utiliserez.  Ces deux notes ne seront pas couvertes ici.

Dans l’exemple ci-dessous, le nom du cluster de basculement est CLUSTER et les noms de nœuds sont NODE1 et NODE2.

## <a name="step-1-evict-first-node-and-upgrade-to-windows-server-2016"></a>Étape 1 : Supprimer le premier nœud et effectuer une mise à niveau vers Windows Server 2016

1. Dans Gestionnaire du cluster de basculement, vous pouvez vider toutes les ressources de NODE1 à NODE2 en cliquant avec le bouton droit sur le nœud et en sélectionnant les rôles **suspendre** et **purger**.  Vous pouvez également utiliser la commande PowerShell [suspend-CLUSTERNODE](https://docs.microsoft.com/powershell/module/failoverclusters/suspend-clusternode).

    ![Nœud drain](media/In-Place-Upgrade/In-Place-Upgrade-2.png)

2. Supprimez NODE1 du cluster en cliquant avec le bouton droit de la souris sur le nœud et en sélectionnant **plus d’actions** et **supprimer**.  Vous pouvez également utiliser la commande PowerShell [Remove-CLUSTERNODE](https://docs.microsoft.com/powershell/module/failoverclusters/remove-clusternode).

    ![Nœud drain](media/In-Place-Upgrade/In-Place-Upgrade-3.png)

3. Par précaution, détachez NODE1 du stockage que vous utilisez.  Dans certains cas, la déconnexion des câbles de stockage à partir de la machine est suffisante.  Si nécessaire, contactez votre fournisseur de stockage pour connaître les étapes de détachement appropriées.  Selon votre stockage, cela peut ne pas être nécessaire.

4. Régénérez NODE1 avec Windows Server 2016.  Vérifiez que vous avez ajouté tous les rôles, fonctionnalités, pilotes et mises à jour de sécurité nécessaires.

5. Créez un nouveau cluster appelé CLUSTER1 avec NODE1.  Ouvrez Gestionnaire du cluster de basculement et dans le volet **gestion** , choisissez **créer un cluster** et suivez les instructions de l’Assistant.

    ![Nœud drain](media/In-Place-Upgrade/In-Place-Upgrade-4.png)

6. Une fois le cluster créé, les rôles doivent être migrés du cluster d’origine vers ce nouveau cluster.  Sur le nouveau cluster, cliquez avec le bouton droit sur le nom du cluster (CLUSTER1), puis sélectionnez **autres actions** et **copier les rôles de cluster**.  Suivez la procédure de l’Assistant pour migrer les rôles.

    ![Nœud drain](media/In-Place-Upgrade/In-Place-Upgrade-5.png)

7.  Une fois que toutes les ressources ont été migrées, arrêtez le nœud NODE2 (cluster d’origine) et déconnectez le stockage afin de ne pas provoquer d’interférences.  Connectez le stockage à NODE1.  Une fois que tout est connecté, mettez toutes les ressources en ligne et assurez-vous qu’elles fonctionnent comme dans le cas.

## <a name="step-2-rebuild-second-node-to-windows-server-2019"></a>Étape 2 : Régénérer le second nœud sur Windows Server 2019

Une fois que vous avez vérifié que tout fonctionne comme il le devrait, NODE2 peut être reconstruit sur Windows Server 2019 et être joint au cluster.

1. Effectuez une nouvelle installation de Windows Server 2019 sur NODE2. Vérifiez que vous avez ajouté tous les rôles, fonctionnalités, pilotes et mises à jour de sécurité nécessaires.

2. Maintenant que le cluster d’origine (CLUSTER) a disparu, vous pouvez conserver le nouveau nom du cluster en tant que CLUSTER1 ou revenir au nom d’origine.  Si vous souhaitez revenir au nom d’origine, procédez comme suit :
   
   a. Sur NODE1, dans Gestionnaire du cluster de basculement cliquez avec le bouton droit sur le nom du cluster (CLUSTER1), puis choisissez **Propriétés**.
   
   b. Sous l’onglet **général** , renommez le CLUSTER en cluster.

   c. Lorsque vous choisissez OK ou appliquer, la boîte de dialogue ci-dessous s’affiche.

    ![Nœud drain](media/In-Place-Upgrade/In-Place-Upgrade-6.png)

    d. Le service de cluster va être arrêté et doit être redémarré pour que le changement de nom se termine.

3. Sur NODE1, ouvrez Gestionnaire du cluster de basculement.  Cliquez avec le bouton droit de la souris sur les **nœuds** et sélectionnez **Ajouter un nœud**.  Suivez les étapes de l’Assistant Ajout de NODE2 au cluster.

4. Attachez le stockage à NODE2. Cela peut inclure la reconnexion des câbles de stockage. 

5. Déchargez toutes les ressources de NODE1 à NODE2 en cliquant avec le bouton droit sur le nœud et en sélectionnant les rôles **suspendre** et **purger**.  Vous pouvez également utiliser la commande PowerShell [suspend-CLUSTERNODE](https://docs.microsoft.com/powershell/module/failoverclusters/suspend-clusternode).  Assurez-vous que toutes les ressources sont en ligne et qu’elles fonctionnent comme dans le cas.

## <a name="step-3-rebuild-first-node-to-windows-server-2019"></a>Étape 3 : Régénérer le premier nœud sur Windows Server 2019

1. Supprimez NODE1 du cluster et déconnectez le stockage du nœud comme vous l’avez fait précédemment.

2. Reconstruisez ou mettez à niveau NODE1 vers Windows Server 2019.  Vérifiez que vous avez ajouté tous les rôles, fonctionnalités, pilotes et mises à jour de sécurité nécessaires.

3. Rattachez le stockage et rajoutez NODE1 au cluster.

4. Déplacez toutes les ressources vers NODE1 et assurez-vous qu’elles sont en ligne et fonctionnent en fonction des besoins.

5. Le niveau fonctionnel actuel du cluster reste sur Windows 2016.  Mettez à jour le niveau fonctionnel sur Windows 2019 avec la commande PowerShell [Update-CLUSTERFUNCTIONALLEVEL](https://docs.microsoft.com/powershell/module/failoverclusters/update-clusterfunctionallevel).

Vous êtes maintenant en cours d’exécution avec un cluster de basculement Windows Server 2019 entièrement fonctionnel.

## <a name="additional-notes"></a>Remarques supplémentaires

- Comme expliqué précédemment, la déconnexion du stockage peut être nécessaire ou non.  Dans notre documentation, nous souhaitons vous faire part de prudence.  Consultez votre fournisseur de stockage.
- Si votre point de départ correspond à des clusters Windows Server 2008 ou 2008 R2, il peut s’avérer nécessaire d’exécuter des étapes supplémentaires.
- Si le cluster exécute des machines virtuelles, veillez à mettre à niveau le niveau de l’ordinateur virtuel une fois le niveau fonctionnel du cluster terminé avec la commande PowerShell [Update-VMVERSION](https://docs.microsoft.com/powershell/module/hyper-v/update-vmversion).
- Notez que si vous exécutez une application telle que SQL Server, Exchange Server, etc., l’application ne sera pas migrée à l’aide de l’Assistant copie de rôles de cluster.  Vous devez consulter le fournisseur de votre application pour connaître les étapes de migration appropriées de l’application.