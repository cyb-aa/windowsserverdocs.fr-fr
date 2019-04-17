---
title: Options de sélection de sous-réseau DHCP
description: Cette rubrique fournit des informations sur les options de sélection de sous-réseau DHCP pour DHCP Dynamic Host Configuration Protocol () dans Windows Server2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dhcp
ms.topic: get-started-article
ms.assetid: ca19e7d1-e445-48fc-8cf5-e4c45f561607
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 43bc3d165f895767ded921b41118ecaccf9734e8
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="dhcp-subnet-selection-options"></a>Options de sélection de sous-réseau DHCP

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette rubrique pour plus d’informations sur les nouvelles options de sélection de sous-réseau DHCP.

DHCP prend désormais en charge les options 118 et 82 \(sub-option 5\). Vous pouvez utiliser ces options pour autoriser les clients du proxy DHCP et les agents de relais demander une adresse IP d’un sous-réseau spécifique et à partir d’une plage d’adresses IP et l’étendue.

Si vous utilisez un client du proxy DHCP configuré avec l’option DHCP 118, tel qu’un serveur de réseau privé virtuel (VPN) qui exécute Windows Server2016 et le rôle de serveur d’accès à distance, le serveur VPN peut demander des baux d’adresses IP pour les clients VPN à partir d’une plage d’adresses IP spécifique.

Si vous utilisez un agent de relais DHCP qui est configuré avec l’option DHCP 82, inférieur option 5, l’agent de relais peut demander un bail d’adresse IP pour les clients DHCP à partir d’une plage d’adresses IP spécifique.

Voici des liens vers la demande les rubriques de commentaires pour ces options.

- **Option 118**: [Option de sélection de sous-réseau RFC3011 IPv4 DHCP](http://www.rfc-base.org/rfc-3011.html)
- **Option 82 Sub Option 5**: [RFC3527 lien sélection sous-section option pour l’Option d’informations de l’Agent relais DHCPv4](https://tools.ietf.org/html/rfc3527)


## <a name="dhcp-option-118-client-subnet-and-relay-agent-link-selection"></a>DHCP Option 118: Sous-réseau de Client et le lien-sélection de l’Agent de relais

L’option de sélection de sous-réseau DHCP fournit un mécanisme pour les serveurs proxy DHCP spécifier un sous-réseau IP à partir de laquelle le serveur DHCP doit attribuer des adresses IP et les options.

### <a name="use-case-scenario"></a>Scénario d’utilisation

Dans ce scénario, un serveur \(VPN\) de réseau privé virtuel alloue des adresses IP aux clients VPN. 

Dans ce cas, le serveur VPN est connecté à intranet où le serveur DHCP est installé et à Internet, afin que les clients VPN peuvent se connecter au serveur VPN à partir d’emplacements distants.

Avant que le serveur VPN peut fournir des baux d’adresses IP aux clients VPN, le serveur contacte le serveur DHCP sur un sous-réseau interne et un bloc d’adresses IP les réserves de ressources. Le serveur VPN puis gère les adresses IP qu'il obtenu à partir du serveur DHCP. Si le serveur VPN fournit toutes les adresses IP réservées dans des baux aux clients VPN, le serveur VPN obtienne alors des adresses IP supplémentaires à partir du serveur DHCP.

En configurant le serveur VPN avec l’option DHCP 118, vous pouvez spécifier la plage d’adresses IP et l’étendue que vous souhaitez utiliser pour les clients VPN. Après avoir configuré l’option 118, le serveur VPN demande des adresses IP à partir d’une plage d’adresses IP et l’étendue du serveur DHCP.

### <a name="the-dhcp-subnet-selection-option-field"></a>Le champ d’option DHCP sous-réseau sélection

Le champ d’option DHCP sous-réseau sélection contient un seul IPv4 adresse utilisée pour représenter l’adresse de sous-réseau origine pour une demande de bail DHCP.  Un serveur DHCP qui est configuré pour répondre à cette option alloue l’adresse à partir du:

1. Le sous-réseau qui est spécifié dans l’option de sélection de sous-réseau.
2. Un sous-réseau qui se trouve sur le même segment réseau que le sous-réseau qui est spécifié dans l’option de sélection de sous-réseau.

## <a name="option-82-sub-option-5-link-selection-sub-option"></a>Option 82 Sub Option 5: Lien sélection Sub Option

La sélection de lien de l’Agent de relais sous-section permet un Agent de relais DHCP spécifier un sous-réseau IP à partir de laquelle le serveur DHCP doit attribuer des adresses IP et les options.

En règle générale, les agents de relais DHCP s’appuient sur le champ de l’adresse IP de passerelle \(GIADDR\) pour communiquer avec les serveurs DHCP. Toutefois, GIADDR est limitée par ses fonctions opérationnelles deux:

1. Pour informer le serveur DHCP sur le sous-réseau sur lequel réside le client DHCP qui demande le bail d’adresse IP.
2. Pour informer le serveur DHCP de l’adresse IP à utiliser pour communiquer avec l’agent de relais.

Dans certains cas, l’adresse IP que l’agent de relais utilise pour communiquer avec le serveur DHCP peut être différente de celle de la plage d’adresses IP à partir de laquelle l’adresse IP du client DHCP doit être affectée. 

Les agents de relais DHCP ne peut pas rendre l’utilisation de l’option 118, comme leurs fonctionnalités sont limitée et peut uniquement d’écriture pour le champ GIADDR ou l’Option de plus de l’Agent de relais \(option 82\). 

L’option Sub de sélection de lien d’option 82 est utile dans ce cas, ce qui permet de l’agent de relais spécifier explicitement le sous-réseau à partir de laquelle il veut que l’adresse IP allouée dans l’écran de l’option DHCP v4 option sub 82 5.

### <a name="use-case-scenario"></a>Scénario d’utilisation

Dans ce scénario, un réseau d’entreprise inclut un serveur DHCP et un Point d’accès sans fil \(AP\) pour les utilisateurs invités. Invités les adresses IP sont affectées à partir du serveur DHCP d’organisation - Toutefois, en raison de restrictions de stratégie de pare-feu, le serveur DHCP ne peut pas accéder au réseau sans fil invité ou les clients sans fil avec des messages pendant.

Pour résoudre cette restriction, le point d’accès est configuré avec l’Option Sub lien sélection 5 pour spécifier le sous-réseau à partir de laquelle il veut que l’adresse IP allouée pour les clients de l’invité, tandis que dans le GIADDR également en spécifiant l’adresse IP de l’interface interne qui conduit au réseau d’entreprise.
