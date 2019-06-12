---
title: Gérer des domaines différents dans le centre d’administration Active Directory
ms.prod: windows-server-threshold
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
ms.openlocfilehash: bd5650724272422d09e87b7eecf10f825b00fabf
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447043"
---
# <a name="manage-different-domains-in-active-directory-administrative-center"></a>Gérer des domaines différents dans le centre d’administration Active Directory

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

  Lorsque vous ouvrez Administration d’Active Directory, le domaine auquel vous êtes actuellement connecté sur cet ordinateur \(le domaine local\) apparaît dans le volet de navigation du centre d’administration Active Directory \(le volet gauche\). En fonction des droits de votre jeu actuel d’informations d’identification d’ouverture de session, vous pouvez afficher ou gérer les objets Active Directory dans ce domaine local.

 Vous pouvez également utiliser le même ensemble d’informations d’identification d’ouverture de session et de la même instance du centre d’administration Active Directory pour afficher ou gérer des objets Active Directory dans n’importe quel autre domaine dans la même forêt ou un domaine dans une autre forêt qui possède une approbation établie avec l’ordinateur local domaine. Les deux une\-approbations bidirectionnelles et deux\-approbations bidirectionnelles sont prises en charge.

> [!NOTE]
>  S’il existe un\-d’approbation bidirectionnelle entre le domaine A et le domaine B via laquelle les utilisateurs du domaine A peuvent accéder aux ressources du domaine B, mais les utilisateurs du domaine B ne peut pas accéder aux ressources du domaine A, si vous exécutez le centre d’administration Active Directory sur l’ordinateur où le domaine A est votre domaine local, vous pouvez vous connecter au domaine B avec l’ensemble d’informations d’identification d’ouverture de session et dans la même instance du centre d’administration Active Directory actuel. Mais si vous exécutez centre d’administration Active Directory sur l’ordinateur où le domaine B est votre domaine local, vous ne pouvez pas vous connecter au domaine A avec le même jeu d’informations d’identification dans la même instance du centre d’administration Active Directory.

 Pour mener à bien cette procédure, il n’est pas nécessaire d’appartenir à un groupe particulier.

### <a name="windows-server-2012-to-manage-a-foreign-domain-in-the-selected-instance-of-active-directory-administrative-center-using-the-current-set-of-logon-credentials"></a>Windows Server 2012 : Pour gérer un domaine étranger dans l’instance sélectionnée du centre d’administration Active Directory à l’aide de l’ensemble actuel des informations d’identification d’ouverture de session

1.  Pour ouvrir le centre d’administration Active Directory, dans **le Gestionnaire de serveur**, cliquez sur **outils**, puis cliquez sur **centre d’administration Active Directory**.

    > [!NOTE]
    >  Une autre façon d’ouvrir le centre d’administration Active Directory consiste à cliquer sur **Démarrer**, puis tapez **dsac.exe**.

2.  Pour ouvrir **ajouter des nœuds de Navigation**, cliquez sur **gérer**, puis cliquez sur **ajouter des nœuds de Navigation** comme indiqué dans l’illustration suivante.

     ![Capture d’écran ** Ajouter Navigation nœuds ** l’interface utilisateur](media/ADDS_ADACAddNavNode.gif)

3.  Dans **ajouter des nœuds de Navigation**, cliquez sur **se connecter à d’autres domaines** comme indiqué dans l’illustration suivante.

     ![Capture d’écran ** Ajouter Navigation nœuds ** l’interface utilisateur](media/ADDS_ADACConnectToDomain.gif)

4.  Dans **se connecter à**, tapez le nom du domaine étranger que vous souhaitez gérer \(, par exemple, **contoso.com**\), puis cliquez sur **OK**.

5.  Lorsque vous êtes correctement connecté au domaine étranger, parcourez les colonnes dans le **ajouter des nœuds de Navigation** fenêtre, sélectionnez l’ou les conteneurs à ajouter au volet de navigation de votre centre d’administration Active Directory, et puis cliquez sur **OK**.

### <a name="windows-server-2008-r2-to-manage-a-foreign-domain-in-the-selected-instance-of-active-directory-administrative-center-using-the-current-set-of-logon-credentials"></a>Windows Server 2008 R2 : Pour gérer un domaine étranger dans l’instance sélectionnée du centre d’administration Active Directory à l’aide de l’ensemble actuel des informations d’identification d’ouverture de session

1. Pour ouvrir le centre d’administration Active Directory, cliquez sur **Démarrer**, cliquez sur **outils d’administration**, puis cliquez sur **centre d’administration Active Directory**.

   > [!NOTE]
   >  Une autre façon d’ouvrir le centre d’administration Active Directory consiste à cliquer sur **Démarrer**, cliquez sur **exécuter**, puis tapez **dsac.exe**.

2. Pour ouvrir **ajouter des nœuds de Navigation**, près du haut de la fenêtre du centre d’administration Active Directory, cliquez **ajouter des nœuds de Navigation** comme indiqué dans l’illustration suivante.

    ![Capture d’écran ** Ajouter Navigation nœuds ** l’interface utilisateur](media/click_add_nav_nodes.gif)

   > [!NOTE]
   >  Une autre façon d’ouvrir **ajouter des nœuds de Navigation** est à droite\-cliquez n’importe où dans l’espace vide dans le volet de navigation du centre d’administration Active Directory, puis cliquez sur **ajouter des nœuds de Navigation**.

3. Dans **ajouter des nœuds de Navigation**, cliquez sur **se connecter à d’autres domaines** comme indiqué dans l’illustration suivante.

    ![Capture d’écran ** Ajouter Navigation nœuds ** ** se connecter aux autres domaines ** l’interface utilisateur](media/add_nav_nodes.gif)

4. Dans **se connecter à**, tapez le nom du domaine étranger que vous souhaitez gérer \(, par exemple, **contoso.com**\), puis cliquez sur **OK**.

5. Lorsque vous êtes correctement connecté au domaine étranger, parcourez les colonnes dans le **ajouter des nœuds de Navigation** fenêtre, sélectionnez l’ou les conteneurs à ajouter au volet de navigation de votre centre d’administration Active Directory, et puis cliquez sur **OK**.

   Pour plus d’informations sur la personnalisation du volet de navigation du centre d’administration Active Directory, consultez [personnaliser le Active Directory Administrative Center volet de Navigation](customize-the-active-directory-administrative-center-navigation-pane.md).

   Vous pouvez également ouvrir le centre d’administration Active Directory à l’aide d’un ensemble d’informations d’identification d’ouverture de session qui est différent de votre jeu actuel d’informations d’identification d’ouverture de session. La commande dans la procédure suivante peut être utile si vous êtes connecté à l’ordinateur qui exécute le centre d’administration Active Directory avec les informations d’identification de l’utilisateur normal, mais que vous souhaitez utiliser le centre d’administration Active Directory sur cet ordinateur pour gérer votre domaine local en tant qu’administrateur. \(Cette commande peut également être utile si vous souhaitez utiliser le centre d’administration Active Directory pour gérer à distance un domaine étranger est différent de votre domaine local avec un ensemble d’informations d’identification qui est différent de votre jeu actuel d’informations d’identification d’ouverture de session. Toutefois, le domaine étranger doit avoir une approbation établie avec le domaine local.\)

   Pour mener à bien cette procédure, il n’est pas nécessaire d’appartenir à un groupe particulier.

### <a name="to-manage-a-domain-using-logon-credentials-that-are-different-from-the-current-set-of-logon-credentials"></a>Pour gérer un domaine à l’aide d’informations d’identification autres que les vôtres

1.  Pour ouvrir le centre d’administration Active Directory, à une invite de commandes, tapez la commande suivante, puis appuyez sur ENTRÉE :

     `runas /user:<domain\user> dsac`

     Où `<domain\user>` est l’ensemble des informations d’identification que vous souhaitez ouvrir le centre d’administration Active Directory avec et `dsac` est le nom de fichier exécutable du centre d’administration Active Directory \(Dsac.exe\).

     Par exemple, tapez la commande suivante et appuyez sur Entrée :

     `runas /user:contoso\administrator dsac`

2.  Lorsque le centre d’administration Active Directory est ouvert, parcourez le volet de navigation pour afficher ou gérer votre domaine Active Directory.

  

