---
title: "Modifier les propriétés de quota automatique"
description: "Cet article décrit la procédure pour modifier les propriétés de quota automatique"
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: aa2155268d42293ade925d53da5e29142d13aae4
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# <a name="edit-auto-apply-quota-properties"></a>Modifier les propriétés de quota automatique

> S’applique à: WindowsServer (canal semi-annuel), WindowsServer2016, WindowsServer2012R2, WindowsServer2012, WindowsServer2008R2

Lorsque vous modifiez un quota automatique, vous avez la possibilité d’étendre ces modifications aux quotas existants dans le chemin d’accès du quota automatique. Vous pouvez choisir de modifier uniquement les quotas qui correspondent encore au quota automatique d’origine ou tous ceux qui figurent dans le chemin d’accès du quota automatique, quelles que soient les modifications qui leur ont été apportées depuis leur création. Cette fonctionnalité simplifie le processus de mise à jour des propriétés des quotas dérivées d'un quota automatique, puisque toutes les modifications peuvent s’effectuer au même endroit.

> [!Note]
> Si vous choisissez d’appliquer les modifications à tous les quotas figurant dans le chemin d’accès du quota automatique, toutes les propriétés du quota personnalisées que vous avez créées seront remplacées.

## <a name="to-edit-an-auto-apply-quota"></a>Pour modifier un quota automatique

1.  Dans **Quotas**, sélectionnez le quota automatique que vous souhaitez modifier. Vous pouvez filtrer les quotas pour afficher uniquement les quotas automatiques.

2.  Cliquez avec le bouton droit sur l’entrée du quota, puis cliquez sur **Modifier les propriétés de quota** (ou dans le volet **Actions**, sous **Quotas sélectionnés**, sélectionnez **Modifier les propriétés de quota**). La boîte de dialogue **Modifier le quota automatique** s’affiche.

3.  Sous **Dériver les propriétés de ce modèle de quota**, sélectionnez le modèle de quota que vous souhaitez appliquer. Vous pouvez vérifier les propriétés de chaque modèle de quota dans la zone de liste récapitulative.

4.  Cliquez sur **OK**. La boîte de dialogue **Mettre à jour les quotas dérivés du quota automatique** s'affiche.

5.  Sélectionnez le type de mise à jour à appliquer:

    -   Si vous avez des quotas qui ont été modifiés depuis leur génération automatique et que vous ne souhaitez pas modifier, sélectionnez **Appliquer le quota automatique uniquement aux quotas dérivés qui correspondent au quota automatique d’origine**. Cette option met à jour uniquement les quotas figurant dans le chemin d’accès du quota automatique qui n’ont pas été modifiés depuis leur génération automatique.
    -   Si vous souhaitez modifier tous les quotas existants dans le chemin d’accès du quota automatique, sélectionnez **Appliquer le quota automatique à tous les quotas dérivés**.
    -   Si vous voulez que les quotas existants restent inchangés, mais que le quota automatique modifié s'applique aux nouveaux sous-dossiers dans le chemin d’accès du quota automatique, sélectionnez **Ne pas appliquer le quota automatique aux quotas dérivés**.

6.  Cliquez sur **OK**.

## <a name="see-also"></a>Articles associés

-   [Gestion de quota](quota-management.md)
-   [Créer un quota automatique](create-auto-apply-quota.md)


