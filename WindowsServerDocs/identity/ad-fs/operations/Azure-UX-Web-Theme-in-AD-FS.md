---
title: Azure AD thème Web UX dans AD FS
description: Le document suivant explique comment modifier la connexion AD FS Forms pour qu’elle ressemble à l’expérience utilisateur Azure AD.
author: billmath
ms.author: billmath
manager: femila
ms.date: 10/24/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: f4dd1d45646475be3788cd6b615b1743976eedae
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358413"
---
# <a name="using-an-azure-ad-ux-web-theme-in-active-directory-federation-services"></a>Utilisation d’un thème Web Azure AD UX dans Services ADFS
Actuellement, la connexion AD FS Forms ne reflète pas l’expérience de connexion Azure/O365.  Pour fournir une expérience plus uniforme et homogène aux utilisateurs finaux, nous avons publié le thème Web suivant de feuille de style en cascade qui peut être appliqué à vos serveurs AD FS.  À l’heure actuelle, la connexion de formulaires pour AD FS sur Windows Server 2016 ressemble à ceci :

![Connexion actuelle](media/Azure-UX-Web-Theme-in-AD-FS/one.png)


Avec la nouvelle feuille de style, l’expérience utilisateur ressemble davantage aux expériences de connexion à Azure et Office 365.

## <a name="download-the-css-style-sheet"></a>Télécharger la feuille de style CSS
Vous pouvez télécharger le thème Web à partir de l' [emplacement](https://github.com/Microsoft/adfsWebCustomization/tree/master/centeredUi)GitHub suivant.


## <a name="enabling-the-new-web-theme"></a>Activation du nouveau thème Web
Pour activer le nouveau thème Web, procédez comme suit :

### <a name="to-enable-the-new-azure-ad-ux-web-theme-in-ad-fs"></a>Pour activer le nouveau thème Web Azure AD UX dans AD FS
1. Démarrer PowerShell en tant qu’administrateur
2. Créer un nouveau thème Web à l’aide de PowerShell :`New-AdfsWebTheme –Name custom –StyleSheet @{path="c:\NewTheme.css"}`
3. Définir le nouveau thème comme thème actif à l’aide de PowerShell :  `Set-AdfsWebConfig -ActiveThemeName custom`
   ![PowerShell](media/Azure-UX-Web-Theme-in-AD-FS/two.png)
4. Tester la connexion en accédant à https://<AD FS name.domain>/ADFS/LS/idpinitiatedsignon.htm ![Sign-On](media/Azure-UX-Web-Theme-in-AD-FS/three.png)

> ! OBSERVE Vous devez vous assurer que idpinitiatedsignon a été activé.  Elle n’est pas activée par défaut.  Pour activer idpinitiatedsignon, utilisez la commande PowerShell suivante :`Set-AdfsProperties –EnableIdpInitiatedSignonPage $True`

## <a name="image-recommendations"></a>Recommandations relatives aux images
L’activation de l’interface utilisateur centrée vous permet d’utiliser les mêmes images pour l’arrière-plan et le logo que vous possédez peut-être déjà pour la personnalisation de Azure Active Directory entreprise. En règle générale, les mêmes recommandations en matière de taille, de ratio et de format s’appliquent.

### <a name="logo"></a>Logo

Description | Contraintes | Recommandations
------- | ------- | ----------
Le logo s’affiche en haut du panneau de connexion. | JPG ou PNG transparent<br>Hauteur max. : 36 PX<br>Largeur maximale : 245 PX | Utilisez le logo de votre organisation ici.<br>Utilisez une image transparente. Ne partez pas du principe que l’arrière-plan sera blanc.<br>N’ajoutez pas de marge intérieure autour de votre logo dans l’image, sinon votre logo aura une petite taille disproportionnée.

### <a name="background"></a>Présentation

Description | Contraintes | Recommandations
------- | ------- | ----------
Cette option s’affiche à l’arrière-plan de la page de connexion, est ancrée au centre de l’espace affichable et met à l’échelle et rogne pour remplir la fenêtre du navigateur.    <br>Sur les écrans étroits tels que les téléphones mobiles, cette image n’est pas affichée.<br>Un masque noir avec une opacité de 0,55 est appliqué sur cette image lorsque la page est chargée. | JPG ou PNG<br>Dimensions de l’image : 1920 x 1080 PX<br>Taille du fichier : &lt;300 KO | <br>Utilisez des images qui n’ont pas de sujet fort. Le formulaire de connexion opaque apparaît au centre de cette image et peut couvrir n’importe quelle partie de l’image, en fonction de la taille de la fenêtre du navigateur.<br>Conservez une taille de fichier réduite pour garantir des temps de chargement rapides.

## <a name="next-steps"></a>Étapes suivantes
- [Personnalisation de la AD FS dans Windows Server 2016](AD-FS-Customization-in-Windows-Server-2016.md)
- [Personnalisation avancée](Advanced-Customization-of-AD-FS-Sign-in-Pages.md)
- [Thèmes Web personnalisés](Custom-Web-Themes-in-AD-FS.md)
