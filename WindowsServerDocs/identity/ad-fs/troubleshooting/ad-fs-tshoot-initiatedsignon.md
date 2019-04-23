---
title: AD FS dépannage - initiées par un fournisseur d’identité d’authentification
description: Ce document décrit comment résoudre les problèmes de la page authentification AD FS.
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/03/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 61e9adc708e95a6ab4a82550280737b2f4bad0a1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844940"
---
# <a name="ad-fs-troubleshooting---idp-initiated-sign-on"></a>AD FS dépannage - initiées par un fournisseur d’identité d’authentification
La page de l’authentification AD FS peut servir à tester l’authentification fonctionne ou non.  Pour cela, en accédant à la page et la connexion.  En outre, nous pouvons utiliser la page de connexion pour vérifier que toutes les parties de confiance SAML 2.0 sont répertoriés.

## <a name="enable-the-idp-intiated-sign-on-page"></a>Activer la page authentification Idp lancée
Par défaut, AD FS dans Windows 2016 n’a pas la connexion sur la page est activée.  Pour l’activer, vous pouvez utiliser la commande PowerShell Set-AdfsProperties.  Utilisez la procédure suivante pour activer la page :

1.  Ouvrez Windows PowerShell
2.  Entrez : `Get-AdfsProperties` puis appuyez sur entrée
3.  Vérifiez que **EnableIdpInitiatedSignonPage** est défini sur false ![False](media/ad-fs-tshoot-initiatedsignon/idp2.png)
4.  Dans PowerShell, entrez :  `Set-AdfsProperties -EnableIdpInitiatedSignonPage $true`
5.  Vous ne verrez pas une confirmation donc entrer à nouveau de Get-AdfsProperties et vérifiez que **EnableIdpInitatedSignonPage** est définie sur true.
![True](media/ad-fs-tshoot-initiatedsignon/idp4.png)

## <a name="test-authentication"></a>Tester l'authentification
Utilisez la procédure suivante pour tester l’authentification AD FS avec la page authentification Idp-Initiated.

1.  Ouvrez un navigateur web et accédez à la page authentification Idp.  Exemple :  https://sts.contoso.com/adfs/ls/idpinitiatedsignon.htm
2.  Vous devez invité pour se connecter.  Entrez vos informations d’identification.
![l’authentification](media/ad-fs-tshoot-initiatedsignon/idp5.png)
3.  Si cela réussit, vous devez être connecté.


## <a name="test-authentication-using-a-seamless-logon-experience"></a>Tester l’authentification à l’aide d’une expérience transparente d’ouverture de session
Vous pouvez tester l’expérience transparente d’ouverture de session en vous assurant que l’URL pour vos serveurs AD FS sont ajoutés à la zone intranet local de vos options internet.  Utilisez la procédure suivante :

1.  Sur un client Windows 10, cliquez sur Démarrer et entrez les options internet et sélectionnez options internet.
2.   Cliquez sur l’onglet sécurité, cliquez sur l’intranet local et cliquez sur le bouton sites.
![Transparente](media/ad-fs-tshoot-initiatedsignon/idp8.png)
1.  Cliquez sur Paramètres avancés.
2.  Entrez votre url et cliquez sur Ajouter.  Cliquez sur Fermer.
![Ajouter des url](media/ad-fs-tshoot-initiatedsignon/idp9.png)
1.  Cliquez sur Ok.  Cliquez sur Ok.  Cela doit fermer les options internet.
2.  Ouvrez un navigateur web et accédez à la page authentification Idp.  Exemple :  https://sts.contoso.com/adfs/ls/idpinitiatedsignon.htm
3.  Cliquez sur le bouton de connexion.  Vous devez vous connecter automatiquement et ne pas invité à entrer d’informations d’identification.
![Transparente](media/ad-fs-tshoot-initiatedsignon/idp6.png)

## <a name="next-steps"></a>Étapes suivantes

- [Résolution des problèmes de AD FS](ad-fs-tshoot-overview.md)