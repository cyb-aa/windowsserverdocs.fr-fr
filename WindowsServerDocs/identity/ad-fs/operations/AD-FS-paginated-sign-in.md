---
title: AD FS paginé connectez-vous
description: Ce document décrit la nouvelle expérience de connexion pour AD FS 2019.
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/19/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2b11454427a65e37604b430a63b5ed745f4a2bb1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59864450"
---
# <a name="ad-fs-paginated-sign-in"></a>AD FS paginé connectez-vous

>S'applique à : Windows Server 2019

Pour AD FS 2019, nous avons remanié l’interface utilisateur de connexion.  À présent, l’authentification dans AD FS aura le même aspect d’Azure AD.  Cela fournit aux utilisateurs une expérience plus cohérente connectez-vous, incorporant un flux utilisateur centrée et paginés. 

## <a name="whats-changing"></a>Quels sont les changements
Dans AD FS 2012 R2 et 2016, votre écran de connexion ressemblait quelque chose comme ceci :

![oldsignin](media/AD-FS-paginated-sign-in/signin1.png)

Nous progressions en dehors de l’affichage d’un formulaire unique situé sur le côté droit de l’écran.

Dans AD FS 2019, voici les modifications de conception importantes que vous verrez :


- **Un centré à l’interface utilisateur**. L’interface utilisateur de connexion existait déjà, sur le côté droit de l’écran, comme indiqué ci-dessus. Nous avons déplacé le début de l’interface utilisateur et le Centre pour moderniser l’expérience.
- **La pagination**. Au lieu de vous fournir un formulaire long à remplir, nous avons incorporé un flux qui vous guident à travers l’expérience de connexion détaillée. Nos données de télémétrie montrent qu’avec cette approche, nos clients ont plus connexion réussie. Il nous fournit également plus de souplesse pour incorporer des différentes méthodes d’authentification, tel que l’authentification par facteur de téléphone. 

![newsignin](media/AD-FS-paginated-sign-in/signin2.png)

Sur la première page, vous devez entrer votre nom d’utilisateur. Vous pouvez également sélectionner l’option pour « Maintenir la connexion » pour réduire la fréquence des invites de connexion et de rester connecté lorsqu’il est possible de le faire. (Cette option est désactivée par défaut).

![newsignin](media/AD-FS-paginated-sign-in/signin3.png)

Dans la deuxième page, s’affiche avec les options d’authentification, configurées par votre administrateur. Si ce qui permet une authentification externe en tant que principal est activé, ce sera inclus également.

![newsignin](media/AD-FS-paginated-sign-in/signin4.png)

Dans la troisième page, vous devrez entrer votre mot de passe (en supposant que vous avez sélectionné « Password » comme option de l’authentification). 

## <a name="how-to-get-the-new-experience"></a>Comment obtenir la nouvelle expérience
Si vous êtes un nouveau client à AD FS, vous recevrez la nouvelle conception par défaut. Toutefois, si vous êtes un client existant avec AD FS 2012 R2 ou 2016, il existe plusieurs étapes, que vous devrez prendre pour recevoir la nouvelle conception : 

1. Mettre à niveau vos serveurs AD FS 2019. 
2.  Activer votre FBL à 2019.
3.  Activer la nouvelle expérience de connexion.
- Autoriser la nouvelle connexion à l’aide de PowerShell. Dans PowerShell, exécutez la commande suivante pour activer la pagination : ``Set-AdfsGlobalAuthenticationPolicy -EnablePaginatedAuthenticationPages $true``
- Autoriser l’authentification externe en tant que principal, via PowerShell ou via le Gestionnaire de serveur AD FS. Dans PowerShell, exécutez la commande suivante pour autoriser l’authentification externe en tant que principal : ``Set-AdfsGlobalAuthenticationPolicy -AllowAdditionalAuthenticationAsPrimary $true``

## <a name="customization"></a>Personnalisation
Les options de personnalisation sera toujours applicables pour AD FS 2019. Voici quelques liens vers d’autres documents de référence. 

• Pour ceux qui ne souhaitez pas mettre à niveau leurs serveurs à AD FS 2019 mais souhaitez toujours que la nouvelle conception : [À l’aide d’un thème Web de l’expérience utilisateur de Azure AD dans Active Directory Federation Services](azure-ux-web-theme-in-ad-fs.md)

• Un emplacement central pour la personnalisation : [Personnalisation utilisateur AD FS sign-in](ad-fs-user-sign-in-customization.md)
