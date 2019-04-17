---
ms.assetid: 626e33fc-4ac2-4d38-9ac9-addaa4c8d9bb
title: "Personnalisation de la découverte de domaine d’accueil"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 02/09/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 14b19e2774cf1eac2f5f23ea6737219611886b4c
ms.sourcegitcommit: 877a50cd8d6e727048cdfac9b614a98ac3220876
ms.translationtype: HT
ms.contentlocale: fr-FR
---
# <a name="home-realm-discovery-customization"></a>Personnalisation de la découverte de domaine d’accueil

>S’applique à: Windows Server2016, Windows Server2012R2

Lorsque le client ADFS demande une ressource, le serveur de fédération de ressources n’a aucune information sur le domaine du client. Le serveur de fédération de ressources répond au client ADFS avec une **découverte de domaine Client** page, où l’utilisateur sélectionne le domaine d’accueil dans une liste. La liste de valeurs est renseignées à partir de la propriété nom d’affichage dans les approbations de fournisseur de revendications. Utilisez les applets de commande Windows PowerShell suivante pour modifier et personnaliser l’expérience ADFS accueil découverte de domaine.  
  
![Domaine d’accueil](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom4.png)  
  
> [!WARNING]  
> N’oubliez pas que le nom de fournisseur de revendications qui s’affiche pour ActiveDirectory local est le nom complet du service FS.  
  
## <a name="configure-identity-provider-to-use-certain-email-suffixes"></a>Configurer le fournisseur d’identité pour utiliser certains suffixes d’e-mail  
Une organisation peut fédérer avec plusieurs fournisseurs de revendications. ADFS fournit désormais la fonctionnalité ou pour les administrateurs répertorier les suffixes, par exemple, @us.contoso.com, @eu.contoso.com, qui est pris en charge par un fournisseur de revendications et l’activer pour la découverte basée sur suffix\. Avec cette configuration, les utilisateurs finaux entrent leur compte organisationnel et ADFS sélectionne automatiquement le fournisseur de revendications correspondant.  
  
Pour configurer un fournisseur d’identité \(IDP\), tel que `fabrikam`, pour utiliser certains suffixes d’e-mail, utilisez l’applet de commande Windows PowerShell et la syntaxe suivantes.  
  

`Set-AdfsClaimsProviderTrust -TargetName fabrikam -OrganizationalAccountSuffix @("fabrikam.com";"fabrikam2.com") ` 
 
  
## <a name="configure-an-identity-provider-list-per-relying-party"></a>Configurer une liste de fournisseurs d’identité par partie de confiance tiers  
Dans certains scénarios, une organisation peut souhaiter que les utilisateurs finaux de n’afficher que les fournisseurs de revendications qui sont spécifiques à une application afin que seul un sous-ensemble de fournisseur de revendications sont affichés sur la page de découverte de domaine d’accueil.  
  
Pour configurer une liste de fournisseurs par partie de confiance tiers \(RP\), utilisez l’applet de commande Windows PowerShell suivante et la syntaxe.  
  
 
`Set-AdfsRelyingPartyTrust -TargetName claimapp -ClaimsProviderName @("Fabrikam","Active Directory") ` 

  
## <a name="bypass-home-realm-discovery-for-the-intranet"></a>Contourner la découverte de domaine d’accueil pour l’intranet  
La plupart des organisations prennent uniquement en charge leur ActiveDirectory local pour tout utilisateur qui accède à partir de leur pare-feu. Dans ce cas, les administrateurs peuvent configurer ADFS pour contourner la découverte de domaine d’accueil pour l’intranet.  
  
Pour l’intranet pour ce FAIRE, utilisez l’applet de commande Windows PowerShell suivante et la syntaxe.  
  

`Set-AdfsProperties -IntranetUseLocalClaimsProvider $true ` 
 
  
> [!IMPORTANT]  
> Veuillez Remarque que si une liste de fournisseurs d’identité pour une partie de confiance tiers a été configuré, même si le paramétrage antérieur a été activé et que l’utilisateur accède à partir de l’intranet, ADFS affiche la page \(HRD\) de découverte de domaine d’accueil. Pour ce FAIRE dans ce cas, vous devez vous assurer que «ActiveDirectory» est également ajouté à la liste de fournisseurs pour cette partie de confiance.  

## <a name="additional-references"></a>Références supplémentaires 
[ADFS Sign-in personnalisation de l’utilisateur](AD-FS-user-sign-in-customization.md)  
