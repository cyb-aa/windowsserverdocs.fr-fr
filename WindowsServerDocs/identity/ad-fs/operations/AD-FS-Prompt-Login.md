---
title: AD FS prompt = connexion
description: Forum aux questions (FAQ) pour AD FS 2016
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/27/2017
ms.topic: article
ms.custom: it-pro
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f4f284d4d970af8f8a672bd88be53f65ba70893f
ms.sourcegitcommit: 6f968368c12b9dd699c197afb3a3d13c2211f85b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68544634"
---
# <a name="active-directory-federation-services-promptlogin-parameter-support"></a>Services ADFS prompt = prise en charge des paramètres de connexion

Le document suivant décrit la prise en charge native du paramètre prompt = login disponible dans AD FS.

## <a name="what-is-promptlogin"></a>Qu’est-ce que prompt = login?

Lorsque les applications doivent demander une nouvelle authentification à partir de Azure ad, ce qui signifie qu’elles ont besoin Azure AD de réauthentifier l’utilisateur, même si l’utilisateur a déjà été `prompt=login` authentifié, il peut envoyer le paramètre à Azure ad dans le cadre de l’authentification. demande.

Lorsque cette demande est destinée à un utilisateur fédéré, Azure AD doit informer le IdP, comme AD FS, que la demande est destinée à une nouvelle authentification.

Par défaut, Azure ad `prompt=login` se `wfresh=0` traduit par et `wauth=http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password` lors de l’envoi de ce type de demandes d’authentification à l’IDP fédéré.

Ces paramètres signifient:

- `wfresh=0`: effectuer une nouvelle authentification
- `wauth=http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password`: utiliser le nom d’utilisateur/mot de passe pour la nouvelle demande d’authentification

Cela peut entraîner des problèmes avec les scénarios intranet d’entreprise et multi-Factor Authentication dans lesquels un type d’authentification autre que le nom d' `wauth` utilisateur et le mot de passe, comme demandé par le paramètre, est souhaité.  

AD FS dans Windows Server 2012 R2 avec le correctif cumulatif de juillet 2016 a introduit la prise `prompt=login` en charge native du paramètre. Cela signifie que Azure AD pouvez désormais envoyer ce paramètre tel quel pour AD FS service dans le cadre de la Azure AD et des demandes d’authentification Office 365.

## <a name="ad-fs-versions-that-support-promptlogin"></a>Versions de AD FS qui prennent en charge prompt = login

La liste suivante répertorie les versions de AD FS qui prennent `prompt=login` en charge le paramètre.

- AD FS dans Windows Server 2012 R2 avec le correctif cumulatif de juillet 2016
- AD FS dans Windows Server 2016

## <a name="how-to-configure-a-federated-domain-to-send-promptlogin-to-ad-fs"></a>Comment configurer un domaine fédéré pour envoyer prompt = Login à AD FS

Utilisez le module Azure AD PowerShell pour configurer le paramètre.

> [!NOTE]
> La `prompt=login` fonctionnalité (activée par la `PromptLoginBehavior` propriété) est actuellement disponible uniquement dans la [version 1,0 du module PowerShell Azure ad](https://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185), dans lequel les applets de commande portent des noms qui incluent «MSOL», tels que Set-MsolDomainFederationSettings.  Elle n’est pas disponible actuellement par le biais de la version 2,0 du module PowerShell Azure AD, dont les applets de commande ont des\*noms tels que «Set-AzureAD».

1. Commencez par obtenir les valeurs actuelles `PreferredAuthenticationProtocol`de `SupportsMfa`, et `PromptLoginBehavior` pour le domaine fédéré en exécutant la commande PowerShell suivante:

```powershell
    Get-MsolDomainFederationSettings -DomainName <your_domain_name> | Format-List *
```

> [!NOTE]
> La sortie de `Get-MsolDomainFederationSettings` par défaut n’affiche pas certaines propriétés dans la console. Pour afficher toutes les propriétés, vous devez diriger`|`() sa sortie `Format-List *` vers pour forcer la sortie de toutes les propriétés de l’objet.

![Get-MsolDomainFederationSettings](media/AD-FS-Prompt-Login/GetMsol.png)

> [!NOTE]
> Si la `PreferredAuthenticationMethod` propriété est vide (`$null`), cela signifie le comportement par `TranslateToFreshPasswordAuth`défaut de.

2. Configurez la valeur `PromptLoginBehavior` souhaitée de en exécutant la commande suivante:

```powershell
    Set-MsolDomainFederationSettings –DomainName <your_domain_name> -PreferredAuthenticationProtocol <current_value_from_step1> -SupportsMfa <current_value_from_step1> -PromptLoginBehavior <TranslateToFreshPasswordAuth|NativeSupport|Disabled>
```

Voici les valeurs possibles des `PromptLoginBehavior` paramètres et leur signification:

- **TranslateToFreshPasswordAuth**: indique le comportement par défaut Azure AD de la `prompt=login` traduction `wauth=http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password` vers `wfresh=0`et.
- **NativeSupport**: signifie que le `prompt=login` paramètre sera envoyé en l’AD FS. Il s’agit de la valeur recommandée si AD FS se trouve dans Windows Server 2012 R2 avec le correctif cumulatif de juillet 2016 ou une version ultérieure.
- Disabled: signifie `wfresh=0` que seul est envoyé à AD FS.
