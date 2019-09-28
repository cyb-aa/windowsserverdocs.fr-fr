---
title: Vue d’ensemble de NTFS
description: Explication du système de fichiers NTFS.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 06/17/2019
ms.localizationpriority: medium
ms.openlocfilehash: b98877d0a94ff8033b65bf74d0118e2a5f1ea092
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402080"
---
# <a name="ntfs-overview"></a>Vue d’ensemble de NTFS

>S’applique à : Windows 10, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 et Windows Server 2008

NTFS, le système de fichiers principal pour les versions récentes de Windows et Windows Server, fournit un ensemble complet de fonctionnalités, notamment les descripteurs de sécurité, le chiffrement, les quotas de disque et les métadonnées enrichies, et peut être utilisé avec des volumes partagés de cluster (CSV) pour fournir en continu volumes disponibles accessibles simultanément à partir de plusieurs nœuds d’un cluster de basculement.

Pour en savoir plus sur les fonctionnalités nouvelles et modifiées de NTFS dans Windows Server 2012 R2, consultez [Nouveautés du système de fichiers NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn466520(v%3dws.11)). Pour plus d’informations sur les fonctionnalités, consultez la section [informations supplémentaires](#additional-information) de cette rubrique. Pour en savoir plus sur le nouveau système de fichiers résilient (ReFS), consultez [vue d’ensemble du système de fichiers résilient (REFS)](../refs/refs-overview.md).

## <a name="practical-applications"></a>Cas pratiques

### <a name="increased-reliability"></a>Fiabilité accrue

NTFS utilise son fichier journal et ses informations de point de contrôle pour restaurer la cohérence du système de fichiers lors du redémarrage de l’ordinateur après une défaillance du système. Après une erreur de secteur incorrect, le système de fichiers NTFS remappe dynamiquement le cluster qui contient le secteur défectueux, alloue un nouveau cluster pour les données, marque le cluster d’origine comme étant incorrect et n’utilise plus l’ancien cluster. Par exemple, après un blocage de serveur, NTFS peut récupérer des données en relisant ses fichiers journaux.

Le système de fichiers NTFS surveille et corrige en permanence les problèmes d’altération temporaires en arrière-plan sans mettre le volume hors connexion (cette fonctionnalité est connue sous le nom de [NTFS à réparation automatique](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771388(v=ws.10)), introduite dans Windows Server 2008). Pour les problèmes de corruption plus importants, l’utilitaire chkdsk, dans Windows Server 2012 et versions ultérieures, analyse et analyse le lecteur pendant que le volume est en ligne, ce qui limite le temps hors connexion au temps nécessaire pour restaurer la cohérence des données sur le volume. Lorsque NTFS est utilisé avec des volumes partagés de cluster, aucun temps d’arrêt n’est nécessaire. Pour plus d’informations, consultez l’article [Intégrité NTFS et Chkdsk](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831536(v%3dws.11)).

### <a name="increased-security"></a>Sécurité accrue

- **Sécurité basée sur la liste de Access Control (ACL) pour les fichiers et les dossiers**: NTFS vous permet de définir des autorisations sur un fichier ou un dossier, de spécifier les groupes et les utilisateurs dont vous souhaitez restreindre ou autoriser l’accès, et de sélectionner le type d’accès.

- **Prise en charge de chiffrement de lecteur BitLocker**: chiffrement de lecteur BitLocker fournit une sécurité supplémentaire pour les informations système critiques et les autres données stockées sur des volumes NTFS. À compter de Windows Server 2012 R2 et Windows 8.1, BitLocker assure la prise en charge du chiffrement des appareils sur les ordinateurs x86 et x64 avec un Module de plateforme sécurisée (TPM) (TPM) qui prend en charge la connexion en veille (précédemment disponible uniquement sur les appareils Windows RT). Le chiffrement de l’appareil permet de protéger les données sur les ordinateurs Windows, et empêche les utilisateurs malveillants d’accéder aux fichiers système qu’ils utilisent pour découvrir le mot de passe de l’utilisateur, ou d’accéder à un lecteur en le supprimant physiquement du PC et en l’installant sur un autre. Pour plus d’informations, consultez [Nouveautés de BitLocker](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn306081(v%3dws.11)).

- **Prise en charge des volumes importants**: NTFS peut prendre en charge des volumes aussi volumineux que 256 téraoctets. Les tailles de volume prises en charge sont affectées par la taille de cluster et le nombre de clusters. Avec (2<sup>32</sup> – 1) clusters (le nombre maximal de clusters pris en charge par NTFS), les tailles de fichier et de volume suivantes sont prises en charge.

  |Taille du cluster|Plus grand volume|Fichier le plus volumineux|
  |---|---|---|
  |4 Ko (taille par défaut)|16 TO|16 TO|
  |8 KO|32 TO|32 TO|
  |16 Ko|64 To|64 To|
  |32 Ko|128 TO|128 TO|
  |64 Ko (taille maximale)|256 To|256 To|

>[!IMPORTANT]
>Les services et les applications peuvent imposer des limites supplémentaires en termes de tailles de fichiers et de volumes. Par exemple, la limite de taille du volume est de 64 to si vous utilisez la fonctionnalité versions précédentes ou une application de sauvegarde qui utilise des captures instantanées Service VSS (VSS) (et que vous n’utilisez pas de boîtier SAN ou RAID). Toutefois, vous devrez peut-être utiliser des tailles de volume inférieures en fonction de votre charge de travail et des performances de votre stockage.

### <a name="formatting-requirements-for-large-files"></a>Exigences de mise en forme pour les fichiers volumineux

Pour permettre une extension correcte des fichiers. vhdx volumineux, de nouvelles recommandations s’offrent à vous pour la mise en forme des volumes. Lors de la mise en forme des volumes qui seront utilisés avec la déduplication des données ou hébergera des fichiers très volumineux, tels que des fichiers. vhdx d’une taille supérieure à 1 to, utilisez l’applet de commande **format-volume** dans Windows PowerShell avec les paramètres suivants.

|Paramètre|Description|
|---|---|
|-AllocationUnitSize 64 Ko|Définit une taille d’unité d’allocation NTFS de 64 Ko.|
|-UseLargeFRS|Active la prise en charge des segments d’enregistrement de fichier volumineux (FRS). Cela est nécessaire pour augmenter le nombre d’étendues autorisées par fichier sur le volume. Pour les enregistrements FRS volumineux, la limite est d’environ 1,5 million étendues à environ 6 millions extensions.|

Par exemple, l’applet de commande suivante met en forme le lecteur D en tant que volume NTFS, avec FRS activé et une taille d’unité d’allocation de 64 Ko.

```PowerShell
Format-Volume -DriveLetter D -FileSystem NTFS -AllocationUnitSize 64KB -UseLargeFRS
```

Vous pouvez également utiliser la commande **format** . À l’invite de commandes système, entrez la commande suivante, où **/l** met en forme un volume FRS volumineux et **/a : 64k** définit une taille d’unité d’allocation de 64 Ko :

```PowerShell
format /L /A:64k
```

### <a name="maximum-file-name-and-path"></a>Nom de fichier et chemin d’accès maximum

NTFS prend en charge les noms de fichiers longs et les chemins d’accès étendus, avec les valeurs maximales suivantes :

- **Prise en charge des noms de fichiers longs, avec compatibilité descendante**: NTFS autorise les noms de fichiers longs, en stockant un alias 8,3 sur le disque (Unicode) pour assurer la compatibilité avec les systèmes de fichiers qui imposent une limite de 8,3 sur les noms de fichiers et les extensions. Si nécessaire (pour des raisons de performances), vous pouvez désactiver de manière sélective les alias 8,3 sur des volumes NTFS individuels dans Windows Server 2008 R2, Windows 8 et les versions plus récentes du système d’exploitation Windows.
  Dans les systèmes Windows Server 2008 R2 et versions ultérieures, les noms courts sont désactivés par défaut lorsqu’un volume est formaté à l’aide du système d’exploitation. Pour la compatibilité des applications, les noms courts sont toujours activés sur le volume système.

- **Prise en charge des chemins d’accès étendus**: de nombreuses fonctions de l’API Windows ont des versions Unicode qui autorisent un chemin d’accès de longueur prolongée d’environ 32 767 caractères, au\_-delà de la limite de 260 caractères définie par le paramètre de chemin d’accès maximal. Pour plus d’informations sur les spécifications de format de fichier et de chemin d’accès, et pour obtenir des conseils sur l’implémentation de chemins d’accès étendus, consultez [nommage des fichiers, chemins d’accès et espaces de noms](https://msdn.microsoft.com/library/windows/desktop/aa365247).

- **Stockage en cluster**: lorsqu’il est utilisé dans les clusters de basculement, NTFS prend en charge les volumes disponibles en continu qui sont accessibles par plusieurs nœuds de cluster simultanément lorsqu’ils sont utilisés conjointement avec le système de fichiers de volumes partagés de cluster (CSV). Pour plus d’informations, consultez [utiliser des volumes partagés de cluster dans un cluster de basculement](../../failover-clustering/failover-cluster-csvs.md).

### <a name="flexible-allocation-of-capacity"></a>Allocation flexible de la capacité

Si l’espace sur un volume est limité, NTFS offre les moyens suivants d’utiliser la capacité de stockage d’un serveur :

- Utilisez les quotas de disque pour suivre et contrôler l’utilisation de l’espace disque sur les volumes NTFS pour des utilisateurs individuels.
- Utilisez la compression du système de fichiers pour maximiser la quantité de données pouvant être stockées.
- Augmentez la taille d’un volume NTFS en ajoutant un espace non alloué à partir du même disque ou à partir d’un autre disque.
- Montez un volume dans un dossier vide sur un volume NTFS local si vous n’avez plus de lettres de lecteur ou si vous avez besoin de créer de l’espace supplémentaire accessible à partir d’un dossier existant.

## <a name="additional-information"></a>Informations complémentaires

- [Recommandations en matière de taille de cluster pour ReFS et NTFS](https://techcommunity.microsoft.com/t5/Storage-at-Microsoft/Cluster-size-recommendations-for-ReFS-and-NTFS/ba-p/425960)
- [Vue d’ensemble du système de fichiers résilient (ReFS)](../refs/refs-overview.md)
- [Nouveautés de NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn466520(v%3dws.11)) (Windows Server 2012 R2)
- [Nouveautés de NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff383236(v=ws.10)) (Windows Server 2008 R2, Windows 7)
- [Intégrité NTFS et Chkdsk](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831536(v%3dws.11))
- [NTFS à réparation automatique](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771388(v=ws.10)) (introduit dans Windows Server 2008)
- [NTFS transactionnel](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc730726(v%3dws.10)) (introduit dans Windows Server 2008)
- [Storage](../storage.md) (Stockage)