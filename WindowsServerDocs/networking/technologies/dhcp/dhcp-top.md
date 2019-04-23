---
title: Protocole DHCP (Dynamic Host Configuration Protocol)
description: Cette rubrique fournit une vue d’ensemble de DHCP Dynamic Host Configuration Protocol () dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dhcp
ms.topic: article
ms.assetid: 0ff29ef3-c458-4432-9065-e50a7de5b4b9
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 08b07e902486ae633b30949270e15f8bf94afaaf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857490"
---
# <a name="dynamic-host-configuration-protocol-dhcp"></a>Protocole DHCP (Dynamic Host Configuration Protocol)

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour une vue d’ensemble du protocole DHCP dans Windows Server 2016.

>[!NOTE]
>Outre cette rubrique, la documentation de DHCP suivante est disponible.
>
>- [Quelles sont les nouveautés dans DHCP](What-s-New-in-DHCP.md)
>- [Déployer DHCP à l’aide de Windows PowerShell](dhcp-deploy-wps.md)

Configuration de protocole DHCP (Dynamic Host) est un protocole client/serveur qui fournit automatiquement un hôte IP (Internet Protocol) avec son adresse IP et d’autres informations de configuration connexes telles que la passerelle par défaut et le masque de sous-réseau. RFC 2131 et 2132 définissent le protocole DHCP en tant qu’un Internet Engineering Task Force (IETF) standard basé sur protocole Bootstrap (BOOTP), un protocole avec lequel DHCP partage de nombreux détails d’implémentation. DHCP permet aux hôtes d’obtenir des informations de configuration TCP/IP requises à partir d’un serveur DHCP.

Windows Server 2016 inclut un serveur DHCP, qui est un rôle de serveur de mise en réseau facultatif que vous pouvez déployer sur votre réseau à louent des adresses IP et d’autres informations aux clients DHCP. Tous les systèmes d’exploitation de clients basés sur Windows incluent le client DHCP dans le cadre de TCP/IP et client DHCP est activé par défaut.

## <a name="why-use-dhcp"></a>Pourquoi utiliser DHCP ?

Chaque appareil sur un réseau basé sur IP doit avoir une adresse IP de monodiffusion unique pour accéder au réseau et ses ressources. Sans DHCP, des adresses IP pour les nouveaux ordinateurs ou des ordinateurs qui sont déplacés d’un sous-réseau à l’autre doivent être configurés manuellement. Adresses IP pour les ordinateurs qui sont supprimés à partir du réseau doivent être récupérés manuellement.

Avec DHCP, ensemble de ce processus est automatisé et gérée de manière centralisée. Le serveur DHCP gère un pool d’adresses IP et loue une adresse à n’importe quel client avec DHCP activé lorsqu’il démarre sur le réseau. Étant donné que les adresses IP sont dynamiques (loué) au lieu de statique (assigné de façon permanente), adresses n’est plus en cours d’utilisation sont automatiquement restitués au pool de réattribution.

L’administrateur réseau établit les serveurs DHCP qui gèrent les informations de configuration TCP/IP et fournissent la configuration d’adresse pour les clients DHCP sous la forme d’une offre de bail. Le serveur DHCP stocke les informations de configuration dans une base de données qui inclut :

- Paramètres de configuration TCP/IP valides pour tous les clients sur le réseau.

- Les adresses IP valides, conservées dans un pool pour l’attribution de clients, ainsi que les adresses exclues.

- Adresses IP réservées associées à des clients DHCP particuliers. Ainsi, cohérente attribution d’une adresse IP unique à un seul client DHCP.

- La durée du bail, ou la longueur de temps pour lequel l’adresse IP peut être utilisé avant un renouvellement de bail est requis.

Un client avec DHCP activé, lorsqu’il accepte une offre de bail, reçoit :

- Une adresse IP valide pour le sous-réseau auquel il se connecte.  
  
- Demandé des options DHCP, qui sont des paramètres supplémentaires pour un serveur DHCP est configuré pour affecter aux clients. Quelques exemples d’options DHCP sont routeur (passerelle par défaut), les serveurs DNS et nom de domaine DNS.

## <a name="benefits-of-dhcp"></a>Avantages du protocole DHCP

DHCP offre les avantages suivants.

- **Configuration d’adresse IP fiable**. DHCP réduit les erreurs de configuration provoquées par la configuration d’adresse IP manuelle, telles que des erreurs typographiques, ou résoudre les conflits provoqués par l’attribution d’une adresse IP à plusieurs ordinateurs en même temps.

- **Réduit l’administration du réseau**. DHCP inclut les fonctionnalités suivantes pour réduire l’administration du réseau :

    - Configuration TCP/IP centralisée et automatisée.

    - La possibilité de définir des configurations TCP/IP à partir d’un emplacement central.

    - La possibilité d’affecter une gamme complète de valeurs de configuration TCP/IP supplémentaires au moyen des options DHCP.

    - La gestion efficace des modifications d’adresses IP pour les clients qui doivent être mis à jour fréquemment, telles que celles pour les périphériques portables qui passent à différents emplacements sur un réseau sans fil.

    - Le transfert des messages DHCP initiaux à l’aide d’un agent de relais DHCP, ce qui élimine la nécessité d’un serveur DHCP sur chaque sous-réseau.

