---
title: Dynamic Host Configuration Protocol (DHCP)
description: Cette rubrique fournit une vue d’ensemble de DHCP Dynamic Host Configuration Protocol () dans Windows Server2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dhcp
ms.topic: article
ms.assetid: 0ff29ef3-c458-4432-9065-e50a7de5b4b9
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 770cfd78c7b9a0e122bd9f9936623a73b56af808
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="dynamic-host-configuration-protocol-dhcp"></a>Dynamic Host Configuration Protocol (DHCP)

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette rubrique pour une vue d’ensemble du protocole DHCP dans Windows Server2016.

>[!NOTE]
>Outre cette rubrique, la documentation de DHCP suivante est disponible.
>
>- [Quelles sont les nouveautés dans DHCP](What-s-New-in-DHCP.md)
>- [Déployer le protocole DHCP à l’aide de Windows PowerShell](dhcp-deploy-wps.md)

DHCP Dynamic Host Configuration Protocol () est un protocole client/serveur qui fournit automatiquement un hôte IP (Internet Protocol) avec son adresse IP et d’autres informations de configuration associées telles que la passerelle par défaut et du masque de sous-réseau. RFC2131 et 2132 définissent DHCP comme un IETF Internet Engineering Task Force () standard basé sur protocole Bootstrap (BOOTP), un protocole avec lequel DHCP partages de nombreux détails d’implémentation. DHCP permet aux hôtes d’obtenir des informations de configuration TCP/IP requises à partir d’un serveur DHCP.

Windows Server2016 inclut un serveur DHCP, qui est un rôle de serveur de mise en réseau facultatives que vous pouvez déployer sur votre réseau pour les adresses IP de bail et d’autres informations sur les clients DHCP. Tous les systèmes d’exploitation de client basé sur Windows incluent le client DHCP dans le cadre de TCP/IP et client DHCP est activé par défaut.

## <a name="why-use-dhcp"></a>Pourquoi utiliser DHCP?

Chaque appareil sur un réseau basé sur TCP doit disposer d’une adresse IP de monodiffusion unique pour accéder au réseau et ses ressources. Sans le protocole DHCP, des adresses IP pour les nouveaux ordinateurs ou les ordinateurs qui sont déplacés d’un sous-réseau à un autre doivent être configurés manuellement. Les adresses IP pour les ordinateurs qui sont supprimés à partir du réseau doivent être récupérés manuellement.

Avec le protocole DHCP, ce processus est automatisé et centralisée. Le serveur DHCP gère un pool d’adresses IP et alloue une adresse à n’importe quel client DHCP activé lorsqu’il démarre sur le réseau. Étant donné que les adresses IP sont dynamiques (allouée) plutôt que statique (attribuée définitivement), adresses n’est plus en cours d’utilisation sont automatiquement retournés au pool pour réaffectation.

L’administrateur réseau établit les serveurs DHCP qui mettre à jour les informations de configuration TCP/IP et fournissent la configuration d’adresse pour les clients DHCP sous la forme d’une offre de bail. Le serveur DHCP stocke les informations de configuration dans une base de données qui inclut:

- Paramètres de configuration TCP/IP valides pour tous les clients sur le réseau.

- Les adresses IP valides, mis à jour dans un pool pour être affectées aux clients, ainsi que les adresses exclues.

- Adresses IP réservées associées aux clients DHCP spécifiques. Cela permet de cohérence attribution d’une adresse IP unique à un seul client DHCP.

- La durée du bail, ou la durée de temps pour lequel l’adresse IP peut être utilisé avant un renouvellement de bail est requis.

Un client DHCP activé, lorsqu’il accepte l’offre de bail, reçoit:

- Une adresse IP valide pour le sous-réseau sur lequel il se connecte.  
  
- Demandé des options DHCP, qui sont des paramètres supplémentaires qu’un serveur DHCP est configuré pour attribuer aux clients. Quelques exemples d’options DHCP sont routeur (passerelle par défaut), les serveurs DNS et nom de domaine DNS.

## <a name="benefits-of-dhcp"></a>Avantages du protocole DHCP

DHCP offre les avantages suivants.

- **Configuration de l’adresse IP fiable**. DHCP réduit dus à la configuration d’adresse IP manuelle, tels que des erreurs typographiques, des erreurs de configuration ou de résoudre les conflits dus à l’attribution d’une adresse IP pour plusieurs ordinateurs en même temps.

- **Réduit l’administration réseau**. DHCP inclut les fonctionnalités suivantes afin de réduire l’administration du réseau:

    - Configuration TCP/IP centralisée et automatisée.

    - La possibilité de définir les configurations TCP/IP à partir d’un emplacement central.

    - Possibilité d’attribuer une gamme complète d’autres valeurs de configuration TCP/IP au moyen des options DHCP.

    - La gestion efficace des modifications d’adresses IP pour les clients qui doivent être mis à jour fréquemment, telles que celles pour les périphériques mobiles qui passent à différents emplacements sur un réseau sans fil.

    - Le transfert des messages DHCP initiaux à l’aide d’un agent de relais DHCP, qui élimine le besoin d’un serveur DHCP sur chaque sous-réseau.

