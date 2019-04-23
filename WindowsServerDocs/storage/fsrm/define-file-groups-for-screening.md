---
title: Définir des groupes de fichiers pour le filtrage
description: Cet article décrit comment définir des groupes de fichiers pour créer un espace de noms pour un filtre de fichiers, une exception au filtre de fichiers ou des rapports de stockage fichiers par groupe de fichiers
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 6e043692500370b6c084a4db068027d13afc957f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838430"
---
# <a name="define-file-groups-for-screening"></a>Définir des groupes de fichiers pour le filtrage

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Un *groupe de fichiers* permet de créer un espace de noms pour un filtre de fichiers, une exception au filtre de fichiers ou un rapport de stockage **Fichiers par groupe de fichiers**. Il se compose d’un ensemble de modèles de nom de fichier, qui sont regroupés par les éléments suivants :

-   **Fichiers à inclure** : fichiers qui appartiennent au groupe
-   **Fichiers à exclure** : fichiers qui n'appartiennent pas au groupe

> [!Note]
> Pour plus de commodité, vous pouvez créer et modifier des groupes de fichiers lorsque vous modifiez les propriétés des filtres de fichiers, les exceptions au filtre de fichiers, les modèles de filtre de fichiers et les rapports **Fichiers par groupe de fichiers**. Les modifications de groupe de fichiers que vous effectuez à partir de ces feuilles de propriétés ne se limitent pas à l’élément que vous utilisez maintenant.

## <a name="to-create-a-file-group"></a>Pour créer un groupe de fichiers

1.  Dans **Gestion du filtrage de fichiers**, cliquez sur le nœud **Groupe de fichiers**.

2.  Dans le volet **Actions**, cliquez sur **Créer un groupe de fichiers**. La boîte de dialogue **Créer les propriétés du groupe de fichiers** s’affiche.

    (Autre possibilité : lorsque vous modifiez les propriétés d'un filtre de fichiers, une exception au filtre de fichiers, un modèle de filtre de fichiers ou un rapport **Fichiers par groupe de fichiers**, sous **Gérer les groupes de fichiers**, cliquez sur **Créer**.)

3.  Dans la boîte de dialogue **Créer les propriétés du groupe de fichiers**, tapez un nom pour le groupe de fichiers.

4.  Ajoutez des fichiers à inclure et des fichiers à exclure :

    -   Pour chaque ensemble de fichiers que vous souhaitez inclure dans le groupe de fichiers, dans la zone **Fichiers à inclure**, entrez un modèle de nom de fichier, puis cliquez sur **Ajouter**.
    -   Pour chaque ensemble de fichiers que vous souhaitez exclure du groupe de fichiers, dans la zone **Fichiers à exclure**, entrez un modèle de nom de fichier, puis cliquez sur **Ajouter**.
        Notez que les règles génériques standard s’appliquent, par exemple,  **\*.exe** sélectionne tous les fichiers exécutables.

5.  Cliquez sur **OK**.

## <a name="see-also"></a>Voir aussi

-   [Gestion des filtres de fichiers](file-screening-management.md)
-   [Créer un filtre de fichiers](create-file-screen.md)
-   [Créer une Exception de filtre de fichier](create-file-screen-exception.md)
-   [Créer un modèle d’écran de fichier](create-file-screen-template.md)
-   [Gestion des rapports de stockage](storage-reports-management.md)


