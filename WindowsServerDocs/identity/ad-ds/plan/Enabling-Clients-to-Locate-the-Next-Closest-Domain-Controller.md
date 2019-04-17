---
ms.assetid: 7dd905ea-4235-4519-8400-31b4fa0ed1bf
title: "Activation des Clients de localiser le contrôleur de domaine le plus proche suivant"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 39b1b79bba944c10b0c74c4bb18f6dcf80f8230e
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="enabling-clients-to-locate-the-next-closest-domain-controller"></a>Activation des Clients de localiser le contrôleur de domaine le plus proche suivant

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Si vous disposez d’un contrôleur de domaine qui exécute Windows Server2008 ou Windows Server2008R2, vous pouvez rendre possible pour les ordinateurs clients qui exécutent WindowsVista, Windows7, Windows Server2008 ou Windows Server2008R2 pour localiser les contrôleurs de domaine plus efficacement en activant le **Site le plus proche suivant essayez** paramètre de stratégie de groupe. Ce paramètre améliore le localisateur de contrôleur de domaine (localisateur de contrôleur de domaine) en vous aidant à rationaliser le trafic réseau, en particulier dans les grandes entreprises qui ont de nombreux sites et les filiales.  
  
Ce nouveau paramètre peut affecter la configuration de coûts de lien de site, car cela affecte l’ordre dans lequel se trouvent les contrôleurs de domaine. Pour les entreprises qui ont de nombreux sites hub et les filiales, vous pouvez réduire considérablement le trafic ActiveDirectory sur le réseau en veillant à ce que les clients basculement vers le site hub le plus proche suivant lorsqu’ils ne peut pas trouver un contrôleur de domaine dans le site le plus proche du concentrateur.  
  
Général recommandé, vous devez simplifier votre topologie de site et les coûts de lien de sites autant que possible si vous activez le **Site le plus proche suivant essayez** paramètre. Dans les entreprises avec de nombreux sites hub, ceci peut simplifier les plans que vous effectuez pour gérer des situations dans lesquelles les clients dans un site doivent basculer vers un contrôleur de domaine dans un autre site.  
  
Par défaut, le **Site le plus proche suivant essayez** n’est pas activé. Lorsque le paramètre n’est pas activé, le localisateur de contrôleur de domaine utilise l’algorithme suivant pour localiser un contrôleur de domaine:  
  
-   Essayez de trouver un contrôleur de domaine dans le même site.  
  
-   Si aucun contrôleur de domaine n’est disponible dans le même site, essayez de rechercher un contrôleur de domaine dans le domaine.  
  
> [!NOTE]  
> Il s’agit du même algorithme localisateur de contrôleur de domaine utilisé dans les versions précédentes d’ActiveDirectory. Pour plus d’informations, voir comment prendre en charge DNS pour ActiveDirectory fonctionne ([https://go.microsoft.com/fwlink/?LinkId=108587](https://go.microsoft.com/fwlink/?LinkId=108587)).  
  
Si vous activez le **Site le plus proche suivant essayez** paramètre, le localisateur de contrôleur de domaine utilise l’algorithme suivant pour localiser un contrôleur de domaine:  
  
-   Essayez de trouver un contrôleur de domaine dans le même site.  
  
-   Si aucun contrôleur de domaine n’est disponible dans le même site, essayez de rechercher un contrôleur de domaine dans le site le plus proche suivant. Un site est plus proche si elle a un lien de sites moindre coût à un autre site avec un lien de site supérieur de coût.  
  
-   Si aucun contrôleur de domaine n’est disponible dans le site le plus proche suivant, essayez de rechercher un contrôleur de domaine dans le domaine.  
  
Par défaut, le localisateur de contrôleur de domaine ne considère pas n’importe quel site qui contient un contrôleur de domaine en lecture seule (RODC) lorsqu’il détermine le site le plus proche suivant. En outre, étant donné qu’uniquement Windows Server2008 et Windows Server2008R2 domaine contrôleurs prise en charge la fonctionnalité site le plus proche suivante, lorsque le client reçoit une réponse à partir d’un contrôleur de domaine qui exécute une version antérieure de Windows Server, le comportement du localisateur de contrôleur de domaine est le même que lorsque puis en définissant n'est pas activé.  
  
Par exemple, supposons qu’une topologie de site a quatre sites avec les valeurs de lien de site dans l’illustration suivante. Dans cet exemple, tous les contrôleurs de domaine sont des contrôleurs de domaine accessible en écriture qui exécutent Windows Server2008 ou Windows Server2008R2.  
  
![Activation des clients de localiser le contrôleur de domaine](media/Enabling-Clients-to-Locate-the-Next-Closest-Domain-Controller/beff4087-fb2a-463b-96ac-d440a9e29b75.gif)  
  
Lorsque le **Site le plus proche suivant essayez** paramètre de stratégie de groupe est activé dans cet exemple, si un ordinateur client qui exécute WindowsVista, Windows7, Windows Server2008, ou Windows Server2008R2 dans Site_B tente de localiser un contrôleur de domaine, il essaie d’abord de trouver un contrôleur de domaine dans son propre Site_B. Si aucun n’est disponible dans Site_B, il essaie de trouver un contrôleur de domaine dans Site_A.  
  
Si le paramètre n’est pas activé, le client tente de trouver un contrôleur de domaine dans Site_A, Site_C ou Site_D si aucun contrôleur de domaine n’est disponible dans Site_B.  
  
> [!NOTE]  
> Le **Site le plus proche suivant essayez** paramètre fonctionne en coordination avec la couverture de site automatique. Par exemple, si le site le plus proche suivant n’a aucun contrôleur de domaine, localisateur de contrôleur de domaine tente de trouver le contrôleur de domaine qui exécute la couverture de site automatique pour le site.  
  
Pour appliquer le **Site le plus proche suivant essayez** définition, vous pouvez créer un objet de stratégie de groupe (GPO) et le lier à l’objet approprié pour votre organisation, ou vous pouvez modifier la stratégie de domaine par défaut pour qu’il soit affectent tous les clients qui exécutent WindowsVista, Windows7, Windows Server2008 ou Windows Server2008R2 dans le domaine. Pour plus d’informations sur la façon de définir la **Site le plus proche suivant essayez** définition, consultez [permettent aux Clients de localiser un contrôleur de domaine dans le Site le plus proche suivant](https://technet.microsoft.com/library/cc772592.aspx).  
  


