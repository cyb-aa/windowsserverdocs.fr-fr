---
title: Créer des certificats racine pour l'authentification VPN avec Azure AD
description: Azure AD utilise le certificat VPN pour signer les certificats émis pour les clients Windows 10 lors de l’authentification à Azure AD pour la connectivité VPN. Le certificat marqué comme principal est l’émetteur qui utilise Azure AD.
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
ms.openlocfilehash: dc24f0275e8639ffd972ae24550d0ada38eff4f1
ms.sourcegitcommit: 0948a1abff1c1be506216eeb51ffc6f752a9fe7e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749639"
---
# <a name="step-72-create-conditional-access-root-certificates-for-vpn-authentication-with-azure-ad"></a>Étape 7.2. Créer des certificats racines pour l’authentification VPN de l’accès conditionnel avec Azure AD

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Précédent :** Étape 7.1. Configurer EAP-TLS pour ignorer la vérification de la liste de révocation de certificats (CRL)](vpn-config-eap-tls-to-ignore-crl-checking.md)
- [**prochain :** Étape 7.3. Configurer la stratégie d’accès conditionnel](vpn-config-conditional-access-policy.md)

Dans cette étape, vous configurez des certificats racine de l’accès conditionnel pour l’authentification VPN auprès d’Azure AD, ce qui crée automatiquement une application Cloud appelée serveur VPN dans le client. Pour configurer l’accès conditionnel pour la connectivité VPN, vous devez :

1. Créer un certificat VPN dans le portail Azure (vous pouvez créer plusieurs certificats).
2. Télécharger le certificat VPN.
3. Déployer le certificat sur vos serveurs VPN et NPS.

Lorsqu’un utilisateur tente une connexion VPN, le client VPN effectue un appel dans le Gestionnaire de compte Web (WAM) sur le client Windows 10. WAM effectue un appel à l’application de cloud de serveur VPN. Lorsque les contrôles dans la stratégie d’accès conditionnel et les Conditions sont remplies, Azure AD émet un jeton sous la forme d’un certificat (1 heure) de courte durée pour le WAM. Le WAM place le certificat dans le magasin de certificats de l’utilisateur et passe le contrôle au client VPN.  

Le client VPN envoie ensuite les problèmes de certificat par Azure AD au VPN pour la validation des informations d’identification.  Azure AD utilise le certificat qui est marqué comme **principal** dans le panneau de la connectivité VPN en tant que l’émetteur. 

Dans le portail Azure, vous créez deux certificats pour gérer la transition quand un certificat est sur le point d’expirer. Lorsque vous créez un certificat, vous choisissez s’il s’agit du certificat principal, qui est utilisé lors de l’authentification pour signer le certificat pour la connexion.

**Procédure :**

1. Connectez-vous à votre [portail](https://portal.azure.com) en tant qu’administrateur.

2. Dans le menu de gauche, cliquez sur **Azure Active Directory**. 

    ![Sélectionnez Azure Active Directory](../../media/Always-On-Vpn/01.png)

3. Sur le **Azure Active Directory** page, dans le **gérer** , cliquez sur **accès conditionnel**.

    ![Sélectionnez l’accès conditionnel](../../media/Always-On-Vpn/02.png)

4. Sur le **accès conditionnel** page, dans le **gérer** , cliquez sur **connectivité VPN (préversion)** .

    ![Sélectionnez la connectivité VPN](../../media/Always-On-Vpn/03.png)

5. Sur le **connectivité VPN** , cliquez sur **nouveau certificat**.

    ![Sélectionnez le nouveau certificat](../../media/Always-On-Vpn/04.png)

6. Sur le **New** page, procédez comme suit :

    ![Sélectionnez la durée et principal](../../media/Always-On-Vpn/05.png)

    a. Pour **sélectionner une durée**, sélectionnez 1 ou 2 ans. Vous pouvez ajouter jusqu'à deux certificats pour gérer les transitions quand le certificat est sur le point d’expirer. Vous pouvez choisir celle qui est le serveur principal (celui utilisé lors de l’authentification pour signer le certificat pour la connectivité).

    b. Pour **principal**, sélectionnez **Oui**.

    c. Sélectionnez **Créer**.

## <a name="next-steps"></a>Étapes suivantes

[Étape 7.3. Configurer la stratégie d’accès conditionnel](vpn-config-conditional-access-policy.md): Dans cette étape, vous configurez la stratégie d’accès conditionnel pour la connectivité VPN. 
