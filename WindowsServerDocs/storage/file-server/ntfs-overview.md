---
title: Vue d’ensemble de NTFS
description: Obtenir une explication de ce qui est NTFS.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/06/2018
ms.localizationpriority: medium
ms.openlocfilehash: 556110fe7bed1aed002ef6d985324ff5171e770e
ms.sourcegitcommit: 5ed023a2ef3a9002daf41c7717feb1df186d2a14
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/02/2019
ms.locfileid: "9122102"
---
# Vue d’ensemble de NTFS

>S’applique à: Windows 10, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

NTFS, le système de fichiers principal des versions récentes de Windows et Windows Server, offre un ensemble complet de fonctionnalités, notamment des descripteurs de sécurité, le chiffrement, les quotas de disque et des métadonnées enrichies et peut être utilisé avec Cluster Shared Volumes (CSV) pour fournir en permanence volumes disponibles qui sont accessibles simultanément à partir de plusieurs nœuds d’un cluster de basculement.

Pour en savoir plus sur les fonctionnalités nouvelles et modifiées dans le système NTFS dans Windows Server 2012 R2, voir [Nouveautés de NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn466520(v%3dws.11)). Pour plus d’informations de fonctionnalité supplémentaire, consultez la section [des informations supplémentaires](#additional-information) de cette rubrique. Pour en savoir plus sur le système de fichiers résilient (ReFS) plus récentes, consultez la [vue d’ensemble du système de fichiers résilient (ReFS)](../refs/refs-overview.md).

## Applications pratiques

### Augmenter la fiabilité

NTFS utilise ses informations de journal des fichiers et de point de contrôle pour restaurer la cohérence du système de fichiers lors du redémarrage de l’ordinateur après une défaillance du système. Après une erreur de secteur défectueux, NTFS remappe dynamiquement le cluster qui contient le secteur défectueux, alloue un nouveau cluster pour les données, marque le cluster d’origine comme étant défectueux et n’est plus utilise l’ancien cluster. Par exemple, après une panne de serveur, NTFS peut récupérer les données en relisant ses fichiers journaux.

NTFS surveille en continu et résout les problèmes de corruption temporaire en arrière-plan sans mettre le volume hors connexion (cette fonctionnalité est appelée [automatiquement une réparation automatique NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771388(v=ws.10)), introduit dans Windows Server 2008). Pour les problèmes de corruption plus grands, l’utilitaire Chkdsk, dans Windows Server 2012 et versions ultérieures, les analyses et analyse le lecteur pendant que le volume est en ligne, limitation de temps en mode hors connexion pour le temps nécessaire pour restaurer la cohérence des données sur le volume. Lorsque NTFS est utilisé avec des Volumes partagés de Cluster, sans interruption de service est requise. Pour plus d’informations, voir [intégrité NTFS et Chkdsk](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831536(v%3dws.11)).

### Sécurité accrue

- **La sécurité basée sur la liste de contrôle d’accès aux fichiers et dossiers**— NTFS vous permet de définir des autorisations sur un fichier ou un dossier, spécifiez les groupes et les utilisateurs dont vous souhaitez restreindre ou autoriser l’accès et sélectionnez le type d’accès.

- **Prise en charge pour le chiffrement de lecteur BitLocker**, le chiffrement de lecteur BitLocker fournit une sécurité supplémentaire pour les informations système critiques et autres données stockées sur des volumes NTFS. Depuis Windows Server 2012 R2 et Windows 8.1, BitLocker prend en charge le chiffrement de périphérique sur les ordinateurs x64 avec un Module de plateforme sécurisée (TPM) et de x86 que prend en charge connecté en veille (disponible uniquement sur les périphériques Windows RT précédemment). Chiffrement de l’appareil permet de protéger les données sur les ordinateurs Windows, et il permet de bloquer les utilisateurs malveillants d’accéder aux fichiers système qu’ils utilisent pour découvrir un mot de passe de l’utilisateur ou d’accéder à un lecteur en physiquement supprimer de l’ordinateur et l’installer sur un un autre. Pour plus d’informations, voir [Nouveautés de BitLocker](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn306081(v%3dws.11)).

- **Prise en charge de volumes importants**— NTFS peuvent prendre en charge les volumes de taille de 256 téraoctets. Prise en charge des tailles sont affectés par la taille de cluster et le nombre de clusters de volume. Avec (2<sup>32</sup> – 1) clusters (le nombre maximal de clusters qui prend en charge de NTFS), le volume suivant et les tailles de fichiers sont pris en charge.

  |Taille de cluster|Plus grand volume|Fichier le plus volumineux|
  |---|---|---|
  |4 Ko (taille par défaut)|16 TO|16 TO|
  |8 KO|32 TO|32 TO|
  |16 KO|64 TO|64 TO|
  |32 KO|128 TO|128 TO|
  |64 Ko (taille maximale)|256To|256To|

>[!IMPORTANT]
>Services et applications peuvent imposer des limites supplémentaires sur les tailles de fichier et le volume. Par exemple, la limite de taille de volume est de 64 To si vous utilisez la fonctionnalité de Versions antérieures ou une application de sauvegarde qui rend utiliser des instantanés de Volume Shadow Copy Service (VSS) (et vous n’utilisez pas un boîtier SAN ou RAID). Toutefois, vous devrez peut-être utiliser plus petites tailles de volume en fonction de votre charge de travail et les performances de votre système de stockage.

### Mise en forme de la configuration requise pour les fichiers volumineux

Pour autoriser l’extension appropriée des fichiers volumineux .vhdx, il existe de nouvelles recommandations pour la mise en forme de volumes. Lors du formatage des volumes qui seront utilisés avec la déduplication des données ou qui hébergeront les fichiers très volumineux, tels que les fichiers .vhdx supérieures à 1 To, utilisez l’applet de commande **Format-Volume** dans Windows PowerShell avec les paramètres suivants.

|Paramètre|Description|
|---|---|
|-AllocationUnitSize 64 Ko|Définit une taille d’unité d’allocation NTFS 64 Ko.|
|-UseLargeFRS|Prise en charge pour les segments d’enregistrement de fichiers volumineux (FRS). Cela est nécessaire pour augmenter le nombre d’extensions autorisées par fichier sur le volume. Pour obtenir les enregistrements FRS volumineux, la limite augmente dans les extensions d’environ 1,5 millions à environ 6 millions extensions.|

Par exemple, les formats d’applet de commande suivant lecteur D comme un volume NTFS, avec FRS est activé et une taille d’unité d’allocation de 64 Ko.

```PowerShell
Format-Volume -DriveLetter D -FileSystem NTFS -AllocationUnitSize 64KB -UseLargeFRS
```

Vous pouvez également utiliser la commande de **format** . À une invite de commandes système, entrez la commande suivante, où **/L** met en forme un volume élevé de FRS et **/A:64 k** définit une taille d’unité d’allocation 64 Ko:

```PowerShell
format /L /A:64k
```

### Chemin d’accès et nom de fichier maximale

NTFS prend en charge les noms de fichiers longs et les chemins de longueur étendu, avec les valeurs maximales suivantes:

- **Prise en charge pour les noms de fichier longs, avec une compatibilité descendante**— NTFS permet aux noms de fichier longs, stockant une 8.3 alias sur le disque (en Unicode) pour assurer la compatibilité avec les systèmes de fichiers qui imposer une limite 8.3 sur les noms de fichiers et extensions. Si nécessaire (pour des raisons de performances), vous pouvez désactiver crénelage 8.3 sur des volumes NTFS dans Windows Server 2008 R2, Windows 8 et les versions plus récentes du système d’exploitation Windows.
  Dans Windows Server 2008 R2 et ultérieures, les noms courts sont désactivées par défaut lorsqu’un volume est formaté à l’aide du système d’exploitation. Compatibilité des applications, les noms courts sont toujours activés sur le volume du système.

- **Prise en charge des chemins d’accès de longueur étendu**: fonctions de nombreuses API de Windows ont des versions qui permettent à un chemin d’accès de longueur étendu d’environ 32 767 caractères Unicode — au-delà de la limite de chemin d’accès de 260 caractères définie par le paramètre MAX\_PATH. Pour le nom de fichier détaillées et exigences relatives au format de chemin d’accès et conseils pour l’implémentation des chemins d’accès de longueur étendu, voir [nommer des fichiers, les chemins d’accès et les espaces de noms](https://msdn.microsoft.com/library/windows/desktop/aa365247).

- **Stockage Clustered**, lorsqu’il est utilisé dans des clusters de basculement, NTFS prend en charge des volumes disponibles en continu qui sont accessibles à plusieurs nœuds du cluster simultanément lorsqu’il est utilisé conjointement avec le système de fichiers de Volumes partagés de Cluster (CSV). Pour plus d’informations, voir [Utilisation Volumes partagés du Cluster dans un Cluster de basculement](../../failover-clustering/failover-cluster-csvs.md).

### Allocation flexible de capacité

Si l’espace sur un volume est limité, NTFS fournit la méthode suivante pour travailler avec la capacité de stockage d’un serveur:

- Utilisez les quotas de disque pour suivre et de contrôler l’utilisation de l’espace disque sur des volumes NTFS pour les utilisateurs individuels.
- Utiliser la compression de système de fichiers pour optimiser la quantité de données qui peuvent être stockées.
- Augmenter la taille d’un volume NTFS en ajoutant un espace non alloué à partir de la même disque ou d’un disque différent.
- Monter un volume sur n’importe quel dossier vide sur un volume NTFS local, si vous exécutez en dehors des lettres de lecteur ou que vous devez créer un espace supplémentaire qui est accessible à partir d’un dossier existant.

## Informations complémentaires

|Type de contenu|Références|
|---|---|
|Évaluation|- [Nouveautés de NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn466520(v%3dws.11)) (Windows Server 2012 R2)<br>- [Nouveautés de NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff383236(v=ws.10)) (Windows Server 2008 R2, Windows 7)<br>- [CHKDSK et intégrité NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831536(v%3dws.11))<br>- [Rétablissement automatique de NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771388(v=ws.10)) (introduit dans Windows Server 2008)<br>- [NTFS transactionnel](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc730726(v%3dws.10)) (introduit dans Windows Server 2008)|
|Ressources de la communauté|- [Blog de l’équipe Windows Storage](https://blogs.msdn.microsoft.com/san/)|
|Technologies associées|- [Stockage dans Windows Server](../storage.md)<br>- [Cluster d’utilisation des Volumes partagés dans un Cluster de basculement](../../failover-clustering/failover-cluster-csvs.md)<br>-Les sections [Volumes partagés de Cluster](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831630(v%3dws.11)#cluster-shared-volumes>) et de [Stockage](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831630(v%3dws.11)#storage-design>) de [La conception de votre Infrastructure Cloud](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831630(v%3dws.11)) <br>- [Espaces de stockage](../storage-spaces/overview.md)<br>- [Vue d’ensemble de RESILIENT File System (ReFS)](../refs/refs-overview.md)
