---
ms.assetid: 25f5aff1-6abf-4dea-b531-f1d9943bc181
title: Personnalisation de la AD FS dans Windows Server 2016
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b8832e7e53e94761a489e850726bbd206b8be62b
ms.sourcegitcommit: 02f1e11ba37a83e12d8ffa3372e3b64b20d90d00
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68863433"
---
# <a name="ad-fs-customization-in-windows-server-2016"></a>Personnalisation de la AD FS dans Windows Server 2016


En réponse aux commentaires des organisations qui utilisent AD FS, nous avons ajouté des outils supplémentaires pour personnaliser l’expérience de connexion utilisateur pour les applications individuelles protégées par AD FS.  
En plus de spécifier le contenu Web par application, tel que le texte de description et les liens, vous pouvez désormais spécifier des thèmes Web entiers par application.  Cela comprend le logo, l’illustration, les feuilles de style ou un fichier OnLoad. js entier.  
  
## <a name="global-settings"></a>Paramètres globaux    
Pour les paramètres globaux généraux, vous pouvez consulter [Personnalisation des pages de connexion AD FS](https://technet.microsoft.com/library/dn280950.aspx) fournies avec AD FS dans Windows Server 2012 R2.  
  
## <a name="pre-requisites"></a>Prérequis  
Avant de tenter les procédures décrites dans ce document, les conditions préalables suivantes sont requises.  
  
-   AD FS dans Windows Server 2016 TP4 ou version ultérieure  
  
## <a name="configure-ad-fs-relying-parties"></a>Configurer AD FS des parties de confiance  
Les éléments et les thèmes Web de connexion par partie de confiance peuvent être configurés à l’aide des exemples PowerShell ci-dessous:  
  
### <a name="customize-messages"></a>Personnaliser les messages  
  
```  
PS C:\>Set-AdfsRelyingPartyWebContent  
    -TargetRelyingPartyName "<RP trust Name>"  
    -CompanyName "This text appears in place of the federation service display name"  
    -OrganizationalNameDescriptionText "This text appears right below the company name"  
    -SignInPageDescription "This text appears below the credential prompt"  
```  
  
### <a name="customize-company-name-logo-and-image"></a>Personnaliser le nom, le logo et l’image de l’entreprise  
  
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
  
## <a name="custom-themes-and-advanced-custom-themes"></a>Thèmes personnalisés et thèmes personnalisés avancés  
  
Pour les thèmes personnalisés, reportez-vous à [Personnalisation des pages de connexion AD FS](https://technet.microsoft.com/library/dn280950.aspx) et à la [personnalisation avancée des pages de connexion AD FS.](https://technet.microsoft.com/library/dn636121.aspx)  
  
## <a name="assigning-custom-web-themes-per-rp"></a>Affectation de thèmes Web personnalisés par RP  
  
Pour affecter un thème personnalisé par RP, utilisez la procédure suivante:  
  
1. Créer un nouveau thème en tant que copie pour le thème global par défaut dans AD FS  
`New-AdfsWebTheme -Name AppSpecificTheme -SourceName default`  
2. Exporter le thème pour la personnalisation  
`Export-AdfsWebTheme -Name AppSpecificTheme -DirectoryPath c:\appspecifictheme`  
3. Personnaliser les fichiers de thème (images, CSS, OnLoad. js)-dans votre éditeur favori ou remplacer le fichier  
4. Importer des fichiers personnalisés à partir du système de fichiers vers AD FS (ciblant le nouveau thème)  
`Set-AdfsWebTheme -TargetName AppSpecificTheme -AdditionalFileResource @{Uri='/adfs/portal/script/onload.js';Path="c:\appspecifictheme\script\onload.js"}`  
5. Appliquer le nouveau thème personnalisé au RP spécifique (ou au RP)  
`Set-AdfsRelyingPartyWebTheme -TargetRelyingPartyName urn:app1 -SourceWebThemeName AppSpecificTheme`  
  
## <a name="home-realm-discovery"></a>Découverte de domaine d'accueil  
Pour la personnalisation de la découverte du domaine d’hébergement, consultez [Personnalisation des pages de connexion AD FS](https://technet.microsoft.com/library/dn280950.aspx).  
  
## <a name="updated-password-page"></a>Page mot de passe mis à jour  
Pour plus d’informations sur la personnalisation de la page de mise à jour du mot de passe, consultez [Personnalisation des pages de connexion AD FS](https://technet.microsoft.com/library/dn280950.aspx).  
  
## <a name="customizing-and-alternate-ids"></a>Personnalisation et autres ID  
Les utilisateurs peuvent se connecter à des applications Services ADFS (AD FS) à l’aide de n’importe quelle forme d’identificateur d’utilisateur acceptée par Active Directory Domain Services (AD DS). Celles-ci incluent les noms d’utilisateur principaljohndoe@contoso.com(UPN) () ou les noms de compte Sam qualifiés de domaine (contoso\johndoe ou contoso. com\johndoe).  Pour plus d’informations, consultez [configuration d’un ID de connexion de substitution.](Configuring-Alternate-Login-ID.md)  
  
Vous pouvez en outre personnaliser la page de connexion AD FS pour fournir aux utilisateurs finaux des indications sur l’ID de connexion de substitution. Vous pouvez le faire en ajoutant la description de la page de connexion personnalisée pour plus d’informations, consultez [Personnalisation des pages de connexion AD FS.](https://technet.microsoft.com/library/dn280950.aspx)   
  
Vous pouvez également effectuer cette opération en personnalisant la chaîne «se connecter avec le compte de l’organisation» au-dessus du champ nom d’utilisateur.  Pour plus d’informations, consultez [personnalisation avancée des pages de connexion AD FS](https://technet.microsoft.com/library/dn636121.aspx).  

## <a name="additional-references"></a>Références supplémentaires 
[Personnalisation de la connexion de l’utilisateur AD FS](AD-FS-user-sign-in-customization.md)  
