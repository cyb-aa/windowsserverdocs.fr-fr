---
title: Résilience imbriquée pour les espaces de stockage Direct
ms.prod: windows-server-threshold
ms.author: jgerend
ms.manager: dansimp
ms.technology: storagespaces
ms.topic: article
author: cosmosdarwin
ms.date: 11/06/2018
ms.openlocfilehash: 206d5d19ec55774f9055e7265d4c87d276c60590
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879570"
---
# <a name="nested-resiliency-for-storage-spaces-direct"></a>Résilience imbriquée pour les espaces de stockage Direct

> S’applique à : Windows Server 2019

La résilience imbriquée est une nouveauté de [espaces de stockage Direct](storage-spaces-direct-overview.md) dans Windows Server 2019 qui permet à un cluster de deux serveurs pour résister à plusieurs défaillances matérielles en même temps sans perte de disponibilité du stockage, pour permettre aux utilisateurs, applications, et les machines virtuelles continuent à fonctionner sans interruption. Cette rubrique explique comment il fonctionne, fournit des instructions détaillées pour la prise en main et des réponses les plus fréquemment posées.

## <a name="prerequisites"></a>Prérequis

### <a name="green-checkmark-iconmedianested-resiliencysupportedpng-consider-nested-resiliency-if"></a>![Icône de coche verte.](media/nested-resiliency/supported.png) Prenez en compte la résilience imbriquée si :

- Votre cluster exécute Windows Server 2019 ; et
- Votre cluster a exactement 2 nœuds de serveur

### <a name="red-x-iconmedianested-resiliencyunsupportedpng-you-cant-use-nested-resiliency-if"></a>![Icône X rouge.](media/nested-resiliency/unsupported.png) Vous ne pouvez pas utiliser la résilience imbriquée si :

- Votre cluster exécute Windows Server 2016 ; ou
- Votre cluster a au moins 3 nœuds de serveur

## <a name="why-nested-resiliency"></a>Pourquoi la résilience imbriquée

Volumes qui utilisent la résilience imbriquée peuvent **reste en ligne et accessible même si plusieurs défaillances matérielles se produisent en même temps**, contrairement à classic [mise en miroir bidirectionnelle](storage-spaces-fault-tolerance.md) la résilience. Par exemple, si deux lecteurs échouent en même temps, ou si un serveur tombe en panne et d’échec d’un lecteur, les volumes qui utilisent la résilience imbriquée restent en ligne et accessible. Pour l’infrastructure Hyper-convergée, cela augmente le temps d’activité pour les applications et les machines virtuelles ; pour les charges de serveur de fichiers, cela signifie que les utilisateurs bénéficient d’un accès ininterrompu à leurs fichiers.

![Disponibilité du stockage](media/nested-resiliency/storage-availability.png)

L’inconvénient est que la résilience imbriquée a **diminuer l’efficacité de capacité à la mise en miroir bidirectionnelle classic**, ce qui signifie que vous obtenez légèrement moins d’espace utilisable. Pour plus d’informations, consultez le [l’efficacité de capacité](#capacity-efficiency) section ci-dessous.

## <a name="how-it-works"></a>Fonctionnement

### <a name="inspiration-raid-51"></a>Inspiration : RAID 5 + 1

RAID 5 + 1 est un formulaire établi de résilience de stockage distribué qui fournit des informations générales utiles pour assurer la résilience imbriquée de présentation. Avec RAID 5 + 1, au sein de chaque serveur, la résilience locale est fourni par RAID-5, ou *unique parité*, pour vous protéger contre la perte de n’importe quel lecteur unique. Puis, plus la résilience est fournie par RAID-1, ou *mise en miroir bidirectionnelle*, entre les deux serveurs pour vous protéger contre la perte de des serveurs.

![RAID 5 + 1](media/nested-resiliency/raid-51.png)

### <a name="two-new-resiliency-options"></a>Deux nouvelles options de résilience

Espaces de stockage Direct dans Windows Server 2019 offre deux nouvelles options de résilience implémentées dans le logiciel, sans nécessiter de matériel RAID spécialisé :

- **Miroir double imbriquée.** Au sein de chaque serveur, la résilience locale est fournie par la mise en miroir bidirectionnelle, et puis la résilience supplémentaire est fournie par la mise en miroir bidirectionnelle entre les deux serveurs. Il est essentiellement un miroir à quatre directions, avec deux copies de chaque serveur. Mise en miroir bidirectionnelle imbriquée fournit des performances optimales : écritures accédez à toutes les copies et les lectures proviennent d’une copie.

  ![Miroir double imbriquée](media/nested-resiliency/nested-two-way-mirror.png)

- **Parité d’accélérée de miroir imbriquée.** Combiner imbriqué de mise en miroir bidirectionnelle, à partir de ci-dessus, avec parité imbriquée. Au sein de chaque serveur, la résilience locale pour la plupart des données est fournie par unique [au niveau du bit parité arithmétique](storage-spaces-fault-tolerance.md#parity), à l’exception de la nouvelle écriture récente qui utilisent la mise en miroir bidirectionnelle. Puis, plus la résilience pour toutes les données est fournie par la mise en miroir bidirectionnelle entre les serveurs. Pour plus d’informations sur le fonctionnement de parité comment accélérée en miroir, consultez [parité avec accélération miroir](https://docs.microsoft.com/windows-server/storage/refs/mirror-accelerated-parity).

  ![Parité d’accélérée de miroir imbriquée](media/nested-resiliency/nested-mirror-accelerated-parity.png)

### <a name="capacity-efficiency"></a>Efficacité de la capacité

L’efficacité de la capacité est le ratio d’espace utilisable à [encombrement de volume](plan-volumes.md#choosing-the-size-of-volumes). Il décrit la surcharge de capacité attribuable à la résilience et dépend de l’option de la résilience que vous choisissez. À titre d’exemple, stockage de données sans la résilience est 100 % de leur capacité efficace (1 To de données occupe 1 To de capacité de stockage physique), lors de la mise en miroir bidirectionnelle est efficace à 50 % (1 To de données occupe 2 To de capacité de stockage physique).

- **Miroir double imbriquée** écrit quatre copies de tous les éléments, ce qui signifie que stocker de 1 To de données, vous avez besoin de 4 To de capacité de stockage physique. Bien que sa simplicité est attrayante, l’efficacité de capacité du miroir bidirectionnel imbriquée de 25 % est le plus bas d’une option de résilience dans les espaces de stockage Direct.

- **Imbriqué parité avec accélération miroir** réalise l’efficacité de capacité supérieure, environ 35 à 40 %, qui dépend de deux facteurs : le nombre de capacité de lecteurs dans chaque serveur et la combinaison de miroir et parité que vous spécifiez pour le volume. Ce tableau fournit une liste de choix pour les configurations courantes :

  | Capacité de disques par serveur | mise en miroir de 10 % | mise en miroir de 20 % | mise en miroir de 30 % |
  |----------------------------|------------|------------|------------|
  | 4                          | 35.7%      | 34.1%      | 32.6%      |
  | 5                          | 37.7%      | 35.7%      | 33.9%      |
  | 6                          | 39.1%      | 36.8%      | 34.7%      |
  | 7+                         | 40.0%      | 37.5%      | 35.3%      |

  > [!NOTE]
  > **Si vous êtes curieux, Voici un exemple de calcul complet.** Supposons que nous avons six disques de capacité dans chacune des deux serveurs, et nous voulons créer un volume de 100 Go composé de 10 Go de mise en miroir et 90 Go de parité. Miroir double de serveur local est 50,0 efficace %, ce qui signifie que les 10 Go de données en miroir prend 20 Go pour stocker sur chaque serveur. Mise en miroir pour les deux serveurs, son encombrement total est de 40 Go. Parité unique du serveur local, est dans ce cas, 5/6 = 83.3 % efficace, ce qui signifie que les 90 Go de données de parité prend 108 Go pour stocker sur chaque serveur. Mise en miroir pour les deux serveurs, son encombrement total est 216 Go. L’encombrement total est donc [(10 Go/50,0 %) + (90 Go / 83.3 %)] × 2 = 256 Go, % 39.1 globale l’efficacité de capacité.

Notez que l’efficacité de la capacité de classique (environ 50 %) de mise en miroir double et imbriquées parité accélérée en miroir (jusqu'à 40 %) ne sont pas très différents. Selon vos besoins, l’efficacité de capacité légèrement inférieure peut être également intéressant l’augmentation significative de la disponibilité du stockage. Vous choisissez la résilience par volume, donc vous pouvez combiner des volumes de résilience imbriquées et des volumes en miroir bidirectionnel classique au sein du même cluster.

![Tradeoff](media/nested-resiliency/tradeoff.png)

## <a name="usage-in-powershell"></a>Utilisation dans PowerShell

Vous pouvez utiliser les applets de commande de stockage familiers dans PowerShell pour créer des volumes avec une résilience imbriquée.

### <a name="step-1-create-storage-tier-templates"></a>Étape 1 : Créer des modèles de couche de stockage

Tout d’abord, créer des modèles de niveau de stockage à l’aide de la `New-StorageTier` applet de commande. Vous devez uniquement effectuer cette opération fois, et ensuite chaque nouveau volume que vous créez permettre référencer ces modèles. Spécifiez le `-MediaType` de vos lecteurs de capacité et, éventuellement, le `-FriendlyName` de votre choix. Ne modifiez pas les autres paramètres.

Si vos lecteurs de capacité sont des lecteurs de disque dur (HDD), lancez PowerShell en tant qu’administrateur et exécutez :

```PowerShell 
# For mirror
New-StorageTier -StoragePoolFriendlyName S2D* -FriendlyName NestedMirror -ResiliencySettingName Mirror -MediaType HDD -NumberOfDataCopies 4

# For parity
New-StorageTier -StoragePoolFriendlyName S2D* -FriendlyName NestedParity -ResiliencySettingName Parity -MediaType HDD -NumberOfDataCopies 2 -PhysicalDiskRedundancy 1 -NumberOfGroups 1 -FaultDomainAwareness StorageScaleUnit -ColumnIsolation PhysicalDisk 
``` 

Si vos lecteurs de capacité sont des disques SSD (SSD), définissez la `-MediaType` à `SSD` à la place. Ne modifiez pas les autres paramètres.

> [!TIP]
> Vérifiez les niveaux créés avec succès avec `Get-StorageTier`.

### <a name="step-2-create-volumes"></a>Étape 2 : Créer des volumes

Ensuite, créer des volumes à l’aide de la `New-Volume` applet de commande.

#### <a name="nested-two-way-mirror"></a>Miroir double imbriquée

Pour utiliser un miroir double imbriquée, référencer la `NestedMirror` couche de modèle et spécifiez la taille. Exemple :

```PowerShell
New-Volume -StoragePoolFriendlyName S2D* -FriendlyName Volume01 -StorageTierFriendlyNames NestedMirror -StorageTierSizes 500GB
```

#### <a name="nested-mirror-accelerated-parity"></a>Parité d’accélérée de miroir imbriquée

Pour utiliser une parité avec accélération miroir imbriquée, faire référence à la fois le `NestedMirror` et `NestedParity` niveau des modèles et de spécifier deux tailles, un pour chaque partie du volume (mise en miroir tout d’abord, parité ensuite). Par exemple, pour créer un volume de 500 Go est en miroir bidirectionnel imbriquée à 20 % et 80 % imbriqués parité, exécutez :

```PowerShell
New-Volume -StoragePoolFriendlyName S2D* -FriendlyName Volume02 -StorageTierFriendlyNames NestedMirror, NestedParity -StorageTierSizes 100GB, 400GB
```

### <a name="step-3-continue-in-windows-admin-center"></a>Étape 3 : Aller plus loin dans Windows Admin Center

Les volumes qui utilisent la résilience imbriquée s’affichent dans [Windows Admin Center](https://docs.microsoft.com/windows-server/manage/windows-admin-center/understand/windows-admin-center) avec un étiquetage clair, comme dans la capture d’écran ci-dessous. Une fois qu’elles sont créées, vous pouvez gérer et surveiller à l’aide de Windows Admin Center comme tout autre volume dans les espaces de stockage Direct.

![](media/nested-resiliency/windows-admin-center.png)

### <a name="optional-extend-to-cache-drives"></a>Facultatif : Étendre aux lecteurs de cache

Avec ses paramètres par défaut, la résilience imbriquée protège contre la perte de plusieurs lecteurs de capacité en même temps, ou un serveur et un seul lecteur de capacité en même temps. Pour étendre cette protection à [mettre en cache les lecteurs](understand-the-cache.md) a un facteur supplémentaire : étant donné que les lecteurs de cache offrent souvent en lecture *et d’écriture* mise en cache pour *plusieurs* de capacité , la seule façon de garantir vous tolérez la perte d’un lecteur de cache lorsque l’autre serveur est arrêté consiste à simplement ne pas mettre en cache les écritures, mais ayant un impact sur les performances.

Pour ce scénario, espaces de stockage Direct permet de désactiver automatiquement le cache d’écriture avec un seul serveur dans un cluster de deux serveurs vers le bas, puis réactivez cache d’écriture une fois le serveur de sauvegarde. Pour permettre les redémarrages de routine sans affecter les performances, écrivez la mise en cache n’est pas désactivé jusqu'à ce que le serveur a été arrêté pendant 30 minutes.

Pour définir ce comportement (facultatif), lancez PowerShell en tant qu’administrateur et exécutez :

```PowerShell
Get-StorageSubSystem Cluster* | Set-StorageHealthSetting -Name "System.Storage.NestedResiliency.DisableWriteCacheOnNodeDown.Enabled" -Value "True"
```

Une fois la valeur est `True`, le comportement de cache est :

| Situation                       | Comportement de cache                           | Tolérez la perte de lecteur de cache ? |
|---------------------------------|------------------------------------------|--------------------------------|
| Les deux serveurs soient configurés                 | Cache lit et écrit des performances optimales. | Oui                            |
| Serveur en panne, les 30 premières minutes   | Cache lit et écrit des performances optimales. | Non (temporairement)               |
| Après les 30 premières minutes          | Cache lit uniquement, les performances affectées   | Oui                            |

## <a name="frequently-asked-questions"></a>Forum Aux Questions

### <a name="can-i-convert-an-existing-volume-between-two-way-mirror-and-nested-resiliency"></a>Puis-je convertir un volume existant entre les miroirs doubles et de résilience imbriquée ?

Non, les volumes ne peut pas être convertis en entre les types de résilience. Pour les nouveaux déploiements sur Windows Server 2019, décider avance quel type de résilience meilleures adaptée à vos besoins. Si vous mettez à niveau à partir de Windows Server 2016, vous pouvez créer de nouveaux volumes avec une résilience imbriquée, migrer vos données, puis supprimez les anciens volumes.

### <a name="can-i-use-nested-resiliency-with-multiple-types-of-capacity-drives"></a>Puis-je utiliser la résilience imbriquée avec plusieurs types de lecteurs de capacité ?

Oui, il suffit de spécifier le `-MediaType` de chaque niveau en conséquence pendant [étape 1](#step-1-create-storage-tier-templates) ci-dessus. Par exemple, avec NVMe, SSD et HDD dans le même cluster, le NVMe fournit cache tandis que les deux derniers fournissent une capacité : définir le `NestedMirror` niveau `-MediaType SSD` et le `NestedParity` niveau pour vous `-MediaType HDD`. Dans ce cas, notez que l’efficacité de capacité de parité varie selon le nombre de lecteurs de disque dur uniquement, et que vous avez besoin d’au moins 4 d'entre eux par serveur.

### <a name="can-i-use-nested-resiliency-with-3-or-more-servers"></a>Puis-je utiliser la résilience imbriquée au moins 3 serveurs ?

Non, utiliser uniquement la résilience imbriquée si votre cluster a exactement 2 serveurs.

### <a name="how-many-drives-do-i-need-to-use-nested-resiliency"></a>Le nombre de lecteurs que je dois utiliser la résilience imbriquée ?

Le nombre minimal de disques requis pour les espaces de stockage Direct est de 4 disques de capacité par nœud de serveur, ainsi que les lecteurs de cache de 2 par nœud de serveur (le cas échéant). Cela reste inchangé à partir de Windows Server 2016. Il n’existe aucune exigence supplémentaire pour garantir la résilience imbriquée, et la recommandation pour réserver de la capacité est inchangée trop.

### <a name="does-nested-resiliency-change-how-drive-replacement-works"></a>Résilience imbriquée modifie le fonctionne de remplacement d’un lecteur ?

Non.

### <a name="does-nested-resiliency-change-how-server-node-replacement-works"></a>La résilience imbriquée modifie-t-il le fonctionnement de remplacement de nœud de serveur ?

Non. Pour remplacer un nœud de serveur et de ses lecteurs, suivez cet ordre :

1. Mettre hors service les lecteurs dans le serveur sortant
2. Ajouter le nouveau serveur, avec ses lecteurs, au cluster
3. Le pool de stockage sera rééquilibrer
4. Supprimer le serveur sortant et ses lecteurs

Pour plus d’informations, consultez le [supprimer des serveurs](remove-servers.md) rubrique.

## <a name="see-also"></a>Voir aussi

- [Vue d’ensemble Direct des espaces de stockage](storage-spaces-direct-overview.md)
- [Comprendre la tolérance de pannes dans les espaces de stockage Direct](storage-spaces-fault-tolerance.md)
- [Planifier les volumes dans les espaces de stockage Direct](plan-volumes.md)
- [Créer des volumes dans les espaces de stockage Direct](create-volumes.md)
