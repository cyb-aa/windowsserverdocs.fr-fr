---
title: Options de sélection du sous-réseau DHCP
description: Cette rubrique fournit des informations sur les options de sélection du sous-réseau DHCP pour le protocole DHCP (Dynamic Host Configuration Protocol) dans Windows Server 2016.
manager: dougkim
ms.prod: windows-server
ms.technology: networking-dhcp
ms.topic: get-started-article
ms.assetid: ca19e7d1-e445-48fc-8cf5-e4c45f561607
ms.author: pashort
author: shortpatti
ms.date: 08/17/2018
ms.openlocfilehash: 4718204fad49b23c84cc73b67164f34a803ddd86
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405755"
---
# <a name="dhcp-subnet-selection-options"></a>Options de sélection du sous-réseau DHCP

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour obtenir des informations sur les nouvelles options de sélection de sous-réseau DHCP.

DHCP prend désormais en charge l’option 82 \(sub-option 5 @ no__t-1. Vous pouvez utiliser ces options pour autoriser les clients de proxy et les agents de relais DHCP à demander une adresse IP pour un sous-réseau spécifique, et à partir d’une plage d’adresses IP et d’une étendue spécifiques.  Pour plus d’informations, consultez **option 82 Sub option 5**: [Sous-option de sélection de lien RFC 3527 pour l’option d’informations de l’agent de relais pour DHCPv4](https://tools.ietf.org/html/rfc3527).

Si vous utilisez un agent de relais DHCP configuré avec l’option DHCP 82, sous-option 5, l’agent de relais peut demander un bail d’adresse IP pour les clients DHCP à partir d’une plage d’adresses IP spécifique.


## <a name="option-82-sub-option-5-link-selection-sub-option"></a>Option 82 Sub 5 : Sous-option de sélection de lien

La sous-option sélection du lien de l’agent de relais permet à un agent de relais DHCP de spécifier un sous-réseau IP à partir duquel le serveur DHCP doit affecter des adresses IP et des options.

En règle générale, les agents de relais DHCP s’appuient sur le champ d’adresse IP de la passerelle \(GIADDR @ no__t-1 pour communiquer avec les serveurs DHCP. Toutefois, GIADDR est limité par ses deux fonctions opérationnelles :

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

Dans ce scénario, un réseau d’organisation comprend un serveur DHCP et un point d’accès sans fil \(AP @ no__t-1 pour les utilisateurs invités. Les adresses IP des clients invités sont attribuées à partir du serveur DHCP de l’organisation. Toutefois, en raison des restrictions de stratégie de pare-feu, le serveur DHCP ne peut pas accéder au réseau sans fil invité ni aux clients sans fil avec des messages broadcase.

Pour résoudre cette restriction, le point d’accès est configuré avec la sous-option 5 de la sélection de lien afin de spécifier le sous-réseau à partir duquel il veut que l’adresse IP soit allouée pour les clients invités, tandis que dans le GIADDR spécifient également l’adresse IP de l’interface interne qui amène à la réseau d’entreprise.
