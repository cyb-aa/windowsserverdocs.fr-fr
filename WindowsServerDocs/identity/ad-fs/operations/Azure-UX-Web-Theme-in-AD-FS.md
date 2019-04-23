---
title: Thème Web de l’expérience utilisateur de Azure AD dans AD FS
description: Le document suivant explique comment modifier le AD FS forms sign-in afin qu’il ressemble à l’expérience utilisateur Azure AD.
author: billmath
ms.author: billmath
manager: femila
ms.date: 10/24/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 8d6afd7829c92382815e95b8c43a054b000359e2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887880"
---
# <a name="using-an-azure-ad-ux-web-theme-in-active-directory-federation-services"></a>À l’aide d’un thème Web de l’expérience utilisateur de Azure AD dans Active Directory Federation Services
Les services AD FS forms signe dans actuellement ne reflète pas l’expérience de connexion Azure/Office 365.  Pour fournir une expérience plus uniforme et transparente pour les utilisateurs finaux, nous avons publié le suivi en cascade style feuille thème web qui peut être appliqué à vos serveurs AD FS.  Actuellement, les formulaires connectez-vous pour AD FS sur Windows Server 2016 ressemble au suivant :

![Connectez-vous en cours](media/Azure-UX-Web-Theme-in-AD-FS/one.png)


Avec la nouvelle feuille de style, l’expérience utilisateur sera ressemble plus aux expériences de connexion Azure et Office 365.

## <a name="download-the-css-style-sheet"></a>Télécharger la feuille de style CSS
Vous pouvez télécharger le thème web à partir de Github suivant [emplacement](https://github.com/Microsoft/adfsWebCustomization/tree/master/centeredUi).


## <a name="enabling-the-new-web-theme"></a>L’activation du nouveau thème web
Pour activer le nouveau thème web procédez comme suit :

### <a name="to-enable-the-new-azure-ad-ux-web-theme-in-ad-fs"></a>Pour activer le nouveau thème web de l’expérience utilisateur de Azure AD dans AD FS
1.  Démarrez PowerShell en tant qu’administrateur
2.  Créer un nouveau thème web à l’aide de PowerShell :  `New-AdfsWebTheme –Name custom –StyleSheet @{path="c:\NewTheme.css"}`
3.  Définissez le nouveau thème comme thème actif à l’aide de PowerShell :  `Set-AdfsWebConfig -ActiveThemeName custom`
![PowerShell](media/Azure-UX-Web-Theme-in-AD-FS/two.png)
4.  Tester l’authentification dans en accédant à https://<AD FS name.domain>/adfs/ls/idpinitiatedsignon.htm ![Sign-on](media/Azure-UX-Web-Theme-in-AD-FS/three.png)

> ! [REMARQUE] Vous devez vous assurer qu’authentification idpinitiatedsignon a été activée.  Il n’est pas activé par défaut.  Pour activer l’authentification idpinitiatedsignon, utilisez la commande PowerShell suivante :  `Set-AdfsProperties –EnableIdpInitiatedSignonPage $True`

## <a name="image-recommendations"></a>Recommandations de l’image
L’activation de l’interface utilisateur centré vous permet d’utiliser les images mêmes pour l’arrière-plan et le logo que vous disposez peut-être déjà pour la marque de société Azure Active Directory. En règle générale, les mêmes recommandations pour la taille, de rapport et de format s’appliquent.

### <a name="logo"></a>Logo
Description | Contraintes | Recommandations
------- | ------- | ----------
Le logo s’affiche en haut du Panneau de connexion. | JPG ou PNG transparent<br>Hauteur maximale : 36 px<br>Largeur max. : 245 px | Utiliser le logo de votre organisation ici.<br>Utiliser une image transparente. Ne supposez pas que l’arrière-plan sera blanc.<br>N’ajoutez pas de marge intérieure autour de votre logo dans l’image, sinon votre logo s’affichera disproportionnellement petit.

### <a name="background"></a>Arrière-plan
Description | Contraintes | Recommandations
------- | ------- | ----------
Cette option s’affiche dans l’arrière-plan de la page de connexion, est ancrée au centre de l’espace visible et de mises à l’échelle et rognée pour remplir la fenêtre du navigateur.    <br>Sur les écrans étroits tels que les téléphones mobiles, cette image n’est pas affichée.<br>Un masque noir avec une opacité de 0,55 est appliqué sur cette image lors du chargement de la page. | JPG ou PNG<br>Dimensions de l’image : 1920 x 1080 px<br>Taille du fichier : &lt; 300 KO | <br>Utiliser des images lorsqu’il n’est pas un focus forte sur l’objet. Le formulaire de connexion opaque apparaît au centre de cette image et peut couvrir une partie de l’image, selon la taille de la fenêtre du navigateur.<br>Minimiser la taille du fichier pour garantir des temps de chargement rapides.

## <a name="next-steps"></a>Étapes suivantes
- [Personnalisation d’AD FS dans Windows Server 2016](AD-FS-Customization-in-Windows-Server-2016.md)
- [Personnalisation avancée](Advanced-Customization-of-AD-FS-Sign-in-Pages.md)
- [Thèmes web personnalisés](Custom-Web-Themes-in-AD-FS.md)
