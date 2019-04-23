---
title: La mise à niveau des Clusters de basculement utilisant le même matériel
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.date: 02/28/2019
description: Cet article décrit la mise à niveau un Cluster de basculement de 2 nœuds avec le même matériel
ms.localizationpriority: medium
ms.openlocfilehash: 0bfeb05c8cbc205745dc16bc7ef04052481668ea
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854830"
---
# <a name="upgrading-failover-clusters-on-the-same-hardware"></a>La mise à niveau des Clusters de basculement sur le même matériel

> S'applique à : Windows Server 2019, Windows Server 2016

Un cluster de basculement est un groupe d'ordinateurs indépendants qui travaillent conjointement pour accroître la disponibilité des applications et des services. Les serveurs en cluster (appelés « nœuds ») sont connectés par des câbles physiques et par des logiciels. En cas de défaillance de l'un des nœuds, un autre prend sa place pour fournir le service (processus appelé « basculement »), garantissant ainsi des interruptions minimales de service pour les utilisateurs.

Ce guide décrit les étapes de mise à niveau les nœuds de cluster Windows Server 2019 ou Windows Server 2016 à partir d’une version antérieure à l’aide de la même matériel.

## <a name="overview"></a>Vue d'ensemble

La mise à niveau le système d’exploitation sur un basculement existant cluster est uniquement pris en charge lorsque vous passez de Windows Server 2016 pour Windows 2019.  Si le cluster de basculement s’exécute une version antérieure, par exemple, comme Windows Server 2012 R2 et versions antérieures, la mise à niveau alors que les services de cluster sont en cours d’exécution n’autorise pas joindre des nœuds.  Si vous utilisez le même matériel, peuvent prendre des mesures pour l’obtenir à la version plus récente.  

Avant toute mise à niveau de votre cluster de basculement, veuillez consulter la [centre de mise à niveau Windows](https://www.microsoft.com/upgradecenter).  Lorsque vous mettez à niveau une serveur Windows sur place, vous passer d’une version de système d’exploitation existant à une version plus récente, tout en restant sur le même matériel. Windows Server peut être mis à niveau sur place au moins et parfois deux versions vers l’avant. Par exemple, le Windows Server 2012 R2 et Windows Server 2016 peuvent être mis à niveau sur place vers Windows Server 2019.  Également n’oubliez pas que le [Assistant de Migration de Cluster](https://blogs.msdn.microsoft.com/clustering/2012/06/25/how-to-move-highly-available-clustered-vms-to-windows-server-2012-with-the-cluster-migration-wizard/) peut être utilisé, mais est uniquement pris en charge jusqu'à deux versions précédent. Le graphique suivant montre les chemins de mise à niveau pour Windows Server. Flèches pointant vers le bas représentent le chemin d’accès de mise à niveau pris en charge déplacement à partir de versions antérieures jusqu'à Windows Server 2019.

![Diagramme de mise à niveau sur place](media\In-Place-Upgrade\In-Place-Upgrade-1.png)

Les étapes suivantes sont un exemple de l’impression à partir d’un serveur de cluster de basculement Windows Server 2012 et Windows Server 2019 utilisant le même matériel.  

Avant de commencer toute mise à niveau, vérifiez qu’une sauvegarde actuelle, y compris l’état du système, a été effectuée.  Vérifiez également tous les microprogrammes et les pilotes ont été mis à jour aux niveaux certifiées pour le système d’exploitation que vous utilisez.  Ces deux notes ne seront pas abordées ici.

Dans l’exemple ci-dessous, le nom du cluster de basculement est le CLUSTER et les noms de nœud sont NODE1 et NODE2.

## <a name="step-1-evict-first-node-and-upgrade-to-windows-server-2016"></a>Étape 1 : Supprimez le premier nœud et de mettre à niveau vers Windows Server 2016

1. Dans le Gestionnaire de Cluster de basculement, vider toutes les ressources à partir de NODE1 pour NODE2 en droit de la souris en cliquant sur le nœud et en sélectionnant **Pause** et **vider les rôles**.  Ou bien, vous pouvez utiliser la commande PowerShell [SUSPEND-CLUSTERNODE](https://docs.microsoft.com/powershell/module/failoverclusters/suspend-clusternode).

    ![Drainage de nœud](media\In-Place-Upgrade\In-Place-Upgrade-2.png)

2. Supprimez NODE1 du cluster en droit de la souris en cliquant sur le nœud et en sélectionnant **autres Actions** et **supprimer**.  Ou bien, vous pouvez utiliser la commande PowerShell [REMOVE-CLUSTERNODE](https://docs.microsoft.com/powershell/module/failoverclusters/remove-clusternode).

    ![Drainage de nœud](media\In-Place-Upgrade\In-Place-Upgrade-3.png)

3. Par précaution, détachez NODE1 à partir du stockage que vous utilisez.  Dans certains cas, déconnecter les câbles de stockage à partir de la machine est suffisante.  Vérifiez auprès de votre fournisseur de stockage pour les étapes de détachement approprié si nécessaire.  En fonction de votre stockage, cela peut être pas nécessaire.

4. Reconstruire NODE1 avec Windows Server 2016.  Assurez-vous de qu'avoir ajouté tous les rôles nécessaires, fonctionnalités, pilotes et mises à jour de sécurité.

5. Créer un cluster appelé CLUSTER1 avec NODE1.  Ouvrez le Gestionnaire du Cluster de basculement et dans le **gestion** volet, choisissez **création d’un Cluster** et suivez les instructions de l’Assistant.

    ![Drainage de nœud](media\In-Place-Upgrade\In-Place-Upgrade-4.png)

6. Une fois que le Cluster est créé, les rôles seront doivent être migrés à partir du cluster d’origine vers ce nouveau cluster.  Sur le nouveau cluster, avec le bouton droit de la souris sur le nom de cluster (CLUSTER1) et en sélectionnant **autres Actions** et **copier les rôles de Cluster**.  Suivre la procédure dans l’Assistant pour migrer les rôles.

    ![Drainage de nœud](media\In-Place-Upgrade\In-Place-Upgrade-5.png)

7.  Une fois que toutes les ressources ont été migrés, hors tension, NODE2 (cluster d’origine) et déconnectez le stockage afin de ne provoquent ne pas toute interférence.  Connecter le stockage à NODE1.  Une fois connecté, tous les mettre toutes les ressources en ligne et assurez-vous qu’ils fonctionnent comme le devrait.

## <a name="step-2-rebuild-second-node-to-windows-server-2019"></a>Étape 2 : Reconstruire le second nœud à Windows Server 2019

Une fois que vous avez vérifié que tout fonctionne comme il le devrait, NODE2 pouvant être reconstruites pour Windows Server 2019 et joint au Cluster.

1. Effectuez une nouvelle installation de Windows Server 2019 sur NODE2. Assurez-vous de qu'avoir ajouté tous les rôles nécessaires, fonctionnalités, pilotes et mises à jour de sécurité.

2. Maintenant que le cluster d’origine (CLUSTER) a disparu, vous pouvez laisser le nouveau nom de cluster CLUSTER1 ou revenez en arrière pour le nom d’origine.  Si vous souhaitez revenir en arrière pour le nom d’origine, procédez comme suit :
   
   a. Sur NODE1, dans le Gestionnaire du Cluster de basculement droit de la souris, cliquez sur le nom du cluster (CLUSTER1) et choisissez **propriétés**.
   
   b. Sur le **général** onglet, renommer le cluster à CLUSTER.

   c. Lorsque vous choisissez OK ou appliquer, vous verrez le menu contextuel de la boîte de dialogue ci-dessous.

    ![Drainage de nœud](media\In-Place-Upgrade\In-Place-Upgrade-6.png)

    d. Le Service de Cluster est arrêté et doit être redémarré pour terminer le changement de nom.

3. Sur NODE1, ouvrez le Gestionnaire du Cluster de basculement.  Cliquez sur le droit de la souris sur **nœuds** et sélectionnez **ajouter un nœud**.  Parcourez l’Assistant Ajout de NODE2 au Cluster.

4. Attacher le stockage sur le nœud NODE2. Cela peut inclure la reconnexion des câbles de stockage. 

5. Vider toutes les ressources à partir de NODE1 pour NODE2 en droit de la souris en cliquant sur le nœud et en sélectionnant **Pause** et **vider les rôles**.  Ou bien, vous pouvez utiliser la commande PowerShell [SUSPEND-CLUSTERNODE](https://docs.microsoft.com/powershell/module/failoverclusters/suspend-clusternode).  Vérifiez que toutes les ressources sont en ligne et qu’ils fonctionnent comme le devrait.

## <a name="step-3-rebuild-first-node-to-windows-server-2019"></a>Étape 3 : Reconstruire le premier nœud à Windows Server 2019

1. Supprimez NODE1 du cluster et de déconnecter le stockage à partir du nœud de la manière de que vous précédemment.

2. Reconstruire ou mettre à niveau de NODE1 pour Windows Server 2019.  Assurez-vous de qu'avoir ajouté tous les rôles nécessaires, fonctionnalités, pilotes et mises à jour de sécurité.

3. Rattacher le stockage et ajouter NODE1 dans le cluster.

4. Déplacer toutes les ressources à NODE1 et vérifier qu’ils sont fournis en ligne et fonctionnent en fonction des besoins.

5. Le niveau fonctionnel du cluster actuel reste à Windows 2016.  Mettre à jour le niveau fonctionnel à 2019 de Windows avec la commande PowerShell [UPDATE-CLUSTERFUNCTIONALLEVEL](https://docs.microsoft.com/powershell/module/failoverclusters/update-clusterfunctionallevel).

Vous exécutez à présent avec un Cluster de basculement Windows Server 2019 entièrement fonctionnelle.

## <a name="additional-notes"></a>Remarques supplémentaires

- Comme expliqué précédemment, déconnexion du stockage peut ou ne peut pas être nécessaire.  Dans notre documentation, nous souhaitons err raisonnables.  Veuillez consulter votre fournisseur de stockage.
- Si votre point de départ est Windows Server 2008 ou 2008 R2 clusters, une exécution supplémentaire à travers des étapes peut-être être nécessaires.
- Si le cluster est en cours d’exécution des machines virtuelles, assurez-vous de mettre à niveau le niveau de la machine virtuelle une fois que le niveau fonctionnel du cluster a été effectué avec la commande PowerShell [mise à jour-VMVERSION](https://docs.microsoft.com/powershell/module/hyper-v/update-vmversion).
- Veuillez noter que si vous exécutez une application telle que SQL Server, Exchange Server, etc., l’application ne sera pas migrée avec l’Assistant Copier les rôles de Cluster.  Vous devez consulter votre fournisseur de l’application pour les étapes de migration appropriée de l’application.