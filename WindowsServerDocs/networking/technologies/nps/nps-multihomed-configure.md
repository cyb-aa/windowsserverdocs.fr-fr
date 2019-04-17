---
title: Configurer NPS sur un ordinateur multirésident
description: Cette rubrique fournit des instructions sur la configuration d’un serveur avec plusieurs cartes réseau qui exécute le serveur de stratégie réseau dans Windows Server2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d9d9e9ac-4859-4522-89ed-a23092c9e12a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f80e83a4d79036729b6b442e6362d52fbda12edd
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="configure-nps-on-a-multihomed-computer"></a>Configurer NPS sur un ordinateur multirésident

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette rubrique pour configurer un serveur NPS avec plusieurs cartes réseau.

Lorsque vous utilisez plusieurs cartes réseau sur un serveur exécutant le serveur NPS (Network Policy), vous pouvez configurer les éléments suivants:

- Les cartes réseau qui faire et ne pas envoyer et recevoir le trafic \(RADIUS\) Remote Authentication Dial-In User Service.
- Chaque carte réseau et si NPS surveille le trafic RADIUS sur Internet Protocol version4 \(IPv4\), IPv6, ou à la fois IPv4 et IPv6.
- Les numéros de port UDP via les RADIUS le trafic est envoyé et reçu par protocole \ (IPv4 ou IPv6\), base de carte réseau.

Par défaut, NPS écoute le trafic RADIUS sur les ports 1812, 1813, 1645 et 1646 pour IPv6 et IPv4 pour toutes les cartes réseau installées. Étant donné que NPS utilise automatiquement toutes les cartes réseau pour le trafic RADIUS, vous devez uniquement spécifier les cartes réseau que NPS à utiliser pour RADIUS de trafic lorsque vous souhaitez empêcher le serveur NPS à l’aide d’une carte réseau spécifique.

>[!NOTE]
>Si vous désinstallez IPv4 ou IPv6 sur une carte réseau, NPS ne surveille pas le trafic RADIUS pour le protocole désinstallé.

Sur un serveur NPS qui possède plusieurs cartes réseau installées, vous devrez configurer NPS pour envoyer et recevoir le trafic RADIUS uniquement sur les cartes que vous spécifiez.

Par exemple, une carte réseau installée dans le serveur NPS peut entraîner un segment de réseau qui ne contient-elle pas de clients RADIUS, tandis que fournit une deuxième carte réseau NPS avec un chemin d’accès réseau pour qu’il a configuré les clients RADIUS. Dans ce scénario, il est important diriger NPS pour utiliser la seconde carte réseau pour tout le trafic RADIUS.

Dans un autre exemple, si votre serveur NPS possède trois cartes réseau installées, mais que vous ne voulez que d’utiliser deux des cartes pour le trafic RADIUS, vous pouvez configurer les informations de port pour les deux cartes uniquement. En excluant la configuration du port pour la troisième carte réseau, vous empêchez NPS d’à l’aide de la carte pour le trafic RADIUS.

## <a name="using-a-network-adapter"></a>À l’aide d’une carte réseau

Pour configurer NPS pour écouter et envoyer le trafic RADIUS sur une carte réseau, utilisez la syntaxe suivante dans la boîte de dialogue Propriétés du serveur de stratégie réseau dans la console NPS:

- Syntaxe pour le trafic IPv4: adresseIP: PortUDP, où IPAddress correspond à l’adresse IPv4 configurée sur la carte réseau sur laquelle vous souhaitez envoyer le trafic RADIUS et PortUDP est le numéro de port RADIUS que vous souhaitez utiliser pour l’authentification RADIUS ou le trafic de gestion.
- Syntaxe pour le trafic IPv6: [IPv6Address]: PortUDP, où les crochets autour IPv6Address sont requises, IPv6Address est l’adresse IPv6 qui est configuré sur la carte réseau sur laquelle vous souhaitez envoyer le trafic RADIUS et PortUDP est le numéro de port RADIUS que vous souhaitez utiliser pour l’authentification RADIUS ou le trafic de gestion.

Les caractères suivants peuvent être utilisés comme séparateurs pour la configuration d’adresse IP et les informations de port UDP:

- Adresse/port délimiteur: signe deux-points (:))
- Délimiteur de port: virgule (,)
- Interface délimiteur: point-virgule (;)

## <a name="configuring-network-access-servers"></a>Configuration des serveurs d’accès réseau

Assurez-vous que vos serveurs d’accès réseau sont configurés avec les mêmes numéros de port UDP RADIUS que vous configurez sur vos serveurs NPS. Les ports UDP standards RADIUS définies dans les RFC2865 et 2866 sont 1812 pour l’authentification et 1813 pour la gestion des comptes; toutefois, certains serveurs d’accès sont configurés par défaut pour utiliser le port UDP1645 pour les demandes d’authentification et le port UDP1646 pour les demandes de gestion.

>[!IMPORTANT]
>Si vous n’utilisez pas les numéros de port par défaut RADIUS, vous devez configurer des exceptions sur le pare-feu de l’ordinateur local Autoriser le trafic RADIUS sur les nouveaux ports. Pour plus d’informations, voir [configurer le pare-feu pour le trafic RADIUS](nps-firewalls-configure.md).

## <a name="configure-the-multihomed-nps-server"></a>Configurer le serveur NPS multirésident

Vous pouvez utiliser la procédure suivante pour configurer votre serveur NPS de multirésident.

L’appartenance au groupe **Admins du domaine**, ou équivalente, est la condition minimale requise pour effectuer cette procédure.

### <a name="to-specify-the-network-adapter-and-udp-ports-that-nps-uses-for-radius-traffic"></a>Pour spécifier la carte réseau et les ports UDP que NPS utilise pour le trafic RADIUS

1. Dans le Gestionnaire de serveur, cliquez sur **outils**, puis cliquez sur **serveur NPS** pour ouvrir la console NPS.

2. Avec le bouton droit **serveur NPS**, puis cliquez sur **propriétés**.

3. Cliquez sur le **Ports** onglet et ajoutez l’adresse IP de la carte réseau que vous souhaitez utiliser pour le trafic RADIUS pour les numéros de port existantes. Par exemple, si vous souhaitez utiliser l’adresse IP 192.168.1.2 et les ports RADIUS1812 et 1645 pour les demandes d’authentification, remplacez le paramètre port **1812,1645** à **192.168.1.2:1812,1645**. Si votre authentification RADIUS comptabilité ports UDP et RADIUS sont différentes des valeurs par défaut, modifiez les paramètres de port en conséquence.

4. Pour utiliser plusieurs paramètres de port pour l’authentification ou les demandes de gestion, séparez les numéros de port par des virgules.

Pour plus d’informations sur les ports UDP NPS, voir [configuration des informations de Port UDP NPS](nps-udp-ports-configure.md)


Pour plus d’informations sur le serveur NPS, voir [serveur NPS](nps-top.md)

