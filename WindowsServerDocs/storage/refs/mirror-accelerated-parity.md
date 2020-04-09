---
title: Parité accélérée grâce à la mise en miroir
ms.prod: windows-server
ms.author: gawatu
manager: masriniv
ms.technology: storage-file-systems
ms.topic: article
author: gawatu
ms.date: 10/17/2018
ms.assetid: ''
ms.openlocfilehash: 752073e4f12db3b994261a70a9306d45b9a00d77
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861512"
---
# <a name="mirror-accelerated-parity"></a>Parité accélérée grâce à la mise en miroir

>S’applique à : Windows Server 2019, Windows Server 2016

Les espaces de stockage peuvent assurer une tolérance de pannes pour vos données à l’aide de deux techniques fondamentales : la mise en miroir et la parité. Dans les [espaces de stockage direct](../storage-spaces/storage-spaces-direct-overview.md), ReFS introduit la parité accélérée grâce à la mise en miroir, qui vous permet de créer des volumes qui utilisent à la fois des résiliences en miroir et de parité. La parité accélérée grâce à la mise en miroir offre un stockage efficace et économique, sans nuire aux performances. 

![Volume-de-parité-accélérée-grâce-à-la-mise-en-miroir](media/mirror-accelerated-parity/Mirror-Accelerated-Parity-Volume.png)

## <a name="background"></a>Arrière-plan

Les modèles de résilience en miroir et de parité présentent des caractéristiques fondamentalement différentes en matière de stockage et de performances :
- La résilience en miroir permet aux utilisateurs d’atteindre des performances d’écriture rapides, mais la réplication des données pour chaque copie n’est pas efficace. 
- La résilience de parité, quant à elle, doit recalculer la parité pour chaque écriture, ce qui entraîne une dégradation des performances en matière d’écriture aléatoire. Toutefois, la parité offre aux utilisateurs une efficacité de stockage supérieure de leurs données. Pour plus d’informations, consultez [tolérance aux pannes des espaces de stockage](../storage-spaces/Storage-Spaces-Fault-Tolerance.md).

Par conséquent, la mise en miroir est prédisposée à fournir un stockage sensible aux performances, tandis que la parité offre une meilleure utilisation de la capacité de stockage. Dans le cadre de la parité accélérée grâce à la mise en miroir, ReFS exploite les avantages de chaque type de résilience afin de fournir un stockage à la fois de haute capacité et sensible aux performances, en combinant les deux méthodes de résilience au sein d’un volume unique.

## <a name="data-rotation-on-mirror-accelerated-parity"></a>Rotation de données sur la parité accélérée grâce à la mise en miroir

ReFS assure de façon active et en temps réel la rotation des données entre la mise en miroir et la parité. Cela permet aux écritures entrantes d’être rapidement écrites sur l’espace en miroir, puis transférées vers l’espace de parité afin d’être efficacement stockées. Ce faisant, les E/S entrantes sont traitées rapidement dans l’espace en miroir, tandis que les données passives (« cold ») sont stockées efficacement dans l’espace de parité, ce qui offre à la fois des performances optimales et un stockage économique au sein du même volume. 

Pour assurer la rotation des données entre la mise en miroir et la parité, ReFS divise de façon logique le volume en plusieurs zones de 64 Mio qui correspondent à l’unité de rotation. L’image ci-dessous illustre un volume de parité accélérée grâce à la mise en miroir, divisé en zones. 

![Parité-accélérée-grâce-à-la-mise-en-miroir-avec-conteneurs-de-stockage](media/mirror-accelerated-parity/Mirror-Accelerated-Parity-Volume-with-Storage-Containers.png)

ReFS commence la rotation de zones complètes de l’espace en miroir vers l’espace de parité une fois que le niveau de mise en miroir a atteint un niveau de capacité donné. Au lieu de déplacer immédiatement les données de l’espace en miroir vers l’espace de parité, ReFS attend et conserve les données mises en miroir aussi longtemps que possible, ce qui lui permet de continuer à fournir des performances optimales pour les données (Voir « Performances d’E/S » ci-dessous). 

Lors du déplacement des données de l’espace en miroir vers l’espace de parité, les données sont lues, les encodages de parité sont calculés et ces données sont ensuite écrites dans l’espace de parité. L’animation ci-dessous illustre cela, en utilisant une zone en miroir triple qui est convertie en une zone de codage d’effacement au cours de la rotation :

![Rotation-parité-accélérée-grâce-à-la-mise-en-miroir](media/mirror-accelerated-parity/Container-Rotation.gif)

## <a name="io-on-mirror-accelerated-parity"></a>E/S sur la parité accélérée grâce à la mise en miroir
### <a name="io-behavior"></a>Comportement des E/S
**Écritures :** ReFS traite les écritures entrantes de trois manières différentes :

1.  **Écritures en miroir :**

    - **1a.** Si l’écriture entrante modifie les données existantes en miroir, ReFS modifie les données en place.
    - **ter.** Si l’écriture entrante est une nouvelle écriture et que ReFS trouve suffisamment d’espace libre dans l’espace en miroir pour la traiter, ReFS transfère cette écriture en miroir.
    ![](media/mirror-accelerated-parity/Write-to-Mirror.png) d’écriture en miroir

2. **Écritures sur le miroir, réallouées à partir de la parité :**

    Si l’écriture entrante modifie des données qui sont dans la parité et que ReFS parvient à trouver suffisamment d’espace libre en miroir pour traiter l’écriture entrante, ReFS invalidera d’abord les données précédentes dans la parité, puis écrira en miroir. Cette invalidation est une opération de métadonnées rapide et peu coûteuse, qui permet d’améliorer significativement les performances d’écriture dans l’espace de parité.
    ![réallouées-écrire](media/mirror-accelerated-parity/Reallocated-Write.png)

3. **Écrit dans la parité :**
    
    Si ReFS ne peut pas trouver suffisamment d’espace libre dans l’espace en miroir, il écrit les nouvelles données dans l’espace de parité ou modifie les données de parité existantes. La section « Optimisation des performances » ci-dessous fournit des conseils pour vous aider à minimiser les écritures dans l’espace de parité.
    ![](media/mirror-accelerated-parity/Write-to-Parity.png) d’écriture sur parité

**Lectures :** ReFS lit les données directement à partir du niveau contenant les données pertinentes. Si l’espace de parité comporte des HDD, le cache dans les espaces de stockage direct mettra en cache ces données afin d’accélérer les lectures ultérieures. 

> [!NOTE]
> Les lectures n’entraînent jamais de rotation de données vers le niveau en miroir de la part de ReFS. 

### <a name="io-performance"></a>Performances d’e/s

**Écritures :** chaque type d’écriture décrit ci-dessus possède ses propres caractéristiques de performances. Globalement, les écritures vers le niveau en miroir sont beaucoup plus rapides que les écritures réallouées, et les écritures réallouées sont nettement plus rapides que les écritures effectuées directement dans le niveau de parité. Cette relation est illustrée par l’inégalité ci-dessous : 


- **Le niveau miroir > les écritures réallouées > > niveau de parité**


**Lectures :** Il n’existe aucun impact négatif significatif sur les performances lors des activités de lecture effectuées depuis l’espace de parité :
- Si l’espace en miroir et l’espace de parité sont construits avec le même type de média, les performances de lecture seront équivalentes. 
- Si l’espace en miroir et l’espace de parité sont construits avec différents types de média (SSD en miroir, HDD de parité, par exemple) [le cache dans les espaces de stockage direct](../storage-spaces/understand-the-cache.md) aidera à mettre en cache les données actives (« hot ») afin d’accélérer les lectures à partir de l’espace de parité.

## <a name="refs-compaction"></a>Compression ReFS

Dans cette version semi-annuelle de l’automne, ReFS introduit un compactage, ce qui améliore considérablement les performances des volumes de parité à accélération en miroir qui sont pleins de 90%. 

**Informations générales :** Auparavant, quand les volumes de parité accélérée grâce à la mise en miroir arrivaient à saturation, leurs performances pouvaient se dégrader. La dégradation des performances est due au fait que les données actives (« hot ») et les données passives (« cold ») se mélangent à l’intérieur du volume au fil du temps. Cela signifie que moins de données actives peuvent être stockées en miroir, car les données passives occupent l’espace en miroir qui pourrait servir au stockage de données actives. Stocker des données actives en miroir est essentiel pour maintenir de hautes performances, car les écritures effectuées directement dans l’espace en miroir sont beaucoup plus rapides que les écritures réallouées, lesquelles sont plus rapides que les écritures effectuées directement dans l’espace de parité. Par conséquent, le fait de stocker les données passives en miroir est mauvais pour les performances, car cela réduit la probabilité que ReFS puisse effectuer des écritures directement dans l’espace en miroir. 

La compression ReFS résout ces problèmes de performances en libérant de l’espace en miroir pour les données actives. La compression consolide dans un premier temps toutes les données (mises en miroir ou de parité) dans l’espace de parité. Cela réduit la fragmentation au sein du volume et augmente la quantité d’espace adressable en miroir. Plus important encore, ce processus permet à ReFS de consolider les données actives dans l’espace en miroir :
-   Lorsque de nouvelles écritures arriveront, elles seront traitées dans l’espace en miroir. Par conséquent, les nouvelles données actives écrites résident en miroir. 
-   Lorsqu’une écriture de modification est apportée aux données de parité, ReFS effectue une écriture réallouée, de sorte que cette écriture est également traitée en miroir. Par conséquent, les données actives qui ont été déplacées dans l’espace de parité pendant la compression seront réallouées dans l’espace en miroir. 

## <a name="performance-optimizations"></a>Optimisation des performances

>[!IMPORTANT]
> Nous vous recommandons de placer des disques durs virtuels en écriture lourds dans différents sous-répertoires. Cela est dû au fait que ReFS écrit des modifications de métadonnées au niveau d’un répertoire et de ses fichiers. Par conséquent, si vous distribuez des fichiers à écriture intensive dans plusieurs répertoires, les opérations de métadonnées sont plus petites et s’exécutent en parallèle, ce qui réduit la latence des applications.

### <a name="performance-counters"></a>Compteurs de performances

ReFS gère des compteurs de performance afin de vous aider à évaluer les performances de la parité accélérée grâce à la mise en miroir. 
- Comme décrit ci-dessus dans la section écriture dans la parité, ReFS écrit directement dans la parité lorsqu’il ne trouve pas d’espace libre en miroir. En règle générale, cela se produit lorsque le niveau en miroir se remplit plus rapidement que ReFS ne peut effectuer la rotation de données vers l’espace de parité. En d’autres termes, lorsque la rotation ReFS n’arrive pas à suivre le taux d’ingestion. Les compteurs de performances ci-dessous identifient les cas où ReFS écrit directement dans l’espace de parité :

  ```
  # Windows Server 2016
  ReFS\Data allocations slow tier/sec
  ReFS\Metadata allocations slow tier/sec

  # Windows Server 2019
  ReFS\Allocation of Data Clusters on Slow Tier/sec
  ReFS\Allocation of Metadata Clusters on Slow Tier/sec
  ```

- Si ces compteurs indiquent une valeur non nulle, cela indique que ReFS n’effectue pas assez vite la rotation des données depuis l’espace en miroir. Pour résoudre ce problème, vous pouvez soit modifier l’intensité de la rotation, soit augmenter la taille du niveau en miroir.

### <a name="rotation-aggressiveness"></a>Intensité de la rotation

ReFS commence la rotation des données une fois que l’espace en miroir a atteint un seuil de capacité donné.
-   Le fait d’augmenter ce seuil de rotation fait que ReFS conserve les données plus longtemps dans le niveau en miroir. Laisser des données actives dans le niveau en miroir permet d’obtenir des performances optimales. Toutefois, ReFS ne sera pas en mesure de traiter efficacement de grandes quantités d’E/S entrantes. 
-   Avec des valeurs plus faibles, ReFS peut retirer de manière proactive les données et mieux ingérer les E/S entrantes. Cela peut s’appliquer aux lourdes charges de travail à ingérer, comme le stockage de données d’archive. Des valeurs inférieures, cependant, peuvent dégrader les performances des charges de travail à usage général. Toute rotation de données inutile depuis le niveau en miroir impact de façon négative les performances. 

ReFS introduit un paramètre réglable pour ajuster ce seuil, qui est configurable à l’aide d’une clé de registre. Cette clé de registre doit être configurée sur **chaque nœud dans un déploiement des espaces de stockage direct**, et un redémarrage est nécessaire pour que les modifications prennent effet. 
-   **Key:** HKEY_LOCAL_MACHINE\System\CurrentControlSet\Policies
-   **ValueName (DWORD):** DataDestageSsdFillRatioThreshold
-   **ValueType:** Percentage

Si cette clé de registre n’est pas définie, ReFS utilisera la valeur 85 % par défaut.  Cette valeur par défaut est recommandée pour la plupart des déploiements, les valeurs inférieures à 50 % n’étant pas recommandées. La commande PowerShell ci-dessous montre comment définir cette clé de Registre avec une valeur de 75 % : 
```PowerShell
Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Policies -Name DataDestageSsdFillRatioThreshold -Value 75
 ```
 Pour configurer cette clé de Registre sur chaque nœud dans un déploiement d’espaces de stockage direct, vous pouvez utiliser la commande PowerShell suivante :
 ```PowerShell
 $Nodes = 'S2D-01', 'S2D-02', 'S2D-03', 'S2D-04'
 Invoke-Command $Nodes {Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Policies -Name DataDestageSsdFillRatioThreshold -Value 75}
 ```

### <a name="increasing-the-size-of-the-mirrored-tier"></a>Amélioration de la taille du niveau en miroir

L’augmentation de la taille du niveau en miroir permet à ReFS de conserver une plus grande partie des données de travail en miroir. Cela améliore la probabilité que ReFS puisse écrire directement dans l’espace en miroir, ce qui vous permettra d’obtenir de meilleures performances. Les applets de commande PowerShell ci-dessous montrent comment augmenter la taille du niveau en miroir :
```PowerShell
Resize-StorageTier -FriendlyName “Performance” -Size 20GB
Resize-StorageTier -InputObject (Get-StorageTier -FriendlyName “Performance”) -Size 20GB
```
>[!TIP]
>Veillez à redimensionner la **Partition** et le **Volume** après avoir redimensionné l’objet **StorageTier**. Pour plus d’informations et des exemples, voir [Redimensionner les volumes](../storage-spaces/resize-volumes.md).

## <a name="creating-a-mirror-accelerated-parity-volume"></a>Création d’un volume de parité accélérée grâce à la mise en miroir

L’applet de commande PowerShell ci-dessous crée un volume de parité accélérée grâce à la mise en miroir avec un ratio miroir : parité de 20 : 80, qui est la configuration recommandée pour la plupart des charges de travail. Pour obtenir plus d’informations et des exemples, voir [Création de volumes dans les espaces de stockage direct](../storage-spaces/Create-volumes.md).

```PowerShell
New-Volume – FriendlyName “TestVolume” -FileSystem CSVFS_ReFS -StoragePoolFriendlyName “StoragePoolName” -StorageTierFriendlyNames Performance, Capacity -StorageTierSizes 200GB, 800GB
```

## <a name="see-also"></a>Voir aussi

-   [Vue d’ensemble des ReFS](refs-overview.md)
-   [Clonage de bloc ReFS](block-cloning.md)
-   [Flux d’intégrité ReFS](integrity-streams.md)
-   [Présentation de espaces de stockage direct](../storage-spaces/storage-spaces-direct-overview.md)
