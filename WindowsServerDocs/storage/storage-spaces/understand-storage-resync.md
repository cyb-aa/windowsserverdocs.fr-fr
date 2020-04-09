---
title: Comprendre et voir la resynchronisation du stockage
description: Informations détaillées sur le moment où se produit la resynchronisation du stockage et son affichage dans Windows Server 2019.
ms.prod: windows-server
ms.author: adagashe
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 01/14/2019
ms.localizationpriority: medium
ms.openlocfilehash: 1209a3e08922a380ef46d3be6d28ce489b748f22
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80820932"
---
# <a name="understand-and-monitor-storage-resync"></a>Comprendre et contrôler la resynchronisation du stockage

>S’applique à : Windows Server 2019

Les alertes de resynchronisation de stockage constituent une nouvelle fonctionnalité de [espaces de stockage direct](storage-spaces-direct-overview.md) dans Windows Server 2019 qui permet aux service de contrôle d’intégrité de déclencher une erreur lors de la resynchronisation de votre stockage. L’alerte est utile pour vous avertir lorsque la resynchronisation se produit, afin que vous ne deviez pas mettre en panne un plus grand nombre de serveurs (ce qui peut entraîner l’affectation de plusieurs domaines d’erreur, entraînant la baisse de votre cluster). 

Cette rubrique fournit des informations générales et les étapes à suivre pour comprendre et voir la resynchronisation du stockage dans un cluster de basculement Windows Server avec espaces de stockage direct.

## <a name="understanding-resync"></a>Fonctionnement de la resynchronisation

Commençons par un exemple simple pour comprendre comment le stockage n’est plus synchronisé. N’oubliez pas qu’une solution de stockage distribué sans partage (lecteurs locaux uniquement) présente ce comportement. Comme vous le verrez ci-dessous, si un nœud de serveur tombe en panne, ses lecteurs ne sont pas mis à jour tant qu’il n’est pas remis en ligne. cela est vrai pour toute architecture hyper-convergée. 

Supposons que nous voulons stocker la chaîne « HELLO ». 

![ASCII de chaîne « Hello »](media/understand-storage-resync/hello.png)

Asssuming que nous disposons d’une résilience en miroir triple, nous avons trois copies de cette chaîne. Maintenant, si nous prenons le serveur #1 temporairement (pour maintenance), nous ne pouvons pas accéder au #1 de copie.

![Impossible d’accéder au #1 de copie](media/understand-storage-resync/copy1.png)

Supposons que nous mettons à jour notre chaîne de « HELLO » à « HELP ! » à ce stade.

![ASCII de chaîne "Help !"](media/understand-storage-resync/help.png)

Une fois la chaîne mise à jour, copiez #2 et #3 sera correctement mise à jour. Toutefois, la copie #1 toujours pas accessible car le #1 du serveur est momentanément indisponible (pour maintenance). 

![GIF d’écriture pour copier #2 et #2»](media/understand-storage-resync/write.gif)

À présent, nous avons une copie #1 qui contient des données qui ne sont pas synchronisées. Le système d’exploitation utilise le suivi granulaire des régions de modification pour effectuer le suivi des bits qui ne sont pas synchronisés. De cette façon, lorsque le serveur #1 revient en ligne, nous pouvons synchroniser les modifications en lisant les données à partir de la copie #2 ou #3 et en remplaçant les données dans la #1 de copie. L’avantage de cette approche est que nous n’avons besoin de copier que les données périmées, plutôt que de resynchroniser toutes les données du serveur #2 ou du serveur #3.

![GIF du remplacement pour copier #1»](media/understand-storage-resync/overwrite.gif)

Ainsi, cela explique comment les données sont désynchronisées. Mais à quoi cela ressemble-t-il à un niveau élevé ? Supposons que cet exemple dispose d’un cluster hyper-convergé à trois serveurs. Lorsque le #1 serveur est en cours de maintenance, vous le verrez comme étant en cours d’inversion. Lorsque vous effectuez une sauvegarde du serveur #1, celle-ci démarre la resynchronisation de tout son stockage à l’aide du suivi des régions de modification granulaires (voir ci-dessus). Une fois que les données sont de nouveau synchronisées, tous les serveurs sont affichés comme étant en cours.

![Image GIF de la vue d’administration de resynchronisation»](media/understand-storage-resync/admin.gif)

## <a name="how-to-monitor-storage-resync-in-windows-server-2019"></a>Comment surveiller la resynchronisation du stockage dans Windows Server 2019

Maintenant que vous comprenez le fonctionnement de la resynchronisation du stockage, voyons comment cela s’affiche dans Windows Server 2019. Nous avons ajouté une nouvelle erreur au [service de contrôle d’intégrité](../../failover-clustering/health-service-overview.md) qui s’affichera lors de la resynchronisation de votre stockage.

Pour afficher cette erreur dans PowerShell, exécutez :

``` PowerShell
Get-HealthFault
```

Il s’agit d’une nouvelle erreur dans Windows Server 2019, qui apparaîtra dans PowerShell, dans le rapport de validation de cluster, et n’importe où sur les erreurs d’intégrité. 

Pour obtenir une vue plus détaillée, vous pouvez interroger la base de données de série chronologique dans PowerShell comme suit :

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

En particulier, le centre d’administration Windows utilise des erreurs d’intégrité pour définir l’État et la couleur des nœuds de cluster. Par conséquent, cette nouvelle erreur entraînera la transition des nœuds de cluster du rouge (vers le haut) au jaune (resynchronisation) au vert (haut), au lieu de passer du rouge au vert, sur le tableau de bord HCI.

![Image de la vue de resynchronisation 2016 vs 2019](media/understand-storage-resync/compare.png)

En indiquant la progression globale de la resynchronisation du stockage, vous pouvez connaître avec précision la quantité de données qui n’est pas synchronisée et si votre système progresse en avance. Lorsque vous ouvrez le centre d’administration Windows et que vous accédez au *tableau de bord*, la nouvelle alerte s’affiche comme suit :

![Image de l’alerte dans le centre d’administration Windows»](media/understand-storage-resync/alert.png)

L’alerte est utile pour vous avertir lorsque la resynchronisation se produit, afin que vous ne deviez pas mettre en panne un plus grand nombre de serveurs (ce qui peut entraîner l’affectation de plusieurs domaines d’erreur, entraînant la baisse de votre cluster). 

Si vous accédez à la page *serveurs* dans le centre d’administration Windows, cliquez sur *inventaire*, puis choisissez un serveur spécifique. vous pouvez ainsi obtenir une vue plus détaillée de la façon dont cette resynchronisation de stockage est effectuée sur une base par serveur. Si vous accédez à votre serveur et que vous examinez le graphique de *stockage* , vous verrez la quantité de données qui doit être réparée sur une ligne *violette* avec le nombre exact juste au-dessus. Cette quantité augmente lorsque le serveur est défaillant (davantage de données doivent être resynchronisées) et diminue progressivement lorsque le serveur revient en ligne (les données sont synchronisées). Lorsque la quantité de données à réparer est 0, votre stockage est en cours de resynchronisation. vous êtes maintenant libre de mettre un serveur en veille si nécessaire. Une capture d’écran de cette expérience dans le centre d’administration Windows est illustrée ci-dessous :

![Image de la vue serveur dans le centre d’administration Windows»](media/understand-storage-resync/server.png)

## <a name="how-to-see-storage-resync-in-windows-server-2016"></a>Procédure de resynchronisation du stockage dans Windows Server 2016

Comme vous pouvez le voir, cette alerte est particulièrement utile pour obtenir une vue holistique de ce qui se passe au niveau de la couche de stockage. Elle résume efficacement les informations que vous pouvez obtenir à partir de l’applet de commande StorageJob, qui retourne des informations sur les tâches du module de stockage de longue durée, telles qu’une opération de réparation sur un espace de stockage. Un exemple est illustré ci-dessous :

```PowerShell
Get-StorageJob
```

Voici un exemple de sortie :

```
Name                  ElapsedTime           JobState              PercentComplete       IsBackgroundTask
----                  -----------           --------              ---------------       ----------------
Regeneration          00:01:19              Running               50                    True

```

Cette vue est beaucoup plus granulaire étant donné que les tâches de stockage répertoriées sont par volume, vous pouvez voir la liste des travaux en cours d’exécution, et vous pouvez suivre leur progression individuelle. Cette applet de commande fonctionne sur Windows Server 2016 et 2019.

## <a name="see-also"></a>Voir aussi

- [Mise d'un serveur hors connexion pour la maintenance](maintain-servers.md)
- [Présentation de espaces de stockage direct](storage-spaces-direct-overview.md)