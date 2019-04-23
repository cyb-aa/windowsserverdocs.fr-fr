---
title: Scénarios de récupération d’urgence pour l’Infrastructure Hyper-convergée
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: johnmarlin-msft
ms.date: 03/29/2018
description: Cet article décrit les scénarios disponibles dès aujourd'hui pour la récupération d’urgence de Microsoft HCI (espaces de stockage Direct)
ms.localizationpriority: medium
ms.openlocfilehash: 32bbf02ca78d5c6a2147162768c984d0e0b27e36
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879590"
---
# <a name="disaster-recovery-with-storage-spaces-direct"></a>Récupération d’urgence avec les espaces de stockage Direct

> S’applique à : Windows Server 2019, Windows Server 2016

Cette rubrique fournit des scénarios sur comment infrastructure Hyper-convergée (HCL) peuvent être configurés pour la récupération d’urgence.

De nombreuses sociétés sont en cours d’exécution solutions hyperconvergées et planification d’urgence permet de rester dans ou revenir à la production rapidement si un incident se produit. Il existe plusieurs façons de configurer HCL pour la récupération d’urgence et ce document explique les options qui sont actuellement disponibles pour vous.

Lorsque les discussions de la restauration de la disponibilité en cas de sinistre tournent autour de ce qui est connu comme l’objectif de délai récupération (RTO). Il s’agit de la durée de temps ciblé où les services doivent être restaurés pour éviter les conséquences inacceptables pour l’entreprise. Dans certains cas, ce processus peut se produire automatiquement avec production restaurée presque immédiatement. Dans d’autres cas, une intervention manuelle administrateur doit se produire pour restaurer les services.

Les options de récupération d’urgence avec un hyperconvergé sont aujourd'hui :

1. Plusieurs clusters en utilisant le réplica de stockage
2. Réplica Hyper-V entre des clusters
3. Sauvegarde et restauration

## <a name="multiple-clusters-utilizing-storage-replica"></a>Plusieurs clusters en utilisant le réplica de stockage

[Le réplica de stockage](../storage-replica/storage-replica-overview.md) permet la réplication de volumes et prend en charge de la réplication synchrone et asynchrone. Lors du choix entre l’utilisation de la réplication synchrone ou asynchrone, vous devez envisager votre objectif de Point de récupération (RPO). Objectif de Point de récupération est la quantité de perte de données que vous êtes prêt à assumer avant d’être considéré perte majeure. Si vous accédez avec la réplication synchrone, il séquentiellement écrira aux deux extrémités en même temps. Si vous accédez avec asynchrone, écritures répliqueront très rapide mais peuvent toujours être perdus. Vous devez envisager l’utilisation d’application ou le fichier pour voir ce qui fonctionne mieux pour vous.

Réplica de stockage est un mécanisme de copie au niveau bloc et au niveau du fichier ; Autrement dit, peu importe quels types de données en cours de réplication. Cela rend une option populaire pour l’infrastructure Hyper-convergée. Le réplica de stockage permettre également utiliser les différents types de disques entre les partenaires de réplication, ainsi que de tous de stockage d’un type sur un seul HCL et fonctionne parfaitement bien à un autre stockage de type sur l’autre. 

Une fonctionnalité importante du réplica de stockage est qu’il peut être exécuté dans Azure, ainsi que sur site. Vous pouvez configurer en local au niveau local, Azure vers Azure, ou même localement vers Azure (ou vice versa).

Dans ce scénario, il existe deux clusters indépendants distincts. Pour configurer le réplica de stockage entre HCL, vous pouvez suivre les étapes décrites dans [réplication de stockage de Cluster à cluster](../storage-replica/cluster-to-cluster-storage-replication.md).

![Diagramme de la réplication de stockage](media\storage-spaces-direct-disaster-recovery\Disaster-Recovery-Figure1.png)

Les considérations suivantes s’appliquent lors du déploiement de réplica de stockage. 

1.  Configuration de la réplication est effectué en dehors de Clustering de basculement. 
2.  Choix de la méthode de réplication dépendra votre latence du réseau en matière de RPO. Synchrone réplique les données sur des réseaux à faible latence avec cohérence d’incident pour éviter toute perte de données à la fois de défaillance. Asynchrone réplique les données sur les réseaux avec des latences élevées, mais chaque site peut-être pas des copies identiques à la fois de défaillance. 
3.  En cas de sinistre, les basculements entre les clusters ne sont pas automatiques et doivent être orchestré manuellement via les applets de commande PowerShell de réplica de stockage. Dans le diagramme ci-dessus, ClusterA est le principal et ClusterB est la base de données secondaire. Si ClusterA tombe en panne, vous devez définir manuellement ClusterB comme principale avant que vous pouvez afficher les ressources. Une fois que ClusterA est sauvegardé, vous devez rendre la base de données secondaire. Une fois toutes les données a été synchronisé des, apporter la modification et échange les rôles à la façon qu'elles ont été définies à l’origine.
4.  Étant donné que le réplica de stockage réplique uniquement les données, un nouvel ordinateur virtuel ou une mise à l’échelle des serveur de fichiers (SOFS) utilisant ces données devra être créé à l’intérieur de gestionnaire du Cluster de basculement sur le partenaire de réplica.

Le réplica de stockage peut être utilisé si vous avez des machines virtuelles ou un serveur SOFS en cours d’exécution sur votre cluster. Connexion des ressources dans le réplica HCL peut être manuel ou automatisé via l’utilisation de l’écriture de scripts PowerShell.

## <a name="hyper-v-replica"></a>Réplication Hyper-V

[Réplica Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/set-up-hyper-v-replica) fournit la réplication au niveau des machines virtuelles pour la récupération d’urgence sur des infrastructures hyperconvergé. Ce que peut faire le réplica Hyper-V est à prendre une machine virtuelle et la réplique sur un site secondaire ou sur Azure (réplica). Puis à partir du site secondaire, réplica Hyper-V peut répliquer la machine virtuelle à un tiers (réplica étendu).

![Diagramme de la réplication Hyper-V](media\storage-spaces-direct-disaster-recovery\Disaster-Recovery-Figure2.png)

Avec le réplica Hyper-V, la réplication est pris en charge par Hyper-V. Lorsque vous activez tout d’abord une machine virtuelle pour la réplication, il existe trois options pour la façon dont vous souhaitez la copie initiale pour être envoyées pour les clusters de réplica correspondant.

1.  Envoyer la copie initiale sur le réseau
2.  Envoyer la copie initiale sur des supports externes afin qu’il peut être copié manuellement sur votre serveur
3.  Utiliser une machine virtuelle existante déjà créée sur les hôtes de réplica

L’autre option concerne lorsque vous devez activer que la réplication initiale doit avoir lieu.

1.  Démarrer la réplication immédiatement
2.  Planifiez une heure pour laquelle la réplication initiale a lieu. 

Autres considérations, que vous devez sont :

- Du VHD/VHDX vous souhaitez répliquer. Vous pouvez choisir de répliquer toutes les ou qu’un seul d'entre eux.
- Nombre de points de récupération que vous souhaitez enregistrer. Si vous souhaitez avoir plusieurs options sur quel point dans le temps que vous souhaitez restaurer, vous pouvez spécifier le nombre souhaité. Si vous souhaitez uniquement un point de restauration, vous pouvez le choisir également.
- La fréquence à laquelle vous souhaitez que les Volume Shadow Copy Service (VSS) à répliquer un cliché instantané incrémentiel.
- La fréquence à laquelle ces modifications sont répliquées (30 secondes, 5 minutes, 15 minutes).

Lorsque HCL participer de réplica Hyper-V, vous devez disposer du [Hyper-V Replica Broker](https://blogs.technet.microsoft.com/virtualization/2012/03/27/why-is-the-hyper-v-replica-broker-required/) ressource créée dans chaque cluster. Cette ressource fait plusieurs choses :

1.  Vous offre un espace de noms unique pour chaque cluster pour réplica Hyper-V pour vous connecter à.
2.  Détermine quel nœud au sein de ce cluster, le réplica (ou réplica étendu) résideront sur lorsqu’il reçoit tout d’abord la copie.
3.  Effectue le suivi de nœud qui possède le réplica (ou réplica étendu), au cas où la machine virtuelle est déplacée vers un autre nœud. Il doit effectuer ce suivi afin que lors de la réplication a lieu, il peut envoyer les informations sur le nœud approprié.

## <a name="backup-and-restore"></a>Sauvegarde et restauration

Une option de récupération d’urgence traditionnelles qui n’est pas parlée à beaucoup, mais est tout aussi importante est l’échec de l’ensemble du cluster ou un nœud du cluster. L’option choisie avec ce scénario se sert de sauvegarde de Windows NT. 

Il est toujours une recommandation ait des sauvegardes périodiques de l’infrastructure Hyper-convergée. Bien que le Service de Cluster est en cours d’exécution, si vous prenez une sauvegarde d’état système, la base de données de Registre de cluster serait une partie de cette sauvegarde. Restauration de base de données ou le cluster a deux méthodes différentes (Non-faisant autorité et faisant autorité).

### <a name="non-authoritative"></a>Faisant

Une restauration faisant peut être accomplie à l’aide de la sauvegarde de Windows NT et équivaut à une restauration complète du nœud de cluster lui-même. Si vous avez besoin uniquement de restaurer un nœud de cluster (et la base de données de Registre de cluster) et tous les clusters plus d’informations bonne, vous restaurez à l’aide ne faisant pas autorité. Les restaurations faisant peuvent être effectuées via l’interface de sauvegarde de Windows NT ou de la ligne de commande WBADMIN. EXE.

Une fois que vous restaurez le nœud, laissez-le rejoindre le cluster. Que se passera-t-il est qu’il accède au cluster existant en cours d’exécution et toutes ses informations de mise à jour avec ce qui est actuellement.

### <a name="authoritative"></a>Faisant autorité

Une restauration faisant autorité de la configuration du cluster prend quant à eux, la configuration du cluster dans le temps. Ce type de restauration doit uniquement être réalisée lorsque les informations du cluster lui-même a été perdues et doit être restaurées. Par exemple, quelqu'un a supprimé accidentellement un serveur de fichiers contenant des partages de plus de 1000 et vous devez le retour. Fin d’une restauration faisant autorité du cluster nécessite l’exécution à partir de la ligne de commande sauvegarde.

Lorsqu’une restauration faisant autorité est lancée sur un nœud de cluster, le service de cluster est arrêté sur tous les autres nœuds dans la vue de cluster et la configuration du cluster est figée. C’est pourquoi il est essentiel que le service de cluster sur le nœud sur lequel la restauration a été exécutée être démarrés en premier pour le cluster est formé à l’aide de la nouvelle copie de la configuration du cluster.

Pour exécuter une restauration faisant autorité, les étapes suivantes peuvent être accomplies.

1.  Exécutez WBADMIN. EXE à partir d’une invite de commandes d’administration pour obtenir la dernière version des sauvegardes que vous souhaitez installer et assurez-vous que l’état du système est l’un des composants que vous pouvez restaurer.

    ```powershell
    Wbadmin get versions
    ```

2.  Déterminer si la sauvegarde de version que vous avez comporte les informations de Registre de cluster en tant que composant. Il existe quelques éléments qui que vous seront nécessaires à partir de cette commande, la version et le composant / d’application pour une utilisation à l’étape 3. Pour la version, par exemple, la sauvegarde a été effectuée le 3 janvier 2018 à 2 h 04 et c’est celui-ci que vous devez restaurée.

    ```powershell
    wbadmin get items -backuptarget:\\backupserver\location
    ```

3.  Démarrer la restauration faisant autorité pour récupérer uniquement la version de Registre de cluster que vous avez besoin. 

    ```powershell
    wbadmin start recovery -version:01/03/2018-02:04 -itemtype:app -items:cluster
    ```

Une fois que la restauration a eu lieu, ce nœud doit être celui dans lequel démarrer le Service de Cluster tout d’abord et former un cluster. Tous les autres nœuds puis deviez démarrer et rejoindre le cluster.

## <a name="summary"></a>Récapitulatif 

Pour additionner tout, hyperconvergé reprise d’activité est quelque chose qui doit être planifiée avec soin out. Il existe plusieurs scénarios qui peuvent mieux à vos besoins et doivent être testées. À noter est que si vous êtes familiarisé avec les Clusters de basculement dans le passé, clusters étendus ont été une option très populaire au fil des années. Il était un peu d’une modification de conception avec la solution hyperconvergée et elle est basée sur la résilience. Si vous perdez les deux nœuds dans un cluster hyperconvergé, l’ensemble du cluster s’arrêtent. Avec ce qui est le cas, dans un environnement hyperconvergé, le scénario d’extension n’est pas pris en charge.


