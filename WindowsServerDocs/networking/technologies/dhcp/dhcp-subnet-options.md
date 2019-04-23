---
title: Options de sélection de sous-réseau DHCP
description: Cette rubrique fournit des informations sur les options de sélection de sous-réseau DHCP pour DHCP Dynamic Host Configuration Protocol () dans Windows Server 2016.
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-dhcp
ms.topic: get-started-article
ms.assetid: ca19e7d1-e445-48fc-8cf5-e4c45f561607
ms.author: pashort
author: shortpatti
ms.date: 08/17/2018
ms.openlocfilehash: 034ca48ef13a6bdac63ca99ac753fc9826460922
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870810"
---
# <a name="dhcp-subnet-selection-options"></a>Options de sélection de sous-réseau DHCP

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour plus d’informations sur les nouvelles options de sélection de sous-réseau DHCP.

DHCP maintenant prend en charge l’option 82 \(sous-composant option 5\). Vous pouvez utiliser ces options pour autoriser les clients de proxy DHCP et les agents de relais demander une adresse IP d’un sous-réseau spécifique et d’une plage d’adresses IP et de portée.  Pour plus d’informations, consultez **Option 82 Sub Option 5**: [Option de sélection de lien de RFC 3527 secondaires pour l’Option informations d’Agent relais pour DHCPv4](https://tools.ietf.org/html/rfc3527).

Si vous utilisez un agent de relais DHCP qui est configuré avec l’option DHCP 82, sous-composant option 5, l’agent de relais peut demander un bail d’adresse IP pour les clients DHCP à partir d’une plage d’adresses IP spécifique.


## <a name="option-82-sub-option-5-link-selection-sub-option"></a>Option 82 sous-option 5 : Sélection de lien sous-option

sous-L’option de sélection de lien de l’Agent de relais permet à un Agent de relais DHCP spécifier un sous-réseau IP à partir de laquelle le serveur DHCP doit affecter des adresses IP et les options.

En règle générale, les agents de relais DHCP s’appuient sur l’adresse IP de passerelle \(GIADDR\) champ pour communiquer avec les serveurs DHCP. Toutefois, GIADDR est limité par ses fonctions opérationnelles deux :

1. Pour informer le serveur DHCP sur le sous-réseau sur lequel réside le client DHCP qui demande le bail d’adresse IP.
2. Pour informer le serveur DHCP de l’adresse IP à utiliser pour communiquer avec l’agent de relais.

Dans certains cas, l’adresse IP par l’agent de relais pour communiquer avec le serveur DHCP peut être différente de celle de la plage d’adresses IP à partir de laquelle l’adresse IP du client DHCP doit être affectée. 

L’option Sub de sélection de lien d’option 82 est utile dans ce cas, ce qui permet de déclarer explicitement le sous-réseau à partir de laquelle il veut l’adresse IP allouée dans le formulaire de l’option DHCP v4 sous-option 82 5, l’agent de relais.

> [!NOTE]
>
> Toutes les adresses IP de l’agent relais (GIADDR) doivent faire partie d’une plage d’adresses IP d’étendue DHCP active. N’importe quel GIADDR en dehors des plages d’adresses IP DHCP étendue est considéré comme un relais non fiables et serveur DHCP de Windows ne reconnaît pas les demandes du client à partir de ces agents de relais DHCP.
>
> Une portée spéciale peut être créée pour les agents de relais « autoriser ». Créer une étendue avec le GIADDR (ou plusieurs si le GIADDR est des adresses IP séquentiels), exclure les adresses GIADDR de la distribution, puis activer l’étendue. Cela autorise les agents de relais tout en empêchant les adresses GIADDR leur soient assignés.


### <a name="use-case-scenario"></a>Scénario d’utilisation

Dans ce scénario, un réseau d’entreprise inclut un serveur DHCP et un Point d’accès sans fil \(AP\) pour les utilisateurs invités. Adresses IP des clients invités sont affectées à partir du serveur DHCP d’organisation : Toutefois, en raison de restrictions de stratégie de pare-feu, le serveur DHCP ne peut pas accéder au réseau sans fil invité ou les clients sans fil avec les messages pendant.

Pour résoudre cette restriction, le point d’accès est configuré avec l’Option Sub lien sélection 5 pour spécifier le sous-réseau à partir de laquelle il veut l’adresse IP allouée pour les clients de l’invité, tandis que dans le GIADDR également en spécifiant l’adresse IP de l’interface interne qui mène à la réseau d’entreprise.
