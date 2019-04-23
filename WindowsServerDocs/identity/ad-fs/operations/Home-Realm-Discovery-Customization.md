---
ms.assetid: 626e33fc-4ac2-4d38-9ac9-addaa4c8d9bb
title: Personnalisation de la découverte de domaine d’accueil
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 1198d8b76f2ecdad728e2de6ce7a5c0d053f779f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868930"
---
# <a name="home-realm-discovery-customization"></a>Personnalisation de la découverte de domaine d’accueil

>S'applique à : Windows Server 2016, Windows Server 2012 R2

Lorsque le client AD FS demande tout d’abord une ressource, le serveur de fédération de ressources n’a aucune information sur le domaine du client. Le serveur de fédération de ressources répond au client AD FS avec un **découverte du domaine Client** page, où l’utilisateur sélectionne le domaine d’accueil dans une liste. Les valeurs de la liste proviennent de la propriété des noms complets dans les approbations de fournisseur de revendications. Utiliser les applets de commande Windows PowerShell suivante pour modifier et personnaliser l’expérience de découverte AD FS d’accueil.  
  
![domaine d’accueil](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom4.png)  
  
> [!WARNING]  
> Gardez à l'esprit que le nom du fournisseur de revendications qui apparaît pour l'annuaire Active Directory local est le nom complet du service de fédération.  
  



## <a name="configure-identity-provider-to-use-certain-email-suffixes"></a>Configurer le fournisseur d'identité pour utiliser certains suffixes d'adresse de messagerie  
Une organisation peut fédérer avec plusieurs fournisseurs de revendications. AD FS fournit désormais le dans\-zone permettant aux administrateurs répertorier les suffixes, par exemple, @us.contoso.com, @eu.contoso.com, qui est pris en charge par un fournisseur de revendications et l’activer pour le suffixe\-en fonction de découverte. Avec cette configuration, les utilisateurs peuvent saisir leur compte professionnel et AD FS sélectionne automatiquement le fournisseur de revendications correspondant.  
  
Pour configurer un fournisseur d’identité \(IDP\), tel que `fabrikam`, pour utiliser certains suffixes d’e-mail, utilisez l’applet de commande Windows PowerShell et la syntaxe suivante.  
  

`Set-AdfsClaimsProviderTrust -TargetName fabrikam -OrganizationalAccountSuffix @("fabrikam.com";"fabrikam2.com") ` 
 
>[!NOTE]
> Lors de la fédération entre deux serveurs AD FS, définissez PromptLoginFederation propriété sur l’approbation de fournisseur de revendications à ForwardPromptAndHintsOverWsFederation.  Il s’agit donc que AD FS transfère le login_hint et le paramètre invite vers le fournisseur d’identité.  Cela est possible en exécutant l’applet de commande PowerShell suivante :
>
>`Set-AdfsclaimsProviderTrust -PromptLoginFederation ForwardPromptAndHintsOverWsFederation`

## <a name="configure-an-identity-provider-list-per-relying-party"></a>Configurer une liste de fournisseurs d'identité par partie de confiance  
Dans certains scénarios, une organisation peut souhaiter que les utilisateurs finaux ne voient que les fournisseurs de revendications propres à une application, afin que la page de la découverte de domaine d'accueil n'affiche qu'une partie des fournisseurs de revendications.  
  
Pour configurer une liste de fournisseur d’identité par partie de confiance tiers \(RP\), utilisez l’applet de commande Windows PowerShell suivante et la syntaxe.  
  
 
`Set-AdfsRelyingPartyTrust -TargetName claimapp -ClaimsProviderName @("Fabrikam","Active Directory") ` 

  
## <a name="bypass-home-realm-discovery-for-the-intranet"></a>Contourner la découverte de domaine d'accueil pour l'intranet  
La plupart des organisations ne prennent en charge un annuaire Active Directory local que pour l'accès utilisateur à partir de leur pare-feu. Dans ce cas, les administrateurs peuvent configurer AD FS pour contourner la découverte de domaine d’accueil pour l’intranet.  
  
Pour ce faire pour l’intranet, utilisez l’applet de commande Windows PowerShell suivante et la syntaxe.  
  

`Set-AdfsProperties -IntranetUseLocalClaimsProvider $true ` 
 
  
> [!IMPORTANT]  
> Veuillez noter que si une liste de fournisseurs d’identité pour une partie de confiance a été configurée, même si le paramètre précédent a été activé et l’utilisateur se connecte à partir de l’intranet, AD FS affiche toujours la découverte du domaine d’accueil \(HRD\) page. Dans ce cas, pour contourner la découverte de domaine d'accueil, vous devez vous assurer que l'option « Active Directory » est également ajoutée à la liste de fournisseurs d'identité pour cette partie de confiance.  

## <a name="additional-references"></a>Références supplémentaires 
[AD FS Sign-in personnalisation de l’utilisateur](AD-FS-user-sign-in-customization.md)  
