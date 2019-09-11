---
title: Connexion AD FS paginée
description: Ce document décrit la nouvelle expérience de connexion pour AD FS 2019.
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/19/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 41938aef1c22f78a49e2817d0764b8110ef30f54
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70866144"
---
# <a name="ad-fs-paginated-sign-in"></a>Connexion AD FS paginée


Pour AD FS dans Windows Server 2019, nous avons remanié l’interface utilisateur de connexion.  À présent, la connexion AD FS aura le même aspect que Azure AD.  Cela offre aux utilisateurs une expérience de connexion plus cohérente, en incorporant un workflow utilisateur centré et paginé.

## <a name="whats-changing"></a>Nouveautés
Dans AD FS dans Windows Server 2012 R2 et 2016, votre écran de connexion ressemble à ceci :

![oldsignin](media/AD-FS-paginated-sign-in/signin1.png)

Nous ne déplaçons pas l’affichage d’un formulaire unique situé sur le côté droit de l’écran.

Dans AD FS dans Windows Server 2019, il s’agit des principales modifications de conception que vous verrez :


- **Interface utilisateur centrée**. Auparavant, l’interface utilisateur de connexion existait sur le côté droit de l’écran, comme indiqué ci-dessus. Nous avons déplacé le front et le centre de l’interface utilisateur pour moderniser l’expérience.
- **Pagination**. Au lieu de vous fournir un formulaire long à remplir, nous avons incorporé un nouveau Flow qui vous guidera tout au long de la procédure de connexion. Nos données de télémétrie montrent qu’avec cette approche, nos clients ont des connexions plus réussies. Il offre également une plus grande flexibilité pour incorporer différentes méthodes d’authentification, telles que l’authentification par facteur de téléphone.

![newsignin](media/AD-FS-paginated-sign-in/signin2.png)

Sur la première page, vous êtes invité à entrer votre nom d’utilisateur. Vous pouvez également sélectionner l’option « maintenir la connexion » pour réduire la fréquence des invites de connexion et rester connecté lorsqu’il est possible de le faire. (Cette option est désactivée par défaut.)

![newsignin](media/AD-FS-paginated-sign-in/signin3.png)

Dans la deuxième page, les options d’authentification, configurées par votre administrateur, s’affichent. Si vous autorisez l’authentification externe comme principale est activée, elle sera également incluse.

![newsignin](media/AD-FS-paginated-sign-in/signin4.png)

Sur la troisième page, vous êtes invité à entrer votre mot de passe (en supposant que vous avez sélectionné « mot de passe » comme option d’authentification).

## <a name="how-to-get-the-new-experience"></a>Comment obtenir la nouvelle expérience

### <a name="new-installation-of-ad-fs"></a>Nouvelle installation de AD FS
Si vous êtes un nouveau client à AD FS, vous recevrez la nouvelle conception par défaut.

### <a name="upgrading-a-farm"></a>Mise à niveau d’une batterie de serveurs
Si vous êtes un client existant AD FS 2012 R2 ou 2016, il existe deux façons de recevoir la nouvelle conception après la mise à niveau des serveurs vers AD FS 2019 et l’activation de FBL à 2019.

- Autorisez la nouvelle connexion via PowerShell. Exécutez la commande suivante pour activer la pagination :``Set-AdfsGlobalAuthenticationPolicy -EnablePaginatedAuthenticationPages $true``

 - Activez l’authentification externe comme principale, soit via PowerShell, soit via le Gestionnaire de serveur AD FS. Les nouvelles pages de connexion paginée sont activées lorsque cette fonctionnalité est activée.
Si vous êtes un nouveau client à AD FS, vous recevrez la nouvelle conception par défaut. Toutefois, si vous êtes un client existant avec AD FS 2012 R2 ou 2016, vous devez suivre plusieurs étapes pour recevoir la nouvelle conception :``Set-AdfsGlobalAuthenticationPolicy -AllowAdditionalAuthenticationAsPrimary $true``

## <a name="customization"></a>Personnalisation
Les options de personnalisation seront toujours applicables pour AD FS 2019.
Vous trouverez ci-dessous des liens vers d’autres documents relatifs à votre référence.

• Pour ceux qui ne prévoient pas la mise à niveau de leurs serveurs vers AD FS 2019, mais veulent toujours la nouvelle conception : [Utilisation d’un thème Web Azure AD UX dans Services ADFS](azure-ux-web-theme-in-ad-fs.md)

• Un emplacement central pour la personnalisation : [Personnalisation de la connexion utilisateur AD FS](ad-fs-user-sign-in-customization.md)
