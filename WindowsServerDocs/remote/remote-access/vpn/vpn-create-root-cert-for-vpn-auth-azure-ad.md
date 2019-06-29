---
title: Créer des certificats racine pour l'authentification VPN avec Azure AD
description: Azure AD utilise le certificat VPN pour signer les certificats émis pour les clients Windows 10 lors de l’authentification à Azure AD pour la connectivité VPN. Le certificat marqué comme principal est l’émetteur qui utilise Azure AD.
services: active-directory
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.workload: identity
ms.topic: article
ms.date: 06/28/2019
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: 40403f6be65c84cc1e2506a222a009400fcdaacc
ms.sourcegitcommit: 34232723f15c7b4d6a32ca38b7348a417ba30ae0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67464959"
---
# <a name="step-72-create-conditional-access-root-certificates-for-vpn-authentication-with-azure-ad"></a>Étape 7.2. Créer des certificats racines pour l’authentification VPN de l’accès conditionnel avec Azure AD

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Précédent :** Étape 7.1. Configurer EAP-TLS pour ignorer la vérification de la liste de révocation de certificats (CRL)](vpn-config-eap-tls-to-ignore-crl-checking.md)
- [**prochain :** Étape 7.3. Configurer la stratégie d’accès conditionnel](vpn-config-conditional-access-policy.md)

Dans cette étape, vous configurez des certificats racine de l’accès conditionnel pour l’authentification VPN auprès d’Azure AD, ce qui crée automatiquement une application Cloud appelée serveur VPN dans le client. Pour configurer l’accès conditionnel pour la connectivité VPN, vous devez :

1. Créer un certificat VPN dans le portail Azure.
2. Télécharger le certificat VPN.
3. Déployer le certificat sur vos serveurs VPN et NPS.

> [!IMPORTANT]
> Une fois un certificat VPN est créé dans le portail Azure, Azure AD commence immédiatement à l’aide d’émettre des certificats à durée de vie courtes au client VPN. Il est essentiel que le certificat VPN déployé immédiatement au serveur VPN pour éviter tout problème avec la validation des informations d’identification du client VPN.

Lorsqu’un utilisateur tente une connexion VPN, le client VPN effectue un appel dans le Gestionnaire de compte Web (WAM) sur le client Windows 10. WAM effectue un appel à l’application de cloud de serveur VPN. Lorsque les contrôles dans la stratégie d’accès conditionnel et les Conditions sont remplies, Azure AD émet un jeton sous la forme d’un certificat (1 heure) de courte durée pour le WAM. Le WAM place le certificat dans le magasin de certificats de l’utilisateur et passe le contrôle au client VPN.  

Le client VPN envoie ensuite les problèmes de certificat par Azure AD au VPN pour la validation des informations d’identification.  

> [!NOTE]
> Azure AD utilise le certificat créé récemment dans le panneau de la connectivité VPN en tant que l’émetteur.

**Procédure :**

1. Connectez-vous à votre [portail](https://portal.azure.com) en tant qu’administrateur.
2. Dans le menu de gauche, cliquez sur **Azure Active Directory**.
3. Sur le **Azure Active Directory** page, dans le **gérer** , cliquez sur **accès conditionnel**.
4. Sur le **accès conditionnel** page, dans le **gérer** , cliquez sur **connectivité VPN (préversion)** .
5. Sur le **connectivité VPN** , cliquez sur **nouveau certificat**.
6. Sur le **New** page, procédez comme suit : un. Pour **sélectionner une durée**, sélectionnez 1, 2 ou 3 ans.
   b. Sélectionnez **Créer**.

## <a name="next-steps"></a>Étapes suivantes

[Étape 7.3. Configurer la stratégie d’accès conditionnel](vpn-config-conditional-access-policy.md): Dans cette étape, vous configurez la stratégie d’accès conditionnel pour la connectivité VPN.
