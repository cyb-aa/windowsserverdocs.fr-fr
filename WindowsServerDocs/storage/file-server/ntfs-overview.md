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
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402080"
---
# <a name="ntfs-overview"></a>Vue d’ensemble de NTFS

>S'applique à : Windows 10, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Le système de fichiers principal des versions récentes de Windows et Windows Server, NTFS, offre un ensemble complet de fonctionnalités, comme les descripteurs de sécurité, le chiffrement, les quotas de disque et des métadonnées enrichies. Il peut être utilisé avec des volumes partagés de cluster (CSV) pour fournir des volumes disponibles en continu auxquels plusieurs nœuds d’un cluster de basculement peuvent accéder simultanément.

Pour en savoir plus sur les fonctionnalités nouvelles et modifiées de NTFS dans Windows Server 2012 R2, consultez [Nouveautés du système de fichiers NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn466520(v%3dws.11)). Pour plus d’informations sur les fonctionnalités, consultez la section [Informations complémentaires](#additional-information) de cette rubrique. Pour en savoir plus sur le nouveau système de fichiers résilient (ReFS), consultez [Vue d’ensemble du système de fichiers résilient (ReFS)](../refs/refs-overview.md).

## <a name="practical-applications"></a>Cas pratiques

### <a name="increased-reliability"></a>Fiabilité accrue

NTFS utilise son fichier journal et ses informations de point de contrôle pour rétablir la cohérence du système de fichiers lors du redémarrage de l’ordinateur après une défaillance du système. Après une erreur liée à un secteur défectueux, NTFS remappe dynamiquement le cluster qui contient le secteur défectueux, alloue un nouveau cluster pour les données, marque le cluster d’origine comme étant défectueux et n’utilise plus l’ancien cluster. Par exemple, après un incident de serveur, NTFS peut récupérer des données en relisant ses fichiers journaux.

NTFS supervise et corrige en permanence les problèmes d’altération temporaires en arrière-plan sans mettre le volume hors connexion (cette fonctionnalité est connue sous le nom d’[auto-réparation](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771388(v=ws.10)), introduite dans Windows Server 2008). Pour les problèmes d’altération plus importants, l’utilitaire Chkdsk, dans Windows Server 2012 et versions ultérieures, permet d’analyser le lecteur pendant que le volume est en ligne, ce qui limite le temps hors connexion au temps nécessaire pour restaurer la cohérence des données sur le volume. Lorsque NTFS est utilisé avec des volumes partagés de cluster, aucun temps d’arrêt n’est nécessaire. Pour plus d’informations, consultez l’article [Intégrité NTFS et Chkdsk](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831536(v%3dws.11)).

### <a name="increased-security"></a>Sécurité accrue

- **Sécurité basée sur liste de contrôle d’accès (ACL) pour les fichiers et dossiers** : NTFS vous permet de définir des autorisations sur un fichier ou un dossier, de spécifier les groupes et les utilisateurs dont vous souhaitez restreindre ou autoriser l’accès et de sélectionner le type d’accès.

- **Prise en charge de Chiffrement de lecteur BitLocker** : La fonctionnalité Chiffrement de lecteur BitLocker renforce la sécurité des informations système critiques et d’autres données stockées sur les volumes NTFS. À compter de Windows Server 2012 R2 et Windows 8.1, BitLocker assure la prise en charge du chiffrement des appareils sur les ordinateurs x86 et x64 avec un module de plateforme sécurisée (TPM) qui prend en charge la veille connectée (auparavant uniquement disponible sur les appareils Windows RT). Le chiffrement des appareils permet de protéger les données sur les ordinateurs Windows, et empêche les utilisateurs malveillants d’accéder aux fichiers système qu’ils utilisent pour découvrir le mot de passe de l’utilisateur, ou d’accéder à un lecteur en le supprimant physiquement du PC et en l’installant sur un autre. Pour plus d’informations, consultez [Nouveautés de BitLocker](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn306081(v%3dws.11)).

- **Prise en charge de grands volumes** : NTFS peut prendre en charge des volumes pouvant atteindre 256 téraoctets. Les tailles de volume prises en charge sont affectées par la taille du cluster et le nombre de clusters. Avec(2<sup>32</sup> – 1) clusters (nombre maximal de clusters pris en charge par NTFS), les tailles de fichiers et de volumes suivantes sont prises en charge.

  |Taille du cluster|Volume le plus élevé|Fichier le plus volumineux|
  |---|---|---|
  |4 Ko (taille par défaut)|16 To|16 To|
  |8 Ko|32 To|32 To|
  |16 Ko|64 To|64 To|
  |32 Ko|128 To|128 To|
  |64 Ko (taille maximale)|256 To|256 To|

>[!IMPORTANT]
>Les services et les applications peuvent imposer des limites supplémentaires aux tailles des fichiers et volumes. Par exemple, la limite de la taille du volume s’élève à 64 To si vous utilisez la fonctionnalité Versions précédentes ou une application de sauvegarde qui utilise des captures instantanées effectuées par le service VSS (et que vous n’utilisez pas de boîtier SAN ou RAID). En revanche, vous devrez peut-être utiliser des tailles de volume inférieures en fonction de votre charge de travail et des performances de votre stockage.

### <a name="formatting-requirements-for-large-files"></a>Exigences de mise en forme pour les fichiers volumineux

Pour avoir une bonne extension des gros fichiers .vhdx, de nouvelles recommandations s’appliquent au formatage des volumes. Quand vous formatez des volumes qui sont utilisés avec la déduplication des données ou qui hébergent des fichiers très volumineux, comme des fichiers .vhdx dont la taille excède 1 To, utilisez l’applet de commande **Format-Volume** dans Windows PowerShell avec les paramètres suivants.

|Paramètre|Description|
|---|---|
|-AllocationUnitSize 64KB|Définit une taille d’unité d’allocation NTFS de 64 Ko.|
|-UseLargeFRS|Active la prise en charge des segments d’enregistrement de fichiers volumineux (FRS). Cela est nécessaire pour augmenter le nombre d’étendues autorisées par fichier sur le volume. Pour les enregistrements FRS volumineux, la limite passe d’environ 1,5 million d’étendues à environ 6 millions.|

Par exemple, l’applet de commande suivante formate le lecteur D en tant que volume NTFS, avec FRS activé et une taille d’unité d’allocation de 64 Ko.

```PowerShell
Format-Volume -DriveLetter D -FileSystem NTFS -AllocationUnitSize 64KB -UseLargeFRS
```

Vous pouvez également utiliser la commande **format**. À l’invite de commandes système, entrez la commande suivante, où **/L** formate un grand volume FRS et **/A:64k** définit une taille d’unité d’allocation de 64 Ko :

```PowerShell
format /L /A:64k
```

### <a name="maximum-file-name-and-path"></a>Longueur maximale du nom de fichier et du chemin

NTFS prend en charge les noms de fichiers longs et les chemins étendus, avec les valeurs maximales suivantes :

- **Prise en charge des noms de fichiers longs, avec compatibilité descendante** : NTFS autorise les noms de fichiers longs, en stockant un alias 8.3 sur le disque (au format Unicode) pour assurer la compatibilité avec les systèmes de fichiers qui imposent une limite 8.3 aux noms de fichiers et extensions. Si nécessaire (pour des raisons de performances), vous pouvez désactiver de manière sélective les alias 8.3 sur des volumes NTFS individuels dans Windows Server 2008 R2, Windows 8 et les versions plus récentes du système d’exploitation Windows.
  Dans les systèmes Windows Server 2008 R2 et les versions ultérieures, les noms courts sont désactivés par défaut quand un volume est formaté à l’aide du système d’exploitation. À des fins de compatibilité des applications, les noms courts sont toujours activés sur le volume système.

- **Prise en charge des chemins étendus** : De nombreuses fonctions d’API Windows ont des versions Unicode qui autorisent un chemin étendu comprenant environ 32 767 caractères (bien au-delà de la limite de 260 caractères définie par le paramètre MAX\_PATH). Pour plus d’informations sur les spécifications de format de fichier et de chemin, et pour obtenir des conseils sur l’implémentation des chemins étendus, consultez [Nommage des fichiers, chemins et espaces de noms](https://msdn.microsoft.com/library/windows/desktop/aa365247).

- **Espace de stockage en cluster** : Utilisé dans les clusters de basculement, NTFS prend en charge les volumes disponibles en continu qui sont accessibles par plusieurs nœuds de cluster en même temps dans le cadre d’une utilisation conjointe avec le système de fichiers Volume partagé de cluster (CSV). Pour plus d’informations, consultez [Utiliser les volumes partagés de cluster dans un cluster de basculement](../../failover-clustering/failover-cluster-csvs.md).

### <a name="flexible-allocation-of-capacity"></a>Allocation flexible de la capacité

Si l’espace sur un volume est limité, NTFS offre les moyens suivants d’utiliser la capacité de stockage d’un serveur :

- Utilisez des quotas de disque pour suivre et contrôler l’utilisation de l’espace disque sur les volumes NTFS pour des utilisateurs individuels.
- Utilisez la compression de système de fichiers pour maximiser la quantité de données pouvant être stockées.
- Augmentez la taille d’un volume NTFS en ajoutant un espace non alloué issu du même disque ou d’un autre disque.
- Montez un volume dans un dossier vide sur un volume NTFS local si vous n’avez plus de lettres de lecteur ou si vous avez besoin de créer de l’espace supplémentaire accessible à partir d’un dossier existant.

## <a name="additional-information"></a>Informations complémentaires

- [Recommandations en matière de taille de cluster pour ReFS et NTFS](https://techcommunity.microsoft.com/t5/Storage-at-Microsoft/Cluster-size-recommendations-for-ReFS-and-NTFS/ba-p/425960)
- [Vue d’ensemble du système de fichiers résilient (ReFS)](../refs/refs-overview.md)
- [Nouveautés de NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn466520(v%3dws.11)) (Windows Server 2012 R2)
- [Nouveautés de NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff383236(v=ws.10)) (Windows Server 2008 R2, Windows 7)
- [Intégrité NTFS et Chkdsk](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831536(v%3dws.11))
- [NTFS à réparation automatique](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771388(v=ws.10)) (introduit dans Windows Server 2008)
- [NTFS transactionnel](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc730726(v%3dws.10)) (introduit dans Windows Server 2008)
- [Storage](../storage.md) (Stockage)