---
title: Configurer la stratégie d'accès conditionnel
description: Une fois un certificat racine a été créé, la connectivité « VPN » déclenche la création de l’application de cloud « Serveur VPN » dans le client.
services: active-directory
ms.prod: windows-server-threshold
ms.technology: networking-ras
documentationcenter: ''
ms.assetid: ''
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2018
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: 466e76d01ca99a1e1ed72fa955ccd287ae63c5df
ms.sourcegitcommit: 0948a1abff1c1be506216eeb51ffc6f752a9fe7e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749496"
---
# <a name="step-73-configure-the-conditional-access-policy"></a>Étape 7.3. Configurer la stratégie d’accès conditionnel

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Précédent :** Étape 7.2. Créer des certificats racine pour l’authentification VPN avec Azure AD](vpn-create-root-cert-for-vpn-auth-azure-ad.md)
- [**prochain :** Étape 7.4. Déployer des certificats racine de l’accès conditionnel en local AD](vpn-deploy-cond-access-root-cert-to-on-premise-ad.md)

Dans cette étape, vous configurez la stratégie d’accès conditionnel pour la connectivité VPN. Quand le premier certificat racine est créé dans le panneau « Connectivité VPN », il crée automatiquement une application en nuage « Serveur VPN » dans le client.

Créer une stratégie d’accès conditionnel qui est affectée au groupe d’utilisateurs VPN et de la portée du **application Cloud** à **serveur VPN**:

- **Utilisateurs** : Utilisateurs VPN
- **Application cloud**: Serveur VPN
- **Grant (contrôle d’accès)** : « Exiger une authentification multifacteur ». Autres contrôles peuvent être utilisés si vous le souhaitez.

**Procédure :** Cette étape couvre la création de la stratégie d’accès conditionnel plus simple.  Si vous le souhaitez, des Conditions et des contrôles supplémentaires peuvent être utilisés.


1. Sur le **accès conditionnel** , dans la barre d’outils en haut, sélectionnez **ajouter**.

    ![Sélectionnez Ajouter dans la page accès conditionnel](../../media/Always-On-Vpn/07.png)

2. Sur le **New** page, dans le **nom** , entrez un nom pour votre stratégie. Par exemple, entrez **stratégie VPN**.

    ![Ajouter le nom de stratégie dans la page accès conditionnel](../../media/Always-On-Vpn/08.png)

3. Dans le **attribution** section, sélectionnez **utilisateurs et groupes**.

    ![Sélectionnez les utilisateurs et groupes](../../media/Always-On-Vpn/09.png)

4. Sur le **utilisateurs et groupes** page, procédez comme suit :

    ![Utilisateur de sélectionner un test](../../media/Always-On-Vpn/10.png)

    a. Sélectionnez **sélectionner des utilisateurs et groupes**.

    b. Sélectionnez **sélectionnez**.

    c. Sur le **sélectionnez** , sélectionnez le **utilisateurs VPN** de groupe, puis sélectionnez **sélectionnez**.

    d. Sur le **utilisateurs et groupes** page, sélectionnez **fait**.

5. Sur le **New** page, procédez comme suit :

    ![Sélectionnez les applications cloud](../../media/Always-On-Vpn/11.png)

    a. Dans le **affectations** section, sélectionnez **applications Cloud**.

    b. Sur le **applications Cloud** page, sélectionnez **sélectionner les applications**.

    d. Sélectionnez **serveur VPN**.

6.  Sur le **New** page, pour ouvrir le **Grant** page, dans le **contrôles** section, sélectionnez **Grant**.

    ![Sélectionnez octroyer](../../media/Always-On-Vpn/13.png)

7.  Sur le **Grant** page, procédez comme suit :

    ![Sélectionnez exiger une authentification multifacteur](../../media/Always-On-Vpn/14.png)

    a. Sélectionnez **exiger une authentification multifacteur**.

    b. Sélectionnez **sélectionnez**.

8.  Sur le **New** page sous **activer la stratégie**, sélectionnez **sur**.

    ![Activer la stratégie](../../media/Always-On-Vpn/15.png)

9.  Sur le **New** page, sélectionnez **créer**.


## <a name="next-steps"></a>Étapes suivantes
[Étape 7.4. Déployer des certificats racine de l’accès conditionnel en local AD](vpn-deploy-cond-access-root-cert-to-on-premise-ad.md): Dans cette étape, vous déployez le certificat racine de l’accès conditionnel en tant que certificat racine approuvé pour l’authentification VPN sur votre réseau local AD.
