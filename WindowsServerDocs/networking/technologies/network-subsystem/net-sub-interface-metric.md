---
title: Configurer l’ordre des interfaces réseau
description: Cette rubrique fait partie du guide de réglage de performances du sous-système de réseau pour Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 3266328c-ca82-40d2-90ca-854b7088ccaa
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 18bc9a268b4e69e4b87b6b1e310f1162473adb10
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821770"
---
# <a name="configure-the-order-of-network-interfaces"></a>Configurer l’ordre des interfaces réseau

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Dans Windows Server 2016 et Windows 10, vous pouvez utiliser la métrique de l’interface pour configurer l’ordre des interfaces réseau.

Cela est différent dans les versions précédentes de Windows et Windows Server, ce qui vous permet de configurer l’ordre de liaison des cartes réseau à l’aide de l’interface utilisateur ou les commandes autorisé **INetCfgComponentBindings::MoveBefore**et **INetCfgComponentBindings::MoveAfter**. Ces deux méthodes pour le classement des interfaces réseau ne sont pas disponibles dans Windows Server 2016 et Windows 10.

Au lieu de cela, vous pouvez utiliser la nouvelle méthode pour définir l’ordre énumérée de cartes réseau en configurant la métrique de l’interface de chaque carte. Vous pouvez configurer la métrique de l’interface à l’aide de la [Set-NetIPInterface](https://docs.microsoft.com/powershell/module/nettcpip/set-netipinterface) commande Windows PowerShell.

Lorsque les itinéraires du trafic réseau sont sélectionnés et que vous avez configuré le **InterfaceMetric** paramètre de la **Set-NetIPInterface** , de la mesure globale qui est utilisée pour déterminer l’interface de commande préférence est la somme de la métrique de routage et la métrique de l’interface. En règle générale, la métrique de l’interface donne la préférence à une interface particulière, telles que l’utilisation câblé si câblé et sans fil sont disponibles.

L’exemple de commande Windows PowerShell suivant montre l’utilisation de ce paramètre.

    Set-NetIPInterface -InterfaceIndex 12 -InterfaceMetric 15

L’ordre dans lequel les cartes apparaissent dans une liste est déterminé par la métrique de l’interface IPv4 ou IPv6.  Pour plus d’informations, consultez [GetAdaptersAddresses fonction](https://msdn.microsoft.com/library/windows/desktop/aa365915%28v=vs.85%29.aspx?f=255&MSPPError=-2147217396).

Pour obtenir des liens vers toutes les rubriques de ce guide, consultez [réglage des performances réseau sous-système](net-sub-performance-top.md).
