---
title: Comprendre et voir resynchronisation du stockage
description: Obtenir des informations détaillées sur lorsque resynchronisation du stockage se produit et comment elle voir dans Windows Server 2019.
keywords: Espaces de stockage Direct, resynchronisation du stockage, resynchroniser, stockage, S2D
ms.prod: windows-server-threshold
ms.author: adagashe
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 01/14/2019
ms.localizationpriority: medium
ms.openlocfilehash: 81b1136a4b6a5cf8423a99e898b482a9b2849b5f
ms.sourcegitcommit: 748eccd10fc0c3c962d6e64ff6ead08017ac1947
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/15/2019
ms.locfileid: "9009624"
---
# Comprendre et contrôler la resynchronisation du stockage

>S’applique à: Windows Server 2019

Les alertes de resynchronisation de stockage sont une nouvelle fonctionnalité d' [Espaces de stockage Direct](storage-spaces-direct-overview.md) dans Windows Server 2019 qui permet au Service d’intégrité lever une erreur lors de l’espace de stockage est en cours de synchronisation. L’alerte est utile pour vous informer lors de la resynchronisation se produit, afin que vous ne prenez accidentellement des serveurs supplémentaires vers le bas (ce qui pourrait entraîner plusieurs domaines d’erreur en être affectées, ce qui entraîne dans votre cluster va vers le bas). 

Cette rubrique fournit des étapes pour comprendre et voir resynchronisation du stockage dans un cluster de basculement Windows Server avec des espaces de stockage Direct et en arrière-plan.

## Resynchronisation de présentation

Commençons par un exemple simple de comprendre la façon dont le stockage récupère désynchronisé. N’oubliez pas que tout sans partage (les lecteurs locaux) solution de stockage distribué ayant ce comportement. Comme vous le verrez ci-dessous, si un nœud de serveur tombe en panne, puis ses lecteurs ne sont pas mis à jour jusqu'à ce qu’il revienne en ligne - cela est vrai pour n’importe quelle architecture hyperconvergé. 

Supposons que nous voulons stocker la chaîne «HELLO». 

![ASCII de la chaîne «hello»](media/understand-storage-resync/hello.png)

Asssuming que nous avons la résilience en miroir triple, nous avons trois copies de cette chaîne. Maintenant, si nous entraînera pas le serveur #1 temporairement (pour la maintenance), nous ne pouvons pas accéder copie #1.

![Ne peut pas accéder à la copie #1](media/understand-storage-resync/copy1.png)

Supposons que nous mettons à jour notre chaîne à partir de «HELLO» à «Aide!» à ce stade.

![ASCII de chaîne «aide!»](media/understand-storage-resync/help.png)

Une fois que nous mettons à jour la chaîne, copie #2 et 3 # sera correctement mise à jour. Toutefois, la copie #1 toujours ne sont pas accessibles dans la mesure où le serveur #1 est arrêté temporairement (pour la maintenance). 

![GIF de l’écriture copier #2 et 2 #»](media/understand-storage-resync/write.gif)

Maintenant, nous avons copy #1 qui comporte des données qui sont désynchronisées. Le système d’exploitation utilise granulaire région modifiée de suivi pour suivre les bits qui ne sont pas synchronisées. De cette manière lorsque le serveur #1 revienne en ligne, nous pouvons synchroniser les modifications en lisant les données à partir de la copie #2 ou #3 et en remplaçant les données de copie #1. Les avantages de cette approche sont que nous avons uniquement besoin à copier les données qui sont obsolète, au lieu de synchroniser toutes les données à partir de serveur #2 ou 3 #.

![GIF d’écrasement de copier #1»](media/understand-storage-resync/overwrite.gif)

Par conséquent, ce qui explique la façon dont les données récupère désynchronisées. Mais à quoi cette ressemble à un niveau élevé? Supposons que dans cet exemple que nous avons un cluster hyperconvergé trois serveurs. Lorsque le serveur #1 se trouve sur la maintenance, vous le verrez comme étant vers le bas. Lorsque vous mettez server #1 arrière vers le haut, il démarrera synchroniser tous de son stockage utilisent le suivi de la région modifiée granulaire (expliqué ci-dessus). Une fois que les données sont les replacer synchronisées, tous les serveurs seront affichent comme étant en service.

![GIF de vue de l’administrateur de resynchronisation»](media/understand-storage-resync/admin.gif)

## Comment faire pour surveiller la resynchronisation du stockage dans Windows Server 2019

Maintenant que vous comprenez le fonctionne de resynchronisation du stockage, examinons comment il affiche dans Windows Server 2019. Nous avons ajouté une nouvelle tolérance au [Service d’intégrité](../../failover-clustering/health-service-overview.md) qui s’affichent lorsque l’espace de stockage est en cours de synchronisation.

Pour afficher cette tolérance dans PowerShell, exécutez:

``` PowerShell
Get-HealthFault
```

Cela est une erreur de nouveau dans Windows Server 2019 et s’affichent dans PowerShell, dans le rapport de validation du cluster et ailleurs s’appuie sur les erreurs d’intégrité. 

Pour obtenir une présentation plus approfondie, vous pouvez interroger la base de données de série de temps dans PowerShell comme suit:

```PowerShell
Get-ClusterNode | Get-ClusterPerf -ClusterNodeSeriesName ClusterNode.Storage.Degraded
```
Voici quelques exemples de sortie:

```
Object Description: ClusterNode Server1

Series                       Time                Value Unit
------                       ----                ----- ----
ClusterNode.Storage.Degraded 01/11/2019 16:26:48     214 GB
```

En particulier, Windows Admin Center utilise des défauts d’intégrité pour définir l’état et la couleur des nœuds de cluster. Par conséquent, cette tolérance nouvelle entraîne des nœuds de cluster à la transition à partir de rouge (vers le bas) au jaune (synchroniser) en vert (haut), au lieu d’accédant directement à partir de rouge, vert, sur le tableau de bord HCI.

![Image de la vue 2016 vs 2019 de resynchronisation»](media/understand-storage-resync/compare.png)

En affichant la progression de resynchronisation du stockage global, vous pouvez sait précisément la quantité de données est désynchronisé et indique si votre système est progression. Lorsque vous ouvrez Windows Admin Center et accédez au *tableau de bord*, vous verrez la nouvelle alerte comme suit:

![Image de l’alerte dans Windows Admin Center»](media/understand-storage-resync/alert.png)

L’alerte est utile pour vous informer lors de la resynchronisation se produit, afin que vous ne prenez accidentellement des serveurs supplémentaires vers le bas (ce qui pourrait entraîner plusieurs domaines d’erreur en être affectées, ce qui entraîne dans votre cluster va vers le bas). 

Si vous accédez à la page *serveurs* dans Windows Admin Center, cliquez sur *l’inventaire*et puis choisissez un serveur spécifique, vous pouvez obtenir une vue plus détaillée de l’apparence de cette resynchronisation du stockage sur chaque serveur. Si vous accédez à votre serveur et que vous observez le graphique de *stockage* , vous verrez la quantité de données qui doit être résolu dans une ligne avec le nombre exact *violet* juste au-dessus. Ce montant augmente lorsque le serveur est vers le bas (plus de données doit être resynchronisées) et diminuer progressivement lorsque le serveur revienne en ligne (données sont en cours de synchronisation). Lorsque la quantité de données qui doivent être réparation est égal à 0, votre système de stockage est terminé synchroniser - vous pouvez désormais libre de mettre un serveur vers le bas si vous avez besoin de. Une capture d’écran de cette expérience dans Windows Admin Center est indiqué ci-dessous:

![Image de la vue de serveur dans Windows Admin Center»](media/understand-storage-resync/server.png)

## Comment faire pour afficher la resynchronisation du stockage dans Windows Server 2016

Comme vous pouvez le constater, cette alerte est particulièrement utile pour l’obtention d’une vue holistique de ce qui se passe au niveau de la couche de stockage. Elle résume efficacement les informations que vous pouvez obtenir à partir de l’applet de commande Get-StorageJob, qui renvoie des informations sur les travaux de module de stockage longue, par exemple, une opération de réparation sur un espace de stockage. Vous trouverez ci-dessous un exemple:

```PowerShell
Get-StorageJob
```

Voici un exemple de sortie:

```
Name                  ElapsedTime           JobState              PercentComplete       IsBackgroundTask
----                  -----------           --------              ---------------       ----------------
Regeneration          00:01:19              Running               50                    True

```

Cette vue est beaucoup plus précise dans la mesure où les travaux de stockage répertoriés sont par volume, vous pouvez afficher la liste des tâches qui sont en cours d’exécution et vous pouvez suivre leur progression individuelle. Cette applet de commande fonctionne sur Windows Server 2016 et 2019.

## Voir aussi

- [Mise d'un serveur hors connexion pour la maintenance](maintain-servers.md)
- [Présentation des espaces de stockage direct](storage-spaces-direct-overview.md)