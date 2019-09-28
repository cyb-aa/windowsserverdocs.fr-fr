---
title: Créer des certificats racine pour l'authentification VPN avec Azure AD
description: Azure AD utilise le certificat VPN pour signer les certificats émis aux clients Windows 10 lors de l’authentification auprès d’Azure AD pour la connectivité VPN. Le certificat marqué comme principal est l’émetteur que Azure AD utilise.
services: active-directory
ms.prod: windows-server
ms.technology: networking-ras
ms.workload: identity
ms.topic: article
ms.date: 06/28/2019
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: 41e98648ab963347f8370233c320f5e38b5d4d96
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388009"
---
# <a name="step-72-create-conditional-access-root-certificates-for-vpn-authentication-with-azure-ad"></a>Étape 7.2. Créer des certificats racines d’accès conditionnel pour l’authentification VPN avec Azure AD

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Premier** Étape 7.1. Configurer EAP-TLS pour ignorer la vérification de la liste de révocation de certificats (CRL)](vpn-config-eap-tls-to-ignore-crl-checking.md)
- [**Situé** Étape 7.3. Configurer la stratégie d’accès conditionnel](vpn-config-conditional-access-policy.md)

Dans cette étape, vous configurez des certificats racines d’accès conditionnel pour l’authentification VPN avec Azure AD, ce qui crée automatiquement une application Cloud appelée serveur VPN dans le locataire. Pour configurer l’accès conditionnel pour la connectivité VPN, vous devez :

1. Créez un certificat VPN dans le Portail Azure.
2. Téléchargez le certificat VPN.
3. Déployez le certificat sur vos serveurs VPN et NPS.

> [!IMPORTANT]
> Une fois qu’un certificat VPN a été créé dans le Portail Azure, Azure AD commencera à l’utiliser immédiatement pour émettre des certificats à courte durée de vie vers le client VPN. Il est essentiel de déployer immédiatement le certificat VPN sur le serveur VPN afin d’éviter tout problème avec la validation des informations d’identification du client VPN.

Lorsqu’un utilisateur tente une connexion VPN, le client VPN effectue un appel dans le gestionnaire de compte Web (WAM) sur le client Windows 10. WAM effectue un appel à l’application Cloud du serveur VPN. Lorsque les conditions et les contrôles de la stratégie d’accès conditionnel sont remplis, Azure AD émet un jeton sous la forme d’un certificat de courte durée (1 heure) à WAM. Le WAM place le certificat dans le magasin de certificats de l’utilisateur et transmet le contrôle au client VPN.  

Le client VPN envoie ensuite les problèmes de certificat en Azure AD au VPN pour la validation des informations d’identification.  

> [!NOTE]
> Azure AD utilise le certificat le plus récemment créé dans le panneau connectivité VPN en tant qu’émetteur.

**Procédures**

1. Connectez-vous à votre [portail Azure](https://portal.azure.com) en tant qu’administrateur général.
2. Dans le menu de gauche, cliquez sur **Azure Active Directory**.
3. Dans la page **Azure Active Directory** , dans la section **gérer** , cliquez sur **accès conditionnel**.
4. Dans la page **accès conditionnel** , dans la section **gérer** , cliquez sur **connectivité VPN (version préliminaire)** .
5. Dans la page **connectivité VPN** , cliquez sur **nouveau certificat**.
6. Sur la page **nouveau** , procédez comme suit : a. Pour **Sélectionner une durée**, sélectionnez 1, 2 ou 3 ans.
   b. Sélectionnez **Créer**.

## <a name="next-steps"></a>Étapes suivantes

[Étape 7.3. Configurer la stratégie d’accès conditionnel @ no__t-0 : Dans cette étape, vous configurez la stratégie d’accès conditionnel pour la connectivité VPN.
