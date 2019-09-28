---
ms.assetid: 7e804590-6d6c-4cca-ac14-02d4dff06cec
title: Mettre à jour la personnalisation du mot de passe
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: e007d4449cb62e7888c30f5b5929e393d7b571ef
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407451"
---
# <a name="update-password-customization"></a>Mettre à jour la personnalisation du mot de passe 


Dans certaines situations, les utilisateurs ne peuvent pas se connecter au réseau d'entreprise pour modifier le mot de passe de leur compte. Cela peut être gênant, notamment pour les employés distants qui habitent loin du bureau de l'entreprise le plus proche. Dans ces cas, l'utilisateur doit se connecter à Internet pour utiliser la page de mise à jour du mot de passe.  
  
Vous pouvez personnaliser la page de mise à jour du mot de passe en fournissant votre propre description pour la page.  
  
> Pour activer la page de mise à jour du mot de passe, accédez à Gestion AD FS sous Points de terminaison. Le point de terminaison pour la mise à jour du mot de passe se trouve en bas, sous Autre - /adfs/portal/updatepassword/. Après avoir activé le point de terminaison, vous devez redémarrer le service AD FS manuellement. Vous pouvez ensuite accéder à l'adresse https://<fqdn>/adfs/portal/updatepassword/ depuis un appareil joint à un espace de travail ; la page de mise à jour du mot de passe doit normalement apparaître.  
  
![mise à jour](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom5.png)  
  
## <a name="customize-the-update-password-page-description"></a>Personnaliser la description de la page de mise à jour du mot de passe  
Pour personnaliser la description de la page mettre à jour le mot de passe, utilisez l’applet de commande Windows PowerShell et la syntaxe suivantes.  
  

    Set-AdfsGlobalWebContent -UpdatePasswordPageDescriptionText "This is the Contoso Update Password page."  

## <a name="additional-references"></a>Références supplémentaires 
[Personnalisation de la connexion de l’utilisateur AD FS](AD-FS-user-sign-in-customization.md)  
