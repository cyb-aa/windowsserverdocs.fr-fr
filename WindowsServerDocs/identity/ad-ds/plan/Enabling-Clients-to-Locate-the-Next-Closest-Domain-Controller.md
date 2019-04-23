---
ms.assetid: 7dd905ea-4235-4519-8400-31b4fa0ed1bf
title: Permettre aux clients de localiser le contrôleur de domaine suivant le plus proche
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 7550bdcea4e7b06d31463744bfdc3319c012c62c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880360"
---
# <a name="enabling-clients-to-locate-the-next-closest-domain-controller"></a>Permettre aux clients de localiser le contrôleur de domaine suivant le plus proche

>S'applique à : Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Si vous avez un contrôleur de domaine qui exécute Windows Server 2008 ou version ultérieure, vous pouvez rendre cela possible pour les ordinateurs clients qui exécutent Windows Vista ou version ultérieure ou Windows Server 2008 ou plus récent à localiser les contrôleurs de domaine plus efficacement en activant le **essayez suivant Site le plus proche** paramètre de stratégie de groupe. Ce paramètre améliore le localisateur de contrôleur de domaine (DC Locator) en vous aidant à rationaliser le trafic de réseau, en particulier dans les grandes entreprises qui ont plusieurs filiales et sites.

Ce nouveau paramètre peut affecter la façon dont vous configurez les coûts de lien de sites, car il affecte l’ordre dans lequel les contrôleurs de domaine sont situés. Pour les entreprises qui ont de nombreux sites concentrateurs et les succursales, vous pouvez réduire considérablement le trafic Active Directory sur le réseau en veillant à ce que les clients pour basculer le site hub le plus proche suivant lorsqu’ils ne peut pas trouver un contrôleur de domaine dans le site le plus proche de concentrateur.

Comme une meilleure pratique générale, vous devez simplifier votre topologie de site et de lien de sites coûte autant que possible si vous activez le **essayer le Site le plus proche suivant** paramètre. Dans les entreprises avec de nombreux sites hub, cela peut simplifier tous les plans que vous effectuez pour la gestion des situations dans lesquelles les clients dans un site doivent basculer vers un contrôleur de domaine dans un autre site.

Par défaut, le **essayer le Site le plus proche suivant** n’est pas activé. Lorsque le paramètre n’est pas activé, le localisateur de contrôleur de domaine utilise l’algorithme suivant pour localiser un contrôleur de domaine :

- Essayez de rechercher un contrôleur de domaine dans le même site.
- Si aucun contrôleur de domaine n’est disponible dans le même site, essayez de rechercher n’importe quel contrôleur de domaine dans le domaine.

> [!NOTE]
> Il s’agit de l’algorithme même localisateur de contrôleur de domaine utilisé dans les versions précédentes d’Active Directory. Pour plus d’informations, consultez l’article [la prise en charge DNS pour Active Directory fonctionne](https://go.microsoft.com/fwlink/?LinkId=108587).

Si vous activez le **essayer le Site le plus proche suivant** définition, le localisateur de contrôleur de domaine utilise l’algorithme suivant pour localiser un contrôleur de domaine :

- Essayez de rechercher un contrôleur de domaine dans le même site.
- Si aucun contrôleur de domaine n’est disponible dans le même site, essayez de rechercher un contrôleur de domaine dans le site le plus proche suivant. Un site est plus proche si elle a un lien de sites inférieur à un autre site de coût avec un lien de sites plus élevé de coût.
- Si aucun contrôleur de domaine n’est disponible dans le site le plus proche suivant, essayez de rechercher n’importe quel contrôleur de domaine dans le domaine.

Le **essayer le Site le plus proche suivant** paramètre fonctionne conjointement avec la couverture de site automatique. Par exemple, si le site le plus proche suivant n’a aucun contrôleur de domaine, le localisateur de contrôleur de domaine tente de trouver le contrôleur de domaine qui exécute la couverture de site automatique pour ce site.

Par défaut, le localisateur de contrôleur de domaine ne considère pas n’importe quel site qui contient un contrôleur de domaine en lecture seule (RODC) lorsqu’il détermine le site le plus proche suivant. En outre, lorsque le client obtient une réponse à partir d’un contrôleur de domaine qui exécute une version antérieure à Windows Server 2008, le comportement de localisateur de contrôleur de domaine est le même que lorsque la définition n’est pas activée.

Par exemple, supposons qu’une topologie de site a quatre sites avec les valeurs de lien de site dans l’illustration suivante. Dans cet exemple, tous les contrôleurs de domaine sont des contrôleurs de domaine accessible en écriture qui exécutent Windows Server 2008 ou version ultérieure.

![activation des clients de localiser le contrôleur de domaine](media/Enabling-Clients-to-Locate-the-Next-Closest-Domain-Controller/beff4087-fb2a-463b-96ac-d440a9e29b75.gif)

Lorsque le **essayer le Site le plus proche suivant** paramètre de stratégie de groupe est activé dans cet exemple, si un ordinateur client dans Site_B tente de localiser un contrôleur de domaine, il essaie d’abord de trouver un contrôleur de domaine dans son propre Site_B. Si aucun n’est disponible dans Site_B, il essaie de trouver un contrôleur de domaine du Site_A.

Si le paramètre n’est pas activé, le client tente de trouver un contrôleur de domaine dans Site_A, Site_C ou Site_D si aucun contrôleur de domaine n’est disponible dans Site_B.

> [!NOTE]
> Le **essayer le Site le plus proche suivant** paramètre fonctionne conjointement avec la couverture de site automatique. Par exemple, si le site le plus proche suivant n’a aucun contrôleur de domaine, le localisateur de contrôleur de domaine tente de trouver le contrôleur de domaine qui exécute la couverture de site automatique pour ce site.

Pour appliquer le **essayer le Site le plus proche suivant** définition, vous pouvez créer un objet de stratégie de groupe (GPO) et liez-le à l’objet approprié pour votre organisation, ou vous pouvez modifier la stratégie de domaine par défaut pour qu’il affecte tous les clients qui exécutent Windows Vista ou version ultérieure et Windows Server 2008 ou version ultérieure dans le domaine. Pour plus d’informations sur la façon de définir le **essayer le Site le plus proche suivant** définition, consultez [permettent aux Clients de localiser un contrôleur de domaine dans le Site le plus proche suivant](https://technet.microsoft.com/library/cc772592.aspx).
