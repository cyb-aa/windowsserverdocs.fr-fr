---
title: Homologation de réseau virtuel
description: ''
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.date: 08/08/2018
ms.openlocfilehash: aab4ec7c69ec5b52eae926cd1065d777415b1124
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446211"
---
# <a name="virtual-network-peering"></a>Homologation de réseau virtuel

>S’applique à : Windows Server

Homologation de réseaux virtuels vous permet de connecter deux réseaux virtuels en toute transparence. Une fois homologués, à des fins de connectivité, les réseaux virtuels apparaissent comme un seul. 

Avantages de l’homologation de réseau virtuel :

-   Le trafic entre les machines virtuelles dans les réseaux virtuels homologués est acheminé via l’infrastructure principale via *privé* uniquement des adresses IP. La communication entre les réseaux virtuels ne nécessite pas Internet publics ou des passerelles.

-   Une connexion à faible latence et haut débit entre les ressources dans différents réseaux virtuels.

-   La capacité des ressources dans un réseau virtuel à communiquer avec les ressources dans un autre réseau virtuel.

-   Aucune interruption des ressources dans un réseau virtuel lors de la création de l’homologation.

## <a name="requirements-and-constraints"></a>Exigences et contraintes

Homologation de réseaux virtuels a quelques exigences et contraintes :

- Réseaux virtuels homologués doivent :

  -   Avoir sans chevauchement des espaces d’adressage IP

  -   Être gérés par le même contrôleur de réseau

- Une fois que vous homologuer un réseau virtuel avec un autre réseau virtuel, vous ne pouvez pas ajouter ou supprimer des plages d’adresses dans l’espace d’adressage.

  >[!TIP]
  >Si vous avez besoin ajouter des plages d’adresses :<ol><li>Supprimer l’homologation.</li><li>Ajouter l’espace d’adressage.</li><li>Ajouter à nouveau l’homologation.</li></ol>

- Étant donné que l’homologation se fait entre deux réseaux virtuels, il n’existe aucune relation transitive entre homologations. Par exemple, si vous homologuer virtualNetworkA avec virtualNetworkB et virtualNetworkB avec virtualNetworkC, puis virtualNetworkA ne pas obtenir homologué avec virtualNetworkC.

  [image ici]

## <a name="connectivity"></a>Connectivité

Une fois que vous homologuer des réseaux virtuels, les ressources dans un réseau virtuel peuvent se connecter directement avec les ressources du réseau virtuel homologué.

-   Latence du réseau entre les machines virtuelles de réseaux virtuels homologués est identique à la latence dans un seul réseau virtuel.

-   Débit du réseau repose sur la bande passante autorisée pour la machine virtuelle. Il n’existe pas de toute restriction supplémentaire sur la bande passante au sein de l’homologation.

-   Le trafic entre les machines virtuelles de réseaux virtuels homologués est acheminé directement via l’infrastructure principale, pas par le biais d’une passerelle ou via l’Internet public.

-   Machines virtuelles dans un réseau virtuel peuvent accéder à l’équilibreur de charge interne dans le réseau virtuel homologué.

Vous pouvez appliquer des listes de contrôle d’accès (ACL) dans un réseau virtuel pour bloquer l’accès à d’autres réseaux virtuels ou les sous-réseaux si vous le souhaitez. Si vous ouvrez totalement la connectivité entre réseaux virtuels homologués (qui est l’option par défaut), vous pouvez appliquer des ACL à des sous-réseaux ou des ordinateurs virtuels pour bloquer ou refuser certains accès. Pour en savoir plus sur les ACL, consultez [utiliser Access Control Lists (ACL) pour le flux du trafic réseau centre de données gérer](https://docs.microsoft.com/windows-server/networking/sdn/manage/use-acls-for-traffic-flow).

## <a name="service-chaining"></a>Chaînage de services

Vous pouvez configurer des itinéraires définis par l’utilisateur qui pointent vers les machines virtuelles de réseaux virtuels homologués en tant que l’adresse IP tronçon suivant pour activer le chaînage de services. Chaînage de services vous permet de diriger le trafic à partir d’un réseau virtuel vers une appliance virtuelle, dans un réseau virtuel homologué, via les itinéraires définis par l’utilisateur.

Vous pouvez déployer des réseaux hub-and-spoke, où le réseau virtuel hub peut héberger des composants d’infrastructure comme une appliance virtuelle réseau. Tous les réseaux virtuels spoke homologués avec le réseau virtuel hub. Trafic peut transiter via les appliances virtuelles réseau dans le réseau virtuel hub.

Homologation de réseaux virtuels permet le tronçon suivant dans un itinéraire défini par l’utilisateur sur l’adresse IP d’un ordinateur virtuel dans le réseau virtuel homologué. Pour en savoir plus sur les itinéraires définis par l’utilisateur, consultez [utilisez Appliances virtuelles sur un réseau virtuel](https://docs.microsoft.com/windows-server/networking/sdn/manage/use-network-virtual-appliances-on-a-vn).

## <a name="gateways-and-on-premises-connectivity"></a>Connectivité des passerelles et en local

Chaque réseau virtuel, quel que soit si homologué avec un autre réseau virtuel, peut avoir toujours sa propre passerelle pour se connecter à un réseau local. Lorsque vous homologuer des réseaux virtuels, vous pouvez également configurer la passerelle du réseau virtuel homologué en tant que point de transit vers un réseau local. Dans ce cas, le réseau virtuel qui utilise une passerelle distante ne peut pas posséder sa propre passerelle. Un réseau virtuel peut avoir qu’une seule passerelle peut être une passerelle locale ou distante (dans le réseau virtuel homologué).

## <a name="monitor"></a>Surveiller

Lorsque vous homologuer deux réseaux virtuels, vous devez configurer une homologation pour chaque réseau virtuel dans l’homologation.

Vous pouvez surveiller l’état de votre connexion d’homologation, qui peut être dans un des états suivants :

-   **Initié par :** Indiqué lorsque vous créez l’homologation du premier réseau virtuel vers le second réseau virtuel.

-   **Connecté :** Affiche une fois que vous avez créé l’homologation à partir du deuxième réseau virtuel au premier réseau virtuel. L’état d’homologation pour le premier réseau virtuel passe d’initiée à connectée. Les deux homologues de réseau virtuel doivent avoir l’état de connecté avant d’établir un réseau virtuel, l’homologation avec succès.

-   **Déconnecté :** Indiqué si un réseau virtuel déconnecte un autre réseau virtuel.

[infographie des États]

## <a name="next-steps"></a>Étapes suivantes
[Configurer l’homologation de réseau virtuel](sdn-configure-vnet-peering.md): Dans cette procédure, vous utilisez Windows PowerShell pour rechercher le HNV réseau logique de fournisseur pour créer deux réseaux virtuels, chacun avec un sous-réseau. Vous configurez également l’homologation entre les deux réseaux virtuels.

