---
title: Planifier le déploiement de VPN Toujours actif (AlwaysOn)
description: Cette rubrique fournit des instructions de planification pour le déploiement VPN Always On dans Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: 3c9de3ec-4bbd-4db0-b47a-03507a315383
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 11/05/2018
ms.openlocfilehash: db309f451eb9187463f71dfd85a82d214c464e2b
ms.sourcegitcommit: 0948a1abff1c1be506216eeb51ffc6f752a9fe7e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749457"
---
# <a name="step-1-plan-the-always-on-vpn-deployment"></a>Étape 1. Planifiez le déploiement de VPN Toujours actif (AlwaysOn)

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Précédent :** En savoir plus sur le flux de travail pour le déploiement VPN Always On](always-on-vpn-deploy-deployment.md)
- [**prochain :** Étape 2. Configurer l’Infrastructure de serveur](vpn-deploy-server-infrastructure.md)

Dans cette étape, vous commencer à planifier et préparer votre déploiement VPN Always On. Avant d’installer le rôle de serveur d’accès à distance sur l’ordinateur que vous avez l’intention d’utiliser comme un serveur VPN, procédez comme suit. Après une planification appropriée, vous pouvez déployer VPN Toujours actif (AlwaysOn) et éventuellement configurer l’accès conditionnel pour une connectivité VPN à l’aide d’Azure AD.

[!INCLUDE [always-on-vpn-standard-config-considerations-include](../../../includes/always-on-vpn-standard-config-considerations-include.md)]

## <a name="prepare-the-remote-access-server"></a>Préparer le serveur d’accès à distance

Vous devez effectuer les opérations suivantes sur l’ordinateur utilisé comme un serveur VPN :

- **Vérifiez que la configuration logicielle et matérielle du serveur VPN est correcte**. Installez Windows Server 2016 sur l’ordinateur que vous projetez d’utiliser comme un serveur VPN d’accès à distance. Ce serveur doit posséder deux cartes réseau physiques, une pour la connexion au réseau de périmètre externe et une pour vous connecter au réseau de périmètre interne.

- **Identifier la carte réseau qui se connecte à Internet et la carte réseau qui se connecte à votre réseau privé**. Configurez la carte réseau accessible sur Internet avec une adresse IP publique, tandis que l’adaptateur accessible sur l’Intranet peut utiliser une adresse IP à partir du réseau local.

    >[!TIP]
    >Si vous préférez ne pas utiliser une adresse IP publique sur votre réseau de périmètre, vous pouvez configurer le pare-feu de périmètre avec une adresse IP publique, puis configurer le pare-feu pour transférer les demandes de connexion VPN au serveur VPN.

- **Connecter le serveur VPN au réseau**. Installez le serveur VPN sur un réseau de périmètre, entre le pare-feu de périmètre et le pare-feu de périmètre.

## <a name="plan-authentication-methods"></a>Planifier des méthodes d’authentification

IKEv2 est décrit dans protocole de tunnel VPN [Internet Engineering Task Force demande de commentaires 7296](https://datatracker.ietf.org/doc/rfc7296/). Le principal avantage de IKEv2 est qu’il tolère les interruptions de la connexion réseau sous-jacente. Par exemple, si une perte temporaire de connexion ou si un utilisateur déplace un ordinateur client à partir d’un réseau à un autre, lors du rétablissement de la connexion réseau IKEv2 restaure la connexion VPN automatiquement, sans intervention de l’utilisateur.

>[!TIP]
>Vous pouvez configurer le serveur d’accès à distance VPN pour prendre en charge les connexions IKEv2 lors d’également la désactivation des protocoles inutilisés, ce qui réduit l’encombrement de sécurité du serveur. 

## <a name="plan-ip-addresses-for-remote-clients"></a>Planifier des adresses IP pour les Clients distants

Vous pouvez configurer le serveur VPN pour affecter des adresses aux clients VPN à partir d’un pool d’adresses statiques que vous configurez ou à partir d’un serveur DHCP. 

## <a name="prepare-the-environment"></a>Préparer l’environnement

- **Assurez-vous que vous disposez des autorisations pour configurer votre pare-feu externe et que vous disposez d’une adresse IP publique valide**. Ports ouverts sur le pare-feu pour prendre en charge des connexions VPN IKEv2. Vous avez également besoin d’une adresse IP publique à accepter les connexions des clients externes.

- **Choisissez une plage d’adresses IP statiques pour les clients VPN**. Déterminer le nombre maximal de clients VPN simultanées que vous souhaitez prendre en charge. En outre, envisagez une plage d’adresses IP statiques sur le réseau de périmètre interne pour répondre à cette exigence, à savoir le *pool d’adresses statiques*. Si vous utilisez DHCP pour fournir des adresses IP sur le réseau de périmètre interne, vous devrez peut-être également créer une exclusion pour les adresses IP statiques dans DHCP.

- **Assurez-vous que vous pouvez modifier votre zone DNS publique**. Ajouter des enregistrements DNS à votre domaine DNS public pour prendre en charge l’infrastructure VPN. 

- **Assurez-vous que tous les utilisateurs VPN ont des comptes d’utilisateur dans utilisateur Active Directory (AD DS)** . Avant que les utilisateurs peuvent se connecter au réseau avec les connexions VPN, elles doivent avoir des comptes d’utilisateur dans AD DS.

## <a name="prepare-routing-and-firewall"></a>Préparer le routage et de pare-feu 

Installez le serveur VPN à l’intérieur du réseau de périmètre, qui partitionne le réseau de périmètre en réseaux de périmètre internes et externes. Selon votre environnement réseau, vous devrez peut-être apporter plusieurs modifications de routage.

- **(Facultatif) Configurez le réacheminement de port.** Votre pare-feu de périmètre doit ouvrir les ports et le protocole ID associés à un réseau VPN IKEv2 et les transférer vers le serveur VPN. Dans la plupart des environnements, cela requiert donc vous permettent de configurer le réacheminement de port. Rediriger les ports universelle UDP (Datagram Protocol) 500 et 4500 auprès du serveur VPN.

- **Configurer le routage de sorte que les serveurs DNS et les serveurs VPN puissent atteindre Internet**. Ce déploiement utilise IKEv2 et la traduction d’adresses réseau (NAT). Assurez-vous que le serveur VPN peut atteindre tous les réseaux internes requises et les ressources réseau. N’importe quel réseau ou ressource inaccessible à partir du serveur VPN est également inaccessible via des connexions VPN à partir d’emplacements distants.

Dans la plupart des environnements, pour atteindre le nouveau réseau de périmètre interne, ajustez les itinéraires statiques sur le pare-feu de périmètre et le serveur VPN. Toutefois, dans des environnements plus complexes, vous devrez peut-être ajouter des itinéraires statiques vers les routeurs internes ou d’ajuster les règles de pare-feu interne pour le serveur VPN et le bloc d’adresses IP associées aux clients VPN.

## <a name="next-steps"></a>Étapes suivantes

[Étape 2. Configurer l’Infrastructure de serveur](vpn-deploy-server-infrastructure.md): Dans cette étape, vous installez et configurez les composants côté serveur nécessaires pour prendre en charge de la connexion VPN. Les composants côté serveur incluent la configuration d’infrastructure à clé publique pour distribuer les certificats utilisés par les utilisateurs, le serveur VPN et le serveur NPS.