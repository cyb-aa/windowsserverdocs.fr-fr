---
ms.assetid: 7e804590-6d6c-4cca-ac14-02d4dff06cec
title: "Personnalisation de mot de passe de mise à jour"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4b06992bfb398b66988ad4882217a8a83738365e
ms.sourcegitcommit: 78d8839ccafa9530784cb9e38c3127ed2c215423
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/09/2018
---
# <a name="update-password-customization"></a>Personnalisation de mot de passe de mise à jour 

>S’applique à: Windows Server2016, Windows Server2012R2

Dans certains cas, les utilisateurs n’est peut-être pas en mesure de se connecter au réseau d’entreprise de modifier leur mot de passe du compte. Cela peut être gênant, notamment pour les employés distants qui habitent loin du plus proche office d’entreprise. Pour ces cas, la page de mot de passe de mise à jour peut être utilisée par se connecter à Internet.  
  
Vous pouvez personnaliser la page de mot de passe de mise à jour en fournissant votre propre description de la page.  
  
> Pour activer la page de mise à jour du mot de passe, accédez à gestion ADFS sous points de terminaison. Le point de terminaison pour le mot de passe de mise à jour se trouve en bas, sous autre - / adfs/portail/updatepassword /. Une fois que vous avez activé le point de terminaison, vous devez redémarrer le service ADFS. Cela doit être fait manuellement. Vous pouvez ensuite accéder à https://<fqdn>/adfs/portail/updatepassword/sur un espace de travail joint l’appareil et vous devez voir la page de mot de passe de mise à jour.  
  
![Mise à jour](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom5.png)  
  
## <a name="customize-the-update-password-page-description"></a>Personnaliser la description de la page mot de passe de mise à jour  
Pour personnaliser la description de page de mot de passe de mise à jour, utilisez l’applet de commande Windows PowerShell suivante et la syntaxe.  
  

    Set-AdfsGlobalWebContent -UpdatePasswordPageDescriptionText "This is the Contoso Update Password page."  

## <a name="additional-references"></a>Références supplémentaires 
[ADFS Sign-in personnalisation de l’utilisateur](AD-FS-user-sign-in-customization.md)  
