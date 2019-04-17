---
title: "Thème Azure AD expérience utilisateur dans ADFS"
description: "Le document suivant explique comment modifier ADFS par formulaire de la connexion, afin qu’il ressemble à l’expérience utilisateur Azure AD."
author: billmath
ms.author: billmath
manager: femila
ms.date: 10/24/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e7bac1db17eb4facc7643fe0db0ccf00c119a45d
ms.sourcegitcommit: 9278435cbfa8dbeb30d0557ed0d27832b154edd2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/09/2018
---
# <a name="using-an-azure-ad-ux-web-theme-in-active-directory-federation-services"></a>À l’aide d’un thème Web d’Azure AD expérience utilisateur dans ActiveDirectory Federation Services
Connectez-vous les formulaires ADFS miroir actuellement pas de l’expérience de connexion Azure/O365.  Pour fournir une expérience plus homogène et transparente pour les utilisateurs finaux, nous avons publié le suivi en cascade style feuille thème web qui peut être appliqué à vos serveurs ADFS.  Actuellement, les formulaires de la connexion pour ADFS dans Windows Server2016 se présente comme suit:

![Connectez-vous en cours](media/Azure-UX-Web-Theme-in-AD-FS/one.png)


Avec la nouvelle feuille de style, l’expérience utilisateur sera ressemble plus à l’expérience de connexion Azure et Office365.

## <a name="download-the-css-style-sheet"></a>Télécharger la feuille de style CSS
Vous pouvez télécharger le thème web à partir de la Github suivant [emplacement ](https://github.com/Microsoft/adfsWebCustomization/tree/master/centeredUi).


## <a name="enabling-the-new-web-theme"></a>L’activation de nouveau thème web
Pour activer le nouveau thème web utilisez la procédure suivante:

### <a name="to-enable-the-new-azure-ad-ux-web-theme-in-ad-fs"></a>Pour activer le nouveau thème web Azure ADUX dans ADFS
1.  Démarrez PowerShell en tant qu’administrateur
2.  Créer un nouveau thème web à l’aide de PowerShell:  `New-AdfsWebTheme –Name custom –StyleSheet @{path="c:\NewTheme.css"}`
3.  Définir le nouveau thème comme le thème actif à l’aide de PowerShell: <ph x="1">'Set-AdfsWebConfig - ActiveThemeName custom'
! [</ph>PowerShell](media/Azure-UX-Web-Theme-in-AD-FS/two.png)
4.  Test de la connexion en accédant à https://<AD FS name.domain>/adfs/ls/idpinitiatedsignon.htm ![Sign-on](media/Azure-UX-Web-Theme-in-AD-FS/three.png)

>! [REMARQUE] Vous devez vous assurer qu’idpinitiatedsignon a été activée.  Il n’est pas activée par défaut.  Pour activer idpinitiatedsignon utilisez la commande PowerShell suivante:  `Set-AdfsProperties –EnableIdpInitiatedSignonPage $True`

## <a name="image-recommendations"></a>Recommandations d’image
Les recommandations en matière de taille de l’image d’arrière-plan et l’image du logo sont les suivantes:

### <a name="logo"></a>Logo
- 24px hauteur, largeur 256px
- N’ajoutez pas tout remplissage autour du logo au sein de l’actif.  Assurez-vous que l’arrière-plan actif est transparent.

### <a name="background"></a>En arrière-plan
- Taille de 1024 x 1080pixels avec une taille de fichier n’est supérieure à 200Ko.  Vous devez utiliser la résolution la plus élevée possible avec les proportions 16:9 / 16:10 qui maintient la taille des images sous la restriction.

## <a name="next-steps"></a>Étapes suivantes
- [ADFS personnalisation dans Windows Server2016](AD-FS-Customization-in-Windows-Server-2016.md)
- [Personnalisation avancée](Advanced-Customization-of-AD-FS-Sign-in-Pages.md)
- [Thèmes web personnalisés](Custom-Web-Themes-in-AD-FS.md)
