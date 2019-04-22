---
title: Comprendre et voir la resynchronisation de stockage
description: Obtenir des informations détaillées sur la quand la resynchronisation de stockage se produit et comment visualiser dans Windows Server 2019.
keywords: Espaces de stockage Direct, la resynchronisation de stockage, resync, stockage, S2D
ms.prod: windows-server-threshold
ms.author: adagashe
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 01/14/2019
ms.localizationpriority: medium
ms.openlocfilehash: 81b1136a4b6a5cf8423a99e898b482a9b2849b5f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813460"
---
# <a name="understand-and-monitor-storage-resync"></a>Comprendre et contrôler la resynchronisation du stockage

>S’applique à : Windows Server 2019

Alertes de resynchronisation de stockage sont une nouveauté de [espaces de stockage Direct](storage-spaces-direct-overview.md) dans Windows Server 2019 qui permet au Service de contrôle d’intégrité lever une erreur lors de votre espace de stockage est en cours de synchronisation. L’alerte est utile à vous avertir lors de la resynchronisation se produit, afin que vous ne prenez pas accidentellement des serveurs plus bas (ce qui peut entraîner à plusieurs domaines d’erreur en être affectées, résultant dans votre cluster en panne). 

Cette rubrique fournit des informations et des étapes pour comprendre et voir la resynchronisation de stockage dans un cluster de basculement Windows Server avec des espaces de stockage Direct.

## <a name="understanding-resync"></a>Resynchronisation de la présentation

Commençons par un exemple simple de comprendre comment le stockage est désynchronisé. N’oubliez pas que tout partage (lecteurs locaux uniquement) solution de stockage distribué a ce comportement. Comme vous le verrez ci-dessous, si un nœud de serveur tombe en panne, ses lecteurs ne sont pas mis à jour jusqu'à ce qu’il revient en ligne - cela est vrai pour toutes les architectures hyperconvergé. 

Supposons que nous voulons stocker la chaîne « HELLO ». 

![ASCII de la chaîne « hello »](media/understand-storage-resync/hello.png)

Asssuming que nous avons une résilience en miroir triple, nous avons trois copies de cette chaîne. Maintenant, si nous prenons server #1 vers le bas temporairement (pour maintenance), nous ne pouvons pas accéder copie #1.

![Ne peut pas accéder à la copie #1](media/understand-storage-resync/copy1.png)

Supposons que nous mettons à jour notre chaîne à partir de « HELLO » à « HELP » ! pour l’instant.

![ASCII de la chaîne « help » !](media/understand-storage-resync/help.png)

Une fois que nous mettre à jour la chaîne, copie #2 et #3 sera correctement mis à jour. Toutefois, la copie #1 toujours pas accessible, car #1 est momentanément indisponible (pour la maintenance). 

![GIF d’écriture copier #2 et #2 »](media/understand-storage-resync/write.gif)

Maintenant, nous avons copie #1, ce qui a des données sont désynchronisées. Le système d’exploitation utilise des régions modifiées granulaire de suivi pour suivre les bits qui ne sont pas synchronisés. Ainsi, quand le serveur #1 revient en ligne, nous pouvons synchroniser les modifications en lisant les données à partir de la copie #2 ou #3 et de remplacer les données de copie #1. Les avantages de cette approche sont qu’il nous suffit de copier les données périmées, plutôt que de resynchronisation des toutes les données de serveur #2 ou #3.

![GIF d’écrasement pour copier #1 »](media/understand-storage-resync/overwrite.gif)

Par conséquent, ce qui explique comment les données sont désynchronisées. Mais ce que cela ressemble à un niveau élevé ? Supposons que nous avons un cluster hyperconvergé de trois serveurs. Lorsque le serveur #1 est en cours de maintenance, elle s’affiche comme étant indisponibles. Lorsque vous faites server #1 sauvegarder, il démarrera resynchronisation des partie de son stockage utilisant le suivi des régions modifiées granulaire (décrits ci-dessus). Une fois que les données sont tout retrouver la synchronisation, tous les serveurs seront affichera comme étant en cours.

![GIF de vue administrateur de resynchronisation »](media/understand-storage-resync/admin.gif)

## <a name="how-to-monitor-storage-resync-in-windows-server-2019"></a>Guide pratique pour surveiller la resynchronisation de stockage dans Windows Server 2019

Maintenant que vous comprenez le fonctionne de la resynchronisation de stockage, nous allons voir comment cela s’observe à Windows Server 2019. Nous avons ajouté une nouvelle erreur pour le [Service d’intégrité](../../failover-clustering/health-service-overview.md) qui s’affiche lors de votre espace de stockage est en cours de synchronisation.

Pour afficher cette erreur dans PowerShell, exécutez :

``` PowerShell
Get-HealthFault
```

Ceci est une nouvelle erreur dans Windows Server 2019 et apparaîtra dans PowerShell, dans le rapport de validation de cluster et nulle part ailleurs qui s’appuie sur les erreurs d’intégrité. 

Pour obtenir une vue plus approfondie, vous pouvez interroger la base de données de séries chronologiques dans PowerShell comme suit :

```PowerShell
Get-ClusterNode | Get-ClusterPerf -ClusterNodeSeriesName ClusterNode.Storage.Degraded
```
Voici quelques exemples de sortie :

```
Object Description: ClusterNode Server1

Series                       Time                Value Unit
------                       ----                ----- ----
ClusterNode.Storage.Degraded 01/11/2019 16:26:48     214 GB
```

En particulier, Windows Admin Center utilise les erreurs d’intégrité pour définir l’état et la couleur des nœuds de cluster. Par conséquent, cette nouvelle erreur entraîne la transition du rouge (vers le bas), jaune (en cours de synchronisation) vert (vers le haut), au lieu de passer directement à partir de rouge, vert, des nœuds de cluster sur le tableau de bord de HCL.

![Image de vue de vs 2016 2019 de resynchronisation »](media/understand-storage-resync/compare.png)

En affichant la progression globale de resynchronisation de stockage, vous saurez précisément la quantité de données est désynchronisée, et si votre système est progresse vers l’avant. Lorsque vous ouvrez Windows Admin Center et accédez à la *tableau de bord*, vous verrez la nouvelle alerte comme suit :

![Image de l’alerte dans Windows Admin Center »](media/understand-storage-resync/alert.png)

L’alerte est utile à vous avertir lors de la resynchronisation se produit, afin que vous ne prenez pas accidentellement des serveurs plus bas (ce qui peut entraîner à plusieurs domaines d’erreur en être affectées, résultant dans votre cluster en panne). 

Si vous accédez à la *serveurs* page Windows Admin Center, cliquez sur *inventaire*, puis choisissez un serveur spécifique, vous pouvez obtenir une vue plus détaillée de l’aspect de la resynchronisation de stockage sur un serveur par serveur. Si vous accédez à votre serveur et examinez le *stockage* graphique, vous verrez la quantité de données qui devant être réparé dans un *violet* ligne avec le nombre exact juste au-dessus. Ce montant sera augmentent lorsque le serveur est arrêté (plus de données doit être resynchronisées) et à diminuer progressivement lorsque le serveur revient en ligne (données en cours de synchronisation). Lorsque la quantité de données nécessitant une réparation est 0, il n’est effectuée en cours de synchronisation - vous êtes maintenant libre d’arrêter un serveur si vous avez besoin. Une capture d’écran de cette expérience dans Windows Admin Center est indiquée ci-dessous :

![Image de vue de serveur dans Windows Admin Center »](media/understand-storage-resync/server.png)

## <a name="how-to-see-storage-resync-in-windows-server-2016"></a>Guide pratique pour afficher la resynchronisation de stockage dans Windows Server 2016

Comme vous pouvez le voir, cette alerte est particulièrement utile lors de l’obtention d’une vue holistique de ce qui se passe au niveau de la couche de stockage. Il récapitule efficacement les informations que vous pouvez obtenir à partir de l’applet de commande Get-StorageJob, qui retourne des informations sur les tâches du module de stockage longues, tel qu’une opération de réparation sur un espace de stockage. Voici un exemple :

```PowerShell
Get-StorageJob
```

Voici un exemple de sortie :

```
Name                  ElapsedTime           JobState              PercentComplete       IsBackgroundTask
----                  -----------           --------              ---------------       ----------------
Regeneration          00:01:19              Running               50                    True

```

Cette vue est beaucoup plus granulaire dans la mesure où les tâches de stockage répertoriés sont par volume, vous pouvez voir la liste des tâches qui sont en cours d’exécution, et vous pouvez suivre leur progression individuelle. Cette applet de commande fonctionne sur Windows Server 2016 et 2019.

## <a name="see-also"></a>Voir aussi

- [Mettre un serveur hors connexion pour la maintenance](maintain-servers.md)
- [Vue d’ensemble Direct des espaces de stockage](storage-spaces-direct-overview.md)