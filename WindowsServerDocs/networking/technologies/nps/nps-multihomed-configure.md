---
title: Configurer un serveur NPS sur un ordinateur multirésident
description: Cette rubrique fournit des instructions sur la configuration d’un serveur avec plusieurs cartes réseau qui est en cours d’exécution serveur NPS dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d9d9e9ac-4859-4522-89ed-a23092c9e12a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 55eccf3afc649e84c5b6f5ce7932ed97617ddca9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856800"
---
# <a name="configure-nps-on-a-multihomed-computer"></a>Configurer un serveur NPS sur un ordinateur multirésident

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour configurer un serveur NPS avec plusieurs cartes réseau.

Lorsque vous utilisez plusieurs cartes réseau dans un serveur exécutant le serveur NPS (Network Policy Server), vous pouvez configurer les éléments suivants :

- Les cartes réseau qui faire et ne pas envoyer et recevoir Remote Authentication Dial-In User Service \(RADIUS\) le trafic.
- Sur une base de carte réseau, si NPS surveille le trafic RADIUS sur Internet Protocol version 4 \(IPv4\), IPv6, ou à la fois IPv4 et IPv6.
- Les numéros de port UDP via les RADIUS le trafic est envoyé et reçu sur un protocole par \(IPv4 ou IPv6\), chaque carte de réseau.

Par défaut, NPS écoute le trafic RADIUS sur les ports 1812, 1813, 1645 et 1646 pour IPv6 et IPv4 pour toutes les cartes réseau installées. Étant donné que NPS utilise automatiquement toutes les cartes réseau pour le trafic RADIUS, vous devez uniquement spécifier les cartes réseau que NPS à utiliser pour RADIUS du trafic lorsque vous souhaitez empêcher que NPS à l’aide d’une carte réseau spécifique.

>[!NOTE]
>Si vous désinstallez IPv4 ou IPv6 sur une carte réseau, NPS ne surveille pas le trafic RADIUS pour le protocole désinstallé.

Sur le serveur NPS qui a plusieurs cartes réseau installées, vous souhaiterez configurer NPS pour envoyer et recevoir du trafic RADIUS uniquement sur les cartes que vous spécifiez.

Par exemple, une carte réseau installée dans le serveur NPS peut entraîner un segment de réseau qui ne contient-elle pas de clients RADIUS, tandis qu’une deuxième carte réseau fournit à un chemin d’accès réseau NPS à ses clients RADIUS configurés. Dans ce scénario, il est important diriger le serveur NPS à utiliser la deuxième carte réseau pour tout le trafic RADIUS.

Dans un autre exemple, si votre serveur NPS a trois cartes réseau installées, mais que vous ne voulez que d’utiliser deux des adaptateurs pour le trafic RADIUS, vous pouvez configurer les informations de port pour les deux cartes uniquement. À l’exclusion de la configuration du port pour la troisième carte, vous empêchez NPS d’utilisation de l’adaptateur pour le trafic RADIUS.

## <a name="using-a-network-adapter"></a>À l’aide d’une carte réseau

Pour configurer NPS pour écouter et envoyer le trafic RADIUS sur une carte réseau, utilisez la syntaxe suivante dans la boîte de dialogue Propriétés de serveur NPS dans la console NPS :

- Syntaxe pour le trafic IPv4 : AdresseIP : PortUDP, où Adresse_ip est l’adresse IPv4 qui est configuré sur la carte réseau sur laquelle vous souhaitez envoyer le trafic RADIUS et PortUDP est le numéro de port RADIUS que vous souhaitez utiliser pour l’authentification RADIUS ou le trafic de gestion.
- Syntaxe pour le trafic IPv6 : [IPv6Address] : PortUDP, où les crochets autour de IPv6Address sont nécessaires, IPv6Address est l’adresse IPv6 configurée sur la carte réseau sur laquelle vous souhaitez envoyer le trafic RADIUS et PortUDP est le numéro de port RADIUS que vous souhaitez utiliser pour l’authentification RADIUS ou trafic de gestion.

Les caractères suivants peuvent être utilisés en tant que délimiteurs pour la configuration d’adresse IP et des informations sur le port UDP :

- Adresse/port délimiteur : signe deux-points ( :))
- Délimiteur de port : virgule (,)
- Interface délimiteurs : point-virgule ( ;)

## <a name="configuring-network-access-servers"></a>Configuration des serveurs d’accès réseau

Assurez-vous que vos serveurs d’accès réseau sont configurés avec les mêmes numéros de port UDP RADIUS que vous configurez sur votre NPSs. Les ports UDP RADIUS standard définies dans les RFC 2865 et 2866 sont 1812 pour l’authentification et 1813 pour la gestion des comptes ; Toutefois, certains serveurs d’accès sont configurés par défaut pour utiliser le port UDP 1645 pour les demandes d’authentification et le port UDP 1646 pour les demandes de gestion.

>[!IMPORTANT]
>Si vous n’utilisez pas les numéros de port par défaut RADIUS, vous devez configurer des exceptions sur le pare-feu pour l’ordinateur local Autoriser le trafic RADIUS sur les nouveaux ports. Pour plus d’informations, consultez [configurer le pare-feu pour le trafic RADIUS](nps-firewalls-configure.md).

## <a name="configure-the-multihomed-nps"></a>Configurer le serveur NPS multirésident

Vous pouvez utiliser la procédure suivante pour configurer votre serveur NPS multirésidents.

L'appartenance au groupe **Admins du domaine**, ou équivalent, est la condition minimale requise pour effectuer cette procédure.

### <a name="to-specify-the-network-adapter-and-udp-ports-that-nps-uses-for-radius-traffic"></a>Pour spécifier la carte réseau et les ports UDP que NPS utilise pour le trafic RADIUS

1. Dans le Gestionnaire de serveur, cliquez sur **outils**, puis cliquez sur **Network Policy Server** pour ouvrir la console NPS.

2. Cliquez avec le bouton droit sur **Serveur NPS (Network Policy Server)**, puis cliquez sur **Propriétés**.

3. Cliquez sur le **Ports** onglet et ajouter l’adresse IP de la carte réseau que vous souhaitez utiliser pour le trafic RADIUS pour les numéros de port existant. Par exemple, si vous souhaitez utiliser l’adresse IP 192.168.1.2 et les ports RADIUS 1812 et 1645 pour les demandes d’authentification, remplacez le paramètre port **1812,1645** à **192.168.1.2:1812,1645**. Si votre authentification RADIUS et les ports UDP de gestion des comptes RADIUS sont différents des valeurs par défaut, modifiez les paramètres de port en conséquence.

4. Pour utiliser plusieurs paramètres de port pour l’authentification ou les demandes de gestion, séparez les numéros de port par des virgules.

Pour plus d’informations sur les ports UDP de NPS, consultez [configurer les informations de Port UDP NPS](nps-udp-ports-configure.md)


Pour plus d’informations sur le serveur NPS, consultez [serveur NPS](nps-top.md)

