---
title: La mise à niveau des Clusters de basculement à l’aide de la même matériel
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.date: 02/28/2019
description: Cet article décrit la mise à niveau d’un Cluster de basculement de 2 nœuds à l’aide de la même matériel
ms.localizationpriority: medium
ms.openlocfilehash: 0bfeb05c8cbc205745dc16bc7ef04052481668ea
ms.sourcegitcommit: 2c2027b597e2483eea8967d0710d65c2247b6751
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "9121306"
---
# La mise à niveau des Clusters de basculement sur le même matériel

> S’applique à: Windows Server 2019, Windows Server 2016

Un cluster de basculement est un groupe d’ordinateurs indépendants qui travaillent conjointement pour accroître la disponibilité des applications et services. Les serveurs en cluster (appelés « nœuds ») sont connectés par des câbles physiques et par des logiciels. Si un des nœuds du cluster échoue, un autre nœud commence à fournir un service (processus appelé basculement). Les utilisateurs rencontrer des interruptions de service minimales.

Ce guide décrit les étapes de la mise à niveau des nœuds de cluster vers Windows Server 2019 ou Windows Server 2016 à partir d’une version antérieure à l’aide de la même matériel.

## Vue d'ensemble

La mise à niveau le système d’exploitation sur un basculement existant cluster est uniquement pris en charge lors du passage à partir de Windows Server 2016 à Windows 2019.  Si le cluster de basculement exécute une version antérieure, par exemple, par exemple, Windows Server 2012 R2 et versions antérieures, la mise à niveau alors que les services de clusters sont en cours d’exécution n’autorise pas la jonction de nœuds entre eux.  Si vous utilisez le même matériel, peuvent prendre des mesures accéder, vers la version plus récente.  

Avant toute mise à niveau de votre cluster de basculement, veuillez consulter le [Centre de mise à niveau de Windows](https://www.microsoft.com/upgradecenter).  Lorsque vous mettez à niveau un Windows Server sur place, vous migrez à partir d’une version de système d’exploitation existant vers une version plus récente, sur le même matériel. Windows Server peut être mis à niveau sur place au moins et parfois deux versions vers l’avant. Par exemple, le Windows Server 2012 R2 et Windows Server 2016 peuvent être mis à niveau sur place vers Windows Server 2019.  Gardez à l’esprit que l' [Assistant de Migration de Cluster](https://blogs.msdn.microsoft.com/clustering/2012/06/25/how-to-move-highly-available-clustered-vms-to-windows-server-2012-with-the-cluster-migration-wizard/) peut être utilisé, mais est uniquement pris en charge jusqu'à deux versions précédent. Le graphique suivant montre les chemins de mise à niveau pour Windows Server. Les flèches de pointage vers le bas représentent le chemin d’accès de mise à niveau pris en charge déplacement des versions antérieures à Windows Server 2019.

![Diagramme de mise à niveau sur place](media\In-Place-Upgrade\In-Place-Upgrade-1.png)

Les étapes suivantes sont un exemple de migration à partir d’un serveur de cluster de basculement Windows Server 2012 sur Windows Server 2019 avec le même matériel.  

Avant de commencer une mise à niveau, vérifiez qu’une sauvegarde actuelle, notamment l’état du système a été effectuée.  Vérifiez également tous les pilotes et microprogrammes ont été mis à jour aux niveaux certifiées pour le système d’exploitation que vous utiliserez.  Ces deux notes ne sont pas couverts ici.

Dans l’exemple ci-dessous, le nom du cluster de basculement est CLUSTER et les noms de nœud sont nœud 1 et nœud 2.

## Étape 1: Supprimer le premier nœud et mettre à niveau vers Windows Server 2016

1. Dans le Gestionnaire de Cluster de basculement, décharger toutes les ressources à partir du nœud 1 au nœud 2 par le droit de la souris en cliquant sur le nœud et en sélectionnant **mise en Pause** et **Drainer les rôles**.  Sinon, vous pouvez utiliser la commande PowerShell [Suspension-CLUSTERNODE](https://docs.microsoft.com/powershell/module/failoverclusters/suspend-clusternode).

    ![Nœud de pause](media\In-Place-Upgrade\In-Place-Upgrade-2.png)

2. Excluez nœud 1 du cluster en droit de la souris en cliquant sur le nœud et en sélectionnant les **Autres Actions** et les **Supprimer**.  Sinon, vous pouvez utiliser la commande PowerShell [REMOVE-CLUSTERNODE](https://docs.microsoft.com/powershell/module/failoverclusters/remove-clusternode).

    ![Nœud de pause](media\In-Place-Upgrade\In-Place-Upgrade-3.png)

3. Par précaution, détacher nœud 1 à partir du stockage que vous utilisez.  Dans certains cas, les câbles stockage la déconnexion de l’ordinateur suffira.  Contactez votre fournisseur de stockage pour les étapes de détachement approprié si nécessaire.  En fonction de votre stockage, cela peut être pas nécessaire.

4. Générez à nouveau nœud 1 avec Windows Server 2016.  Assurez-vous que vous avez ajouté tous les rôles nécessaires, fonctionnalités, les pilotes et mises à jour de sécurité.

5. Créer un nouveau cluster appelé CLUSTER1 avec nœud 1.  Ouvrez le Gestionnaire du Cluster de basculement et dans le volet de **gestion** , choisissez **Créer un Cluster** et suivez les instructions de l’Assistant.

    ![Nœud de pause](media\In-Place-Upgrade\In-Place-Upgrade-4.png)

6. Une fois que le Cluster est créé, les rôles devez être migrés à partir du cluster d’origine sur ce nouveau cluster.  Dans le nouveau cluster, droit de la souris cliquez sur le nom du cluster (CLUSTER1) et sélection **Autres Actions** et **Copie les rôles de Cluster**.  Suivez le long de l’Assistant pour migrer les rôles.

    ![Nœud de pause](media\In-Place-Upgrade\In-Place-Upgrade-5.png)

7.  Une fois que toutes les ressources ont été migrés, hors tension nœud 2 (cluster d’origine) et déconnectez le stockage, ce afin de n’importe quel n'interférences pas.  Connectez le stockage à nœud 1.  Une fois que tout est connecté, mettez toutes les ressources en ligne et vous assurer qu’ils fonctionnent comme devrait.

## Étape 2: Générez à nouveau nœud deuxième à Windows Server 2019

Une fois que vous avez vérifié que tout fonctionne comme elle ne devrait, nœud 2 peut être régénéré à Windows Server 2019 et joint au Cluster.

1. Effectuer une nouvelle installation de Windows Server 2019 sur le nœud 2. Assurez-vous que vous avez ajouté tous les rôles nécessaires, fonctionnalités, les pilotes et mises à jour de sécurité.

2. Maintenant que le cluster d’origine (CLUSTER) a disparu, vous pouvez laisser le nouveau nom de cluster CLUSTER1 ou revenir en arrière vers le nom d’origine.  Si vous souhaitez revenir en arrière vers le nom d’origine, procédez comme suit:
   
   a. Sur le nœud 1, dans le Gestionnaire du Cluster de basculement droit de la souris, cliquez sur le nom du cluster (CLUSTER1) et sélectionnez **Propriétés**.
   
   b. Sous l’onglet **Général** , renommez le cluster à CLUSTER.

   c. Lorsque vous choisissez OK ou appliquer, vous verrez le ci-dessous contextuelle de boîte de dialogue.

    ![Nœud de pause](media\In-Place-Upgrade\In-Place-Upgrade-6.png)

    d. Le Service de Cluster est arrêté et nécessaire pour être redémarrée pour le changement de nom terminer.

3. Sur le nœud 1, ouvrez le Gestionnaire du Cluster de basculement.  Avec le bouton droit sur des **nœuds de** clic de souris et sélectionnez **Ajouter un nœud**.  Passer par l’Assistant Ajout de nœud 2 au Cluster.

4. Joignez le stockage à nœud 2. Cela peut inclure la reconnexion les câbles de stockage. 

5. Décharger toutes les ressources à partir du nœud 1 au nœud 2 par le droit de la souris en cliquant sur le nœud et en sélectionnant **mise en Pause** et **Drainer les rôles**.  Sinon, vous pouvez utiliser la commande PowerShell [Suspension-CLUSTERNODE](https://docs.microsoft.com/powershell/module/failoverclusters/suspend-clusternode).  Assurez-vous que toutes les ressources en ligne et qu’ils fonctionnent comme devrait.

## Étape 3: Régénérer le premier nœud à Windows Server 2019

1. Supprimer le nœud 1 du cluster et déconnectez le stockage à partir du nœud de la manière de que vous précédemment.

2. Générez à nouveau ou mettre à niveau de nœud 1 vers Windows Server 2019.  Assurez-vous que vous avez ajouté tous les rôles nécessaires, fonctionnalités, les pilotes et mises à jour de sécurité.

3. Attacher de nouveau le stockage et ajoutez nœud 1 au cluster.

4. Déplacez toutes les ressources au nœud 1 et vous assurer qu’ils apparaissent et fonctionnent si nécessaire.

5. Le niveau fonctionnel du cluster en cours reste à Windows 2016.  Mettre à jour le niveau fonctionnel Windows 2019 avec la commande PowerShell [UPDATE-CLUSTERFUNCTIONALLEVEL](https://docs.microsoft.com/powershell/module/failoverclusters/update-clusterfunctionallevel).

Maintenant, vous travaillez avec un Cluster de basculement Windows Server 2019 entièrement fonctionnelle.

## Remarques supplémentaires

- Comme expliqué précédemment, le stockage de la déconnexion peut ou ne peut pas être nécessaire.  Dans notre documentation, nous voulons raisonnables.  Consultez votre fournisseur de stockage.
- Si votre point de départ est Windows Server 2008 ou 2008 R2 clusters, une exécution supplémentaires par le biais des étapes peut-être être nécessaires.
- Si le cluster est en cours d’exécution machines virtuelles, assurez-vous que vous mettez à niveau le niveau de la machine virtuelle une fois que le niveau fonctionnel du cluster a été effectué avec la commande PowerShell [VMVERSION-mise à jour](https://docs.microsoft.com/powershell/module/hyper-v/update-vmversion).
- Veuillez noter que si vous exécutez une application, par exemple, SQL Server, Exchange Server, etc., l’application ne sera pas migrée avec l’Assistant de copie des rôles de Cluster.  Vous devez consulter votre revendeur pour connaître les étapes appropriées de migration de l’application.