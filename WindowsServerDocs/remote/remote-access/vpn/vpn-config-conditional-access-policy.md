---
title: Configurer la stratégie d'accès conditionnel
description: Après la création d’un certificat racine, la « connectivité VPN » déclenche la création de l’application de Cloud « serveur VPN » dans le locataire du client.
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.date: 05/25/2018
ms.author: v-tea
author: Teresa-MOTIV
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: 754182cc3f60e1e30625c11d8778cf24b6d098ac
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80819012"
---
# <a name="step-73-configure-the-conditional-access-policy"></a>Étape 7.3. configurer la stratégie d'accès conditionnel

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

    ![Activer une stratégie](../../media/Always-On-Vpn/15.png)

9.  Dans la page **nouveau** , sélectionnez **créer**.


## <a name="next-steps"></a>Étapes suivantes :
[Étape 7,4. Déployer des certificats racines d’accès conditionnel vers AD local](vpn-deploy-cond-access-root-cert-to-on-premise-ad.md): au cours de cette étape, vous allez déployer le certificat racine d’accès conditionnel en tant que certificat racine approuvé pour l’authentification VPN sur votre annuaire Active Directory local.
