---
title: Autres méthodes d’authentification dans AD FS 2019
description: Ce document décrit les méthodes d’authentification dans AD FS 2019.
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/19/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b0d5754a4622df9ca26a80bd4e32c355dda0f684
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190065"
---
# <a name="configure-3rd-party-authentication-providers-as-primary-authentication-in-ad-fs-2019"></a>Configurer les fournisseurs d’authentification tiers 3e comme authentification principale dans AD FS 2019


Les organisations rencontrent les attaques visant à force brute, compromettre ou sinon verrouiller les comptes d’utilisateur en envoyant des demandes d’authentification de mot de passe.  Pour aider à protéger les entreprises contre la compromission, AD FS a introduit des fonctionnalités telles que le verrouillage extranet « intelligent » et le blocage de basées sur des adresses IP.  

Toutefois, ces atténuations sont réactives.  Pour fournir de manière proactive, afin de réduire la gravité de ces attaques, AD FS a la possibilité de demander à des facteurs externes sans mot de passe avant de collecter le mot de passe.  

Par exemple, AD FS 2016 a introduit Azure MFA comme authentification principale afin que les codes de secret à usage unique à partir de l’application Authenticator peut être utilisées comme le premier facteur.
Génération à ce sujet, avec AD FS 2019 vous pouvez configurer des fournisseurs d’authentification externes comme des facteurs d’authentification principale.

Il existe deux principaux scénarios ainsi :

## <a name="scenario-1-protect-the-password"></a>Scénario 1 : protéger le mot de passe
Protéger par mot de passe de connexion à partir des attaques par force brute et les verrouillages en demandant un facteur supplémentaire, externe tout d’abord.  Uniquement si l’authentification externe est terminée avec succès l’utilisateur puis voit une invite de mot de passe.  Cela élimine un moyen pratique des attaquants essaient de compromettre ou désactiver des comptes.

Ce scénario se compose de deux composants :
- Demande d’un facteur d’authentification externe ou Azure MFA comme authentification principale
- Nom d’utilisateur et mot de passe comme authentification supplémentaire dans AD FS

## <a name="scenario-2-password-free"></a>Scénario 2 : sans mot de passe !
Éliminer entièrement les mots de passe, mais un nom fort à la fin, l’authentification multifacteur à l’aide du mot de passe entièrement non basé méthodes dans AD FS
- Azure MFA avec application d’authentification
- Windows 10 Hello entreprise
- Authentification par certificat
- Fournisseurs d’authentification externes

## <a name="concepts"></a>Concepts
Ce que **l’authentification principale** vraiment signifie est qu’il est la méthode que l’utilisateur est invité pour la première, avant d’autres facteurs.  Précédemment les méthodes principales uniquement disponibles dans AD FS ont été générés dans les méthodes pour Active Directory ou Azure MFA ou une autre authentification LDAP magasins.  Méthodes externes peut être configurés en tant que l’authentification « supplémentaire », ce qui a lieu une fois l’authentification principale est terminée.

Dans AD FS 2019, l’authentification externe en tant que fonctionnalité principale signifie que les fournisseurs d’authentification externes inscrits sur la batterie de serveurs AD FS (à l’aide de Register-AdfsAuthenticationProvider) deviennent disponibles pour l’authentification principale ainsi que « supplémentaire » authentification. Ils peuvent être activées à la même façon que les fournisseurs intégrés tels que l’authentification par formulaire et l’authentification par certificat, pour intranet et/ou extranet utilisation.

![authentification](media/Additional-Authentication-Methods-AD-FS/auth1.png)

Une fois qu’un fournisseur externe est activé pour l’intranet, extranet, ou les deux, il devient disponible pour les utilisateurs à utiliser.  Si plus d’une méthode est activée, les utilisateurs voir une page de choix et être en mesure de choisir une méthode principale, exactement comme pour une authentification supplémentaire.

## <a name="pre-requisites"></a>Conditions préalables
Avant de configurer les fournisseurs d’authentification externes en tant que principal, assurez-vous de qu'avoir les conditions préalables en place
- Le niveau de comportement de batterie de serveurs AD FS (FBL) est passée à « 4 » (cette valeur se traduit par AD FS 2019)
    - C’est la valeur FBL par défaut pour nouveau batteries de serveurs AD FS 2019
    - Pour les batteries de serveurs AD FS basés sur Windows Server 2012 R2 ou 2016, le FBL peut être déclenché à l’aide de l’applet de commande Invoke-AdfsFarmBehaviorLevelRaise.  Pour plus d’informations sur la mise à niveau une batterie de serveurs AD FS, consultez la batterie de serveurs mise à niveau de l’article pour les batteries de serveurs SQL ou des batteries de serveurs WID 
    - Vous pouvez vérifier la valeur FBL à l’aide de l’applet de commande Get-AdfsFarmInformation
- La batterie de serveurs AD FS 2019 est configuré pour utiliser le nouvel utilisateur « paginés » 2019 accessible sur les pages
    - Il s’agit du comportement par défaut pour nouveau batteries de serveurs AD FS 2019
    - Pour les batteries de serveurs AD FS mis à niveau à partir de Windows Server 2012 R2 ou 2016, les flux paginés sont activées automatiquement lors de l’authentification externe comme principale (la fonctionnalité décrite dans ce document) est activée, comme décrit ci-dessous.

## <a name="enable-external-authentication-methods-as-primary"></a>Activer des méthodes d’authentification externe en tant que principal
Une fois que vous avez vérifié la configuration requise, il existe deux façons de configurer les fournisseurs d’authentification supplémentaire AD FS en tant que principal :

### <a name="using-powershell"></a>À l'aide de PowerShell


```powershell
PS C:\> Set-AdfsGlobalAuthenticationPolicy -AllowAdditionalAuthenticationAsPrimary $true
``` 


Le service AD FS doit être redémarré après activation ou désactivation d’une authentification supplémentaire en tant que principal.

### <a name="using-the-ad-fs-management-console"></a>À l’aide de la console de gestion AD FS
Dans la console de gestion AD FS, sous **Service** -> **méthodes d’authentification**, sous **des méthodes d’authentification principal**, cliquez sur Modifier

Cliquez sur la case à cocher pour **autoriser des fournisseurs d’authentification supplémentaires en tant que principal**.

Le service AD FS doit être redémarré après activation ou désactivation d’une authentification supplémentaire en tant que principal.

## <a name="enable-username-and-password-as-additional-authentication"></a>Activer le nom d’utilisateur et mot de passe comme authentification supplémentaire
Pour réaliser le scénario « protéger le mot de passe », activer le nom d’utilisateur et mot de passe comme authentification supplémentaire à l’aide de PowerShell ou la console de gestion AD FS
### <a name="using-powershell"></a>À l'aide de PowerShell



```powershell
PS C:\> $providers = (Get-AdfsGlobalAuthenticationPolicy).AdditionalAuthenticationProvider

PS C:\>$providers = $providers + "FormsAuthentication"

PS C:\>Set-AdfsGlobalAuthenticationPolicy -AdditionalAuthenticationProvider $providers
``` 

### <a name="using-the-ad-fs-management-console"></a>À l’aide de la console de gestion AD FS
Dans la console de gestion AD FS, sous **Service** -> **méthodes d’authentification**, sous **méthodes d’authentification supplémentaires**, cliquez sur  **Modifier**

Cliquez sur la case à cocher pour **l’authentification par formulaire** pour activer le nom d’utilisateur et mot de passe comme authentification supplémentaire.
