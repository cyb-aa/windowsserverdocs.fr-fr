---
title: Vue d’ensemble de NTFS
description: Une explication des nouveautés de NTFS.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/06/2018
ms.localizationpriority: medium
ms.openlocfilehash: 556110fe7bed1aed002ef6d985324ff5171e770e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885370"
---
# <a name="ntfs-overview"></a>Vue d’ensemble de NTFS

>S’applique à : Windows 10, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

NTFS-système de fichiers principal pour les versions récentes de Windows et Windows Server, fournit un ensemble complet de fonctionnalités, notamment les descripteurs de sécurité, le chiffrement, les quotas de disque et des métadonnées enrichies et peut être utilisé avec des Volumes CSV (Cluster Shared) pour fournir en permanence volumes disponibles qui peuvent accéder simultanément à partir de plusieurs nœuds d’un cluster de basculement.

Pour en savoir plus sur les fonctionnalités nouvelles et modifiées dans NTFS dans Windows Server 2012 R2, consultez [What ' s New in NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn466520(v%3dws.11)). Pour des informations supplémentaires, consultez le [des informations supplémentaires](#additional-information) section de cette rubrique. Pour en savoir plus sur le système de fichiers résilient (ReFS) plus récente, consultez [vue d’ensemble du système de fichiers résilient (ReFS)](../refs/refs-overview.md).

## <a name="practical-applications"></a>Cas pratiques

### <a name="increased-reliability"></a>Fiabilité accrue

NTFS utilise ses informations de point de contrôle et de fichier journal pour restaurer la cohérence du système de fichiers lorsque l’ordinateur est redémarré après une défaillance du système. Après une erreur de secteur défectueux, NTFS remappe dynamiquement le cluster qui contient le secteur défectueux, alloue un nouveau cluster pour les données, marque le cluster d’origine comme étant défectueux et n’utilise plus l’ancien cluster. Par exemple, après une défaillance du serveur, NTFS peuvent récupérer des données en relisant ses fichiers journaux.

NTFS surveille en continu et corrige les problèmes d’altération temporaires en arrière-plan sans mettre le volume hors connexion (cette fonctionnalité est appelée [auto-adaptation NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771388(v=ws.10)), introduite dans Windows Server 2008). Pour les problèmes d’altération du plus grands, l’utilitaire Chkdsk, dans Windows Server 2012 et versions ultérieures, analyse et analyse le lecteur pendant que le volume est en ligne, limitation de durée hors connexion pour le temps nécessaire pour restaurer la cohérence des données sur le volume. Lorsque NTFS est utilisé avec les Volumes partagés de Cluster, sans temps d’arrêt est nécessaire. Pour plus d’informations, consultez l’article [Intégrité NTFS et Chkdsk](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831536(v%3dws.11)).

### <a name="increased-security"></a>Sécurité accrue

- **Accéder à la sécurité basée sur les ACL de liste de contrôle pour les fichiers et dossiers**— NTFS vous permet de définir des autorisations sur un fichier ou dossier, spécifiez les groupes et utilisateurs dont vous souhaitez bloquer ou autoriser l’accès, sélectionnez le type d’accès.

- **Prise en charge pour le chiffrement de lecteur BitLocker**: chiffrement de lecteur BitLocker fournit une sécurité supplémentaire pour les informations système critiques et d’autres données stockées sur les volumes NTFS. À compter de Windows Server 2012 R2 et Windows 8.1, BitLocker prend en charge pour le chiffrement de périphérique sur des ordinateurs de x64 64 avec un Module de plateforme sécurisée (TPM) et x86 que prend en charge est connecté en veille (auparavant disponible uniquement sur les périphériques Windows RT). Chiffrement de l’appareil permet de protéger les données sur les ordinateurs basés sur Windows, et bloquer les utilisateurs malveillants d’accéder aux fichiers système qu’ils utilisent pour découvrir le mot de passe ou d’accéder à un lecteur en le supprimant de l’ordinateur physiquement, de l’installer sur un un autre. Pour plus d’informations, consultez [quelles sont les nouveautés dans BitLocker](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn306081(v%3dws.11)).

- **Prise en charge de grands volumes**— NTFS peut prendre en charge des volumes aussi grands que 256 téraoctets. Prise en charge des tailles sont affectés par la taille du cluster et le nombre de clusters de volume. Avec (2<sup>32</sup> – 1) clusters (le nombre maximal de clusters NTFS prend en charge), le volume suivant et les tailles de fichiers sont pris en charge.

  |Taille du cluster|Plus grand volume|Plus grand fichier|
  |---|---|---|
  |4 Ko (taille par défaut)|16 TO|16 TO|
  |8 KO|32 TO|32 TO|
  |16 Ko|64 To|64 To|
  |32 KO|128 TO|128 TO|
  |64 Ko (taille maximale)|256 To|256 To|

>[!IMPORTANT]
>Services et applications peuvent imposer des limites supplémentaires sur les tailles de fichier et de volume. Par exemple, la limite de taille de volume est 64 To si vous utilisez la fonctionnalité Versions précédentes ou une application de sauvegarde qui rend utiliser des instantanés de Volume Shadow Copy Service (VSS) (et vous n’utilisez pas un boîtier SAN ou RAID). Toutefois, vous devrez peut-être utiliser plus petites tailles de volume en fonction de votre charge de travail et les performances de votre stockage.

### <a name="formatting-requirements-for-large-files"></a>Mise en forme de la configuration requise pour les fichiers volumineux

Pour autoriser l’extension appropriée des fichiers .vhdx volumineux, il existe de nouvelles recommandations pour la mise en forme de volumes. Lors du formatage des volumes qui seront utilisées avec la déduplication des données ou qui hébergeront des fichiers très volumineux, tels que les fichiers .vhdx supérieures à 1 To, utilisez le **Format-Volume** applet de commande dans Windows PowerShell avec les paramètres suivants.

|Paramètre|Description|
|---|---|
|-AllocationUnitSize 64KB|Définit une taille d’unité d’allocation NTFS 64 Ko.|
|-UseLargeFRS|Prise en charge pour les segments d’enregistrement de fichiers volumineux (FRS). Cela est nécessaire pour augmenter le nombre d’étendues autorisées par fichier sur le volume. Pour les enregistrements grandes FRS, la limite passe à partir d’étendues d’environ 1,5 million à environ 6 millions étendues.|

Par exemple, l’applet de commande suivant met en forme le lecteur D comme un volume NTFS, avec FRS est activé et une taille d’unité d’allocation de 64 Ko.

```PowerShell
Format-Volume -DriveLetter D -FileSystem NTFS -AllocationUnitSize 64KB -UseLargeFRS
```

Vous pouvez également utiliser le **format** commande. À une invite de commandes système, entrez la commande suivante, où **/l** met en forme un gros volume de FRS et **/A:64 k** définit une taille d’unité d’allocation 64 Ko :

```PowerShell
format /L /A:64k
```

### <a name="maximum-file-name-and-path"></a>Chemin d’accès et nom de fichier maximale

NTFS prend en charge les noms de fichiers longs et les chemins de longueur étendu, avec les valeurs maximum suivantes :

- **Prise en charge des noms de fichiers longs, compatibilité descendante**— NTFS permet à durée pendant laquelle les noms de fichiers, de stocker un 8.3 alias sur le disque (en Unicode) pour assurer une compatibilité avec les systèmes de fichiers qui impose une limite au format 8.3 sur les noms de fichiers et extensions. Si nécessaire (pour des raisons de performances), vous pouvez désactiver de manière sélective des alias au format 8.3 sur des volumes NTFS dans Windows Server 2008 R2, Windows 8 et les versions plus récentes du système d’exploitation Windows.
  Dans Windows Server 2008 R2 et les systèmes ultérieurs, les noms courts sont désactivés par défaut lorsqu’un volume est formaté à l’aide du système d’exploitation. Compatibilité des applications, les noms courts sont toujours activées sur le volume système.

- **Prise en charge des chemins d’accès de longueur étendu**: fonctions de nombreuses API de Windows ont des versions d’Unicode qui autorise un chemin d’accès de longueur étendu d’environ 32 767 caractères, au-delà de la limite de chemin d’accès de 260 caractères définie par le nombre maximal de\_paramètre de chemin d’accès. Pour nom de fichier détaillées et les exigences de format de chemin d’accès et des conseils pour implémenter des chemins d’accès de la longueur de l’étendue, consultez [d’affectation de noms de fichiers, chemins d’accès et espaces de noms](https://msdn.microsoft.com/library/windows/desktop/aa365247).

- **Stockage en cluster**, lorsqu’il est utilisé dans les clusters de basculement, NTFS prend en charge les volumes disponibles en continu qui sont accessibles par plusieurs nœuds de cluster simultanément lorsqu’il est utilisé conjointement avec le système de fichiers des Volumes partagés de Cluster (CSV). Pour plus d’informations, consultez [Use Cluster Shared Volumes dans un Cluster de basculement](../../failover-clustering/failover-cluster-csvs.md).

### <a name="flexible-allocation-of-capacity"></a>Affectation flexible de la capacité

Si l’espace sur un volume est limité, NTFS fournit les méthodes suivantes pour travailler avec la capacité de stockage d’un serveur :

- Utiliser des quotas de disque pour suivre et contrôler l’utilisation de l’espace disque sur les volumes NTFS pour les utilisateurs individuels.
- Utilisez la compression de fichiers système pour optimiser la quantité de données qui peuvent être stockées.
- Augmenter la taille d’un volume NTFS en ajoutant un espace non alloué à partir du même disque ou à partir d’un autre disque.
- Monter un volume sur un dossier vide sur un volume NTFS local si vous exécutez court de lettres de lecteur ou que vous devez créer un espace supplémentaire qui est accessible à partir d’un dossier existant.

## <a name="additional-information"></a>Informations complémentaires

|Type de contenu|Références|
|---|---|
|Évaluation|- [Nouveautés de NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn466520(v%3dws.11)) (Windows Server 2012 R2)<br>- [Nouveautés de NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff383236(v=ws.10)) (Windows Server 2008 R2, Windows 7)<br>- [Intégrité NTFS et chkdsk](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831536(v%3dws.11))<br>- [Réparation automatique de NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771388(v=ws.10)) (introduite dans Windows Server 2008)<br>- [NTFS transactionnel](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc730726(v%3dws.10)) (introduite dans Windows Server 2008)|
|Ressources de la communauté|- [Blog de l’équipe stockage Windows](https://blogs.msdn.microsoft.com/san/)|
|Technologies associées|- [Stockage dans Windows Server](../storage.md)<br>- [Cluster d’utilisation des Volumes partagés dans un Cluster de basculement](../../failover-clustering/failover-cluster-csvs.md)<br>-Le [Volumes partagés de Cluster](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831630(v%3dws.11)#cluster-shared-volumes>) et [conception du stockage](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831630(v%3dws.11)#storage-design>) sections de [conception de l’Infrastructure de Cloud](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831630(v%3dws.11)) <br>- [Espaces de stockage](../storage-spaces/overview.md)<br>- [Vue d’ensemble de RESILIENT File System (ReFS)](../refs/refs-overview.md)
