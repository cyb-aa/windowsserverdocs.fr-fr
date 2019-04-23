---
title: créer et gérer des groupes de serveurs
description: Gestionnaire de serveur
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 32e20040e2cb075e447c0d03d48676c7011a5a92
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889480"
---
# <a name="create-and-manage-server-groups"></a>créer et gérer des groupes de serveurs

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette rubrique décrit comment créer des groupes personnalisés définis par l’utilisateur de serveurs dans le Gestionnaire de serveur dans Windows Server.

## <a name="BKMK_groups"></a>Groupes de serveurs
Les serveurs que vous ajoutez au pool de serveurs sont affichés dans le **tous les serveurs** page dans le Gestionnaire de serveur. Vous pouvez créer des groupes de serveurs personnalisés avec les serveurs que vous avez ajoutés. Serveur les groupes vous permettent d’afficher et gérer un plus petit sous-ensemble de votre pool de serveurs comme une unité logique ; par exemple, vous pouvez créer un groupe appelé **serveurs de comptabilité** pour tous les serveurs de votre organisation du service comptabilité, ou un groupe nommé **Chicago** pour tous les serveurs situés géographiquement à Chicago. Après avoir créé un groupe de serveurs, la page d’accueil du groupe dans le Gestionnaire de serveur affiche des informations sur les événements, les services, les compteurs de performance, résultats de Best Practices Analyzer et installé des rôles et des fonctionnalités pour le groupe dans son ensemble.

Les serveurs peuvent être membres de plusieurs groupes.

#### <a name="to-create-a-new-server-group"></a>Pour créer un nouveau groupe de serveurs

1.  Sur le **gérer** menu, cliquez sur **créer le groupe de serveurs**.

2.  Dans la zone de texte **Nom du groupe de serveurs**, tapez un nom convivial pour le groupe de serveurs, tel que **Serveurs de comptabilité**.

3.  ajouter des serveurs à la **sélectionné** liste du pool de serveurs, ou ajouter d’autres serveurs au groupe à l’aide de la **active directory**, **DNS**, ou **importer**onglets. Pour plus d’informations sur l’utilisation de ces onglets, consultez [ajouter des serveurs au Gestionnaire de serveur](add-servers-to-server-manager.md) dans ce guide.

4.  Une fois que vous avez fini d’ajouter des serveurs au groupe, cliquez sur **OK**. Le nouveau groupe s’affiche dans le volet de navigation de gestionnaire de serveur sous la **tous les serveurs** groupe.

#### <a name="to-edit-an-existing-server-group"></a>Pour modifier un groupe de serveurs existant

1.  Effectuez l’une des opérations suivantes :

    -   Dans le volet de navigation du Gestionnaire de serveur avec le bouton droit à un groupe de serveurs, puis cliquez sur **modifier le groupe de serveurs**.

    -   Sur la page d’accueil du groupe de serveurs, ouvrez le **tâches** menu sur le **serveurs** vignette, puis cliquez sur **modifier le groupe de serveurs**.

2.  modifier le nom du groupe, ou ajouter ou supprimer des serveurs du groupe.

    > [!NOTE]
    > suppression de serveurs à partir d’un groupe de serveurs ne supprime pas les serveurs à partir du Gestionnaire de serveur. Les serveurs que vous supprimez dans un groupe restent dans le groupe **Tous les serveurs**, dans le pool de serveurs.

3.  Une fois que vous avez fini de modifier le groupe, cliquez sur **OK**.

#### <a name="to-delete-an-existing-server-group"></a>Pour supprimer un groupe de serveurs existant

1.  Effectuez l’une des opérations suivantes :

    -   Dans le volet de navigation du Gestionnaire de serveur avec le bouton droit à un groupe de serveurs, puis cliquez sur **supprimer le groupe de serveurs**.

    -   Sur la page d’accueil du groupe de serveurs, ouvrez le **tâches** menu sur le **serveurs** vignette, puis cliquez sur **supprimer le groupe de serveurs**.

2.  Lorsque vous êtes invité à confirmer la suppression du groupe de serveurs, cliquez sur **Oui**.

    > [!NOTE]
    > suppression d’un groupe de serveurs ne supprime pas les serveurs à partir du Gestionnaire de serveur. Les serveurs qui figuraient dans un groupe que vous avez supprimé restent dans le groupe **Tous les serveurs** , dans le pool de serveurs.

3.  Une fois que vous avez fini de modifier le groupe, cliquez sur **OK**.

## <a name="see-also"></a>Voir aussi
[ajouter des serveurs au Gestionnaire de serveur](add-servers-to-server-manager.md)
[le Gestionnaire de serveur](server-manager.md)



