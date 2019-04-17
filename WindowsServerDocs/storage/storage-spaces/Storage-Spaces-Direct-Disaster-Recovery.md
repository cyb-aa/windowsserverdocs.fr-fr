---
title: Scénarios de récupération d’urgence pour l’Infrastructure Hyperconvergée
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: johnmarlin-msft
ms.date: 03/29/2018
description: Cet article décrit les scénarios disponibles aujourd'hui pour la récupération d’urgence de Microsoft HCI (espaces de stockage Direct)
ms.localizationpriority: medium
ms.openlocfilehash: 32bbf02ca78d5c6a2147162768c984d0e0b27e36
ms.sourcegitcommit: dfd25348ea3587e09ea8c2224237a3e8078422ae
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/16/2018
ms.locfileid: "4678608"
---
# Récupération d’urgence avec des espaces de stockage Direct

> S’applique à: Windows Server 2019, Windows Server 2016

Cette rubrique fournit des scénarios sur la façon dont infrastructure hyperconvergée (HCI) peuvent être configurés pour la récupération d’urgence.

De nombreuses sociétés sont en cours d’exécution solutions hyperconvergées et prévoir un incident donne la possibilité de rester dans ou revenir à la production rapidement s’agissait d’un incident se produise. Il existe plusieurs façons de configurer HCI de récupération d’urgence et ce document explique les options qui soient offrent à vous aujourd'hui.

Lorsque les discussions de restauration de disponibilité si incident sont axés sur ce que l'on comme objectif (délai de récupération). Il s’agit de la durée de temps ciblées où les services doivent être restaurés d’éviter des conséquences inacceptables pour l’entreprise. Dans certains cas, ce processus peut se produire automatiquement avec la production restaurée presque immédiatement. Dans les autres cas, une intervention manuelle administrateur doit se produire pour restaurer les services.

Les options de récupération d’urgence avec un hyperconvergée sont aujourd'hui:

1. Plusieurs clusters utilisant le réplica de stockage
2. Réplica Hyper-V entre les clusters
3. Sauvegarde et restauration

## Plusieurs clusters utilisant le réplica de stockage

[Le réplica de stockage](../storage-replica/storage-replica-overview.md) permet la réplication de volumes et prend en charge la réplication synchrone et asynchrone. Lors du choix entre l’utilisation de la réplication synchrone ou asynchrone, vous devez envisager votre objectif de Point de récupération (RPO). Objectif de Point de récupération est la quantité de perte de données possible que vous êtes prêt à appliquer avant qu’il est considéré comme une perte majeure. Si vous passez avec la réplication synchrone, il séquentiellement écrira aux deux extrémités en même temps. Si vous optez asynchrone, les écritures répliqueront très rapide, mais peuvent toujours être perdus. Vous devez envisager l’utilisation d’application ou le fichier pour voir ce qui fonctionne mieux pour vous.

Le réplica de stockage est un mécanisme de copie au niveau de blocage par rapport à niveau de fichier; Cela signifie que n’importe quels types de données en cours de réplication. Cela permet une option populaires d’une infrastructure hyperconvergée. Le réplica de stockage peut également utiliser des différents types de lecteurs entre les partenaires de réplication, par conséquent, ayant tous les seul type de stockage sur un seul HCI et fonctionne parfaitement bien à un autre stockage de type sur l’autre. 

Une des principales caractéristiques du réplica de stockage sont qu’il peut être exécuté dans Azure, ainsi que sur site. Vous pouvez configurer local à local, Azure sur Azure, ou même local à Azure (ou vice versa).

Dans ce scénario, il existe deux clusters indépendants distincts. Pour configurer le réplica de stockage entre HCI, vous pouvez suivre les étapes décrites dans [la réplication de stockage de Cluster à cluster](../storage-replica/cluster-to-cluster-storage-replication.md).

![Diagramme de réplication de stockage](media\storage-spaces-direct-disaster-recovery\Disaster-Recovery-Figure1.png)

Les considérations suivantes s’appliquent lors du déploiement de réplica de stockage. 

1.  La configuration de la réplication est effectuée en dehors du Clustering de basculement. 
2.  Choix de la méthode de la réplication sera varie selon vos exigences de RPO et de latence du réseau. Synchrone réplique les données sur les réseaux de faible latence avec la cohérence d’incident afin d’aucune perte de données à la fois de défaillance. Effectue une réplication asynchrone les données sur les réseaux à latence élevée, mais chaque site peut-être pas des copies identiques à la fois de défaillance. 
3.  Dans le cas d’incident, basculements entre les clusters ne sont pas automatiques et doivent être manuellement orchestrés par le biais d’applets de commande PowerShell de réplica de stockage. Dans le diagramme ci-dessus, ClusterA est le principal et ClusterB est secondaire. Si ClusterA tombe en panne, vous devez définir manuellement ClusterB comme principale avant que vous pouvez afficher les ressources. Une fois ClusterA arrière vers le haut, vous devez rendre secondaire. Une fois que toutes les données a été synchronisées haut, apportez les modifications et les rôles à la façon dont ils ont été définies à l’origine d’échange.
4.  Dans la mesure où le réplica de stockage réplique uniquement les données, une machine virtuelle ou une mise à l’échelle en fichier de serveur en puissance parallèle utilisant ces données doivent être créés à l’intérieur de gestionnaire du Cluster de basculement sur le partenaire de réplica.

Le réplica de stockage peut être utilisé si vous disposez d’ordinateurs virtuels ou un en puissance en cours d’exécution sur votre cluster. Intégration de ressources en ligne dans le réplica HCI peut être manuelle ou automatique par le biais de l’écriture de scripts PowerShell.

## RéplicaHyper-V

[Réplica Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/set-up-hyper-v-replica) fournit machine virtuelle de réplication au niveau de récupération d’urgence sur les infrastructures hyperconvergées. Ce que le réplica Hyper-V peut faire consiste à prendre une machine virtuelle et la réplique sur un site secondaire ou Azure (réplica). Puis à partir du site secondaire, le réplica Hyper-V peut répliquer la machine virtuelle à un tiers (réplica étendu).

![Diagramme de réplication Hyper-V](media\storage-spaces-direct-disaster-recovery\Disaster-Recovery-Figure2.png)

Avec le réplica Hyper-V, la réplication est prises en charge par Hyper-V. Lorsque vous activez tout d’abord une machine virtuelle pour la réplication, il existe trois options pour la façon dont vous souhaitez la copie initiale à envoyer aux clusters de réplica correspondant.

1.  Envoyer la copie initiale via le réseau
2.  Envoyer la copie initiale vers des médias externes afin qu’il peut être copié sur votre serveur manuellement
3.  Utilisez un ordinateur virtuel existant déjà créé sur les hôtes du réplica du système

L’autre option est si vous le souhaitez que cette réplication initiale doit avoir lieu.

1.  Démarrer la réplication immédiatement
2.  Planifiez une heure de lorsque la réplication initiale a lieu. 

Autres considérations que vous devez sont les suivantes:

- Du VHD/VHDX vous souhaitez répliquer. Vous pouvez choisir de répliquer chacun d’eux ou uniquement un d'entre eux.
- Nombre de points de récupération que vous souhaitez enregistrer. Si vous souhaitez disposent de plusieurs options sur quel point dans le temps que vous souhaitez restaurer, vous pouvez spécifier le nombre souhaité. Si vous souhaitez uniquement un point de restauration, vous pouvez en choisir qui également.
- La fréquence à laquelle vous souhaitez que les Volume Shadow Copy Service (VSS) à répliquer un cliché instantané incrémentiel.
- La fréquence à laquelle ces modifications sont répliquées (30 secondes, 5 minutes, 15 minutes).

Lorsque HCI participer au réplica Hyper-V, vous devez disposer de la ressource [Broker de réplica Hyper-V](https://blogs.technet.microsoft.com/virtualization/2012/03/27/why-is-the-hyper-v-replica-broker-required/) créée dans chaque cluster. Cette ressource effectue plusieurs choses:

1.  Vous donne un espace de noms unique pour chaque cluster pour le réplica Hyper-V pour se connecter à.
2.  Détermine quel nœud appartenant à ce cluster le réplica (ou un réplica étendu) se trouvera sur lorsqu’il reçoit d’abord la copie.
3.  Gère de nœud qui possède le réplica (ou un réplica étendu), au cas où la machine virtuelle se déplace vers un autre nœud. Il doit s’assurer le suivi afin que lors de la réplication a lieu, il peut envoyer les informations sur le nœud approprié.

## Sauvegarde et restauration

Une option de récupération d’urgence classiques qui n’est pas abordée très bien, mais est tout aussi importants est l’échec de l’ensemble du cluster ou un nœud du cluster. Des options avec ce scénario utilise de la sauvegarde Windows NT. 

Il est toujours une recommandation de disposer des sauvegardes périodiques de l’infrastructure hyperconvergée. Même si le Service de Cluster est en cours d’exécution, si vous prenez une sauvegarde d’état système, la base de données du Registre de cluster est une partie de la sauvegarde. Restauration de cluster ou la base de données dispose de deux méthodes différentes (Non-faisant autorité et faisant autorité).

### Non forcée

Une restauration Non forcée peut être obtenue à l’aide de la sauvegarde Windows NT et équivaut à une restauration complète du nœud de cluster lui-même. Si vous avez uniquement besoin restaurer un nœud de cluster (et la base de données du Registre de cluster) et toutes les cluster informations actuelles bonnes, vous restaurez à l’aide de non forcée. Restaurations non-faisant autorité peuvent être effectuées par le biais de l’interface Windows NT sauvegarde ou de la ligne de commande WBADMIN. EXE.

Une fois que vous restaurez le nœud, laissez-le rejoindre le cluster. Ce qui se passera est qu’il sera vont vers le cluster existant en cours d’exécution et toutes ses informations de mise à jour par ce qui est actuellement.

### Faisant autorité

Une restauration forcée de la configuration du cluster, prend en revanche, la configuration du cluster en arrière. Ce type de restauration doit uniquement être réalisée lorsque les informations du cluster lui-même a été perdues et doit être restaurées. Par exemple, un utilisateur supprimé par inadvertance un serveur de fichiers contenant plus de 1000 partages et vous en avez besoin précédent. Effectué une restauration forcée du cluster nécessite que sauvegarde être exécuté à partir de la ligne de commande.

Lorsqu’une restauration faisant autorité est lancée sur un nœud de cluster, le service de cluster est arrêté sur tous les autres nœuds dans la vue de cluster et la configuration du cluster est figée. C’est pourquoi il est essentiel que le service de cluster sur le nœud sur lequel la restauration a été exécutée être démarrés en premier afin que le cluster est formé à l’aide de la nouvelle copie de la configuration du cluster.

Pour exécuter par le biais d’une restauration forcée, les étapes suivantes peuvent être accomplies.

1.  Exécutez WBADMIN. EXE à partir d’une invite de commandes d’administration pour obtenir la dernière version de sauvegardes que vous souhaitez installer et vous assurer qu’état du système est l’un des composants que vous pouvez restaurer.

    ```powershell
    Wbadmin get versions
    ```

2.  Déterminer si la sauvegarde de version que vous avez comporte les informations de Registre de cluster en tant que composant. Il existe deux éléments que vous devez à partir de cette commande, la version et le composant / d’application pour une utilisation à l’étape 3. Pour la version, par exemple, la sauvegarde a été effectuée 3 janvier 2018 à 2 h 04 et c’est celle que vous devez restaurée.

    ```powershell
    wbadmin get items -backuptarget:\\backupserver\location
    ```

3.  Démarrez la restauration faisant autorité pour récupérer uniquement la version de Registre de cluster que vous avez besoin. 

    ```powershell
    wbadmin start recovery -version:01/03/2018-02:04 -itemtype:app -items:cluster
    ```

Une fois que la restauration a eu lieu, ce nœud doit être celui d’abord démarrer le Service de Cluster et former le cluster. Tous les autres nœuds doivent alors être démarré et rejoindre le cluster.

## Résumé 

Pour additionner tout cela, récupération d’urgence hyperconvergée est quelque chose qui doit être planifié avec soin. Il existe plusieurs scénarios qui peuvent mieux adapté à vos besoins et doivent être testées. Un seul élément à noter est que si vous êtes familiarisé avec les Clusters de basculement dans le passé, les clusters étendus ont été une option très populaire au fil des ans. Il n’existait un peu d’une modification de conception avec la solution hyperconvergée et elle est basée sur la résilience. Si vous perdez deux nœuds dans un cluster hyperconvergé, l’ensemble du cluster est soumis vers le bas. Avec le cas échéant, dans un environnement hyperconvergée, le scénario étendu n’est pas pris en charge.


