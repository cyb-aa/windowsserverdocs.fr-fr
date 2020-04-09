---
ms.assetid: 626e33fc-4ac2-4d38-9ac9-addaa4c8d9bb
title: Personnalisation de la découverte de domaine d’hébergement
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 1667199e99545d3b29ba31512bd0c25718b93c4b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80816322"
---
# <a name="home-realm-discovery-customization"></a>Personnalisation de la découverte de domaine d’hébergement


Lorsque le client AD FS demande d’abord une ressource, le serveur de Fédération de ressources ne dispose d’aucune information sur le domaine du client. Le serveur de Fédération de ressources répond au client AD FS à l’aide d’une page de **découverte de domaine du client** , où l’utilisateur sélectionne le domaine d’origine dans une liste. Les valeurs de la liste proviennent de la propriété des noms complets dans les approbations de fournisseur de revendications. Utilisez les applets de commande Windows PowerShell suivantes pour modifier et personnaliser le AD FS expérience de la découverte de domaine d’hébergement.  
  
![domaine d’hébergement](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom4.png)  
  
> [!WARNING]  
> Gardez à l'esprit que le nom du fournisseur de revendications qui apparaît pour l'annuaire Active Directory local est le nom complet du service de fédération.  
  



## <a name="configure-identity-provider-to-use-certain-email-suffixes"></a>Configurer le fournisseur d'identité pour utiliser certains suffixes d'adresse de messagerie  
Une organisation peut fédérer avec plusieurs fournisseurs de revendications. AD FS fournit à présent la fonctionnalité in\-Box permettant aux administrateurs de répertorier les suffixes, par exemple, @us.contoso.com, @eu.contoso.com, qui est pris en charge par un fournisseur de revendications et de l’activer pour la découverte par suffixe\-. Avec cette configuration, les utilisateurs finaux peuvent taper leur compte professionnel et AD FS sélectionne automatiquement le fournisseur de revendications correspondant.  
  
Pour configurer un fournisseur d’identité \(IDP\), tel que `fabrikam`, pour utiliser certains suffixes d’adresse de messagerie, utilisez l’applet de commande Windows PowerShell et la syntaxe suivantes.  
  

`Set-AdfsClaimsProviderTrust -TargetName fabrikam -OrganizationalAccountSuffix @("fabrikam.com";"fabrikam2.com") ` 
 
>[!NOTE]
> Lors de la Fédération entre deux serveurs AD FS, affectez à la propriété PromptLoginFederation de l’approbation de fournisseur de revendications la valeur ForwardPromptAndHintsOverWsFederation.  Cela permet à AD FS de transférer le login_hint et d’inviter paramètre à l’IDP.  Pour ce faire, exécutez l’applet de commande PowerShell suivante :
>
>`Set-AdfsclaimsProviderTrust -PromptLoginFederation ForwardPromptAndHintsOverWsFederation`

## <a name="configure-an-identity-provider-list-per-relying-party"></a>Configurer une liste de fournisseurs d'identité par partie de confiance  
Dans certains scénarios, une organisation peut souhaiter que les utilisateurs finaux ne voient que les fournisseurs de revendications propres à une application, afin que la page de la découverte de domaine d'accueil n'affiche qu'une partie des fournisseurs de revendications.  
  
Pour configurer une liste IDP par partie de confiance \(RP\), utilisez l’applet de commande Windows PowerShell et la syntaxe suivantes.  
  
 
`Set-AdfsRelyingPartyTrust -TargetName claimapp -ClaimsProviderName @("Fabrikam","Active Directory") ` 

  
## <a name="bypass-home-realm-discovery-for-the-intranet"></a>Contourner la découverte de domaine d'accueil pour l'intranet  
La plupart des organisations ne prennent en charge un annuaire Active Directory local que pour l'accès utilisateur à partir de leur pare-feu. Dans ce cas, les administrateurs peuvent configurer AD FS pour contourner la découverte de domaine d’hébergement pour l’intranet.  
  
Pour contourner découverte du domaine pour l’intranet, utilisez l’applet de commande Windows PowerShell et la syntaxe suivantes.  
  

`Set-AdfsProperties -IntranetUseLocalClaimsProvider $true ` 
 
  
> [!IMPORTANT]  
> Notez que si une liste de fournisseurs d’identité pour une partie de confiance a été configurée, même si le paramètre précédent a été activé et que l’utilisateur accède à partir de l’intranet, AD FS affiche toujours la page de la découverte du domaine d’origine \(découverte du domaine\). Dans ce cas, pour contourner la découverte de domaine d'accueil, vous devez vous assurer que l'option « Active Directory » est également ajoutée à la liste de fournisseurs d'identité pour cette partie de confiance.  

## <a name="additional-references"></a>Références supplémentaires 
[Personnalisation de la connexion de l’utilisateur AD FS](AD-FS-user-sign-in-customization.md)  
