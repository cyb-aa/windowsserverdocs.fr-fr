---
title: Créer des certificats racine pour l'authentification VPN avec AzureAD
description: Azure AD utilise le certificat VPN pour signer des certificats émis pour les clients Windows 10 lors de l’authentification à Azure AD pour une connexion VPN. Le certificat marqué comme principal est l’émetteur qui utilise Azure AD.
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
ms.openlocfilehash: 14ef17ab403cc4e7c9891f4ede48e41c25e8522d
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067115"
---
# Étape7.2. Créer des certificats racines pour l’authentification VPN accès conditionnel à Azure AD

>S’applique à: Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows 10

& #171;  [ **Précédente:** étape 7.1. Configurer EAP-TLS pour ignorer la vérification de la liste de révocation de certificats (CRL)](vpn-config-eap-tls-to-ignore-crl-checking.md)<br>
& #187; [ **Suivant:** étape 7.3. Configurer la stratégie d’accès conditionnel](vpn-config-conditional-access-policy.md)

Dans cette étape, vous configurez des certificats racine de l’accès conditionnel pour l’authentification VPN avec Azure AD, qui crée automatiquement une application Cloud appelée serveur VPN dans le client. Pour configurer l’accès conditionnel pour une connexion VPN, vous devez:

1. Créer un certificat VPN dans le portail Azure (vous pouvez créer plusieurs certificats).
2. Télécharger le certificat VPN.
2. Déployez le certificat à vos serveurs VPN et NPS.

Lorsqu’un utilisateur tente une connexion VPN, le client VPN effectue un appel dans le compte de gestionnaire WAM (Web) sur le client Windows 10. WAM effectue un appel à l’application de cloud serveur VPN. Lorsque les contrôles dans la stratégie d’accès conditionnel et les Conditions sont remplies, Azure AD émet un jeton sous la forme d’un certificat (1 heure) courte durée pour le WAM. Le WAM place le certificat dans le magasin de certificats de l’utilisateur et transmet le contrôle et le client VPN.  

Le client VPN envoie ensuite les problèmes de certificat par Azure AD pour la connexion VPN pour la validation des informations d’identification.Azure AD utilise le certificat est marqué comme**principal**dans le panneau de connectivité VPN en tant que l’émetteur. 

Dans le portail Azure, vous créez deux certificats pour gérer la transition quand un certificat est sur le point d’expirer. Lorsque vous créez un certificat, vous choisissez s’il s’agit de la certification principale, qui est utilisée lors de l’authentification pour signer le certificat pour la connexion.

**Procédure:**

1. Connectez-vous à votre [portail Azure](https://portal.azure.com) en tant qu’administrateur général.

2. Dans le menu de gauche, cliquez sur **Azure Active Directory**. 

    ![Sélectionnez Azure Active Directory](../../media/Always-On-Vpn/01.png)

3. Dans la page **d’Azure Active Directory** , dans la section **Gérer** , cliquez sur **l’accès conditionnel**.

    ![Sélectionnez l’accès conditionnel](../../media/Always-On-Vpn/02.png)

4. Dans la page de **l’accès conditionnel** , dans la section **Gérer** , cliquez sur **une connectivité VPN (aperçu)**.

    ![Sélectionnez une connectivité VPN](../../media/Always-On-Vpn/03.png)

5. Sur la page de **connexion VPN** , cliquez sur **nouveau certificat**.

    ![Sélectionnez un nouveau certificat](../../media/Always-On-Vpn/04.png)

6. Sur la page de **Nouveau** , procédez comme suit:

    ![Sélectionnez les principales et durée](../../media/Always-On-Vpn/05.png)

    a. Pour **Sélectionner la durée**, sélectionnez 1 ou 2 ans. Vous pouvez ajouter jusqu'à deux certificats pour gérer les transitions lorsque le certificat est sur le point d’expirer. Vous pouvez choisir celle qui est le principal (celui utilisé lors de l’authentification pour signer le certificat pour la connectivité).

    b. Pour les **principales**, sélectionnez **«Oui»**.

    c. Cliquez sur **Créer**.

## Étape suivante
Étape [7.3. Configurer la stratégie d’accès conditionnel](vpn-config-conditional-access-policy.md): dans cette étape, vous configurez la stratégie d’accès conditionnel pour une connexion VPN. 

---