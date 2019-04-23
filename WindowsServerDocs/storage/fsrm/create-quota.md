---
title: Créer un quota
description: Cet article explique comment créer un quota basé sur un modèle
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: f3c677f5ebf7dda44f4b99a64d0fbf8d2c72b92e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883190"
---
# <a name="create-a-quota"></a>Créer un quota

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Vous pouvez créer des quotas à partir d’un modèle ou avec des propriétés personnalisées. La procédure suivante décrit comment créer un quota basé sur un modèle (recommandé). Si vous avez besoin de créer un quota avec des propriétés personnalisées, vous pouvez enregistrer ces propriétés sous forme de modèle à réutiliser ultérieurement.

Quand vous créez un quota, vous choisissez un chemin d’accès, c'est-à-dire un volume ou un dossier auquel la limite de stockage s’applique. Sur un chemin d’accès de quota donné, vous pouvez utiliser un modèle pour créer un des types de quota suivants :

-   Un quota unique qui limite l’espace pour un volume ou un dossier entier.
-   Un quota automatique qui affecte le modèle de quota à un dossier ou un volume. Les quotas basés sur ce modèle sont automatiquement générés et appliqués à tous les sous-dossiers. Pour plus d’informations sur la création de quotas automatiques, voir [Créer un quota automatique](create-auto-apply-quota.md).


> [!Note]
> En créant des quotas exclusivement à partir de modèles, vous pouvez centraliser leur gestion en mettant à jour les modèles au lieu de chaque quota. Vous pouvez ensuite appliquer les modifications à tous les quotas basés sur le modèle modifié. Cette fonctionnalité simplifie la mise en œuvre de modifications dans la stratégie de stockage, puisque toutes les mises à jour peuvent s’effectuer au même endroit.

## <a name="to-create-a-quota-that-is-based-on-a-template"></a>Pour créer un quota basé sur un modèle

1.  Dans **Gestion de quota**, cliquez sur le nœud **Modèles de quotas**.

2.  Dans le volet des résultats, sélectionnez le modèle sur la base duquel vous allez créer votre nouveau quota.

3.  Cliquez avec le bouton droit sur le modèle, puis cliquez sur **Créer un quota à partir d’un modèle** (ou sélectionnez **Créer un quota à partir d’un modèle** dans le volet **Actions**). La boîte de dialogue **Créer un Quota** s'affiche avec les propriétés de résumé du modèle de quota affiché.

4.  Sous **Chemin d'accès du quota**, tapez le dossier auquel s’applique le quota ou accédez-y.

5.  Cliquez sur l' option **Créer un quota sur le chemin d’accès**. Notez que les propriétés de quota s'appliqueront à la totalité du dossier.

     > [!Note]
     > Pour créer un quota automatique, cliquez sur l'option **Appliquer automatiquement le modèle pour créer des quotas sur les sous-dossiers existants et les nouveaux sous-dossiers**. Pour plus d’informations sur les quotas automatiques, voir [Créer un quota automatique](create-auto-apply-quota.md).

6.  Sous **Dériver les propriétés de ce modèle de quota**, le modèle que vous avez utilisé à l’étape 2 pour créer le quota est présélectionné (ou vous pouvez sélectionner un autre modèle dans la liste). Notez que les propriétés du modèle sont affichées sous **Résumé des propriétés de quota**.

7.  Cliquez sur **Create (Créer)**.

## <a name="see-also"></a>Voir aussi

-   [Gestion de quota](quota-management.md)
-   [Créer une fonction automatique d’un quota](create-auto-apply-quota.md)
-   [Créer un modèle de Quota](create-quota-template.md)


