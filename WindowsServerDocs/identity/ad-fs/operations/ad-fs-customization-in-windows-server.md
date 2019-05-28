---
ms.assetid: 25f5aff1-6abf-4dea-b531-f1d9943bc181
title: Personnalisation d’AD FS dans Windows Server 2016
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b5aa22ad99529d99e2d7381a434916e8e749f185
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188764"
---
# <a name="ad-fs-customization-in-windows-server-2016"></a>Personnalisation d’AD FS dans Windows Server 2016


En réponse aux commentaires des organisations à l’aide d’AD FS, nous avons ajouté des outils supplémentaires pour personnaliser l’utilisateur se connecter pour les applications individuelles protégés par AD FS.  
En plus de spécifier le contenu de chaque application web tels que le texte de description et des liens, vous pouvez maintenant spécifier thèmes web tout entier par application.  Cela inclut le logo, illustration, feuilles de style ou un fichier onload.js entière.  
  
## <a name="global-settings"></a>Paramètres globaux    
Pour les paramètres globaux générales vous pouvez faire référence à [personnalisation des Pages de AD FS Sign-in](https://technet.microsoft.com/library/dn280950.aspx) fournie avec AD FS dans Windows Server 2012 R2.  
  
## <a name="pre-requisites"></a>Conditions préalables  
Les conditions préalables suivantes sont requises avant d’essayer les procédures décrites dans ce document.  
  
-   AD FS dans Windows Server 2016 TP4 ou version ultérieure  
  
## <a name="configure-ad-fs-relying-parties"></a>Configurez les parties de la partie de confiance AD FS  
Par partie de confiance tiers connexion web éléments et thèmes peuvent être configurés à l’aide de PowerShell ci-après :  
  
### <a name="customize-messages"></a>Personnaliser les messages  
  
```  
PS C:\>Set-AdfsRelyingPartyWebContent  
    -TargetRelyingPartyName "<RP trust Name>"  
    -CompanyName "This text appears in place of the federation service display name"  
    -OrganizationalNameDescriptionText "This text appears right below the company name"  
    -SignInPageDescription "This text appears below the credential prompt"  
```  
  
### <a name="customize-company-name-logo-and-image"></a>Personnaliser l’image, logo et nom de la société  
  
```  
PS C:\>Set-AdfsRelyingPartyWebTheme  
    -TargetRelyingPartyName "<RP trust Name>"  
    -Logo @{path="C:\Images\applogo.png"}  
    -Illustration @{path="C:\Images\appillustration.jpg"}  
```  
  
### <a name="customize-entire-page"></a>Personnaliser la page entière  
  
```  
PS C:\>Set-AdfsRelyingPartyWebTheme  
    -TargetRelyingPartyName "<RP trust Name>"  
    -OnLoadScriptPath @{path="c:\scripts\adfstheme\onload.js"}  
```  
  
## <a name="custom-themes-and-advanced-custom-themes"></a>Thèmes personnalisés et des thèmes personnalisés avancées  
  
Pour des thèmes personnalisés, consultez [personnalisation des Pages de AD FS Sign-in](https://technet.microsoft.com/library/dn280950.aspx) et [Advanced Customization of AD FS Sign-in Pages.](https://technet.microsoft.com/library/dn636121.aspx)  
  
## <a name="assigning-custom-web-themes-per-rp"></a>Affectation de thèmes web personnalisés par le fournisseur de ressources  
  
Pour affecter un thème personnalisé par RP, procédez comme suit :  
  
1. Créer un nouveau thème en tant que copie de la valeur par défaut, le thème global dans AD FS  
<code>New-AdfsWebTheme -Name AppSpecificTheme -SourceName default</code> 2.  Exporter le thème de personnalisation <code>Export-AdfsWebTheme -Name AppSpecificTheme -DirectoryPath c:\appspecifictheme</code>  
3. Personnaliser les fichiers de thème (images, css, onload.js -) dans votre éditeur favori ou remplacer le fichier 4. Importer des fichiers personnalisés du système de fichiers à AD FS (en ciblant le nouveau thème) <code>Set-AdfsWebTheme -TargetName AppSpecificTheme -AdditionalFileResource @{Uri='/adfs/portal/script/onload.js';Path="c:\appspecifictheme\script\onload.js"}</code>  
5. Appliquer le thème personnalisée au fournisseur de ressources spécifiques (ou du fournisseur de ressources) <code>Set-AdfsRelyingPartyWebTheme -TargetRelyingPartyName urn:app1 -SourceWebThemeName AppSpecificTheme</code>  
  
## <a name="home-realm-discovery"></a>Découverte de domaine d'accueil  
Pour la détection du domaine d’accueil personnalisation consultez [personnalisation des Pages de AD FS Sign-in](https://technet.microsoft.com/library/dn280950.aspx).  
  
## <a name="updated-password-page"></a>Page mot de passe mis à jour  
Pour plus d’informations sur la personnalisation de la page de mot de passe de mise à jour, consultez [Customizing the AD FS Sign-in Pages](https://technet.microsoft.com/library/dn280950.aspx).  
  
## <a name="customizing-and-alternate-ids"></a>ID de personnalisation et de remplacement  
Les utilisateurs peuvent se connecter à Active Directory Federation Services (AD FS)-activé des applications à l’aide de n’importe quel écran de l’identificateur d’utilisateur qui est acceptée par les Services de domaine Active Directory (AD DS). Ceux-ci incluent des noms d’utilisateur Principal (UPN) (johndoe@contoso.com) ou de domaine complet des noms de compte sam (contoso\johndoe ou contoso.com\johndoe).  Pour plus d’informations à ce sujet, consultez [ID de configuration une autre connexion.](Configuring-Alternate-Login-ID.md)  
  
Vous pouvez souhaiter également personnaliser la page de connexion AD FS pour donner aux utilisateurs finaux certain indicateur sur l’ID de connexion de substitution. Vous pouvez le faire en ajoutant la description de la page de connexion personnalisée pour plus d’informations, consultez [Customizing the AD FS Sign-in Pages.](https://technet.microsoft.com/library/dn280950.aspx)   
  
Vous pouvez également faire personnalisation « Connectez-vous avec un compte professionnel » la chaîne ci-dessus champ nom d’utilisateur.  Pour plus d’informations à ce sujet, consultez [Advanced Customization of AD FS Sign-in Pages](https://technet.microsoft.com/library/dn636121.aspx).  

## <a name="additional-references"></a>Références supplémentaires 
[AD FS Sign-in personnalisation de l’utilisateur](AD-FS-user-sign-in-customization.md)  
