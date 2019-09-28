---
title: Créer un quota automatique
description: Cet article décrit la création de quotas automatiques basée sur un modèle de quota
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 68967ff920f25c05affc206ed45bad9275e781b6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394236"
---
# <a name="create-an-auto-apply-quota"></a>Créer un quota automatique

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

À l'aide de quotas automatiques, vous pouvez affecter un modèle de quota à un volume ou un dossier parent. Le Gestionnaire de ressources du serveur de fichiers génère automatiquement des quotas basés sur ce modèle. Les quotas sont générés pour chacun des sous-dossiers existants et pour les sous-dossiers créés à l’avenir.

Par exemple, vous pouvez définir un quota automatique pour les sous-dossiers créés à la demande, pour des utilisateurs à profil itinérant ou pour les nouveaux utilisateurs. Chaque fois qu’un sous-dossier est créé, une nouvelle entrée de quota est automatiquement générée à l'aide du modèle à partir du dossier parent. Ces entrées de quota générées automatiquement peuvent ensuite être affichées en tant que quotas individuels sous le nœud **Quotas**. Chaque entrée de quota peut être gérée séparément.

## <a name="to-create-an-auto-apply-quota"></a>Pour créer un quota automatique

1.  Dans **Gestion de quota**, cliquez sur le nœud **Quotas**.

2.  Cliquez avec le bouton droit sur **Quotas**, puis cliquez sur **Créer un quota** (ou sélectionnez **Créer un quota** dans le volet **Actions**). La boîte de dialogue **Créer un quota** s’affiche.

3.  Sous **Chemin d'accès du quota**, tapez le nom du dossier parent auquel s’applique le profil du quota ou accédez-y. Le quota automatique s'appliquera à chacun des sous-dossiers (actuels et futurs) dans ce dossier.

4.  Cliquez sur **Appliquer automatiquement le modèle et créer des quotas sur les sous-dossiers existants et nouveaux**.

5.  Sous **Dériver les propriétés de ce modèle de quota**, sélectionnez le modèle de quota que vous souhaitez appliquer à partir de la liste déroulante. Notez que les propriétés de chaque modèle sont affichées sous **Résumé des propriétés de quota**.

6.  Cliquez sur **Create (Créer)** .

> [!Note]
> Vous pouvez vérifier tous les quotas générés automatiquement en sélectionnant le nœud **Quotas**, puis en sélectionnant **Actualiser**. Un quota individuel pour chaque sous-dossier et le profil de quota automatique sont répertoriés dans le volume ou dossier parent.

## <a name="see-also"></a>Voir aussi

-   [Gestion de quota](quota-management.md)
-   [Modifier les propriétés de quota automatique](edit-auto-apply-quota-properties.md)