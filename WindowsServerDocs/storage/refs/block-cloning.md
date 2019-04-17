---
ms.assetid: fd427da3-3869-428f-bf2a-56c4b7d99b40
title: Clonage de bloc sur ReFS
description: 
author: gawatu
ms.author: gawatu
manager: gawatu
ms.date: 12/6/2016
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage-file-systems
ms.openlocfilehash: 25cc523eaa2ed266e5b07c53ede4bc9e9be20e93
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# <a name="block-cloning-on-refs"></a>Clonage de bloc sur ReFS
>S’applique à: Windows Server (canal semi-annuel), WindowsServer2016

Le clonage de bloc demande au système de fichiers de copier une plage d’octets de ficher pour le compte d’une application. Les fichiers source et de destination utilisés peuvent être identiques ou différents. Toutefois, ces opérations de copie sont coûteuses, car elles déclenchent des lectures et des écritures coûteuses sur les données physiques sous-jacentes. 

Dans ReFS, le clonage de bloc effectue les copies dans le cadre d’une opération de métadonnées moins coûteuse que la lecture et l’écriture des données de fichier. ReFS permet à un ensemble de fichiers de partager les mêmes clusters logiques (emplacements physiques sur un volume). Les opérations de copie ont donc uniquement à remapper une région d’un fichier vers un emplacement physique distinct, ce qui est plus rapide et logique que d’effectuer des opérations physiques coûteuses. Ce processus accélère les opérations de copie et réduit le nombre d’E/S générées dans le stockage sous-jacent. Cette amélioration profite également aux charges de travail virtualisées, car elle accélère considérablement les opérations de fusion des points de contrôle.vhdx dans le cadre d’opérations de clonage de bloc. De plus, comme plusieurs fichiers peuvent partager les mêmes clusters logiques, les données identiques ne sont pas stockées à plusieurs emplacements physiques, ce qui augmente la capacité de stockage. 
  
## <a name="how-it-works"></a>Fonctionnement 

Le clonage de bloc sur ReFS transforme les opérations de données de fichier en opérations de métadonnées. Pour rendre possible cette optimisation, ReFS introduit des nombres de références dans ses métadonnées pour les régions copiées. Ce nombre de références indique le nombre de régions du fichier distinctes qui font référence aux mêmes régions physiques. Cela permet à plusieurs fichiers de partager les mêmes données physiques:

![Mise à jour du nombre de références quand plusieurs fichiers font référence à la même région](media/ref-count-example.gif)

ReFS enregistre le nombre de références pour chaque cluster logique, ce qui maintient l’isolation entre les fichiers: les opérations d’écriture dans les régions partagées déclenchent un mécanisme d’allocation sur écriture, où ReFS alloue une nouvelle région pour l’opération d’écriture entrante. Ce mécanisme préserve l’intégrité des clusters logiques partagés. 

### <a name="example"></a>Exemple
Prenons l’exemple de deux fichiers, appelés XetY, qui contiennent chacun trois régions mappées à des clusters logiques distincts.

![Deux fichiers contenant chacun trois régions distinctes, toutes mappées à des régions avec un nombre de références égal à1](media/block-clone-1.png)

Supposons maintenant qu’une application émette une opération de clonage de bloc du fichierX vers le fichierY pour que les régionsA etB soient copiées au niveau de la régionE. Nous obtenons alors l’état du système de fichiers suivant:

![Nombre de références égal à2 pour la région du clonage de bloc](media/block-clone-2.png)

Cet état du système de fichiers indique que la région de clonage de bloc a été dupliquée. ReFS effectue cette opération de copie uniquement en mettant à jour les mappages VCN en LCN, sans que cela nécessite de lecture de données physiques, ni de remplacement de données physiques dans le fichierY. Les fichiersX etY partagent maintenant des clusters logiques, comme le montre les nombres de références dans le tableau. Du fait qu’il n’y a pas de données physiques à copier, ReFS permet d’utiliser moins de capacité de stockage sur le volume. 

Supposons maintenant que l’application tente de remplacer la régionA dans le fichierX. ReFS va dupliquer la région partagée, mettre à jour les nombres de références en conséquence et effectuer l’opération de copie entrante dans la nouvelle région dupliquée. Cela permet de préserver l’isolation entre les fichiers.   

![Isolation préservée grâce à l’écriture dans une nouvelle régionG et la mise à jour des nombres de références](media/block-clone-3.png)

Après cette modification, la régionB est toujours partagée par les deuxfichiers. Notez que si la régionA avait été plus grande qu’un cluster, seul le cluster modifié aurait été dupliqué, et la partie restante aurait continué à être partagée.


## <a name="functionality-restrictions-and-remarks"></a>Remarques et restrictions relatives à la fonctionnalité
- Les régions source et de destination doivent commencer et finir à la limite d’un cluster. 
- La taille de la région clonée doit être inférieure à4Go. 
- Le nombre maximal de régions d’un fichier pouvant être mappées à la même région physique est de8175.
- La région de destination ne doit pas dépasser la fin du fichier. Pour que l’application puisse étendre la destination des données clonées, elle doit d’abord appeler [SetEndOfFile](https://msdn.microsoft.com/library/windows/desktop/aa365531(v=vs.85).aspx). 
- Si les régions source et de destination se trouvent dans le même fichier, elles ne doivent pas se chevaucher. (L’application doit pouvoir continuer l’opération de clonage de bloc en la fractionnant en plusieurs clones de bloc qui ne se chevauchent plus.)
- Les fichiers source et de destination doivent se trouver sur le même volume ReFS. 
- Les fichiers source et de destination doivent être définis avec le même paramètre [Flux d’intégrité](https://msdn.microsoft.com/library/windows/desktop/gg258117(v=vs.85).aspx). 
- Si le fichier source est partiellement alloué, le fichier de destination doit l’être également. 
- L’opération de clonage de bloc supprime les verrous opportunistes partagés (également appelés [verrous opportunistes de niveau2](https://msdn.microsoft.com/library/windows/desktop/aa365713(v=vs.85).aspx)).
- Le volume ReFS doit avoir été formaté avec Windows Server2016 et, si le clustering de basculement est utilisé, le niveau fonctionnel du clustering doit avoir été défini sur Windows Server2016 ou une version ultérieure au moment du formatage. 

## <a name="see-also"></a>Voir aussi

-   [Vue d’ensemble du système ReFS](refs-overview.md)
-   [Flux d’intégrité ReFS](integrity-streams.md)
-   [Vue d’ensemble des espaces de stockage direct](../storage-spaces/storage-spaces-direct-overview.md)
-   [DUPLICATE_EXTENTS_DATA](https://msdn.microsoft.com/library/windows/desktop/mt590821(v=vs.85).aspx)
-   [FSCTL_DUPLICATE_EXTENTS_TO_FILE](https://msdn.microsoft.com/library/windows/desktop/mt590823(v=vs.85).aspx)
