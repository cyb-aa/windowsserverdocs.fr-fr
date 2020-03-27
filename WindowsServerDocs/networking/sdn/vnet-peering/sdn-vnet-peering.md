---
title: Homologation de réseau virtuel
description: ''
manager: dougkim
ms.prod: windows-server
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: ''
ms.author: lizross
author: eross-msft
ms.date: 08/08/2018
ms.openlocfilehash: 01c768aefa685b688c2ed3f777c44a4665b5e4a7
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80309683"
---
# <a name="virtual-network-peering"></a>Homologation de réseau virtuel

>S’applique à : Windows Server

L’homologation de réseaux virtuels vous permet de connecter deux réseaux virtuels en toute transparence. Une fois homologués, à des fins de connectivité, les réseaux virtuels apparaissent comme un seul. 

Les avantages de l’homologation de réseaux virtuels sont les suivants :

-   Le trafic entre les machines virtuelles dans les réseaux virtuels homologués est acheminé via l’infrastructure de réseau principal par le biais d’adresses IP *privées* uniquement. La communication entre les réseaux virtuels ne requiert pas de passerelle ou d’Internet public.

-   Une connexion à faible latence et à bande passante élevée entre les ressources de différents réseaux virtuels.

-   Possibilité pour les ressources d’un réseau virtuel de communiquer avec les ressources d’un autre réseau virtuel.

-   Aucun temps d’arrêt pour les ressources dans un réseau virtuel lors de la création de l’homologation.

## <a name="requirements-and-constraints"></a>Exigences et contraintes

L’homologation de réseaux virtuels a quelques exigences et contraintes :

- Les réseaux virtuels homologués doivent :

  -   Avoir des espaces d’adressage IP qui ne se chevauchent pas

  -   Être géré par le même contrôleur de réseau

- Une fois que vous avez homologuer un réseau virtuel avec un autre réseau virtuel, vous ne pouvez pas ajouter ou supprimer des plages d’adresses dans l’espace d’adressage.

  >[!TIP]
  >Si vous devez ajouter des plages d’adresses :<ol><li>Supprimez l’homologation.</li><li>Ajoutez l’espace d’adressage.</li><li>Rajoutez l’homologation.</li></ol>

- Étant donné que l’homologation de réseaux virtuels se trouve entre deux réseaux virtuels, il n’existe aucune relation transitive dérivée entre les homologations. Par exemple, si vous Homologuez virtualNetworkA avec virtualNetworkB et virtualNetworkB avec virtualNetworkC, virtualNetworkA n’est pas homologué avec virtualNetworkC.

  [image ici]

## <a name="connectivity"></a>Connectivité

Une fois les réseaux virtuels homologues, les ressources des deux réseaux virtuels peuvent se connecter directement aux ressources du réseau virtuel homologué.

-   La latence du réseau entre les machines virtuelles dans les réseaux virtuels homologués est la même que la latence au sein d’un seul réseau virtuel.

-   Le débit du réseau est basé sur la bande passante autorisée pour la machine virtuelle. Il n’existe aucune restriction supplémentaire sur la bande passante au sein de l’homologation.

-   Le trafic entre les machines virtuelles dans les réseaux virtuels homologués est acheminé directement par le biais de l’infrastructure du réseau principal, et non via une passerelle ou via l’Internet public.

-   Les machines virtuelles d’un réseau virtuel peuvent accéder à l’équilibreur de charge interne dans le réseau virtuel homologué.

Vous pouvez appliquer des listes de contrôle d’accès (ACL) dans un réseau virtuel pour bloquer l’accès à d’autres réseaux virtuels ou sous-réseaux si vous le souhaitez. Si vous ouvrez une connectivité complète entre des réseaux virtuels homologués (option par défaut), vous pouvez appliquer des listes de contrôle d’accès à des sous-réseaux ou des machines virtuelles spécifiques pour bloquer ou refuser un accès spécifique. Pour en savoir plus sur les listes de contrôle d’accès, consultez [utiliser des listes de Access Control (ACL) pour gérer le flux de trafic réseau du centre de](https://docs.microsoft.com/windows-server/networking/sdn/manage/use-acls-for-traffic-flow)connaissances.

## <a name="service-chaining"></a>Chaînage de services

Vous pouvez configurer des itinéraires définis par l’utilisateur qui pointent vers des machines virtuelles dans des réseaux virtuels homologués en tant qu’adresse IP de tronçon suivant, pour activer le chaînage de services. Le chaînage de services vous permet de diriger le trafic d’un réseau virtuel vers une appliance virtuelle, dans un réseau virtuel homologué, via des itinéraires définis par l’utilisateur.

Vous pouvez déployer des réseaux Hub-and-spoke, où le réseau virtuel Hub peut héberger des composants d’infrastructure tels qu’une appliance virtuelle réseau. Tous les réseaux virtuels spoke homologuent avec le réseau virtuel Hub. Le trafic peut transiter via des appliances virtuelles réseau dans le réseau virtuel Hub.

L’homologation de réseaux virtuels permet au tronçon suivant dans un itinéraire défini par l’utilisateur d’être l’adresse IP d’un ordinateur virtuel dans le réseau virtuel homologué. Pour en savoir plus sur les itinéraires définis par l’utilisateur, consultez [utiliser des appliances virtuelles réseau sur un réseau virtuel](https://docs.microsoft.com/windows-server/networking/sdn/manage/use-network-virtual-appliances-on-a-vn).

## <a name="gateways-and-on-premises-connectivity"></a>Passerelles et connectivité locale

Chaque réseau virtuel, qu’il soit homologué ou non avec un autre réseau virtuel, peut toujours avoir sa propre passerelle pour se connecter à un réseau local. Lorsque vous Homologuez des réseaux virtuels, vous pouvez également configurer la passerelle dans le réseau virtuel homologué en tant que point de transit vers un réseau local. Dans ce cas, le réseau virtuel qui utilise une passerelle distante ne peut pas avoir sa propre passerelle. Un réseau virtuel ne peut avoir qu’une seule passerelle qui peut être une passerelle locale ou distante (dans le réseau virtuel homologué).

## <a name="monitor"></a>Surveiller

Lorsque vous Homologuez deux réseaux virtuels, vous devez configurer une homologation pour chaque réseau virtuel dans l’homologation.

Vous pouvez surveiller l’état de votre connexion d’homologation, qui peut être dans l’un des États suivants :

-   **Initié le :** S’affiche lorsque vous créez l’homologation à partir du premier réseau virtuel vers le second réseau virtuel.

-   **Connecté :** Indiqué après avoir créé l’homologation à partir du deuxième réseau virtuel vers le premier réseau virtuel. L’état d’homologation pour le premier réseau virtuel passe de initié à connecté. Les deux homologues de réseau virtuel doivent avoir l’état connecté avant d’établir correctement une homologation de réseau virtuel.

-   **Déconnecté :** Indique si un réseau virtuel se déconnecte d’un autre réseau virtuel.

[infographie des États]

## <a name="next-steps"></a>Étapes suivantes :
[Configuration de l’homologation de réseaux virtuels](sdn-configure-vnet-peering.md): dans cette procédure, vous utilisez Windows PowerShell pour rechercher le réseau logique de fournisseur HNV pour créer deux réseaux virtuels, chacun avec un sous-réseau. Vous configurez également l’homologation entre les deux réseaux virtuels.

