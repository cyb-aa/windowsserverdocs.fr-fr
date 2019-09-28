---
title: Scénarios de récupération d’urgence pour une infrastructure hyper-convergée
ms.prod: windows-server
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: johnmarlin-msft
ms.date: 03/29/2018
description: Cet article décrit les scénarios disponibles aujourd’hui pour la récupération d’urgence de Microsoft HCI (espaces de stockage direct)
ms.localizationpriority: medium
ms.openlocfilehash: 8e6372ec7b4759f672c13f4bd822172afaf3faf3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393747"
---
# <a name="disaster-recovery-with-storage-spaces-direct"></a>Récupération d’urgence avec espaces de stockage direct

> S’applique à : Windows Server 2019, Windows Server 2016

Cette rubrique fournit des scénarios sur la façon dont la configuration de l’infrastructure hyper-convergée (HCI) peut être configurée pour la récupération d’urgence.

De nombreuses entreprises exécutent des solutions hyper-convergées et la planification d’un sinistre donne la possibilité de rester dans ou de revenir rapidement en production en cas de sinistre. Il existe plusieurs façons de configurer HCI pour la récupération d’urgence. ce document décrit les options disponibles aujourd’hui.

Lorsque vous parcourez la restauration de la disponibilité en cas de sinistre, reportez-vous à ce que l’on appelle l’objectif de délai de récupération (RTO). Il s’agit de la durée pendant laquelle les services doivent être restaurés afin d’éviter des conséquences inacceptables pour l’entreprise. Dans certains cas, ce processus peut se produire automatiquement avec la production restaurée presque immédiatement. Dans d’autres cas, une intervention manuelle de l’administrateur doit se produire pour restaurer les services.

Les options de récupération d’urgence avec Hyper-convergé aujourd’hui sont les suivantes :

1. Plusieurs clusters utilisant le réplica de stockage
2. Réplica Hyper-V entre des clusters
3. Sauvegarde et restauration

## <a name="multiple-clusters-utilizing-storage-replica"></a>Plusieurs clusters utilisant le réplica de stockage

Le [réplica de stockage](../storage-replica/storage-replica-overview.md) permet la réplication des volumes et prend en charge la réplication synchrone et asynchrone. Lorsque vous choisissez entre l’utilisation d’une réplication synchrone ou asynchrone, vous devez prendre en compte votre objectif de point de récupération (RPO). L’objectif de point de récupération est la quantité de perte de données que vous êtes disposé à subir avant qu’il ne soit considéré comme une perte majeure. Si vous utilisez la réplication synchrone, les deux extrémités sont écrites de manière séquentielle en même temps. Si vous utilisez le mode asynchrone, les écritures sont répliquées très rapidement, mais elles peuvent toujours être perdues. Vous devez tenir compte de l’utilisation de l’application ou du fichier pour voir ce qui vous convient le mieux.

Le réplica de stockage est un mécanisme de copie au niveau du bloc et au niveau du fichier ; Cela signifie que les types de données répliquées n’ont pas d’importance. Cela en fait une option populaire pour l’infrastructure hyper-convergée. Le réplica de stockage peut également utiliser différents types de lecteurs entre les partenaires de réplication. ainsi, le fait d’avoir tout le stockage de type sur un HCI et un autre stockage de type sur l’autre est parfait. 

L’une des fonctionnalités importantes du réplica de stockage est qu’il peut être exécuté dans Azure et localement. Vous pouvez configurer l’environnement local vers le site local, Azure vers Azure ou même localement vers Azure (ou vice versa).

Dans ce scénario, il existe deux clusters indépendants distincts. Pour configurer un réplica de stockage entre HCI, vous pouvez suivre les étapes de la section [réplication du stockage de cluster à cluster](../storage-replica/cluster-to-cluster-storage-replication.md).

![Diagramme de réplication du stockage](media/storage-spaces-direct-disaster-recovery/Disaster-Recovery-Figure1.png)

Les considérations suivantes s’appliquent lors du déploiement du réplica de stockage. 

1.  La configuration de la réplication s’effectue en dehors du clustering de basculement. 
2.  Le choix de la méthode de réplication dépend de la latence du réseau et des exigences de RPO. Le mode synchrone réplique les données sur les réseaux à faible latence avec cohérence des incidents pour garantir l’absence de perte de données en cas de défaillance. Asynchrone réplique les données sur des réseaux avec des latences plus élevées, mais chaque site peut ne pas avoir de copies identiques à la fois. 
3.  En cas de sinistre, les basculements entre les clusters ne sont pas automatiques et doivent être orchestrés manuellement par le biais des applets de commande PowerShell du réplica de stockage. Dans le schéma ci-dessus, ClusterA est le serveur principal et ClusterB est le réplica secondaire. Si ClusterA tombe en panne, vous devez définir manuellement ClusterB comme principal avant de pouvoir mettre les ressources à l’haut. Une fois le ClusterA sauvegardé, vous devez le rendre secondaire. Une fois que toutes les données ont été synchronisées, effectuez la modification et échangez les rôles de manière à ce qu’ils aient été définis à l’origine.
4.  Étant donné que le réplica de stockage réplique uniquement les données, un nouvel ordinateur virtuel ou serveur de fichiers Scale Out (SOFS) utilisant ces données doit être créé dans Gestionnaire du cluster de basculement sur le partenaire de réplication.

Le réplica de stockage peut être utilisé si vous avez des machines virtuelles ou un SOFS s’exécutant sur votre cluster. La mise en ligne des ressources dans le HCI du réplica peut être manuelle ou automatisée grâce à l’utilisation de scripts PowerShell.

## <a name="hyper-v-replica"></a>Réplication Hyper-V

Le [réplica Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/set-up-hyper-v-replica) fournit la réplication au niveau de l’ordinateur virtuel pour la récupération d’urgence sur les infrastructures hyper-convergées. La réplication Hyper-V permet de prendre une machine virtuelle et de la répliquer sur un site secondaire ou Azure (réplica). Ensuite, à partir du site secondaire, le réplica Hyper-V peut répliquer l’ordinateur virtuel sur un troisième (réplica étendu).

![Diagramme de réplication Hyper-V](media/storage-spaces-direct-disaster-recovery/Disaster-Recovery-Figure2.png)

Avec la réplication Hyper-V, la réplication est prise en charge par Hyper-V. Lorsque vous activez pour la première fois une machine virtuelle pour la réplication, vous pouvez choisir la façon dont vous souhaitez que la copie initiale soit envoyée aux clusters de réplication correspondants.

1.  Envoyer la copie initiale sur le réseau
2.  Envoyer la copie initiale sur un support externe afin qu’il puisse être copié manuellement sur votre serveur
3.  Utiliser un ordinateur virtuel existant déjà créé sur les hôtes de réplica

L’autre option concerne le moment où vous souhaitez que cette réplication initiale ait lieu.

1.  Démarrer immédiatement la réplication
2.  Planifiez une heure à laquelle la réplication initiale a lieu. 

Les autres éléments à prendre en compte sont les suivants :

- Les disques durs virtuels/VHDX que vous souhaitez répliquer. Vous pouvez choisir de répliquer tous les éléments ou un seul d’entre eux.
- Nombre de points de récupération que vous souhaitez enregistrer. Si vous souhaitez disposer de plusieurs options sur le point dans le temps que vous souhaitez restaurer, vous devez spécifier le nombre de votre choix. Si vous n’avez besoin que d’un point de restauration, vous pouvez également le choisir.
- La fréquence à laquelle vous souhaitez que le Service VSS (VSS) réplique un cliché instantané incrémentiel.
- La fréquence à laquelle les modifications sont répliquées (30 secondes, 5 minutes, 15 minutes).

Lorsque HCI participe à la réplication Hyper-V, vous devez avoir créé la ressource de [Service Broker de réplication Hyper-v](https://blogs.technet.microsoft.com/virtualization/2012/03/27/why-is-the-hyper-v-replica-broker-required/) dans chaque cluster. Cette ressource effectue plusieurs opérations :

1.  Vous donne un espace de noms unique pour chaque cluster auquel le réplica Hyper-V se connecte.
2.  Détermine quel nœud dans le cluster sur lequel le réplica (ou réplica étendu) résidera lorsqu’il reçoit pour la première fois la copie.
3.  Effectue le suivi du nœud propriétaire du réplica (ou du réplica étendu) au cas où l’ordinateur virtuel se déplace vers un autre nœud. Il doit effectuer le suivi afin que, lorsque la réplication a lieu, il puisse envoyer les informations au nœud approprié.

## <a name="backup-and-restore"></a>Sauvegarde et restauration

Une option de récupération d’urgence traditionnelle qui n’est pas encore parlée, mais qui est tout aussi importante, est la défaillance de l’ensemble du cluster ou d’un nœud du cluster. L’une ou l’autre des options de ce scénario utilise la sauvegarde Windows NT. 

Il est toujours recommandé d’effectuer des sauvegardes périodiques de l’infrastructure hyper-convergée. Si le service de cluster est en cours d’exécution, si vous effectuez une sauvegarde de l’état du système, la base de données du Registre du cluster fera partie de cette sauvegarde. La restauration du cluster ou de la base de données a deux méthodes différentes (ne faisant pas autorité et faisant autorité).

### <a name="non-authoritative"></a>Ne faisant pas autorité

Une restauration ne faisant pas autorité peut être effectuée à l’aide de la sauvegarde Windows NT et équivaut à une restauration complète du seul nœud de cluster. Si vous devez uniquement restaurer un nœud de cluster (et la base de données du Registre du cluster) et que toutes les informations actuelles du cluster sont correctes, vous devez effectuer une restauration à l’aide de ne faisant pas autorité. Les restaurations ne faisant pas autorité peuvent être effectuées par le biais de l’interface de sauvegarde Windows NT ou de la ligne de commande WBADMIN. Exécutable.

Une fois que vous avez restauré le nœud, laissez-le rejoindre le cluster. Ce qui se passe, c’est qu’il accède au cluster existant et met à jour toutes ses informations avec ce qui est actuellement.

### <a name="authoritative"></a>Manière

Une restauration faisant autorité de la configuration du cluster, en revanche, rétablit la configuration du cluster dans le temps. Ce type de restauration ne doit être effectué que si les informations du cluster ont été perdues et doivent être restaurées. Par exemple, quelqu’un a supprimé accidentellement un serveur de fichiers qui contenait plus de 1000 partages et vous en avez besoin. Pour effectuer une restauration faisant autorité du cluster, vous devez exécuter la sauvegarde à partir de la ligne de commande.

Quand une restauration faisant autorité est lancée sur un nœud de cluster, le service de cluster est arrêté sur tous les autres nœuds de l’affichage de cluster et la configuration du cluster est figée. C’est la raison pour laquelle il est essentiel que le service de cluster sur le nœud sur lequel la restauration a été exécutée démarre d’abord pour que le cluster soit formé à l’aide de la nouvelle copie de la configuration du cluster.

Pour exécuter une restauration faisant autorité, les étapes suivantes peuvent être effectuées.

1.  Exécutez WBADMIN. EXE à partir d’une invite de commandes d’administration pour obtenir la dernière version des sauvegardes que vous souhaitez installer et vérifier que l’état du système est l’un des composants que vous pouvez restaurer.

    ```powershell
    Wbadmin get versions
    ```

2.  Déterminez si la sauvegarde de la version contient les informations du Registre du cluster en tant que composant. Il y a deux éléments dont vous aurez besoin à partir de cette commande, de la version et de l’application/du composant à utiliser à l’étape 3. Pour la version, par exemple, imaginons que la sauvegarde a été effectuée le 3 janvier 2018 à 2:04am et qu’il s’agit de celui que vous avez besoin de restaurer.

    ```powershell
    wbadmin get items -backuptarget:\\backupserver\location
    ```

3.  Démarrez la restauration faisant autorité pour récupérer uniquement la version du registre de cluster dont vous avez besoin. 

    ```powershell
    wbadmin start recovery -version:01/03/2018-02:04 -itemtype:app -items:cluster
    ```

Une fois la restauration effectuée, ce nœud doit être celui pour démarrer le service de cluster en premier et former le cluster. Tous les autres nœuds doivent alors être démarrés et se joindre au cluster.

## <a name="summary"></a>Récapitulatif 

Pour additionner tout cela, la récupération d’urgence hyper-convergée est une opération qui doit être planifiée avec prudence. Il existe plusieurs scénarios qui peuvent mieux répondre à vos besoins et doivent être testés minutieusement. Un élément à noter est que si vous êtes familiarisé avec les clusters de basculement par le passé, les clusters étendus sont une option très populaire au fil des années. Il y avait un peu de modifications de conception avec la solution hyper-convergée et elle est basée sur la résilience. Si vous perdez deux nœuds dans un cluster hyper-convergé, l’ensemble du cluster s’affiche. Dans ce cas, dans un environnement hyper-convergé, le scénario d’étirement n’est pas pris en charge.


