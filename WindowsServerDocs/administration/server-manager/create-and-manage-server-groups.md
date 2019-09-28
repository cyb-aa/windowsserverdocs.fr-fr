---
title: créer et gérer des groupes de serveurs
description: Gestionnaire de serveur
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-server-manager
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9d5b1be8-49fd-4ff7-9580-e4ff21fe4b17
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 74075f091cd707f6faf73567c1dce6f22a2f6753
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383218"
---
# <a name="create-and-manage-server-groups"></a>créer et gérer des groupes de serveurs

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Cette rubrique décrit comment créer des groupes de serveurs personnalisés définis par l’utilisateur dans Gestionnaire de serveur dans Windows Server.

## <a name="BKMK_groups"></a>Groupes de serveurs
Les serveurs que vous ajoutez au pool de serveurs sont affichés dans la page **tous les serveurs** de gestionnaire de serveur. Vous pouvez créer des groupes de serveurs personnalisés avec les serveurs que vous avez ajoutés. Les groupes de serveurs vous permettent d’afficher et de gérer un sous-ensemble plus petit de votre pool de serveurs en tant qu’unité logique ; par exemple, vous pouvez créer un groupe nommé **serveurs de gestion des comptes** pour tous les serveurs du service comptabilité de votre organisation ou un groupe nommé **Chicago** pour tous les serveurs situés géographiquement à Chicago. Après avoir créé un groupe de serveurs, la page d’installation du groupe dans Gestionnaire de serveur affiche des informations sur les événements, les services, les compteurs de performances, les Best Practices Analyzer les résultats et les rôles et fonctionnalités installés pour le groupe dans son ensemble.

Les serveurs peuvent être membres de plusieurs groupes.

#### <a name="to-create-a-new-server-group"></a>Pour créer un nouveau groupe de serveurs

1.  Dans le menu **gérer** , cliquez sur **créer un groupe de serveurs**.

2.  Dans la zone de texte **Nom du groupe de serveurs**, tapez un nom convivial pour le groupe de serveurs, tel que **Serveurs de comptabilité**.

3.  Ajoutez des serveurs à la liste **sélectionnée** à partir du pool de serveurs ou ajoutez d’autres serveurs au groupe à l’aide des onglets **Active Directory**, **DNS**ou **Importer** . Pour plus d’informations sur l’utilisation de ces onglets, consultez [Ajouter des serveurs à gestionnaire de serveur](add-servers-to-server-manager.md) dans ce guide.

4.  Une fois que vous avez fini d’ajouter des serveurs au groupe, cliquez sur **OK**. Le nouveau groupe s’affiche dans le volet de navigation Gestionnaire de serveur sous le groupe **tous les serveurs** .

#### <a name="to-edit-an-existing-server-group"></a>Pour modifier un groupe de serveurs existant

1.  Effectuez l’une des opérations suivantes :

    -   Dans le volet de navigation Gestionnaire de serveur, cliquez avec le bouton droit sur un groupe de serveurs, puis cliquez sur **modifier le groupe de serveurs**.

    -   Sur la page d’hébergement du groupe de serveurs, ouvrez le menu **tâches** sur la vignette **serveurs** , puis cliquez sur modifier le **groupe de serveurs**.

2.  Modifiez le nom du groupe ou ajoutez ou supprimez des serveurs dans le groupe.

    > [!NOTE]
    > la suppression de serveurs d’un groupe de serveurs ne supprime pas les serveurs de Gestionnaire de serveur. Les serveurs que vous supprimez dans un groupe restent dans le groupe **Tous les serveurs**, dans le pool de serveurs.

3.  Une fois que vous avez fini de modifier le groupe, cliquez sur **OK**.

#### <a name="to-delete-an-existing-server-group"></a>Pour supprimer un groupe de serveurs existant

1.  Effectuez l’une des opérations suivantes :

    -   Dans le volet de navigation Gestionnaire de serveur, cliquez avec le bouton droit sur un groupe de serveurs, puis cliquez sur **supprimer le groupe de serveurs**.

    -   Sur la page d’hébergement du groupe de serveurs, ouvrez le menu **tâches** sur la vignette **serveurs** , puis cliquez sur supprimer le **groupe de serveurs**.

2.  Lorsque vous êtes invité à confirmer la suppression du groupe de serveurs, cliquez sur **Oui**.

    > [!NOTE]
    > la suppression d’un groupe de serveurs ne supprime pas les serveurs de Gestionnaire de serveur. Les serveurs qui figuraient dans un groupe que vous avez supprimé restent dans le groupe **Tous les serveurs** , dans le pool de serveurs.

3.  Une fois que vous avez fini de modifier le groupe, cliquez sur **OK**.

## <a name="see-also"></a>Voir aussi
[Ajouter des serveurs à Gestionnaire de serveur](add-servers-to-server-manager.md)
[Gestionnaire de serveur](server-manager.md)



