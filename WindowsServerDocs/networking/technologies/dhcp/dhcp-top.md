---
title: Protocole DHCP (Dynamic Host Configuration Protocol)
description: Cette rubrique fournit une brève vue d’ensemble du protocole DHCP (Dynamic Host Configuration Protocol) dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dhcp
ms.topic: article
ms.assetid: 0ff29ef3-c458-4432-9065-e50a7de5b4b9
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c71517bc742cf9eda62cc7d83128f1ab9bd04547
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355400"
---
# <a name="dynamic-host-configuration-protocol-dhcp"></a>Protocole DHCP (Dynamic Host Configuration Protocol)

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour obtenir une vue d’ensemble de DHCP dans Windows Server 2016.

> [!NOTE]
> En plus de cette rubrique, la documentation DHCP suivante est disponible.
>
> - [Nouveautés de DHCP](What-s-New-in-DHCP.md)
> - [Déployer DHCP à l’aide de Windows PowerShell](dhcp-deploy-wps.md)

Le protocole DHCP (Dynamic Host Configuration Protocol) est un protocole client/serveur qui fournit automatiquement un hôte IP (Internet Protocol) avec son adresse IP et d’autres informations de configuration associées, telles que le masque de sous-réseau et la passerelle par défaut. Les RFC 2131 et 2132 définissent DHCP comme norme IETF (Internet Engineering Task Force) basée sur le protocole Bootstrap (BOOTP), un protocole avec lequel DHCP partage de nombreux détails d’implémentation. DHCP permet aux hôtes d’obtenir les informations de configuration TCP/IP requises à partir d’un serveur DHCP.

Windows Server 2016 comprend le serveur DHCP, qui est un rôle de serveur de mise en réseau facultatif que vous pouvez déployer sur votre réseau pour louer des adresses IP et d’autres informations aux clients DHCP. Tous les systèmes d’exploitation clients Windows incluent le client DHCP dans le cadre du protocole TCP/IP, et le client DHCP est activé par défaut.

## <a name="why-use-dhcp"></a>Pourquoi utiliser DHCP ?

Chaque appareil sur un réseau TCP/IP doit avoir une adresse IP monodiffusion unique pour accéder au réseau et à ses ressources. Sans DHCP, les adresses IP des nouveaux ordinateurs ou des ordinateurs qui sont déplacés d’un sous-réseau vers un autre doivent être configurées manuellement. Les adresses IP des ordinateurs qui sont supprimés du réseau doivent être récupérées manuellement.

Avec DHCP, l’ensemble de ce processus est automatisé et géré de manière centralisée. Le serveur DHCP gère un pool d’adresses IP et loue une adresse à n’importe quel client prenant en charge DHCP lorsqu’il démarre sur le réseau. Étant donné que les adresses IP sont dynamiques (louées) et non statiques (affectées de manière permanente), les adresses qui ne sont plus utilisées sont automatiquement retournées au pool en vue de leur réallocation.

L’administrateur réseau établit les serveurs DHCP qui maintiennent les informations de configuration TCP/IP et fournissent la configuration d’adresse aux clients DHCP sous la forme d’une offre de bail. Le serveur DHCP stocke les informations de configuration dans une base de données qui comprend :

- Paramètres de configuration TCP/IP valides pour tous les clients sur le réseau.

- Adresses IP valides, conservées dans un pool pour être affectées aux clients, ainsi qu’à des adresses exclues.

- IP réservée des adresses associées à des clients DHCP particuliers. Cela permet d’attribuer de manière cohérente une seule adresse IP à un client DHCP unique.

- Durée du bail, ou durée pendant laquelle l’adresse IP peut être utilisée avant le renouvellement d’un bail.

Un client prenant en charge DHCP, lors de l’acceptation d’une offre de bail, reçoit :

- Une adresse IP valide pour le sous-réseau auquel elle se connecte.  
  
- Les options DHCP demandées, qui sont des paramètres supplémentaires qu’un serveur DHCP est configuré pour affecter aux clients. Voici quelques exemples d’options DHCP : routeur (passerelle par défaut), serveurs DNS et nom de domaine DNS.

## <a name="benefits-of-dhcp"></a>Avantages de DHCP

DHCP offre les avantages suivants.

- **Configuration d’adresse IP fiable**. DHCP minimise les erreurs de configuration provoquées par la configuration manuelle des adresses IP, telles que les erreurs typographiques, ou les conflits d’adresses causés par l’attribution d’une adresse IP à plusieurs ordinateurs en même temps.

- **Administration réseau réduite**. DHCP comprend les fonctionnalités suivantes pour réduire l’administration du réseau :

    - Configuration TCP/IP centralisée et automatisée.

    - La possibilité de définir des configurations TCP/IP à partir d’un emplacement central.

    - La possibilité d’attribuer une plage complète de valeurs de configuration TCP/IP supplémentaires à l’aide des options DHCP.

    - Le traitement efficace des modifications d’adresse IP pour les clients qui doivent être mis à jour fréquemment, tels que ceux des appareils portables qui se déplacent vers différents emplacements sur un réseau sans fil.

    - Transfert des messages DHCP initiaux à l’aide d’un agent de relais DHCP, qui élimine la nécessité d’un serveur DHCP sur chaque sous-réseau.

