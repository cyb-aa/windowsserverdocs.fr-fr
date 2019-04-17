---
ms.assetid: 25f5aff1-6abf-4dea-b531-f1d9943bc181
title: ADFS personnalisation dans Windows Server2016
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2e4d463f5f25fe85dc95d767c9c75b722e60b012
ms.sourcegitcommit: a2699e93a0a19cb138c1fde0c9af36774a12f865
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/20/2017
---
# <a name="ad-fs-customization-in-windows-server-2016"></a>ADFS personnalisation dans Windows Server2016

>S’applique à: Windows Server2016

En réponse aux commentaires des organisations à l’aide d’ADFS, nous avons ajouté des outils supplémentaires pour personnaliser l’utilisateur se connecter pour les applications individuelles protégés par ADFS.  
En plus de spécifier le contenu de chaque application web telles que le texte de description et des liens, vous pouvez maintenant spécifier thèmes web ensemble par application.  Cela inclut le logo, illustration, les feuilles de style ou un fichier entier onload.js.  
  
## <a name="global-settings"></a>Paramètres globaux    
Pour les paramètres globaux générales vous pouvez faire référence à [personnalisation des Pages ADFS Sign-in](https://technet.microsoft.com/library/dn280950.aspx) fournie avec ADFS dans Windows Server2012R2.  
  
## <a name="pre-requisites"></a>Conditions préalables  
Les conditions préalables suivantes sont requises avant d’essayer les procédures décrites dans ce document.  
  
-   ADFS dans Windows Server2016 TP4 ou version ultérieure  
  
## <a name="configure-ad-fs-relying-parties"></a>Configurez les parties de partie de confiance ADFS  
Par partie de confiance connexion web des parties de thèmes et les éléments peuvent être configurés à l’aide de PowerShell exemples ci-dessous:  
  
### <a name="customize-messages"></a>Personnaliser les messages  
  
```  
PS C:\>Set-AdfsRelyingPartyWebContent  
    -TargetRelyingPartyName "<RP trust Name>"  
    -CompanyName "This text appears in place of the federation service display name"  
    -OrganizationalNameDescriptionText "This text appears right below the company name"  
    -SignInPageDescription "This text appears below the credential prompt"  
```  
  
### <a name="customize-company-name-logo-and-image"></a>Personnaliser l’image, le logo et le nom de société  
  
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
  
Pour des thèmes personnalisés, consultez [personnalisation des Pages ADFS Sign-in](https://technet.microsoft.com/library/dn280950.aspx) et [Advanced Customization of ADFS Sign-in Pages.](https://technet.microsoft.com/library/dn636121.aspx)  
  
## <a name="assigning-custom-web-themes-per-rp"></a>Affectation de thèmes web personnalisés par RP  
  
Pour attribuer un thème personnalisé par RP, utilisez la procédure suivante:  
  
1. Créer un nouveau thème en tant qu’une copie de la valeur par défaut, le thème global dans ADFS  
<code>New-AdfsWebTheme -Name AppSpecificTheme -SourceName default</code>  
2.  Exporter le thème de personnalisation <code>Export-AdfsWebTheme -Name AppSpecificTheme -DirectoryPath c:\appspecifictheme</code>3.personnaliser les fichiers de thème (images, css, onload.js) dans l’éditeur de votre choix ou remplacer le fichier 4. Importer des fichiers personnalisés du système de fichiers à ADFS (ciblant le nouveau thème) <code>Set-AdfsWebTheme -TargetName AppSpecificTheme -AdditionalFileResource @{Uri='/adfs/portal/script/onload.js';Path="c:\appspecifictheme\script\onload.js"}</code>5.appliquer le thème personnalisée au RP spécifique (ou de RP)
<code>Set-AdfsRelyingPartyWebTheme -TargetRelyingPartyName urn:app1 -SourceWebThemeName AppSpecificTheme</code>  
  
## <a name="home-realm-discovery"></a>Découverte de domaine d’accueil  
Pour la détection du domaine d’accueil personnalisation voir [personnalisation des Pages ADFS Sign-in](https://technet.microsoft.com/library/dn280950.aspx).  
  
## <a name="updated-password-page"></a>Page mot de passe mis à jour  
Pour plus d’informations sur la personnalisation de la page de mot de passe de mise à jour, voir [personnalisation des Pages ADFS Sign-in](https://technet.microsoft.com/library/dn280950.aspx).  
  
## <a name="customizing-and-alternate-ids"></a>ID de personnalisation et de remplacement  
Utilisateurs peuvent se connecter à ActiveDirectory Federation Services (ADFS)-activée des applications à l’aide de n’importe quel écran de l’identificateur d’utilisateur qui est acceptée par les Services de domaine ActiveDirectory (ADDS). Ceux-ci incluent des noms d’utilisateur Principal (UPN) (johndoe@contoso.com) ou de domaine complet des noms de compte sam (contoso\johndoe ou contoso.com\johndoe #).  Pour plus d’informations sur cet voir [ID de la configuration de connexion alternatif.](Configuring-Alternate-Login-ID.md)  
  
Vous voudrez peut-être plus personnaliser la page de connexion ADFS pour donner aux utilisateurs finaux des information sur l’ID de connexion de substitution. Vous pouvez le faire en ajoutant la description de la page de connexion personnalisée pour plus d’informations, voir [personnalisation des Pages ADFS Sign-in.](https://technet.microsoft.com/library/dn280950.aspx)   
  
Vous pouvez également accomplir cela en personnalisant la chaîne de «ouvrir une session avec un compte d’organisation» ci-dessus champ de nom d’utilisateur.  Pour plus d’informations sur cette voir [Advanced Customization of ADFS Sign-in Pages](https://technet.microsoft.com/library/dn636121.aspx).  

## <a name="additional-references"></a>Références supplémentaires 
[ADFS Sign-in personnalisation de l’utilisateur](AD-FS-user-sign-in-customization.md)  
