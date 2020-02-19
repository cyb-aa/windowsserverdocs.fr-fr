---
title: AD FS prompt = connexion
description: Forum aux questions (FAQ) pour AD FS 2016
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/27/2017
ms.topic: article
ms.custom: it-pro
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: a80678f5d2773e3fcd7a95032853249dc36d5616
ms.sourcegitcommit: 2a15de216edde8b8e240a4aa679dc6d470e4159e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77465523"
---
# <a name="active-directory-federation-services-promptlogin-parameter-support"></a>Services ADFS prompt = prise en charge des paramètres de connexion

Le document suivant décrit la prise en charge native du paramètre prompt = login disponible dans AD FS.

## <a name="what-is-promptlogin"></a>Qu’est-ce que prompt = login ?

Lorsque les applications doivent demander une nouvelle authentification à partir de Azure AD, ce qui signifie qu’elles ont besoin Azure AD de réauthentifier l’utilisateur, même si l’utilisateur a déjà été authentifié, il peut envoyer le paramètre `prompt=login` à Azure AD dans le cadre de la demande d’authentification.

Lorsque cette demande est destinée à un utilisateur fédéré, Azure AD doit informer le IdP, comme AD FS, que la demande est destinée à une nouvelle authentification.

Par défaut, Azure AD convertit `prompt=login` en `wfresh=0` et `wauth=https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password` lors de l’envoi de ce type de demandes d’authentification à l’IdP fédéré.

Ces paramètres signifient :

- `wfresh=0`: effectuer une nouvelle authentification
- `wauth=https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password`: utiliser le nom d’utilisateur/mot de passe pour la nouvelle demande d’authentification

Cela peut entraîner des problèmes avec les scénarios intranet d’entreprise et multi-Factor Authentication dans lesquels un type d’authentification autre que le nom d’utilisateur et le mot de passe, comme demandé par le paramètre `wauth`, est souhaité.  

AD FS dans Windows Server 2012 R2 avec le correctif cumulatif de juillet 2016 a introduit la prise en charge native du paramètre `prompt=login`. Cela signifie que Azure AD pouvez désormais envoyer ce paramètre tel quel pour AD FS service dans le cadre de la Azure AD et des demandes d’authentification Office 365.

## <a name="ad-fs-versions-that-support-promptlogin"></a>Versions de AD FS qui prennent en charge prompt = login

La liste suivante répertorie les versions de AD FS qui prennent en charge le paramètre `prompt=login`.

- AD FS dans Windows Server 2012 R2 avec le correctif cumulatif de juillet 2016
- AD FS dans Windows Server 2016

## <a name="how-to-configure-a-federated-domain-to-send-promptlogin-to-ad-fs"></a>Comment configurer un domaine fédéré pour envoyer prompt = Login à AD FS

Utilisez le module Azure AD PowerShell pour configurer le paramètre.

> [!NOTE]
> La fonctionnalité `prompt=login` (activée par la propriété `PromptLoginBehavior`) est actuellement disponible uniquement dans la [version 1,0 du module Powershell Azure ad](https://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185), dans laquelle les applets de commande portent des noms qui incluent « MSOL », tels que Set-MsolDomainFederationSettings.  Elle n’est pas disponible actuellement par le biais de la version 2,0 du module PowerShell Azure AD, dont les applets de commande portent des noms tels que « Set-AzureAD\*».

1. Commencez par obtenir les valeurs actuelles de `PreferredAuthenticationProtocol`, `SupportsMfa`et `PromptLoginBehavior` pour le domaine fédéré en exécutant la commande PowerShell suivante :

```powershell
    Get-MsolDomainFederationSettings -DomainName <your_domain_name> | Format-List *
```

> [!NOTE]
> La sortie de `Get-MsolDomainFederationSettings` par défaut n’affiche pas certaines propriétés dans la console. Pour afficher toutes les propriétés, vous devez diriger (`|`) sa sortie vers `Format-List *` pour forcer la sortie de toutes les propriétés de l’objet.

![Get-MsolDomainFederationSettings](media/AD-FS-Prompt-Login/GetMsol.png)

> [!NOTE]
> Si la valeur de la propriété `PromptLoginBehavior` est vide (`$null`), le comportement de `TranslateToFreshPasswordAuth` est utilisé.

2. Configurez la valeur de `PromptLoginBehavior` souhaitée en exécutant la commande suivante :

```powershell
    Set-MsolDomainFederationSettings –DomainName <your_domain_name> -PreferredAuthenticationProtocol <current_value_from_step1> -SupportsMfa <current_value_from_step1> -PromptLoginBehavior <TranslateToFreshPasswordAuth|NativeSupport|Disabled>
```

Voici les valeurs possibles du paramètre `PromptLoginBehavior` et leur signification :

- **TranslateToFreshPasswordAuth**: indique le comportement Azure ad par défaut de la traduction des `prompt=login` en `wauth=https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password` et `wfresh=0`.
- **NativeSupport**: signifie que le paramètre `prompt=login` sera envoyé en l’AD FS. Il s’agit de la valeur recommandée si AD FS se trouve dans Windows Server 2012 R2 avec le correctif cumulatif de juillet 2016 ou une version ultérieure.
- **Disabled**: signifie que seul `wfresh=0` est envoyé à AD FS.
