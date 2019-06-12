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
ms.openlocfilehash: c528b9c4e944849b7ed9a2fc5213a7b263be70c7
ms.sourcegitcommit: ccc802338b163abdad2e53b55f39addcfea04603
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2019
ms.locfileid: "66687380"
---
# <a name="ad-fs-paginated-sign-in"></a>AD FS paginé connectez-vous


Pour AD FS dans Windows Server 2019, nous avons remanié l’interface utilisateur de connexion.  À présent, l’authentification dans AD FS aura le même aspect d’Azure AD.  Cela fournit aux utilisateurs une expérience plus cohérente connectez-vous, incorporant un flux utilisateur centrée et paginés.

## <a name="whats-changing"></a>Quels sont les changements
AD FS dans Windows Server 2012 R2 et 2016, votre écran de connexion effectue la recherche de quelque chose comme ceci :

![oldsignin](media/AD-FS-paginated-sign-in/signin1.png)

Nous progressions en dehors de l’affichage d’un formulaire unique situé sur le côté droit de l’écran.

Dans AD FS dans Windows Server 2019, voici les modifications de conception importantes que vous verrez :


- **Un centré à l’interface utilisateur**. L’interface utilisateur de connexion existait déjà, sur le côté droit de l’écran, comme indiqué ci-dessus. Nous avons déplacé le début de l’interface utilisateur et le Centre pour moderniser l’expérience.
- **La pagination**. Au lieu de vous fournir un formulaire long à remplir, nous avons incorporé un flux qui vous guident à travers l’expérience de connexion détaillée. Nos données de télémétrie montrent qu’avec cette approche, nos clients ont plus connexion réussie. Il nous fournit également plus de souplesse pour incorporer des différentes méthodes d’authentification, tel que l’authentification par facteur de téléphone.

![newsignin](media/AD-FS-paginated-sign-in/signin2.png)

Sur la première page, vous devez entrer votre nom d’utilisateur. Vous pouvez également sélectionner l’option pour « Maintenir la connexion » pour réduire la fréquence des invites de connexion et de rester connecté lorsqu’il est possible de le faire. (Cette option est désactivée par défaut).

![newsignin](media/AD-FS-paginated-sign-in/signin3.png)

Dans la deuxième page, s’affiche avec les options d’authentification, configurées par votre administrateur. Si ce qui permet une authentification externe en tant que principal est activé, ce sera inclus également.

![newsignin](media/AD-FS-paginated-sign-in/signin4.png)

Dans la troisième page, vous devrez entrer votre mot de passe (en supposant que vous avez sélectionné « Password » comme option de l’authentification).

## <a name="how-to-get-the-new-experience"></a>Comment obtenir la nouvelle expérience

### <a name="new-installation-of-ad-fs"></a>Nouvelle installation d’AD FS
Si vous êtes un nouveau client à AD FS, vous recevrez la nouvelle conception par défaut.

### <a name="upgrading-a-farm"></a>La mise à niveau une batterie de serveurs
Si vous êtes un client existant AD FS 2012 R2 ou 2016, il existe deux manières de recevoir la nouvelle conception après la mise à niveau des serveurs à AD FS 2019 et l’activation de la FBL à 2019.

- Autoriser la nouvelle connexion à l’aide de Powershell. Exécutez la commande suivante pour activer la pagination : ``Set-AdfsGlobalAuthenticationPolicy -EnablePaginatedAuthenticationPages $true``

 - Activer l’authentification externe en tant que principal, via Powershell ou via le Gestionnaire de serveur AD FS. La nouvelle connexion paginée pages obtiendront lorsque cette fonctionnalité est activée.
Si vous êtes un nouveau client à AD FS, vous recevrez la nouvelle conception par défaut. Toutefois, si vous êtes un client existant avec AD FS 2012 R2 ou 2016, il existe plusieurs étapes, que vous devrez prendre pour recevoir la nouvelle conception : ``Set-AdfsGlobalAuthenticationPolicy -AllowAdditionalAuthenticationAsPrimary $true``

## <a name="customization"></a>Personnalisation
Les options de personnalisation sera toujours applicables pour AD FS 2019.
Voici quelques liens vers d’autres documents de référence.

• Pour ceux qui ne souhaitez pas mettre à niveau leurs serveurs à AD FS 2019 mais souhaitez toujours que la nouvelle conception : [À l’aide d’un thème Web de l’expérience utilisateur de Azure AD dans Active Directory Federation Services](azure-ux-web-theme-in-ad-fs.md)

• Un emplacement central pour la personnalisation : [Personnalisation de la connexion utilisateur AD FS](ad-fs-user-sign-in-customization.md)
