---
title: Invite ADFS = connexion
description: Forum aux questions pour ADFS2016
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/27/2017
ms.topic: article
ms.custom: it-pro
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 8b98cdc27c9bd01d9855e98965f59ebe5257ed5c
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="active-directory-federation-services-promptlogin-parameter-support"></a>ActiveDirectory Federation Services invite = prise en charge du paramètre de connexion
Le document suivant explique la prise en charge native pour le paramètre de connexion = invite qui est disponible dans ADFS.

## <a name="what-is-promptlogin"></a>What's invite = connexion?  

Certaines applications Office 365 (avec l’authentification moderne activé) envoient le paramètre d’ouverture de session = invite à Azure AD dans le cadre de chaque demande d’authentification.  Par défaut, Azure AD cela traduit en deux paramètres: <code><b>wauth</b>=urn:oasis:names:tc:SAML:1.0:am:password</code>, et <code><b>wfresh</b>=0</code>.

Cela peut entraîner des problèmes d’intranet d’entreprise et de scénarios d’authentification multifacteur dans lequel vous souhaitez un type d’authentification autre que le nom d’utilisateur et mot de passe.  

ADFS dans Windows Server2012R2 avec le correctif cumulatif de juillet2016 introduit la prise en charge native pour le paramètre de connexion = invite.  Cela signifie, vous avez désormais la possibilité de configurer Azure AD pour envoyer ce paramètre en tant que-est à votre service ADFS dans le cadre d’Azure AD et les demandes d’authentification Office 365.

### <a name="ad-fs-versions-that-support-promptlogin"></a>Les versions ADFS qui prennent en charge invite = connexion
Voici une liste des versions d’ADFS qui prennent en charge le paramètre de connexion = invite.

- Correctif cumulatif de ADFS dans Windows Server2012R2 avec le juillet2016

- ADFS dans Windows Server2016

## <a name="how-do-to-configure-your-azure-ad-tenant-to-send-promptlogin-to-ad-fs"></a>Comment faire pour configurer votre client Azure AD pour envoyer invite = connexion à ADFS

Utilisez le module Azure AD PowerShell pour configurer le paramètre.

> [!NOTE]
> La fonctionnalité de connexion = invite (activée par la propriété PromptLoginBehavior) est actuellement disponible uniquement dans les [' version1.0' Azure AD module Powershell](https://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185), dans laquelle les applets de commande ont des noms qui incluent «Msol», par exemple, Set-MsolDomainFederationSettings.  Il n’est pas actuellement disponible via ' version2.0» Azure AD module PowerShell, dont applets de commande ont des noms comme «définir-AzureAD\ *».

Pour configurer l’invite = le comportement de connexion, la syntaxe de l’applet de commande ci-dessous:

Exemple 1:
```powershell
    Set-MsolDomainFederationSettings –DomainName <your domain name> -PreferredAuthenticationProtocol <your current protocol setting> 
```

Exemple 2:
```powershell
    Set-MsolDomainFederationSettings –DomainName <your domain name> -SupportsMfa <$True|$False>
```

Exemple 3:
```powershell
    Set-MsolDomainFederationSettings –DomainName <your domain name> -PromptLoginBehavior <TranslateToFreshPasswordAuth|NativeSupport|Disabled>
```

 
 Vous trouverez les valeurs de propriété PreferredAuthenticationProtocol SupportsMfa et PromptLoginBehavior en affichant la sortie de l’applet de commande: ![Get-MsolDomainFederationSettings](media/AD-FS-Prompt-Login/GetMsol.png)
```powershell
    Get-MsolDomainFederationSettings -DomainName <your_domain_name> | fl *
 ```
> [!NOTE]
> Par défaut, lors de l’exécution Get-MsolDomainFederationSettings, certaines propriétés ne sont pas affichées dans la console.  Pour afficher ces paramètres, il est recommandé d’utiliser le | fl * pour forcer la sortie de toutes les propriétés de l’objet.


Voici plus d’informations sur le paramètre PromptLoginBehavior et ses paramètres.
   
   - <b>TranslateToFreshPasswordAuth</b> signifie que le comportement d’Azure ActiveDirectory par défaut de l’envoi <b>wauth</b> et <b>wfresh</b> à ADFS au lieu de l’invite de commandes = connexion
   - <b>NativeSupport</b> signifie que le paramètre de connexion = demande sera envoyé en l’état à ADFS
   - <b>Désactivé</b> signifie que rien ne sera envoyée à ADFS

