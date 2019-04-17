---
title: Planifier le déploiement de VPN Toujours actif (AlwaysOn)
description: Cette rubrique fournit des instructions de planification pour le déploiement de toujours actif (AlwaysOn) dans Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: 3c9de3ec-4bbd-4db0-b47a-03507a315383
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 11/05/2018
ms.openlocfilehash: d629f04abda0ce22deb75e487f5b485f50a60a53
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067131"
---
# Étape1. Planifier le déploiement de toujours actif (AlwaysOn)

>S’applique à: Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows 10


& #171;  [ **Précédent:** obtenir des informations sur le flux de travail pour le déploiement de toujours actif (AlwaysOn)](always-on-vpn-deploy-deployment.md)<br>
& #187;  [ **Suivant:** étape 2. Configurer l’Infrastructure de serveur](vpn-deploy-server-infrastructure.md)

Dans cette étape, vous commencez à planifier et préparer votre déploiement toujours actif (AlwaysOn). Avant d’installer le rôle de serveur d’accès à distance sur l’ordinateur que vous avez l’intention d’utiliser comme serveur VPN, effectuez les tâches suivantes. Après une planification appropriée, vous pouvez déployer VPN Toujours actif (AlwaysOn) et éventuellement configurer l’accès conditionnel pour une connectivité VPN à l’aide d’Azure AD. 

[!INCLUDE [always-on-vpn-standard-config-considerations-include](../../../includes/always-on-vpn-standard-config-considerations-include.md)]


## Préparer le serveur d’accès à distance

Vous devez effectuer les opérations suivantes sur l’ordinateur utilisé comme serveur VPN: 

- **Assurez-vous que le logiciel du serveur VPN et la configuration matérielle est correcte**. Installez Windows Server 2016 sur l’ordinateur que vous prévoyez d’utiliser comme serveur VPN accès à distance. Ce serveur doit disposer de deux cartes réseau physiques, un pour vous connecter au réseau de périmètre externe et un pour vous connecter au réseau de périmètre interne.

- **Identifier la carte réseau qui connecte à Internet et la carte réseau qui se connecte à votre privé réseau**. Configurer la carte réseau exposée à Internet avec une adresse IP publique, tandis que la carte face à l’Intranet peut utiliser une adresse IP à partir du réseau local.

    >[!TIP]
    >Si vous préférez ne pas utiliser une adresse IP publique sur votre réseau de périmètre, vous pouvez configurer le pare-feu de périmètre avec une adresse IP publique et ensuite configurer le pare-feu pour transmettre les demandes de connexion VPN vers le serveur VPN.

- **Connectez le serveur VPN au réseau**. Installer le serveur VPN sur un réseau de périmètre entre le pare-feu de périmètre et le pare-feu de périmètre.

## Planifier des méthodes d’authentification

IKEv2 est décrit dans [Internet Engineering Task Force Request pour Comments7296](https://datatracker.ietf.org/doc/rfc7296/)de protocole de tunnel VPN. Le principal avantage de IKEv2 est qu’il tolère d’interruption de la connexion réseau sous-jacente. Par exemple, si une perte de connexion temporaire ou si un utilisateur déplace d’un ordinateur client à partir d’un réseau à l’autre, lorsque le rétablissement de la connexion réseau IKEv2 restaure la connexion VPN automatiquement, sans intervention de l’utilisateur.

>[!TIP]
>Vous pouvez configurer le serveur d’accès à distance VPN pour prendre en charge les connexions de protocole IKEv2 lors de la désactivation également les protocoles non utilisés, ce qui réduit l’encombrement de sécurité du serveur. 

## Planifier des adresses IP pour les Clients distants

Vous pouvez configurer le serveur VPN pour affecter des adresses aux clients VPN à partir d’un pool d’adresses statique que vous configurez ou les adresses IP à partir d’un serveur DHCP. 

## Préparer l’environnement

- **Assurez-vous que vous disposez des autorisations pour configurer votre pare-feu externe et que vous disposez d’une adresse IP publique valide**. Ports ouverts sur le pare-feu pour prendre en charge les connexions VPN IKEv2. Vous devez également une adresse IP publique à accepter les connexions à partir de clients externes.

- **Choisir une plage d’adresses IP statiques pour les clients VPN**. Déterminer le nombre maximal de clients VPN simultanées que vous souhaitez prendre en charge. En outre, planifier une plage d’adresses IP statiques sur le réseau de périmètre interne pour répondre à cette exigence, à savoir le *pool d’adresses statique*. Si vous utilisez DHCP pour fournir des adresses IP sur la DMZ interne, vous devrez également créer un élément d’exclusion pour les adresses IP statiques dans DHCP.

- **Assurez-vous que vous pouvez modifier votre zone DNS public**. Ajouter des enregistrements DNS à votre domaine DNS public pour prendre en charge de l’infrastructure VPN. 

- **Assurez-vous que tous les utilisateurs VPN ont des comptes d’utilisateur dans \(AD DS\) utilisateur Active Directory**. Avant que les utilisateurs peuvent se connecter au réseau avec des connexions VPN, ils doivent avoir des comptes d’utilisateurs dans ADDS.

## Préparer le routage et le pare-feu 

Installer le serveur VPN à l’intérieur du réseau de périmètre, ce qui partitions le réseau de périmètre dans les réseaux de périmètre internes et externes. En fonction de votre environnement réseau, vous devrez peut-être apporter plusieurs modifications de routage.

- **Réacheminement de port \(Optional\) configurer.** Votre pare-feu de périmètre doit ouvrir les ports et le protocole ID sont associés à un VPN IKEv2 et les transmet au serveur VPN. Dans la plupart des environnements, cela requiert vous permettent de configurer réacheminement de port. Rediriger ports universelle UDP (Datagram Protocol) 500 et 4500 vers le serveur VPN.

- **Configurer le routage afin que les serveurs DNS et les serveurs VPN peuvent accéder à Internet**. Ce déploiement utilise IKEv2 et la traduction d’adresses réseau \(NAT\). Assurez-vous que le serveur VPN peut atteindre tous les réseaux internes requis et les ressources réseau. N’importe quel réseau ou une ressource inaccessible à partir du serveur VPN est également inaccessible sur des connexions VPN à partir d’emplacements distants.

Dans la plupart des environnements, pour atteindre le nouveau réseau de périmètre interne, ajustez les itinéraires statiques sur le pare-feu de périmètre et le serveur VPN. Dans les environnements plus complexes, toutefois, vous devrez peut-être ajouter des itinéraires statiques à routeurs internes ou d’ajuster des règles de pare-feu interne pour le serveur VPN et le bloc d’adresses IP associées à des clients VPN.

## Étape suivante
[Étape 2. Configurer l’Infrastructure de serveur](vpn-deploy-server-infrastructure.md): dans cette étape, vous installez et configurez les composants côté serveur nécessaires pour prendre en charge de la connexion VPN. Les composants côté serveur incluent la configuration d’infrastructure à clé publique pour distribuer les certificats utilisés par les utilisateurs, le serveur VPN et le serveur NPS. 

---