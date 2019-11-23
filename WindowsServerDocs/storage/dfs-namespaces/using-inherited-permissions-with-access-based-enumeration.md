---
title: Utilisation d'autorisations héritées avec l'énumération basée sur l'accès
description: Cet article explique comment utiliser des autorisations héritées avec l’énumération basée sur l’accès
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 433fe53a3d580aafc50b152ec20156436b05481f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402141"
---
# <a name="using-inherited-permissions-with-access-based-enumeration"></a>Utilisation d’autorisations héritées avec l’énumération basée sur l’accès

> S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 et Windows Server 2008

Par défaut, les autorisations utilisées pour un dossier DFS sont héritées du système de fichiers local du serveur d’espaces de noms. Les autorisations sont héritées du répertoire racine du lecteur système et accordent au domaine\\les autorisations de lecture du groupe utilisateurs. Par conséquent, même après l’activation de l’énumération basée sur l’accès, tous les dossiers de l’espace de noms restent visibles pour tous les utilisateurs du domaine.

## <a name="advantages-and-limitations-of-inherited-permissions"></a>Avantages et limites des autorisations héritées

Il existe deux avantages principaux à l’utilisation d’autorisations héritées pour contrôler les utilisateurs pouvant afficher des dossiers dans un espace de noms DFS :

-   Vous pouvez rapidement appliquer des autorisations héritées à de nombreux dossiers sans avoir à utiliser de scripts.
-   Vous pouvez appliquer des autorisations héritées à des racines d’espaces de noms et des dossiers sans cibles.

Malgré leurs avantages, les autorisations héritées dans les espaces de noms DFS présentent de nombreuses limites qui les rendent inappropriées pour la plupart des environnements :

-   Les modifications apportées à des autorisations héritées ne sont pas répliquées vers d’autres serveurs d’espaces de noms. Par conséquent, utilisez des autorisations héritées uniquement sur des espaces de noms autonomes ou dans des environnements où vous pouvez implémenter un système de réplication tiers afin de maintenir les listes de contrôle d’accès (ACL) sur tous les serveurs d’espaces de noms synchronisés.
-   La gestion DFS et **Dfsutil** ne peuvent pas afficher ni modifier les autorisations héritées. Par conséquent, vous devez utiliser l’Explorateur Windows ou la commande **Icacls** en plus de la gestion DFS ou de **Dfsutil** pour gérer l’espace de noms.
-   Lorsque vous utilisez des autorisations héritées, la seule façon de modifier les autorisations d’un dossier avec cibles est à l’aide de la commande **Dfsutil**. Les espaces de noms DFS suppriment automatiquement les autorisations des dossiers avec cibles définies à l’aide d’autres outils ou méthodes.
-   Si vous définissez des autorisations sur un dossier avec cibles pendant que vous utilisez des autorisations héritées, la liste ACL définie sur le dossier avec cibles s’associe aux autorisations héritées du parent du dossier dans le système de fichiers. Vous devez examiner les deux jeux d’autorisations pour déterminer les autorisations appliquées.

> [!NOTE]
> Lorsque vous utilisez des autorisations héritées, il est plus simple de définir des autorisations sur des racines d’espaces de noms et des dossiers sans cibles. Utilisez ensuite des autorisations héritées sur des dossiers avec cibles afin qu’ils héritent de toutes les autorisations de leurs parents.

## <a name="using-inherited-permissions"></a>Utilisation d’autorisations héritées

Pour limiter les utilisateurs autorisés à afficher un dossier DFS, vous devez effectuer l’une des tâches suivantes :

-   **Définissez des autorisations explicites pour le dossier, en désactivant l’héritage.** Pour définir des autorisations explicites sur un dossier avec cibles (un lien) à l’aide de la gestion DFS ou de la commande **Dfsutil**, voir [Activer l’énumération basée sur l’accès pour un espace de noms](enable-access-based-enumeration-on-a-namespace.md).
-   **Modifiez les autorisations héritées sur le parent dans le système de fichiers local**. Pour modifier les autorisations héritées par un dossier avec cibles, si vous avez déjà défini des autorisations explicites sur le dossier, basculez des autorisations explicites vers les autorisations héritées, comme indiqué dans la procédure suivante. Utilisez l’Explorateur Windows ou la commande **Icacls** pour modifier les autorisations du dossier à partir duquel le dossier avec cibles hérite de ses autorisations.

> [!NOTE]
> L’énumération basée sur l’accès n’empêche pas les utilisateurs d’obtenir une référence vers une cible de dossier s’ils connaissent déjà le chemin d’accès DFS du dossier avec cibles. Les autorisations définies à l’aide de l’Explorateur Windows ou de la commande **Icacls** sur des racines d’espaces de noms ou des dossiers sans cibles déterminent si les utilisateurs peuvent accéder au dossier DFS ou à la racine de l’espace de noms. Toutefois, elles n’empêchent pas les utilisateurs d’accéder directement à un dossier avec cibles. Seules les autorisations de partage ou les autorisations du système de fichiers NTFS du dossier partagé lui-même peuvent empêcher les utilisateurs d’accéder aux cibles d’un dossier.

## <a name="to-switch-from-explicit-permissions-to-inherited-permissions"></a>Pour basculer des autorisations explicites vers les autorisations héritées

1.  Dans l’arborescence de la console, sous le nœud **Espaces de noms**, recherchez le dossier avec cibles dont vous voulez contrôler la visibilité, cliquez dessus avec le bouton droit, puis cliquez sur **Propriétés**.

2.  Cliquez sur l'onglet **Avancé**.

3.  Cliquez sur **Utiliser les autorisations héritées du système de fichiers local**, puis cliquez sur **OK** dans la boîte de dialogue **Confirmer l’utilisation des autorisations héritées**. Cette opération supprime toutes les autorisations explicitement définies sur ce dossier et restaure les autorisations NTFS héritées du système de fichiers local du serveur d’espaces de noms.

4.  Pour modifier les autorisations héritées de dossiers ou de racines d’espaces de noms dans un espace de noms DFS, utilisez l’Explorateur Windows ou la commande **ICacls**.

## <a name="see-also"></a>Voir également

-   [Créer un espace de noms DFS](create-a-dfs-namespace.md)