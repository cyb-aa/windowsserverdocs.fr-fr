---
title: Configurer la stratégie d'accès conditionnel
description: Après la création d’un certificat racine, la « connectivité VPN » déclenche la création de l’application de Cloud « serveur VPN » dans le locataire du client.
services: active-directory
ms.prod: windows-server
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
ms.openlocfilehash: 22983c085f2b9d9e7e16810e25c6fa50111f9fa6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404342"
---
# <a name="step-73-configure-the-conditional-access-policy"></a>Étape 7.3. Configurer la stratégie d’accès conditionnel

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Précédent :** Étape 7,2. Créer des certificats racines pour l’authentification VPN avec Azure AD](vpn-create-root-cert-for-vpn-auth-azure-ad.md)
- [**Ensuite :** Étape 7,4. Déployer des certificats racine d’accès conditionnel vers AD local](vpn-deploy-cond-access-root-cert-to-on-premise-ad.md)

Dans cette étape, vous configurez la stratégie d’accès conditionnel pour la connectivité VPN. Lorsque le premier certificat racine est créé dans le panneau « connectivité VPN », il crée automatiquement une application Cloud « serveur VPN » dans le locataire.

Créer une stratégie d’accès conditionnel qui est affectée au groupe d’utilisateurs VPN et étendre l' **application Cloud** au **serveur VPN**:

- **Utilisateurs**: utilisateurs VPN
- **Application Cloud**: serveur VPN
- **Grant (contrôle d’accès)** : 'require Multi-Factor Authentication'. D’autres contrôles peuvent être utilisés si vous le souhaitez.

**Procédure :** Cette étape couvre la création de la stratégie d’accès conditionnel la plus basique.  Si vous le souhaitez, des conditions et des contrôles supplémentaires peuvent être utilisés.


1. Dans la page **accès conditionnel** , dans la barre d’outils située en haut, sélectionnez **Ajouter**.

    ![Sélectionnez Ajouter dans la page accès conditionnel](../../media/Always-On-Vpn/07.png)

2. Dans la page **nouveau** , dans la zone **nom** , entrez un nom pour votre stratégie. Par exemple, entrez **stratégie VPN**.

    ![Ajouter un nom pour la stratégie sur la page accès conditionnel](../../media/Always-On-Vpn/08.png)

3. Dans la section **affectation** , sélectionnez **utilisateurs et groupes**.

    ![Sélectionner les utilisateurs et les groupes](../../media/Always-On-Vpn/09.png)

4. Dans la page **utilisateurs et groupes** , procédez comme suit :

    ![Sélectionner un utilisateur de test](../../media/Always-On-Vpn/10.png)

    a. Sélectionnez **Sélectionner les utilisateurs et les groupes**.

    b. Sélectionnez **Sélectionner**.

    c. Dans la page **Sélectionner** , sélectionnez le groupe **utilisateurs VPN** , puis sélectionnez **Sélectionner**.

    d. Dans la page **utilisateurs et groupes** , sélectionnez **terminé**.

5. Sur la page **nouveau** , procédez comme suit :

    ![Sélectionner les applications Cloud](../../media/Always-On-Vpn/11.png)

    a. Dans la section **affectations** , sélectionnez **applications Cloud**.

    b. Dans la page **applications Cloud** , sélectionnez **Sélectionner les applications**.

    d. Sélectionnez le **serveur VPN**.

6.  Sur la page **nouveau** , pour ouvrir la page **octroyer** , dans la section **contrôles** , sélectionnez **accorder**.

    ![Sélectionner l’attribution](../../media/Always-On-Vpn/13.png)

7.  Sur la page **accorder** , procédez comme suit :

    ![Sélectionnez exiger Multi-Factor Authentication](../../media/Always-On-Vpn/14.png)

    a. Sélectionnez **exiger Multi-Factor Authentication**.

    b. Sélectionnez **Sélectionner**.

8.  Dans la page **nouveau** , sous **activer la stratégie**, sélectionnez **activé**.

    ![Activer la stratégie](../../media/Always-On-Vpn/15.png)

9.  Dans la page **nouveau** , sélectionnez **créer**.


## <a name="next-steps"></a>Étapes suivantes
[Étape 7,4. Déployer des certificats racines d’accès conditionnel vers AD local](vpn-deploy-cond-access-root-cert-to-on-premise-ad.md): au cours de cette étape, vous allez déployer le certificat racine d’accès conditionnel en tant que certificat racine approuvé pour l’authentification VPN sur votre annuaire Active Directory local.
