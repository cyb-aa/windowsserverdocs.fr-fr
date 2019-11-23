---
title: Résolution des problèmes de AD FS-authentification initiée par IDP
description: Ce document décrit comment résoudre les problèmes liés à la page de connexion AD FS.
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/03/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 89ee45bd0387cf728bc126529169b6b1ca045d6f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366190"
---
# <a name="ad-fs-troubleshooting---idp-initiated-sign-on"></a>Résolution des problèmes de AD FS-authentification initiée par IDP
La page d’authentification AD FS peut être utilisée pour déterminer si l’authentification fonctionne.  Pour ce faire, accédez à la page et connectez-vous.  En outre, nous pouvons utiliser la page de connexion pour vérifier que toutes les parties de confiance SAML 2,0 sont répertoriées.

## <a name="enable-the-idp-initiated-sign-on-page"></a>Activer la page de connexion initiée par IDP
Par défaut, les AD FS dans Windows 2016 ne sont pas activés sur la page de connexion.  Pour l’activer, vous pouvez utiliser la commande PowerShell Set-AdfsProperties.  Pour activer la page, procédez comme suit :

1.  Ouvrir Windows PowerShell
2.  Entrez : `Get-AdfsProperties` et appuyez sur entrée
3.  Vérifiez que **EnableIdpInitiatedSignonPage** est défini sur false ![false](media/ad-fs-tshoot-initiatedsignon/idp2.png)
4.  Dans PowerShell, entrez : `Set-AdfsProperties -EnableIdpInitiatedSignonPage $true`
5.  Vous ne verrez pas de confirmation, puis entrez à nouveau AdfsProperties et vérifiez que **EnableIdpInitatedSignonPage** est défini sur true.
![true](media/ad-fs-tshoot-initiatedsignon/idp4.png)

## <a name="test-authentication"></a>Tester l'authentification
Utilisez la procédure suivante pour tester l’authentification AD FS avec la page de connexion initiée par IDP.

1.  Ouvrez un navigateur Web et accédez à la page d’authentification IDP.  Exemple : https://sts.contoso.com/adfs/ls/idpinitiatedsignon.htm
2.  Vous devez être invité à vous connecter.  Entrez vos informations d’identification.
](media/ad-fs-tshoot-initiatedsignon/idp5.png) de l’authentification ![
3.  Si cela a réussi, vous devez être connecté.


## <a name="test-authentication-using-a-seamless-logon-experience"></a>Tester l’authentification à l’aide d’une expérience de connexion transparente
Vous pouvez tester l’expérience de connexion transparente en vous assurant que l’URL de vos serveurs AD FS est ajoutée à la zone Intranet local de vos options Internet.  Utilisez la procédure suivante :

1.  Sur un client Windows 10, cliquez sur Démarrer et tapez options Internet, puis sélectionnez Options Internet.
2.   Cliquez sur l’onglet sécurité, cliquez sur intranet local, puis cliquez sur le bouton sites.
](media/ad-fs-tshoot-initiatedsignon/idp8.png) ![transparent
1.  Cliquez sur Paramètres avancés.
2.  Entrez votre URL, puis cliquez sur Ajouter.  Cliquez sur Fermer.
![ajouter une URL](media/ad-fs-tshoot-initiatedsignon/idp9.png)
1.  Cliquez sur OK.  Cliquez sur OK.  Cela doit fermer les options Internet.
2.  Ouvrez un navigateur Web et accédez à la page d’authentification IDP.  Exemple : https://sts.contoso.com/adfs/ls/idpinitiatedsignon.htm
3.  Cliquez sur le bouton se connecter.  Vous devez vous connecter automatiquement et ne pas être invité à entrer vos informations d’identification.
](media/ad-fs-tshoot-initiatedsignon/idp6.png) ![transparent

## <a name="next-steps"></a>Étapes suivantes

- [Résolution des problèmes AD FS](ad-fs-tshoot-overview.md)
