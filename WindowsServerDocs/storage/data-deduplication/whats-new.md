---
ms.assetid: d11acbc2-40c6-4ab2-9514-2bc3ad81499a
title: "Nouveautés de la déduplication des données"
ms.technology: storage-deduplication
ms.prod: windows-server-threshold
ms.topic: article
author: wmgries
manager: klaasl
ms.author: wgries
ms.date: 09/15/2016
ms.openlocfilehash: 4a69221548d9defff5a45413ccfe824f9788755a
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# <a name="whats-new-in-data-deduplication"></a>Nouveautés de la déduplication des données

> S’applique à: WindowsServer (canal semi-annuel), WindowsServer2016

La [déduplication des données](overview.md) dans WindowsServer2016 a été optimisée pour être hautement performante, flexible et facile à gérer à l’échelle du cloud privé. Pour en savoir plus sur la pile de stockage à définition logicielle dans Windows Server2016, voir [Nouveautés du stockage dans WindowsServer2016](../whats-new-in-storage.md).

La déduplication des données présente les améliorations suivantes dans Windows Server2016:

| Fonctionnalités | Nouveauté ou mise à jour | Description |
|---------------|----------------|-------------|
| [Prise en charge de volumes importants](whats-new.md#large-volume-support) | Mis à jour | Avant WindowsServer2016, les volumes devaient être dimensionnés de façon spécifique pour l’activité attendue, avec des tailles de volume supérieures à 10To qui ne convenaient pas à la déduplication. Dans WindowsServer2016, la déduplication des données prend en charge des tailles de volume pouvant atteindre 64To. |
| [Prise en charge de fichiers volumineux](whats-new.md#large-file-support) | Mis à jour | Avant Windows Server2016, les fichiers dont la taille était proche de 1To ne convenaient pas à la déduplication. Dans WindowsServer2016, les fichiers dont la taille peut atteindre1To sont entièrement pris en charge. |
| [Prise en charge de Nano Server](whats-new.md#nano-server-support) | Nouveau | La déduplication des données est disponible et entièrement prise en charge dans la nouvelle option de déploiement de Nano Server pour Windows Server2016. |
| [Prise en charge de sauvegarde simplifiée](whats-new.md#simple-backup-support) | Nouveau | Windows Server2012R2 prenait en charge les applications de sauvegarde virtualisées, telles que [Data Protection Manager](https://technet.microsoft.com/library/hh758173.aspx) de Microsoft, via une série d’étapes de configuration manuelles. Dans Windows Server2016, un nouveau type d’utilisation par défaut (Sauvegarde) a été ajouté pour un déploiement transparent de la déduplication des données pour les applications de sauvegarde virtualisées.|
| [Prise en charge de la mise à niveau propagée de système d’exploitation de cluster](whats-new.md#cluster-upgrade-support) | Nouveau | La déduplication des données prend entièrement en charge la nouvelle fonctionnalité de [mise à niveau propagée de système d’exploitation de cluster](../..//failover-clustering/cluster-operating-system-rolling-upgrade.md) de Windows Server2016. |

## <a name="large-volume-support"></a>Prise en charge de volumes importants

**Quels avantages cette modification procure-t-elle?**  
Afin d’obtenir des performances optimales de la déduplication des données dans WindowsServer2012R2, vous devez dimensionner les volumes correctement pour garantir que la tâche d’optimisation peut suivre le taux de modification des données, ou «évolution». En règle générale, cela signifie que la déduplication des données est performante uniquement sur les volumes de 10To au maximum, en fonction des modèles d’écriture de la charge de travail.

Dans Windows Server2016, la déduplication des données est hautement performante sur des volumes pouvant atteindre 64To.

**En quoi le fonctionnement est-il différent?**  
Dans Windows Server2012R2, le pipeline du travail de déduplication des données utilise un seul thread et la file d’attente d’E/S pour chaque volume. Pour garantir que les tâches d’optimisation ne prennent pas de retard, ce qui entraînerait une diminution du cumul des gains du volume, les jeux de données volumineux doivent être divisés en volumes plus petits. La taille de volume appropriée dépend de l’évolution attendue pour ce volume. En moyenne, la valeur maximale est incluse entre6 et 7To pour les volumes dont l’évolution est élevée, et entre 9 et10To pour les volumes dont l’évolution est faible.

Dans WindowsServer2016, le pipeline de la tâche de déduplication de données a été repensé pour exécuter plusieurs threads en parallèle en utilisant plusieurs files d’attente d’E/S pour chaque volume. Cela permet des performances qui étaient auparavant impossibles à obtenir, grâce à la répartition des données en plusieurs volumes de taille inférieure. Cette modification est représentée dans l’illustration suivante:

![Visualisation comparant le pipeline du travail de déduplication des données dans Windows Server2012R2 et Windows Server2016](media/server-2016-dedup-job-pipeline.png)

Ces optimisations s’appliquent à [tous les travaux de déduplication des données](understand.md#job-info), et non uniquement au travail d’optimisation.

## <a name="large-file-support"></a>Prise en charge de fichiers volumineux
**Quels avantages cette modification procure-t-elle?**  
Dans Windows Server2012R2, les fichiers très volumineux ne sont pas adaptés à la déduplication des données en raison d’une diminution des performances du pipeline de traitement de la déduplication. Dans Windows Server2016, la déduplication de fichiers dont la taille peut atteindre 1To est très performante, permettant aux administrateurs d’appliquer des gains de déduplication à un plus grand nombre de charges de travail. Vous pouvez par exemple dédupliquer des fichiers très volumineux normalement associés aux charges de travail de sauvegarde.

**En quoi le fonctionnement est-il différent ?**  
Dans WindowsServer2016, la déduplication des données utilise des nouvelles structures de mappage de flux et d’autres améliorations d’implémentation pour augmenter les performances de débit et d’accès de l’optimisation. En outre, le pipeline de traitement de la déduplication peut maintenant reprendre le cours de l’optimisation près un basculement, au lieu de redémarrer. Grâce à ces modifications, la déduplication de fichiers pouvant atteindre 1To est hautement performante.

## <a name="nano-server-support"></a>Prise en charge de Nano Server
**Quels avantages cette modification procure-t-elle?**  
NanoServer est une nouvelle option de déploiement sans affichage dans WindowsServer2016, qui présente un encombrement des ressources système très faible, démarre beaucoup plus rapidement et nécessite moins de mises à jour et de redémarrages que l’option de déploiement de WindowsServer principal. La déduplication des données est entièrement prise en charge sur Nano Server. Pour plus d’informations sur Nano Server, voir [Prise en main de Nano Server](../../get-started/getting-started-with-nano-server.md).

## <a name="simple-backup-support">Configuration simplifiée pour les applications de sauvegarde virtualisées</a>
**Quels avantages cette modification procure-t-elle?**  
La déduplication des données pour les applications de sauvegarde virtualisées est un scénario pris en charge dans Windows Server2012R2, mais il est nécessaire de régler manuellement les paramètres de la déduplication. Dans WindowsServer2016, la configuration de la déduplication pour les applications de sauvegarde virtualisées est considérablement simplifiée. Cette fonctionnalité utilise une option de type d’utilisation prédéfinie lors de l’activation de la déduplication pour un volume, tout comme les options proposées pour le serveur de fichiers à usage général et l’infrastructureVDI.

## <a name="cluster-upgrade-support">Prise en charge de la mise à niveau propagée de système d’exploitation de cluster</a>
**Quels avantages cette modification procure-t-elle?**  
Les clusters de basculement WindowsServer exécutant la déduplication des données peuvent comporter un mélange de nœuds exécutant des versions de Windows Server2012R2 et de Windows Server2016 de la déduplication. Cette amélioration fournit un accès complet aux données à tous les volumes dédupliqués pendant une mise à niveau propagée de cluster, permettant le lancement progressif de la nouvelle version de la déduplication sur un cluster Windows Server2012R2 existant sans entraîner de temps d’arrêt pour la mise à niveau de tous les nœuds à la fois.

**En quoi le fonctionnement est-il différent ?**<br />
Dans les versions précédentes de WindowsServer, tous les nœuds du cluster devaient exécuter la même version de Windows Server pour qu’un cluster de basculement WindowsServer fonctionne. Depuis Windows Server2016, la fonctionnalité de mise à niveau propagée de cluster permet à un cluster de s’exécuter en mode mixte. La déduplication prend en charge cette nouvelle configuration de cluster en mode mixte pour activer l’accès complet aux données lors d’une mise à niveau propagée de cluster.
