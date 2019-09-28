---
title: Gérer des domaines différents dans Centre d’administration Active Directory
ms.prod: windows-server
description: Sécurité de Windows Server
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.assetid: 166351c3-4076-48be-aa8f-797adf1e9d68
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 71edf6bb38cc665fe5c780ce986d0c0b8807d6ab
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386931"
---
# <a name="manage-different-domains-in-active-directory-administrative-center"></a>Gérer des domaines différents dans Centre d’administration Active Directory

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2016

  Lorsque vous ouvrez Active Directory administrative, le domaine sur lequel vous êtes actuellement connecté sur cet ordinateur \(the local Domain @ no__t-1 apparaît dans le volet de navigation Centre d’administration Active Directory \(Le volet gauche @ no__t-3. En fonction des droits de votre jeu actuel d’informations d’identification d’ouverture de session, vous pouvez afficher ou gérer les objets Active Directory dans ce domaine local.

 Vous pouvez également utiliser le même jeu d’informations d’identification d’ouverture de session et la même instance de Centre d’administration Active Directory pour afficher ou gérer des objets Active Directory dans n’importe quel autre domaine de la même forêt, ou un domaine dans une autre forêt qui a une approbation établie avec le Domain. Les approbations @ no__t-0way et les deux approbations @ no__t-1way sont toutes deux prises en charge.

> [!NOTE]
>  S’il existe une approbation @ no__t-0way entre le domaine A et le domaine B par le biais desquels les utilisateurs du domaine A peuvent accéder aux ressources du domaine B, mais que les utilisateurs du domaine B ne peuvent pas accéder aux ressources du domaine A, si vous exécutez Centre d’administration Active Directory sur l’ordinateur où Le domaine A est votre domaine local, vous pouvez vous connecter au domaine B avec l’ensemble actuel d’informations d’identification d’ouverture de session et dans la même instance de Centre d’administration Active Directory. Toutefois, si vous exécutez Centre d’administration Active Directory sur l’ordinateur où le domaine B est votre domaine local, vous ne pouvez pas vous connecter au domaine A avec le même jeu d’informations d’identification dans la même instance du Centre d’administration Active Directory.

 Pour mener à bien cette procédure, il n’est pas nécessaire d’appartenir à un groupe particulier.

### <a name="windows-server-2012-to-manage-a-foreign-domain-in-the-selected-instance-of-active-directory-administrative-center-using-the-current-set-of-logon-credentials"></a>Windows Server 2012 : Pour gérer un domaine étranger dans l’instance sélectionnée de Centre d’administration Active Directory à l’aide de l’ensemble actuel d’informations d’identification d’ouverture de session

1.  Pour ouvrir Centre d’administration Active Directory, dans **Gestionnaire de serveur**, cliquez sur **Outils**, puis sur **Centre d’administration Active Directory**.

    > [!NOTE]
    >  Pour ouvrir Centre d’administration Active Directory, vous pouvez également cliquer sur **Démarrer**, puis taper **DSAC. exe**.

2.  Pour ouvrir **Ajouter des nœuds de navigation**, cliquez sur **gérer**, puis sur **Ajouter des nœuds de navigation** comme indiqué dans l’illustration suivante.

     ![Capture d’écran montrant * * ajouter des nœuds de navigation * * UI](media/ADDS_ADACAddNavNode.gif)

3.  Dans **Ajouter des nœuds de navigation**, cliquez sur **se connecter à d’autres domaines** comme indiqué dans l’illustration suivante.

     ![Capture d’écran montrant * * ajouter des nœuds de navigation * * UI](media/ADDS_ADACConnectToDomain.gif)

4.  Dans **se connecter à**, tapez le nom du domaine étranger que vous souhaitez gérer \(Pour exemple, **contoso.com**\), puis cliquez sur **OK**.

5.  Lorsque vous êtes connecté au domaine étranger, parcourez les colonnes de la fenêtre **Ajouter des nœuds de navigation** , sélectionnez le ou les conteneurs à ajouter à votre volet de navigation Centre d’administration Active Directory, puis cliquez sur **OK.** .

### <a name="windows-server-2008-r2-to-manage-a-foreign-domain-in-the-selected-instance-of-active-directory-administrative-center-using-the-current-set-of-logon-credentials"></a>Windows Server 2008 R2 : Pour gérer un domaine étranger dans l’instance sélectionnée de Centre d’administration Active Directory à l’aide de l’ensemble actuel d’informations d’identification d’ouverture de session

1. Pour ouvrir Centre d’administration Active Directory, cliquez sur **Démarrer**, sur **Outils d’administration**, puis sur **Centre d’administration Active Directory**.

   > [!NOTE]
   >  Pour ouvrir Centre d’administration Active Directory, vous pouvez également cliquer sur **Démarrer**, sur **exécuter**, puis taper **DSAC. exe**.

2. Pour ouvrir **Ajouter des nœuds de navigation**, en haut de la fenêtre de centre d’administration Active Directory, cliquez sur **Ajouter des nœuds de navigation** comme indiqué dans l’illustration suivante.

    ![Capture d’écran montrant * * ajouter des nœuds de navigation * * UI](media/click_add_nav_nodes.gif)

   > [!NOTE]
   >  Une autre façon d’ouvrir **Ajouter des nœuds de navigation** est de cliquer avec le bouton droit sur @ no__t-1Cliquez n’importe où dans l’espace vide dans le volet de navigation Centre d’administration Active Directory, puis de cliquer sur Ajouter des **nœuds de navigation**.

3. Dans **Ajouter des nœuds de navigation**, cliquez sur **se connecter à d’autres domaines** comme indiqué dans l’illustration suivante.

    ![Capture d’écran montrant * * ajouter des nœuds de navigation * * * * se connecter à d’autres domaines * * IU](media/add_nav_nodes.gif)

4. Dans **se connecter à**, tapez le nom du domaine étranger que vous souhaitez gérer \(Pour exemple, **contoso.com**\), puis cliquez sur **OK**.

5. Lorsque vous êtes connecté au domaine étranger, parcourez les colonnes de la fenêtre **Ajouter des nœuds de navigation** , sélectionnez le ou les conteneurs à ajouter à votre volet de navigation Centre d’administration Active Directory, puis cliquez sur **OK.** .

   Pour plus d’informations sur la personnalisation du volet de navigation Centre d’administration Active Directory, consultez [personnaliser le volet de navigation des centre d’administration Active Directory](customize-the-active-directory-administrative-center-navigation-pane.md).

   Vous pouvez également ouvrir Centre d’administration Active Directory à l’aide d’un ensemble d’informations d’identification de connexion différent de votre jeu actuel d’informations d’identification d’ouverture de session. La commande de la procédure suivante peut être utile si vous êtes connecté à l’ordinateur qui exécute Centre d’administration Active Directory avec des informations d’identification d’utilisateur normales, mais que vous souhaitez utiliser Centre d’administration Active Directory sur cet ordinateur pour gérer votre domaine local en tant qu’administrateur. la commande \(This peut également être utile si vous souhaitez utiliser Centre d’administration Active Directory pour gérer à distance un domaine étranger différent de votre domaine local avec un ensemble d’informations d’identification différent de votre jeu actuel d’informations d’identification d’ouverture de session. Toutefois, le domaine étranger doit avoir une approbation établie avec le domaine local. \)

   Pour mener à bien cette procédure, il n’est pas nécessaire d’appartenir à un groupe particulier.

### <a name="to-manage-a-domain-using-logon-credentials-that-are-different-from-the-current-set-of-logon-credentials"></a>Pour gérer un domaine à l’aide d’informations d’identification autres que les vôtres

1.  Pour ouvrir Centre d’administration Active Directory, à l’invite de commandes, tapez la commande suivante, puis appuyez sur entrée :

     `runas /user:<domain\user> dsac`

     Où `<domain\user>` est l’ensemble d’informations d’identification que vous souhaitez ouvrir Centre d’administration Active Directory avec et `dsac` est le Centre d’administration Active Directory nom de fichier exécutable @no__t -2Dsac. exe @ no__t-3.

     Par exemple, tapez la commande suivante et appuyez sur Entrée :

     `runas /user:contoso\administrator dsac`

2.  Lorsque Centre d’administration Active Directory est ouvert, parcourez le volet de navigation pour afficher ou gérer votre domaine Active Directory.

  

