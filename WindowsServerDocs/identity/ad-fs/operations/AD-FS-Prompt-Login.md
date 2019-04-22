---
title: Invite d’AD FS = connexion
description: Forum aux questions pour AD FS 2016
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/27/2017
ms.topic: article
ms.custom: it-pro
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 6a4b6cfe98064181824e210be9031a0f67cb4b75
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824630"
---
# <a name="active-directory-federation-services-promptlogin-parameter-support"></a>Active Directory Federation Services invite = prise en charge des paramètres de connexion
Le document suivant explique la prise en charge native pour le paramètre de connexion = prompt est disponible dans AD FS.

## <a name="what-is-promptlogin"></a>What ' s prompt = login ?  

Certaines applications Office 365 (avec authentification moderne activée) envoient le paramètre de connexion = prompt à Azure AD dans le cadre de chaque demande d’authentification.  Par défaut, Azure AD traduit cette situation en deux paramètres : <code> <b> wauth </b> =urn:oasis:names:tc:SAML:1.0:am:password </code>, et <code> <b> wfresh </b> =0 </code> .

Cela peut entraîner des problèmes avec l’intranet d’entreprise et les scénarios de l’authentification multifacteur dans lequel un type d’authentification autre que le nom d’utilisateur et mot de passe est souhaité.  

AD FS dans Windows Server 2012 R2 avec le correctif cumulatif de juillet 2016 a introduit la prise en charge native pour le paramètre de connexion = prompt.  Cela signifie que vous avez maintenant l’option de configuration d’Azure AD pour envoyer ce paramètre en tant que-consiste à votre service AD FS en tant que partie d’Azure AD et les demandes d’authentification Office 365.

### <a name="ad-fs-versions-that-support-promptlogin"></a>Les versions AD FS qui prennent en charge invite = connexion
Voici une liste des versions d’AD FS qui prennent en charge le paramètre de connexion = prompt.

- AD FS dans Windows Server 2012 R2 avec le juillet 2016 correctif cumulatif

- AD FS dans Windows Server 2016

## <a name="how-do-to-configure-your-azure-ad-tenant-to-send-promptlogin-to-ad-fs"></a>Comment faire pour configurer votre client Azure AD pour envoyer prompt = connexion à AD FS

Utiliser le module Azure AD PowerShell pour configurer le paramètre.

> [!NOTE]
> La fonctionnalité de connexion = prompt (activée par la propriété PromptLoginBehavior) est actuellement disponible uniquement dans le [' un module Azure AD Powershell version 1.0'](https://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185), dans lequel les applets de commande ont des noms qui incluent « Msol », comme Set-MsolDomainFederationSettings.  Il n’est pas actuellement disponible via ' version 2.0 » module Azure AD PowerShell, dont applets de commande ont des noms tels que « Set-AzureAD\*».

Pour configurer l’invite = le comportement de connexion, la syntaxe de l’applet de commande ci-dessous :

Exemple 1 :
```powershell
    Set-MsolDomainFederationSettings –DomainName <your domain name> -PreferredAuthenticationProtocol <your current protocol setting> 
```

Exemple 2 :
```powershell
    Set-MsolDomainFederationSettings –DomainName <your domain name> -SupportsMfa <$True|$False>
```

Exemple 3 :
```powershell
    Set-MsolDomainFederationSettings –DomainName <your domain name> -PromptLoginBehavior <TranslateToFreshPasswordAuth|NativeSupport|Disabled>
```

 
 Vous trouverez les valeurs de propriété PreferredAuthenticationProtocol, SupportsMfa et PromptLoginBehavior en affichant la sortie de l’applet de commande : ![Get-MsolDomainFederationSettings](media/AD-FS-Prompt-Login/GetMsol.png)
```powershell
    Get-MsolDomainFederationSettings -DomainName <your_domain_name> | fl *
 ```
> [!NOTE]
> Par défaut, lors de l’exécution de Get-MsolDomainFederationSettings, certaines propriétés ne sont pas affichées dans la console.  Pour afficher ces paramètres, il est recommandé d’utiliser le | fl * pour forcer la sortie de toutes les propriétés de l’objet.


Voici plus d’informations sur le paramètre PromptLoginBehavior et ses paramètres.
   
   - <b>TranslateToFreshPasswordAuth</b> signifie que le comportement d’Azure AD par défaut de l’envoi <b>wauth</b> et <b>wfresh</b> à AD FS au lieu de l’invite de commandes = connexion
   - <b>NativeSupport</b> signifie que le paramètre de connexion = invite sera envoyé comme consiste à AD FS
   - <b>Désactivé</b> signifie que rien ne sera envoyé aux services AD FS

