---
ms.assetid: 7dd905ea-4235-4519-8400-31b4fa0ed1bf
title: Permettre aux clients de localiser le contrôleur de domaine suivant le plus proche
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: ed7663242ae254ecea945a749ee3ce5fac8f96f6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408830"
---
# <a name="enabling-clients-to-locate-the-next-closest-domain-controller"></a>Permettre aux clients de localiser le contrôleur de domaine suivant le plus proche

>S'applique à : Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Si vous disposez d’un contrôleur de domaine qui exécute Windows Server 2008 ou une version plus récente, vous pouvez permettre aux ordinateurs clients qui exécutent Windows Vista ou une version plus récente ou Windows Server 2008 ou une version plus récente de localiser les contrôleurs de domaine plus efficacement en activant le **site essayer le suivant le plus proche.** Paramètre stratégie de groupe. Ce paramètre améliore le localisateur de contrôleur de domaine en aidant à rationaliser le trafic réseau, en particulier dans les grandes entreprises qui ont de nombreux sites et succursales.

Ce nouveau paramètre peut affecter la façon dont vous configurez les coûts des liens de sites, car il affecte l’ordre dans lequel les contrôleurs de domaine sont situés. Pour les entreprises qui possèdent de nombreux sites et succursales Hub, vous pouvez réduire de manière significative le trafic Active Directory sur le réseau en veillant à ce que les clients basculent vers le site concentrateur suivant le plus proche lorsqu’ils ne peuvent pas trouver de contrôleur de domaine dans le site du Hub le plus proche.

En guise de meilleure pratique générale, vous devez simplifier autant que possible la topologie de votre site et les coûts liés aux liaisons de sites si vous activez le paramètre **essayer le site suivant le plus proche** . Dans les entreprises disposant de nombreux sites hub, cela peut simplifier les plans que vous créez pour gérer les situations dans lesquelles les clients d’un site doivent basculer vers un contrôleur de domaine d’un autre site.

Par défaut, le paramètre **essayer le site suivant le plus proche** n’est pas activé. Lorsque le paramètre n’est pas activé, le localisateur de contrôleur de domaine utilise l’algorithme suivant pour localiser un contrôleur de domaine :

- Essayez de trouver un contrôleur de domaine dans le même site.
- Si aucun contrôleur de domaine n’est disponible sur le même site, essayez de trouver un contrôleur de domaine dans le domaine.

> [!NOTE]
> Il s’agit du même algorithme que celui utilisé dans les versions précédentes de Active Directory. Pour plus d’informations, consultez l’article sur le [fonctionnement de la prise en charge de DNS pour Active Directory](https://go.microsoft.com/fwlink/?LinkId=108587).

Si vous activez le paramètre **essayer le site suivant le plus proche** , le localisateur de contrôleur de domaine utilise l’algorithme suivant pour localiser un contrôleur de domaine :

- Essayez de trouver un contrôleur de domaine dans le même site.
- Si aucun contrôleur de domaine n’est disponible sur le même site, essayez de trouver un contrôleur de domaine dans le site suivant le plus proche. Un site est plus proche s’il présente un coût de lien de site inférieur à celui d’un autre site avec un coût de lien de site supérieur.
- Si aucun contrôleur de domaine n’est disponible dans le site suivant le plus proche, essayez de trouver un contrôleur de domaine dans le domaine.

Le paramètre de **site essayer suivant le plus proche** fonctionne en coordination avec la couverture de site automatique. Par exemple, si le site suivant le plus proche n’a pas de contrôleur de domaine, le localisateur de contrôleur de domaine tente de trouver le contrôleur de domaine qui exécute la couverture automatique de site pour ce site.

Par défaut, le localisateur de contrôleur de domaine ne prend pas en compte les sites qui contiennent un contrôleur de domaine en lecture seule (RODC) lorsqu’il détermine le site suivant le plus proche. En outre, lorsque le client reçoit une réponse d’un contrôleur de domaine qui exécute une version antérieure à Windows Server 2008, le comportement du localisateur de contrôleur de domaine est le même que lorsque le paramètre n’est pas activé.

Par exemple, supposons qu’une topologie de site comporte quatre sites avec les valeurs de lien de sites dans l’illustration suivante. Dans cet exemple, tous les contrôleurs de domaine sont des contrôleurs de domaine accessibles en écriture qui exécutent Windows Server 2008 ou une version plus récente.

![activation des clients pour localiser le contrôleur de périphérique](media/Enabling-Clients-to-Locate-the-Next-Closest-Domain-Controller/beff4087-fb2a-463b-96ac-d440a9e29b75.gif)

Lorsque le paramètre **essayer le prochain site le plus proche** stratégie de groupe est activé dans cet exemple, si un ordinateur client dans Site_B tente de localiser un contrôleur de domaine, il tente d’abord de trouver un contrôleur de domaine dans son propre Site_B. Si aucun n’est disponible dans Site_B, il tente de trouver un contrôleur de domaine dans Site_A.

Si le paramètre n’est pas activé, le client tente de trouver un contrôleur de domaine dans Site_A, Site_C ou Site_D si aucun contrôleur de domaine n’est disponible dans Site_B.

> [!NOTE]
> Le paramètre de **site essayer suivant le plus proche** fonctionne en coordination avec la couverture de site automatique. Par exemple, si le site suivant le plus proche n’a pas de contrôleur de domaine, le localisateur de contrôleur de domaine tente de trouver le contrôleur de domaine qui exécute la couverture automatique de site pour ce site.

Pour appliquer le paramètre **essayer le site suivant le plus proche** , vous pouvez créer un objet de stratégie de groupe (GPO) et le lier à l’objet approprié pour votre organisation, ou vous pouvez modifier la stratégie de domaine par défaut pour qu’elle affecte tous les clients qui exécutent Windows Vista ou une version plus récente et Windows Server 2008 ou une version plus récente dans le domaine. Pour plus d’informations sur la façon de définir le paramètre de **site essayer suivant le plus proche** , consultez [activer les clients pour localiser un contrôleur de domaine dans le site suivant le plus proche](https://technet.microsoft.com/library/cc772592.aspx).
