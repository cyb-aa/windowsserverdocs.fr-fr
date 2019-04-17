---
title: Configurer l’ordre des Interfaces réseau
description: Cette rubrique fait partie du guide de réglage des performances sous-système réseau pour Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 3266328c-ca82-40d2-90ca-854b7088ccaa
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2edf9d312e50a0fd8485e0032dee8413675367cf
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="configure-the-order-of-network-interfaces"></a>Configurer l’ordre des Interfaces réseau

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Dans Windows Server 2016 et Windows 10, vous pouvez utiliser la métrique de l’interface pour configurer l’ordre des interfaces réseau.

Cela est différent de celui des versions antérieures de Windows et Windows Server, qui vous permet de configurer l’ordre de liaison des cartes réseau à l’aide de l’interface utilisateur ou les commandes autorisé **INetCfgComponentBindings::MoveBefore** et **INetCfgComponentBindings::MoveAfter**. Ces deux méthodes de classement des interfaces réseau ne sont pas disponibles dans Windows Server 2016 et Windows 10.

Au lieu de cela, vous pouvez utiliser la nouvelle méthode pour définir l’ordre énuméré de cartes réseau en configurant la métrique de l’interface de chaque carte. Vous pouvez configurer la métrique de l’interface à l’aide de la [Set-NetIPInterface](https://docs.microsoft.com/en-us/powershell/module/nettcpip/set-netipinterface) commande Windows PowerShell.

Lorsque les itinéraires du trafic réseau sont choisis et que vous avez configuré le **InterfaceMetric** paramètre de la **Set-NetIPInterface** la métrique globale qui est utilisée pour déterminer la préférence de l’interface est la somme de la métrique de l’itinéraire et la métrique de l’interface de la commande. En règle générale, la métrique de l’interface privilégie une interface particulière, tels que l’utilisation câblé si câblé et sans fil sont disponibles.

L’exemple de commande Windows PowerShell suivant montre l’utilisation de ce paramètre.

    Set-NetIPInterface -InterfaceIndex 12 -InterfaceMetric 15

L’ordre dans lequel les cartes apparaissent dans une liste est déterminée par la métrique de l’interface IPv4 ou IPv6.  Pour plus d’informations, voir [GetAdaptersAddresses fonction](https://msdn.microsoft.com/library/windows/desktop/aa365915%28v=vs.85%29.aspx?f=255&MSPPError=-2147217396).

Pour obtenir des liens vers toutes les rubriques de ce guide, voir [réglage des performances réseau sous-système](net-sub-performance-top.md).
