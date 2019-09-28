---
title: Méthodes d’authentification supplémentaires dans AD FS 2019
description: Ce document décrit les nouvelles méthodes d’authentification dans AD FS 2019.
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/19/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 8a53a4cfca4f34459102b8edc8e6af82f36be70d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358371"
---
# <a name="configure-3rd-party-authentication-providers-as-primary-authentication-in-ad-fs-2019"></a>Configurer des fournisseurs d’authentification tiers comme authentification principale dans AD FS 2019


Les organisations rencontrent des attaques visant à forcer, à compromettre ou à verrouiller les comptes d’utilisateurs en envoyant des demandes d’authentification par mot de passe.  Pour aider à protéger les organisations contre toute compromission, AD FS a introduit des fonctionnalités telles que le verrouillage « intelligent » extranet et le blocage basé sur les adresses IP.  

Toutefois, ces atténuations sont réactives.  Pour garantir une façon proactive de réduire la gravité de ces attaques, AD FS a la possibilité de demander des facteurs de non-mot de passe avant de collecter le mot de passe.  

Par exemple, AD FS 2016 a introduit Azure MFA comme authentification principale afin que les codes OTP de l’application d’authentificateur puissent être utilisés comme premier facteur.
À partir de là, avec AD FS 2019, vous pouvez configurer des fournisseurs d’authentification externes comme facteurs d’authentification principaux.

Il existe deux principaux scénarios qui permettent :

## <a name="scenario-1-protect-the-password"></a>Scénario 1 : protéger le mot de passe
Protégez la connexion basée sur un mot de passe contre les attaques et les verrouillages en force brute en demandant un facteur externe supplémentaire.  L’utilisateur voit une invite de mot de passe uniquement si l’authentification externe est terminée avec succès.  Cela permet d’éviter que des attaquants essaient de compromettre ou de désactiver des comptes.

Ce scénario se compose de deux composants :
- Demande d’authentification MFA Azure ou d’un facteur d’authentification externe comme authentification principale
- Nom d’utilisateur et mot de passe en tant qu’authentification supplémentaire dans AD FS

## <a name="scenario-2-password-free"></a>Scénario 2 : sans mot de passe !
Éliminez entièrement les mots de passe, mais effectuez une authentification multifacteur forte à l’aide de méthodes entièrement non basées sur un mot de passe dans AD FS
- Azure MFA avec application Authenticator
- Windows 10 Hello entreprise
- Authentification par certificat
- Fournisseurs d’authentification externes

## <a name="concepts"></a>Concepts
L' **authentification principale** signifie en fait qu’il s’agit de la méthode à laquelle l’utilisateur est invité en premier, avant les facteurs supplémentaires.  Auparavant, les seules méthodes principales disponibles dans AD FS étaient créées dans des méthodes pour Active Directory ou Azure MFA, ou d’autres magasins d’authentification LDAP.  Les méthodes externes peuvent être configurées comme une authentification « supplémentaire », qui a lieu après l’exécution de l’authentification principale.

Dans AD FS 2019, l’authentification externe comme capacité principale signifie que tous les fournisseurs d’authentification externes inscrits sur la batterie de serveurs AD FS (à l’aide de Register-Adfsauthenticationprovider,) deviennent disponibles pour l’authentification principale et « supplémentaires » identification. Ils peuvent être activés de la même façon que les fournisseurs intégrés, tels que l’authentification par formulaire et l’authentification par certificat, pour une utilisation intranet et/ou extranet.

![authentification](media/Additional-Authentication-Methods-AD-FS/auth1.png)

Une fois qu’un fournisseur externe est activé pour l’extranet, l’intranet, ou les deux, il est disponible pour les utilisateurs.  Si plusieurs méthodes sont activées, les utilisateurs verront une page de choix et pourront choisir une méthode principale, comme c’est le cas pour une authentification supplémentaire.

## <a name="pre-requisites"></a>Prérequis
Avant de configurer les fournisseurs d’authentification externes comme principaux, assurez-vous que les conditions préalables suivantes sont remplies.
- Le niveau de comportement de la batterie de serveurs AD FS (FBL) a été élevé à « 4 » (cette valeur se traduit par AD FS 2019)
    - Il s’agit de la valeur FBL par défaut pour les nouvelles batteries de serveurs AD FS 2019
    - Pour les batteries de AD FS basées sur Windows Server 2012 R2 ou 2016, le FBL peut être déclenché à l’aide de l’applet de PowerShell Invoke-AdfsFarmBehaviorLevelRaise.  Pour plus d’informations sur la mise à niveau d’une batterie de serveurs AD FS, consultez l’article sur la mise à niveau de batterie pour les batteries de serveurs SQL ou WID. 
    - Vous pouvez vérifier la valeur FBL à l’aide de l’applet de commande-AdfsFarmInformation
- La batterie de serveurs AD FS 2019 est configurée pour utiliser les nouvelles pages de l’utilisateur « paginé » de 2019
    - Il s’agit du comportement par défaut des nouvelles batteries de serveurs AD FS 2019
    - Pour les batteries de AD FS mises à niveau à partir de Windows Server 2012 R2 ou 2016, les flux paginés sont activés automatiquement lorsque l’authentification externe comme principale (la fonctionnalité décrite dans ce document) est activée comme décrit ci-dessous.

## <a name="enable-external-authentication-methods-as-primary"></a>Activer les méthodes d’authentification externes comme principale
Une fois que vous avez vérifié les conditions préalables, il existe deux façons de configurer AD FS fournisseurs d’authentification supplémentaires comme principaux :

### <a name="using-powershell"></a>À l'aide de PowerShell


```powershell
PS C:\> Set-AdfsGlobalAuthenticationPolicy -AllowAdditionalAuthenticationAsPrimary $true
``` 


Le service de AD FS doit être redémarré après l’activation ou la désactivation de l’authentification supplémentaire comme principale.

### <a name="using-the-ad-fs-management-console"></a>Utilisation de la console de gestion AD FS
Dans la console de gestion AD FS, sous méthodes**d’authentification**du **service** -> , sous **méthodes d’authentification principales**, cliquez sur modifier.

Cochez la case **autoriser les fournisseurs d’authentification supplémentaires comme principaux**.

Le service de AD FS doit être redémarré après l’activation ou la désactivation de l’authentification supplémentaire comme principale.

## <a name="enable-username-and-password-as-additional-authentication"></a>Activer le nom d’utilisateur et le mot de passe en tant qu’authentification supplémentaire
Pour terminer le scénario « protéger le mot de passe », activez le nom d’utilisateur et le mot de passe en tant qu’authentification supplémentaire à l’aide de PowerShell ou de la console de gestion AD FS
### <a name="using-powershell"></a>À l'aide de PowerShell



```powershell
PS C:\> $providers = (Get-AdfsGlobalAuthenticationPolicy).AdditionalAuthenticationProvider

PS C:\>$providers = $providers + "FormsAuthentication"

PS C:\>Set-AdfsGlobalAuthenticationPolicy -AdditionalAuthenticationProvider $providers
``` 

### <a name="using-the-ad-fs-management-console"></a>Utilisation de la console de gestion AD FS
Dans la console de gestion AD FS, sous méthodes**d'** authentification du **service** -> , sous **méthodes d’authentification supplémentaires**, cliquez sur **modifier** .

Cochez la case **authentification par formulaire** pour activer le nom d’utilisateur et le mot de passe en tant qu’authentification supplémentaire.
