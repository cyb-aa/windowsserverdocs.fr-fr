---
title: Planifier le déploiement de VPN Toujours actif (AlwaysOn)
description: Cette rubrique fournit des instructions de planification pour le déploiement de Always On VPN dans Windows Server 2016.
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: 3c9de3ec-4bbd-4db0-b47a-03507a315383
ms.localizationpriority: medium
ms.author: v-tea
author: Teresa-MOTIV
ms.date: 11/05/2018
ms.openlocfilehash: c1e85f2ee44d241bdc04e63d20de36e5cdeafb1a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80814492"
---
# <a name="step-1-plan-the-always-on-vpn-deployment"></a>Étape 1. Planifiez le déploiement de VPN Toujours actif (AlwaysOn)

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Précédent :** En savoir plus sur le flux de travail pour le déploiement de Always On VPN](always-on-vpn-deploy-deployment.md)
- [**Ensuite :** Étape 2. Configurer l’infrastructure de serveur](vpn-deploy-server-infrastructure.md)

Au cours de cette étape, vous allez commencer à planifier et à préparer votre déploiement Always On VPN. Avant d’installer le rôle serveur d’accès à distance sur l’ordinateur sur lequel vous envisagez d’utiliser en tant que serveur VPN, effectuez les tâches suivantes. Après une planification appropriée, vous pouvez déployer VPN Toujours actif (AlwaysOn) et éventuellement configurer l’accès conditionnel pour une connectivité VPN à l’aide d’Azure AD.

[!INCLUDE [always-on-vpn-standard-config-considerations-include](../../../includes/always-on-vpn-standard-config-considerations-include.md)]

## <a name="prepare-the-remote-access-server"></a>Préparer le serveur d’accès à distance

Vous devez effectuer les opérations suivantes sur l’ordinateur utilisé en tant que serveur VPN :

- Assurez-vous que **la configuration matérielle et logicielle du serveur VPN est correcte**. Installez Windows Server 2016 sur l’ordinateur que vous prévoyez d’utiliser en tant que serveur VPN d’accès à distance. Deux cartes réseau physiques doivent être installées sur ce serveur, une pour se connecter au réseau de périmètre externe et une pour se connecter au réseau de périmètre interne.

- **Identifiez la carte réseau qui se connecte à Internet et la carte réseau qui se connecte à votre réseau privé**. Configurez la carte réseau connectée à Internet avec une adresse IP publique, tandis que la carte de l’intranet peut utiliser une adresse IP du réseau local.

    >[!TIP]
    >Si vous préférez ne pas utiliser une adresse IP publique sur votre réseau de périmètre, vous pouvez configurer le pare-feu de périmètre avec une adresse IP publique, puis configurer le pare-feu pour transférer les demandes de connexion VPN au serveur VPN.

- **Connectez le serveur VPN au réseau**. Installez le serveur VPN sur un réseau de périmètre, entre le pare-feu de périphérie et le pare-feu de périmètre.

## <a name="plan-authentication-methods"></a>Planifier les méthodes d’authentification

IKEv2 est un protocole de tunneling VPN décrit dans [Internet Engineering Task Force Request for comments 7296](https://datatracker.ietf.org/doc/rfc7296/). Le principal avantage de IKEv2 est qu’il tolère les interruptions de la connexion réseau sous-jacente. Par exemple, en cas de perte temporaire de connexion ou si un utilisateur déplace un ordinateur client d’un réseau à un autre, lors de la rétablit la connexion réseau, IKEv2 restaure automatiquement la connexion VPN, sans intervention de l’utilisateur.

>[!TIP]
>Vous pouvez configurer le serveur VPN d’accès à distance pour prendre en charge les connexions IKEv2 tout en désactivant les protocoles inutilisés, ce qui réduit l’encombrement de sécurité du serveur. 

## <a name="plan-ip-addresses-for-remote-clients"></a>Planifier des adresses IP pour les clients distants

Vous pouvez configurer le serveur VPN pour affecter des adresses aux clients VPN à partir d’un pool d’adresses statiques que vous configurez ou des adresses IP à partir d’un serveur DHCP. 

## <a name="prepare-the-environment"></a>Préparer l’environnement

- Vérifiez **que vous disposez des autorisations nécessaires pour configurer votre pare-feu externe et que vous disposez d’une adresse IP publique valide**. Ouvrez les ports sur le pare-feu pour prendre en charge les connexions VPN IKEv2. Vous avez également besoin d’une adresse IP publique pour accepter les connexions des clients externes.

- **Choisissez une plage d’adresses IP statiques pour les clients VPN**. Déterminez le nombre maximal de clients VPN simultanés que vous souhaitez prendre en charge. Planifiez également une plage d’adresses IP statiques sur le réseau de périmètre interne pour répondre à cette exigence, à savoir le *pool d’adresses statiques*. Si vous utilisez DHCP pour fournir des adresses IP sur le réseau DMZ interne, vous devrez peut-être également créer une exclusion pour ces adresses IP statiques dans DHCP.

- Assurez- **vous que vous pouvez modifier votre zone DNS publique**. Ajoutez des enregistrements DNS à votre domaine DNS public pour prendre en charge l’infrastructure VPN. 

- Assurez-vous **que tous les utilisateurs VPN disposent de comptes d’utilisateur dans Active Directory utilisateur (AD DS)** . Pour que les utilisateurs puissent se connecter au réseau avec des connexions VPN, ils doivent disposer de comptes d’utilisateur dans AD DS.

## <a name="prepare-routing-and-firewall"></a>Préparer le routage et le pare-feu 

Installez le serveur VPN à l’intérieur du réseau de périmètre, qui partitionne le réseau de périmètre en réseaux de périmètre internes et externes. Selon votre environnement réseau, vous devrez peut-être effectuer plusieurs modifications de routage.

- **Facultatif Configurez le réacheminement de port.** Votre pare-feu de périmètre doit ouvrir les ports et les ID de protocole associés à un VPN IKEv2 et les transférer au serveur VPN. Dans la plupart des environnements, vous devez configurer le réacheminement de port. Redirigez les ports UDP 500 et 4500 vers le serveur VPN.

- **Configurez le routage de manière à ce que les serveurs DNS et les serveurs VPN puissent accéder à Internet**. Ce déploiement utilise le protocole IKEv2 et la traduction d’adresses réseau (NAT). Assurez-vous que le serveur VPN peut accéder à tous les réseaux internes et ressources réseau requis. Les réseaux ou ressources inaccessibles à partir du serveur VPN sont également inaccessibles aux connexions VPN à partir d’emplacements distants.

Dans la plupart des environnements, pour atteindre le nouveau réseau de périmètre interne, ajustez les itinéraires statiques sur le pare-feu de périmètre et le serveur VPN. Toutefois, dans des environnements plus complexes, vous devrez peut-être ajouter des itinéraires statiques aux routeurs internes ou ajuster les règles de pare-feu internes pour le serveur VPN et le bloc d’adresses IP associées aux clients VPN.

## <a name="next-steps"></a>Étapes suivantes :

[Étape 2. Configuration de l’infrastructure de serveur](vpn-deploy-server-infrastructure.md): au cours de cette étape, vous installez et configurez les composants côté serveur nécessaires pour prendre en charge le VPN. Les composants côté serveur incluent la configuration de l’infrastructure à clé publique pour distribuer les certificats utilisés par les utilisateurs, le serveur VPN et le serveur NPS.