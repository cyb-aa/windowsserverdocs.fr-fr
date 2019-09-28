---
title: Modifier les propriétés du modèle de quota
description: Cet article explique comment modifier les propriétés du modèle de quota pour étendre les modifications aux quotas créés à partir du modèle de quota d’origine
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 37719656e107869b97045af98c1a63744e4f6b38
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403023"
---
# <a name="edit-quota-template-properties"></a>Modifier les propriétés du modèle de quota

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Lorsque vous modifiez un modèle de quota, vous avez la possibilité d’étendre ces modifications aux quotas qui ont été créés à partir du modèle de quota d’origine. Vous pouvez choisir de modifier uniquement les quotas qui correspondent encore au modèle d’origine ou tous les quotas qui en sont dérivés, quelles que soient les modifications apportées aux quotas depuis leur création. Cette fonctionnalité simplifie le processus de mise à jour des propriétés de vos quotas, puisque toutes les modifications peuvent s’effectuer au même endroit.

> [!Note]
> Si vous choisissez d'appliquer les modifications à tous les quotas dérivés du modèle d’origine, toutes les propriétés de quota personnalisées que vous avez créées seront remplacées.

## <a name="to-edit-quota-template-properties"></a>Pour modifier les propriétés du modèle de quota

1.  Dans **Modèles de quotas**, sélectionnez le modèle à modifier.

2.  Cliquez avec le bouton droit sur le modèle de quota, puis cliquez sur **Modifier les propriétés du modèle** (ou dans le volet **Actions**, sous **Modèles de quotas**, sélectionnez **Modifier les propriétés du modèle**). La boîte de dialogue **Propriétés du modèle de quota** s’affiche.

3.  Effectuez toutes les modifications nécessaires. Les paramètres et les options de notification que vous pouvez définir sont les mêmes que pour la création d'un modèle de quota. Si vous le souhaitez, vous pouvez copier les propriétés d’un autre modèle et les modifier pour celui-ci.

4.  Lorsque vous avez terminé de modifier les propriétés du modèle, cliquez sur **OK**. La boîte de dialogue **Mettre à jour les quotas dérivés du modèle** s'affiche.

5.  Sélectionnez le type de mise à jour à appliquer :

    -   Si vous avez des quotas qui ont été modifiés depuis leur création à l’aide du modèle d’origine et que vous ne souhaitez pas modifier, sélectionnez **Appliquer le modèle uniquement aux quotas dérivés qui correspondent au modèle d’origine**. Cette option met à jour uniquement les quotas qui n’ont pas été modifiés depuis leur création avec le modèle d’origine.
    -   Si vous souhaitez modifier tous les quotas existants créés à partir du modèle d’origine, sélectionnez **Appliquer le modèle à tous les quotas dérivés**.
    -   Si vous voulez que les quotas existants restent inchangés, sélectionnez **Ne pas appliquer le modèle aux quotas dérivés**.

6.  Cliquez sur **OK**.

## <a name="see-also"></a>Voir aussi

-   [Gestion de quota](quota-management.md)
-   [Créer un modèle de quota](create-quota-template.md)


