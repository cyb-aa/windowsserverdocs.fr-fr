---
title: "Modifier les propriétés du modèle de filtre de fichiers"
description: "Cet article explique comment modifier les propriétés d'un modèle de filtre de fichiers"
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 31ca46707a32d23a5dd9606c57bcaec5d6e53a80
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# <a name="edit-file-screen-template-properties"></a>Modifier les propriétés du modèle de filtre de fichiers

> S’applique à: WindowsServer (canal semi-annuel), WindowsServer2016, WindowsServer2012R2, WindowsServer2012, WindowsServer2008R2

Lorsque vous modifiez un modèle de filtre de fichiers, vous avez la possibilité d’étendre ces modifications aux filtres de fichiers qui ont été créés avec le modèle de filtre de fichiers d’origine. Vous pouvez choisir de modifier uniquement les filtres de fichiers qui correspondent au modèle d’origine ou tous les filtres de fichiers qui en sont dérivés, quelles que soient les modifications qui leur ont été apportées depuis leur création. Cette fonctionnalité simplifie le processus de mise à jour des propriétés des filtres de fichiers, puisque toutes les modifications peuvent s’effectuer au même endroit.

> [!Note]
> Si vous appliquez les modifications à tous les filtres de fichiers dérivés du modèle d’origine, toutes les propriétés de filtre de fichiers personnalisées que vous avez créées seront remplacées.

## <a name="to-edit-file-screen-template-properties"></a>Pour modifier les propriétés du modèle de filtre de fichiers

1.  Dans **Modèles de filtres de fichiers**, sélectionnez le modèle à modifier.

2.  Cliquez avec le bouton droit sur le modèle de filtre de fichiers, puis cliquez sur **Modifier les propriétés du modèle** (ou dans le volet **Actions**, sous **Modèles de filtres de fichiers sélectionnés**, sélectionnez **Modifier les propriétés du modèle**.) La boîte de dialogue **Propriétés du modèle de filtre de fichiers** s'affiche.

3.  Si vous souhaitez copier les propriétés d'un autre modèle pour les utiliser comme base de votre modèle modifié, sélectionnez un modèle dans la liste déroulante **Copier les propriétés du modèle**. Cliquez ensuite sur **Copier**.

4.  Effectuez toutes les modifications nécessaires. Les paramètres et les options de notification sont les mêmes que pour la création d'un modèle de filtre de fichiers.

5.  Lorsque vous avez terminé de modifier les propriétés du modèle, cliquez sur **OK**. La boîte de dialogue **Mettre à jour les filtres de fichiers dérivés du modèle** s'affiche.

6.  Sélectionnez le type de mise à jour à appliquer:

    -   Si vous avez des filtres de fichiers qui ont été modifiés depuis leur création à l’aide du modèle d’origine et que vous ne souhaitez pas modifier, cliquez sur **Appliquer le modèle uniquement aux filtres de fichiers dérivés qui correspondent au modèle d’origine**. Cette option met à jour uniquement les filtres de fichiers qui n’ont pas été modifiés depuis qu'ils ont été créés avec les propriétés du modèle d’origine.
    -   Si vous souhaitez modifier tous les filtres de fichiers existants créés à l’aide du modèle d’origine, cliquez sur **Appliquer le modèle à tous les filtres de fichiers dérivés**.
    -   Si vous voulez que les filtres de fichiers existants restent inchangés, cliquez sur **Ne pas appliquer le modèle aux filtres de fichiers dérivés**.

7.  Cliquez sur **OK**.

## <a name="see-also"></a>Articles associés

-   [Gestion du filtrage de fichiers](file-screening-management.md)
-   [Créer un modèle de filtre de fichiers](create-file-screen-template.md)


