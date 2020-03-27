---
title: Options de sélection du sous-réseau DHCP
description: Cette rubrique fournit des informations sur les options de sélection du sous-réseau DHCP pour le protocole DHCP (Dynamic Host Configuration Protocol) dans Windows Server 2016.
manager: dougkim
ms.prod: windows-server
ms.technology: networking-dhcp
ms.topic: get-started-article
ms.assetid: ca19e7d1-e445-48fc-8cf5-e4c45f561607
ms.author: lizross
author: eross-msft
ms.date: 08/17/2018
ms.openlocfilehash: 4e83448e95f6a6f77deb9ff7997d715cbc7f13c6
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80312547"
---
# <a name="dhcp-subnet-selection-options"></a>Options de sélection du sous-réseau DHCP

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour obtenir des informations sur les nouvelles options de sélection de sous-réseau DHCP.

DHCP prend désormais en charge l’option 82 \(sous-option 5\). Vous pouvez utiliser ces options pour autoriser les clients de proxy et les agents de relais DHCP à demander une adresse IP pour un sous-réseau spécifique, et à partir d’une plage d’adresses IP et d’une étendue spécifiques.  Pour plus d’informations, consultez option **82 Sub option 5**: [RFC 3527 Link Selection pour l’option d’informations de l’agent de relais pour DHCPv4](https://tools.ietf.org/html/rfc3527).

Si vous utilisez un agent de relais DHCP configuré avec l’option DHCP 82, sous-option 5, l’agent de relais peut demander un bail d’adresse IP pour les clients DHCP à partir d’une plage d’adresses IP spécifique.


## <a name="option-82-sub-option-5-link-selection-sub-option"></a>Option 82 Sub option 5 : option de sous-option de sélection de lien

La sous-option sélection du lien de l’agent de relais permet à un agent de relais DHCP de spécifier un sous-réseau IP à partir duquel le serveur DHCP doit affecter des adresses IP et des options.

En règle générale, les agents de relais DHCP s’appuient sur l’adresse IP de la passerelle \(champ de\) GIADDR pour communiquer avec les serveurs DHCP. Toutefois, GIADDR est limité par ses deux fonctions opérationnelles :

1. Pour informer le serveur DHCP du sous-réseau sur lequel réside le client DHCP qui demande le bail d’adresse IP.
2. Pour informer le serveur DHCP de l’adresse IP à utiliser pour communiquer avec l’agent de relais.

Dans certains cas, l’adresse IP que l’agent de relais utilise pour communiquer avec le serveur DHCP peut être différente de la plage d’adresses IP à partir de laquelle l’adresse IP du client DHCP doit être allouée. 

La sous-option de sélection de lien de l’option 82 est utile dans cette situation, ce qui permet à l’agent de relais d’indiquer explicitement le sous-réseau à partir duquel il veut que l’adresse IP soit allouée sous la forme de l’option 5 de la sous-option 5 DHCP 82 v4.

> [!NOTE]
>
> Toutes les adresses IP de l’agent de relais (GIADDR) doivent faire partie d’une plage d’adresses IP d’étendue DHCP active. Tout GIADDR en dehors des plages d’adresses IP de l’étendue DHCP est considéré comme un relais non autorisé et le serveur DHCP Windows n’accuse pas réception des demandes du client DHCP à partir de ces agents de relais.
>
> Une étendue spéciale peut être créée pour « autoriser » les agents de relais. Créez une étendue avec le GIADDR (ou plusieurs si les GIADDR sont des adresses IP séquentielles), excluez les adresses GIADDR de la distribution, puis activez l’étendue. Cela permet d’autoriser les agents de relais tout en empêchant l’attribution des adresses GIADDR.


### <a name="use-case-scenario"></a>Scénario de cas d’usage

Dans ce scénario, un réseau d’organisation comprend un serveur DHCP et un point d’accès sans fil \(AP\) pour les utilisateurs invités. Les adresses IP des clients invités sont attribuées à partir du serveur DHCP de l’organisation. Toutefois, en raison des restrictions de stratégie de pare-feu, le serveur DHCP ne peut pas accéder au réseau sans fil invité ni aux clients sans fil avec des messages broadcase.

Pour résoudre cette restriction, le point d’accès est configuré avec la sous-option 5 de la sélection de lien afin de spécifier le sous-réseau à partir duquel il veut que l’adresse IP soit allouée pour les clients invités, tandis que dans le GIADDR spécifient également l’adresse IP de l’interface interne qui amène à la réseau d’entreprise.
