---
title: "Gérer des domaines différents dans le centre d’administration ActiveDirectory"
ms.prod: windows-server-threshold
description: "Sécurité de Windows Server"
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.assetid: 166351c3-4076-48be-aa8f-797adf1e9d68
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 5f253bd4952d8a347e97eafdb38d86fa98024b8d
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="manage-different-domains-in-active-directory-administrative-center"></a>Gérer des domaines différents dans le centre d’administration ActiveDirectory

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

  Lorsque vous ouvrez Administration d’ActiveDirectory, le domaine auquel vous êtes actuellement connecté sur cet ordinateur \ (le domain\ local) apparaît dans le volet de navigation du centre d’administration ActiveDirectory \(the left pane\). En fonction des droits de votre jeu actuel d’informations d’identification d’ouverture de session, vous pouvez afficher ou gérer les objets ActiveDirectory dans ce domaine local.

 Vous pouvez également utiliser le même jeu d’informations d’identification d’ouverture de session et la même instance du centre d’administration ActiveDirectory pour afficher ou gérer des objets ActiveDirectory dans n’importe quel autre domaine de la même forêt, ou un domaine dans une autre forêt qui possède une approbation établie avec l’ordinateur local domaine. Les approbations d’one\ voies et contribuent voies sont pris en charge.

> [!NOTE]
>  S’il existe une approbation à sens unique one\ entre le domaine A et le domaine B via les utilisateurs du domaine A peuvent accéder aux ressources du domaine B, mais les utilisateurs du domaine B ne peuvent pas accéder aux ressources dans un domaine, si vous exécutez centre d’administration ActiveDirectory sur l’ordinateur où le domaine A est  votre domaine local, vous pouvez vous connecter au domaine B avec l’ensemble des informations d’identification d’ouverture de session et dans la même instance du centre d’administration ActiveDirectory en cours. Mais si vous exécutez centre d’administration ActiveDirectory sur l’ordinateur où le domaine B est le domaine local, vous ne peut pas se connecter au domaine A avec le même jeu d’informations d’identification dans la même instance du centre d’administration ActiveDirectory.

 Il n’existe aucune l’appartenance au groupe minimale requise pour effectuer cette procédure.

### <a name="windows-server-2012-to-manage-a-foreign-domain-in-the-selected-instance-of-active-directory-administrative-center-using-the-current-set-of-logon-credentials"></a>Windows Server2012: Pour gérer un domaine étranger dans l’instance sélectionnée du centre d’administration ActiveDirectory à l’aide du jeu actuel d’informations d’identification d’ouverture de session

1.  Pour ouvrir le centre d’administration ActiveDirectory, dans **le Gestionnaire de serveur**, cliquez sur **outils**, puis cliquez sur **centre d’administration ActiveDirectory**.

    > [!NOTE]
    >  Une autre façon d’ouvrir le centre d’administration ActiveDirectory consiste à cliquer sur **Démarrer**, puis tapez **dsac.exe**.

2.  Pour ouvrir **ajouter des nœuds de Navigation**, cliquez sur **gérer**, puis cliquez sur **ajouter des nœuds de Navigation** comme indiqué dans l’illustration suivante.

     ![Capture d’écran montrant ** ajouter Navigation nœuds ** l’interface utilisateur](media/ADDS_ADACAddNavNode.gif)

3.  Dans **ajouter des nœuds de Navigation**, cliquez sur **se connecter à d’autres domaines** comme indiqué dans l’illustration suivante.

     ![Capture d’écran montrant ** ajouter Navigation nœuds ** l’interface utilisateur](media/ADDS_ADACConnectToDomain.gif)

4.  Dans **se connecter à**, tapez le nom du domaine étranger que vous souhaitez gérer \ (par exemple, **contoso.com**\), puis cliquez sur **OK**.

5.  Lorsque vous êtes correctement connecté au domaine étranger, parcourez les colonnes dans le **ajouter des nœuds de Navigation** fenêtre, sélectionnez l’ou les conteneurs à ajouter à votre volet de navigation du centre d’administration ActiveDirectory, puis cliquez sur **OK**.

### <a name="windows-server-2008-r2-to-manage-a-foreign-domain-in-the-selected-instance-of-active-directory-administrative-center-using-the-current-set-of-logon-credentials"></a>Windows Server2008R2: Pour gérer un domaine étranger dans l’instance sélectionnée du centre d’administration ActiveDirectory à l’aide du jeu actuel d’informations d’identification d’ouverture de session

1.  Pour ouvrir le centre d’administration ActiveDirectory, cliquez sur **Démarrer**, cliquez sur **outils d’administration**, puis cliquez sur **centre d’administration ActiveDirectory**.

    > [!NOTE]
    >  Une autre façon d’ouvrir le centre d’administration ActiveDirectory consiste à cliquer sur **Démarrer**, cliquez sur **exécuter**, puis tapez **dsac.exe**.

2.  Pour ouvrir **ajouter des nœuds de Navigation**, dans la zone supérieure de la fenêtre du centre d’administration ActiveDirectory, cliquez sur **ajouter des nœuds de Navigation** comme indiqué dans l’illustration suivante.

     ![Capture d’écran montrant ** ajouter Navigation nœuds ** l’interface utilisateur](media/click_add_nav_nodes.gif)

    > [!NOTE]
    >  Une autre façon d’ouvrir **ajouter des nœuds de Navigation** consiste à un clic droit n’importe où dans l’espace vide dans le volet de navigation du centre d’administration ActiveDirectory, puis cliquez sur **ajouter des nœuds de Navigation**.

3.  Dans **ajouter des nœuds de Navigation**, cliquez sur **se connecter à d’autres domaines** comme indiqué dans l’illustration suivante.

     ![Capture d’écran montrant ** ajouter Navigation nœuds ** ** se connecter aux autres domaines ** l’interface utilisateur](media/add_nav_nodes.gif)

4.  Dans **se connecter à**, tapez le nom du domaine étranger que vous souhaitez gérer \ (par exemple, **contoso.com**\), puis cliquez sur **OK**.

5.  Lorsque vous êtes correctement connecté au domaine étranger, parcourez les colonnes dans le **ajouter des nœuds de Navigation** fenêtre, sélectionnez l’ou les conteneurs à ajouter à votre volet de navigation du centre d’administration ActiveDirectory, puis cliquez sur **OK**.

 Pour plus d’informations sur la personnalisation du volet de navigation du centre d’administration ActiveDirectory, voir [personnaliser le ActiveDirectory d’administration du centre de volet de Navigation](customize-the-active-directory-administrative-center-navigation-pane.md).

 Vous pouvez également ouvrir le centre d’administration ActiveDirectory à l’aide d’un ensemble d’informations d’identification d’ouverture de session qui est différent de votre jeu actuel d’informations d’identification d’ouverture de session. La commande dans la procédure suivante peut être utile si vous êtes connecté à l’ordinateur qui exécute le centre d’administration ActiveDirectory avec les informations d’identification utilisateur normales, mais que vous souhaitez utiliser le centre d’administration ActiveDirectory sur cet ordinateur pour gérer votre domaine local en tant qu’administrateur. \ (Cette commande peut également être utile si vous souhaitez utiliser le centre d’administration ActiveDirectory pour gérer à distance un domaine étranger est différent de votre domaine local avec un ensemble d’informations d’identification qui est différent de votre jeu actuel d’informations d’identification d’ouverture de session. Toutefois, le domaine étranger doit avoir une approbation établie avec le domaine local. \)

 Il n’existe aucune l’appartenance au groupe minimale requise pour effectuer cette procédure.

### <a name="to-manage-a-domain-using-logon-credentials-that-are-different-from-the-current-set-of-logon-credentials"></a>Pour gérer un domaine à l’aide des informations d’identification d’ouverture de session qui diffèrent du jeu actuel d’informations d’identification d’ouverture de session

1.  Pour ouvrir le centre d’administration ActiveDirectory, à une invite de commandes, tapez la commande suivante et appuyez sur ENTRÉE:

     `runas /user:<domain\user> dsac`

     Où `<domain\user>`est l’ensemble des informations d’identification que vous souhaitez ouvrir le centre d’administration ActiveDirectory avec et `dsac`est le centre d’administration ActiveDirectory fichier exécutable nom \(Dsac.exe\).

     Par exemple, tapez la commande suivante et appuyez sur ENTRÉE:

     `runas /user:contoso\administrator dsac`

2.  Lorsque le centre d’administration ActiveDirectory est ouvert, parcourez le volet de navigation pour afficher ou gérer votre domaine ActiveDirectory.

  

