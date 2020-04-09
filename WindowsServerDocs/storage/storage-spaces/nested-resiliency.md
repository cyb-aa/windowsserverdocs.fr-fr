---
title: Résilience imbriquée pour espaces de stockage direct
ms.prod: windows-server
ms.author: jgerend
manager: dansimp
ms.technology: storagespaces
ms.topic: article
author: cosmosdarwin
ms.date: 03/15/2019
ms.openlocfilehash: ac4edccf0c1f8882dd2544b2544c3d8555bbc716
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857342"
---
# <a name="nested-resiliency-for-storage-spaces-direct"></a>Résilience imbriquée pour espaces de stockage direct

> S’applique à : Windows Server 2019

La résilience imbriquée est une nouvelle fonctionnalité de [espaces de stockage direct](storage-spaces-direct-overview.md) dans Windows Server 2019 qui permet à un cluster à deux serveurs de résister à plusieurs défaillances matérielles en même temps sans perte de disponibilité du stockage, de sorte que les utilisateurs, les applications et les machines virtuelles continuent de s’exécuter sans interruption. Cette rubrique explique comment elle fonctionne, fournit des instructions pas à pas pour commencer et répond aux questions les plus fréquentes.

## <a name="prerequisites"></a>Composants requis

### <a name="green-checkmark-icon-consider-nested-resiliency-if"></a>![Icône de coche verte.](media/nested-resiliency/supported.png) Considérez la résilience imbriquée dans les cas suivants :

- Votre cluster exécute Windows Server 2019 ; les
- Votre cluster contient exactement 2 nœuds de serveur

### <a name="red-x-icon-you-cant-use-nested-resiliency-if"></a>![Icône X rouge.](media/nested-resiliency/unsupported.png) Vous ne pouvez pas utiliser la résilience imbriquée dans les cas suivants :

- Votre cluster exécute Windows Server 2016 ; ni
- Votre cluster a au moins 3 nœuds de serveur

## <a name="why-nested-resiliency"></a>Pourquoi la résilience imbriquée

Les volumes qui utilisent la résilience imbriquée peuvent **rester en ligne et accessibles même si plusieurs défaillances matérielles se produisent en même temps, contrairement à la**résilience de [mise en miroir bidirectionnelle](storage-spaces-fault-tolerance.md) classique. Par exemple, si deux lecteurs échouent en même temps, ou si un serveur tombe en panne et qu’un lecteur tombe en panne, les volumes qui utilisent la résilience imbriquée restent en ligne et accessibles. Pour l’infrastructure hyper-convergée, cela augmente le temps d’activité des applications et des machines virtuelles. pour les charges de travail de serveur de fichiers, cela signifie que les utilisateurs bénéficient d’un accès ininterrompu à leurs fichiers.

![Disponibilité du stockage](media/nested-resiliency/storage-availability.png)

Le compromis est que la résilience imbriquée a une **efficacité de capacité inférieure à la mise en miroir bidirectionnelle classique**, ce qui signifie que vous disposez d’un espace moins utilisable. Pour plus d’informations, consultez la section efficacité de la [capacité](#capacity-efficiency) ci-dessous.

## <a name="how-it-works"></a>Fonctionnement

### <a name="inspiration-raid-51"></a>Inspiration : RAID 5 + 1

RAID 5 + 1 est une forme établie de résilience du stockage distribué qui fournit un arrière-plan utile pour comprendre la résilience imbriquée. Dans RAID 5 + 1, au sein de chaque serveur, la résilience locale est assurée par RAID-5, ou par *parité unique*, afin de vous protéger contre la perte d’un seul lecteur. Une résilience supplémentaire est ensuite assurée par RAID-1 ou la *mise en miroir bidirectionnelle*entre les deux serveurs pour vous protéger contre la perte de l’un ou l’autre serveur.

![RAID 5 + 1](media/nested-resiliency/raid-51.png)

### <a name="two-new-resiliency-options"></a>Deux nouvelles options de résilience

Espaces de stockage direct dans Windows Server 2019 offre deux nouvelles options de résilience mises en œuvre dans les logiciels, sans nécessiter de matériel RAID spécialisé :

- **Miroir bidirectionnel imbriqué.** Au sein de chaque serveur, la résilience locale est fournie par la mise en miroir bidirectionnelle, puis la résilience supplémentaire est assurée par la mise en miroir bidirectionnelle entre les deux serveurs. Il s’agit essentiellement d’un miroir à quatre voies, avec deux copies sur chaque serveur. La mise en miroir bidirectionnelle imbriquée offre des performances sans compromis : les écritures vont à toutes les copies et les lectures proviennent de n’importe quelle copie.

  ![Miroir bidirectionnel imbriqué](media/nested-resiliency/nested-two-way-mirror.png)

- **Parité à accélération miroir imbriquée.** Combinez la mise en miroir bidirectionnelle imbriquée, ci-dessus, avec la parité imbriquée. Au sein de chaque serveur, la résilience locale pour la plupart des données est fournie par une seule [opération arithmétique de parité au niveau du bit](storage-spaces-fault-tolerance.md#parity), à l’exception des nouvelles écritures récentes qui utilisent la mise en miroir bidirectionnelle. Ensuite, la résilience supplémentaire pour toutes les données est assurée par la mise en miroir bidirectionnelle entre les serveurs. Pour plus d’informations sur le fonctionnement de la parité avec accélération miroir, consultez [Parity-Accelerated Parity](https://docs.microsoft.com/windows-server/storage/refs/mirror-accelerated-parity).

  ![Parité imbriquée avec accélération miroir](media/nested-resiliency/nested-mirror-accelerated-parity.png)

### <a name="capacity-efficiency"></a>Efficacité de la capacité

L’efficacité de la capacité est le rapport entre l’espace utilisable et l' [encombrement du volume](plan-volumes.md#choosing-the-size-of-volumes). Il décrit la surcharge de capacité imputable à la résilience, et dépend de l’option de résilience que vous choisissez. En guise d’exemple simple, le stockage de données sans résilience est 100% d’efficacité de la capacité (1 to de données occupent 1 to de capacité de stockage physique), tandis que la mise en miroir bidirectionnelle est de 50% efficace (1 to de données occupent 2 to de capacité de stockage physique).

- **Le miroir bidirectionnel imbriqué** écrit quatre copies de tout, ce qui signifie que vous avez besoin de 4 to de capacité de stockage physique pour stocker 1 to de données. Bien que sa simplicité soit intéressante, l’efficacité de la capacité d’un miroir bidirectionnel imbriqué de 25% est l’option la plus faible en termes de résilience dans espaces de stockage direct.

- La **parité à accélération miroir imbriquée** augmente l’efficacité de la capacité, environ 35%-40%, ce qui dépend de deux facteurs : le nombre de lecteurs de capacité dans chaque serveur et la combinaison de miroir et de parité que vous spécifiez pour le volume. Ce tableau fournit une recherche pour les configurations courantes :

  | Lecteurs de capacité par serveur | miroir de 10% | mise en miroir à 20% | miroir à 30% |
  |----------------------------|------------|------------|------------|
  | 4                          | 35,7%      | 34,1%      | 32,6%      |
  | 5                          | 37,7%      | 35,7%      | 33,9%      |
  | 6                          | 39,1%      | 36,8%      | 34,7%      |
  | plus de 7                         | 40,0%      | 37,5%      | 35,3%      |

  > [!NOTE]
  > **Si vous êtes curieux, voici un exemple de la mathématique complète.** Supposons que nous ayons six lecteurs de capacité dans chacun des deux serveurs et que nous voulons créer un volume de 1 100 Go composé de 10 Go de miroir et de 90 Go de parité. Le miroir bidirectionnel local du serveur est 50,0% efficace, ce qui signifie que les 10 Go de données miroir prennent 20 Go de stockage sur chaque serveur. Mis en miroir sur les deux serveurs, son encombrement total est de 40 Go. Dans ce cas, le serveur local à parité unique est 5/6 = 83,3% efficace, ce qui signifie que 90 Go de données de parité prend 108 Go pour le stockage sur chaque serveur. Mis en miroir sur les deux serveurs, son encombrement total est de 216 Go. L’encombrement total est donc de [(10 Go/50,0%) + (90 Go/83,3%)] × 2 = 256 Go, pour une efficacité de capacité globale de 39,1%.

Notez que l’efficacité de la capacité de la mise en miroir bidirectionnelle classique (environ 50%) et la parité à accélération miroir imbriquée (jusqu’à 40%) ne sont pas très différents. En fonction de vos besoins, l’efficacité de la capacité légèrement inférieure peut être une augmentation significative de la disponibilité du stockage. Vous choisissez la résilience par volume, de sorte que vous pouvez mélanger des volumes de résilience imbriqués et des volumes miroir bidirectionnels classiques au sein du même cluster.

![Tradeoff](media/nested-resiliency/tradeoff.png)

## <a name="usage-in-powershell"></a>Utilisation dans PowerShell

Vous pouvez utiliser des applets de commande de stockage familières dans PowerShell pour créer des volumes avec une résilience imbriquée.

### <a name="step-1-create-storage-tier-templates"></a>Étape 1 : créer des modèles de niveau de stockage

Tout d’abord, créez de nouveaux modèles de niveau de stockage à l’aide de l’applet de commande `New-StorageTier`. Vous ne devez effectuer cette opération qu’une seule fois, puis chaque nouveau volume que vous créez peut référencer ces modèles. Spécifiez le `-MediaType` de vos lecteurs de capacité et, éventuellement, le `-FriendlyName` de votre choix. Ne modifiez pas les autres paramètres.

Si vos lecteurs de capacité sont des lecteurs de disque dur (HDD), lancez PowerShell en tant qu’administrateur et exécutez :

```PowerShell 
# For mirror
New-StorageTier -StoragePoolFriendlyName S2D* -FriendlyName NestedMirror -ResiliencySettingName Mirror -MediaType HDD -NumberOfDataCopies 4

# For parity
New-StorageTier -StoragePoolFriendlyName S2D* -FriendlyName NestedParity -ResiliencySettingName Parity -MediaType HDD -NumberOfDataCopies 2 -PhysicalDiskRedundancy 1 -NumberOfGroups 1 -FaultDomainAwareness StorageScaleUnit -ColumnIsolation PhysicalDisk 
``` 

Si vos lecteurs de capacité sont des disques SSD (Solid-State Drives), définissez le `-MediaType` sur `SSD` à la place. Ne modifiez pas les autres paramètres.

> [!TIP]
> Vérifiez que les niveaux créés avec `Get-StorageTier`sont corrects.

### <a name="step-2-create-volumes"></a>Étape 2 : créer des volumes

Créez ensuite de nouveaux volumes à l’aide de l’applet de commande `New-Volume`.

#### <a name="nested-two-way-mirror"></a>Miroir bidirectionnel imbriqué

Pour utiliser le miroir bidirectionnel imbriqué, référencez le modèle de niveau `NestedMirror` et spécifiez la taille. Par exemple :

```PowerShell
New-Volume -StoragePoolFriendlyName S2D* -FriendlyName Volume01 -StorageTierFriendlyNames NestedMirror -StorageTierSizes 500GB
```

#### <a name="nested-mirror-accelerated-parity"></a>Parité imbriquée avec accélération miroir

Pour utiliser la parité à accélération miroir imbriquée, référencez les modèles de niveau `NestedMirror` et `NestedParity` et spécifiez deux tailles, une pour chaque partie du volume (miroir en premier, parité seconde). Par exemple, pour créer un volume de 1 500 Go contenant 20% de miroirs bidirectionnels imbriqués et une parité imbriquée de 80%, exécutez :

```PowerShell
New-Volume -StoragePoolFriendlyName S2D* -FriendlyName Volume02 -StorageTierFriendlyNames NestedMirror, NestedParity -StorageTierSizes 100GB, 400GB
```

### <a name="step-3-continue-in-windows-admin-center"></a>Étape 3 : continuer dans le centre d’administration Windows

Les volumes qui utilisent la résilience imbriquée s’affichent dans le [Centre d’administration Windows](https://docs.microsoft.com/windows-server/manage/windows-admin-center/understand/windows-admin-center) avec l’étiquetage clair, comme dans la capture d’écran ci-dessous. Une fois qu’ils sont créés, vous pouvez les gérer et les surveiller à l’aide du centre d’administration Windows, comme n’importe quel autre volume dans espaces de stockage direct.

![](media/nested-resiliency/windows-admin-center.png)

### <a name="optional-extend-to-cache-drives"></a>Facultatif : étendre aux lecteurs de cache

Avec ses paramètres par défaut, la résilience imbriquée protège contre la perte de plusieurs lecteurs de capacité en même temps, ou un serveur et un lecteur de capacité en même temps. Pour étendre cette protection aux [lecteurs de cache](understand-the-cache.md) , il est également important de tenir compte du fait que les lecteurs de cache fournissent souvent une mise en cache en lecture *et en écriture* pour *plusieurs* lecteurs de capacité, le seul moyen de garantir la perte d’un lecteur de cache lorsque l’autre serveur est en panne consiste simplement à ne pas mettre en cache les écritures, mais cela a un impact sur les performances.

Pour traiter ce scénario, espaces de stockage direct offre la possibilité de désactiver automatiquement la mise en cache d’écriture lorsqu’un serveur d’un cluster à deux serveurs est défaillant, puis de réactiver la mise en cache d’écriture une fois que le serveur est sauvegardé. Pour autoriser les redémarrages de routine sans impact sur les performances, la mise en cache d’écriture n’est pas désactivée tant que le serveur n’a pas été arrêté pendant 30 minutes. Une fois que la mise en cache d’écriture est désactivée, le contenu du cache d’écriture est écrit sur les unités de capacité. Après cela, le serveur peut tolérer l’échec d’un périphérique de cache sur le serveur en ligne, bien que les lectures à partir du cache puissent être retardées ou échouer en cas de défaillance d’un périphérique de cache.

Pour définir ce comportement (facultatif), lancez PowerShell en tant qu’administrateur et exécutez :

```PowerShell
Get-StorageSubSystem Cluster* | Set-StorageHealthSetting -Name "System.Storage.NestedResiliency.DisableWriteCacheOnNodeDown.Enabled" -Value "True"
```

Une fois défini sur **true**, le comportement du cache est le suivant :

| Situation                       | Comportement du cache                           | La perte de l’unité de cache peut-elle être tolérée ? |
|---------------------------------|------------------------------------------|--------------------------------|
| Les deux serveurs                 | Lectures et écritures dans le cache, performances complètes | Oui                            |
| Serveur en baisse, 30 premières minutes   | Lectures et écritures dans le cache, performances complètes | Non (temporaire)               |
| Après les 30 premières minutes          | Lectures du cache uniquement, impact sur les performances   | Oui (une fois le cache écrit sur les lecteurs de capacité)                           |

## <a name="frequently-asked-questions"></a>Forum aux questions

### <a name="can-i-convert-an-existing-volume-between-two-way-mirror-and-nested-resiliency"></a>Puis-je convertir un volume existant entre un miroir bidirectionnel et une résilience imbriquée ?

Non, les volumes ne peuvent pas être convertis entre les types de résilience. Pour les nouveaux déploiements sur Windows Server 2019, déterminez à l’avance le type de résilience le mieux adapté à vos besoins. Si vous effectuez une mise à niveau à partir de Windows Server 2016, vous pouvez créer des volumes avec résilience imbriquée, migrer vos données, puis supprimer les volumes plus anciens.

### <a name="can-i-use-nested-resiliency-with-multiple-types-of-capacity-drives"></a>Puis-je utiliser la résilience imbriquée avec plusieurs types de lecteurs de capacité ?

Oui, il vous suffit de spécifier le `-MediaType` de chaque niveau en conséquence à l' [étape 1](#step-1-create-storage-tier-templates) ci-dessus. Par exemple, avec NVMe, SSD et HDD dans le même cluster, l’NVMe fournit le cache, tandis que les deux derniers fournissent de la capacité : définissez le niveau `NestedMirror` sur `-MediaType SSD` et le niveau `NestedParity` sur `-MediaType HDD`. Dans ce cas, Notez que l’efficacité de la capacité de parité dépend du nombre de lecteurs HDD uniquement et que vous avez besoin d’au moins 4 d’entre eux par serveur.

### <a name="can-i-use-nested-resiliency-with-3-or-more-servers"></a>Puis-je utiliser la résilience imbriquée avec 3 serveurs ou plus ?

Non, utiliser la résilience imbriquée uniquement si votre cluster contient exactement 2 serveurs.

### <a name="how-many-drives-do-i-need-to-use-nested-resiliency"></a>De combien de lecteurs ai-je besoin pour utiliser la résilience imbriquée ?

Le nombre minimal de lecteurs requis pour espaces de stockage direct est de 4 lecteurs de capacité par nœud de serveur, plus 2 lecteurs de cache par nœud de serveur (le cas échéant). Cela n’est pas modifié à partir de Windows Server 2016. Il n’existe aucune exigence supplémentaire pour la résilience imbriquée, et la recommandation pour la capacité de réserve est également inchangée.

### <a name="does-nested-resiliency-change-how-drive-replacement-works"></a>La résilience imbriquée change-t-elle le fonctionnement du remplacement de lecteur ?

Non.

### <a name="does-nested-resiliency-change-how-server-node-replacement-works"></a>La résilience imbriquée change-t-elle le fonctionnement du remplacement des nœuds du serveur ?

Non. Pour remplacer un nœud de serveur et ses lecteurs, suivez cet ordre :

1. Mettre hors service les lecteurs du serveur sortant
2. Ajouter le nouveau serveur, avec ses lecteurs, au cluster
3. Le pool de stockage est rééquilibré
4. Supprimer le serveur sortant et ses lecteurs

Pour plus d’informations, consultez la rubrique [supprimer des serveurs](remove-servers.md) .

## <a name="see-also"></a>Voir aussi

- [Présentation de espaces de stockage direct](storage-spaces-direct-overview.md)
- [Comprendre la tolérance de panne dans espaces de stockage direct](storage-spaces-fault-tolerance.md)
- [Planifier des volumes dans espaces de stockage direct](plan-volumes.md)
- [Créer des volumes dans espaces de stockage direct](create-volumes.md)
