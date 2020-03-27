---
title: Configurer l’ordre des interfaces réseau
description: Cette rubrique fait partie du Guide d’optimisation des performances du sous-système réseau pour Windows Server 2016.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 3266328c-ca82-40d2-90ca-854b7088ccaa
manager: brianlic
ms.author: lizross
author: eross-msft
ms.openlocfilehash: d82009a4b6e9bcf20ae9cccb4259956358b53148
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80316609"
---
# <a name="configure-the-order-of-network-interfaces"></a>Configurer l’ordre des interfaces réseau

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Dans Windows Server 2016 et Windows 10, vous pouvez utiliser la métrique de l’interface pour configurer l’ordre des interfaces réseau.

Cela diffère des versions précédentes de Windows et de Windows Server, ce qui vous permettait de configurer l’ordre de liaison des cartes réseau à l’aide de l’interface utilisateur ou des commandes **INetCfgComponentBindings :: MoveBefore** et **INetCfgComponentBindings :: MoveAfter**. Ces deux méthodes pour commander des interfaces réseau ne sont pas disponibles dans Windows Server 2016 et Windows 10.

Au lieu de cela, vous pouvez utiliser la nouvelle méthode pour définir l’ordre d’énumération des cartes réseau en configurant la mesure de l’interface de chaque adaptateur. Vous pouvez configurer la métrique de l’interface à l’aide de la commande Windows PowerShell [Set-NetIPInterface](https://docs.microsoft.com/powershell/module/nettcpip/set-netipinterface) .

Lorsque les itinéraires du trafic réseau sont choisis et que vous avez configuré le paramètre **InterfaceMetric** de la commande **Set-NetIPInterface** , la mesure globale utilisée pour déterminer la préférence de l’interface est la somme de la métrique de l’itinéraire et de la métrique de l’interface. En règle générale, la métrique de l’interface donne la préférence à une interface particulière, telle que l’utilisation de Wired si les réseaux filaires et sans fil sont disponibles.

L’exemple de commande Windows PowerShell suivant illustre l’utilisation de ce paramètre.

    Set-NetIPInterface -InterfaceIndex 12 -InterfaceMetric 15

L’ordre dans lequel les adaptateurs apparaissent dans une liste est déterminé par la métrique de l’interface IPv4 ou IPv6.  Pour plus d’informations, consultez [fonction GetAdaptersAddresses](https://msdn.microsoft.com/library/windows/desktop/aa365915%28v=vs.85%29.aspx?f=255&MSPPError=-2147217396).

Pour obtenir des liens vers toutes les rubriques de ce guide, consultez [réglage des performances du sous-système réseau](net-sub-performance-top.md).
