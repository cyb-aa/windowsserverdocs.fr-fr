---
title: Configurer un serveur NPS sur un ordinateur multirésident
description: Cette rubrique fournit des instructions sur la configuration d’un serveur avec plusieurs cartes réseau qui exécutent le serveur de stratégie réseau dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: d9d9e9ac-4859-4522-89ed-a23092c9e12a
ms.author: lizross
author: eross-msft
ms.openlocfilehash: a937e151954629f7e8775ec68ba8ab5f2b63ee1a
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80315812"
---
# <a name="configure-nps-on-a-multihomed-computer"></a>Configurer un serveur NPS sur un ordinateur multirésident

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour configurer un serveur NPS avec plusieurs cartes réseau.

Lorsque vous utilisez plusieurs cartes réseau sur un serveur exécutant NPS (Network Policy Server), vous pouvez configurer les éléments suivants :

- Les cartes réseau qui n’envoient et n’envoient pas et ne reçoivent pas de protocole RADIUS (Remote Authentication Dial-In User Service) \(le trafic RADIUS\).
- Sur la base d’une carte réseau, si NPS surveille le trafic RADIUS sur le protocole Internet version 4 \(IPv4\), IPv6 ou IPv4 et IPv6.
- Numéros de port UDP sur lesquels le trafic RADIUS est envoyé et reçu sur une base par protocole \(IPv4 ou IPv6\), par carte réseau.

Par défaut, NPS écoute le trafic RADIUS sur les ports 1812, 1813, 1645 et 1646 pour IPv6 et IPv4 pour toutes les cartes réseau installées. Comme NPS utilise automatiquement toutes les cartes réseau pour le trafic RADIUS, il vous suffit de spécifier les cartes réseau que vous souhaitez que NPS utilise pour le trafic RADIUS lorsque vous souhaitez empêcher NPS d’utiliser une carte réseau spécifique.

>[!NOTE]
>Si vous désinstallez IPv4 ou IPv6 sur une carte réseau, NPS n’analyse pas le trafic RADIUS pour le protocole désinstallé.

Sur un serveur NPS sur lequel plusieurs cartes réseau sont installées, vous souhaiterez peut-être configurer NPS pour envoyer et recevoir le trafic RADIUS uniquement sur les cartes que vous spécifiez.

Par exemple, une carte réseau installée dans le serveur NPS peut aboutir à un segment de réseau qui ne contient pas de clients RADIUS, tandis qu’une deuxième carte réseau fournit à NPS un chemin réseau vers ses clients RADIUS configurés. Dans ce scénario, il est important de demander à NPS d’utiliser la deuxième carte réseau pour tout le trafic RADIUS.

Dans un autre exemple, si votre serveur NPS a trois cartes réseau installées, mais que vous souhaitez que NPS utilise deux des adaptateurs pour le trafic RADIUS, vous pouvez configurer les informations de port pour les deux cartes uniquement. En excluant la configuration de port pour le troisième adaptateur, vous empêchez NPS d’utiliser l’adaptateur pour le trafic RADIUS.

## <a name="using-a-network-adapter"></a>Utilisation d’une carte réseau

Pour configurer NPS afin d’écouter et d’envoyer le trafic RADIUS sur une carte réseau, utilisez la syntaxe suivante dans la boîte de dialogue Propriétés du serveur de stratégie réseau dans la console NPS :

- Syntaxe du trafic IPv4 : IPAddress : UDPport, où IPAddress est l’adresse IPv4 qui est configurée sur la carte réseau sur laquelle vous souhaitez envoyer le trafic RADIUS et UDPport est le numéro de port RADIUS que vous souhaitez utiliser pour l’authentification ou la gestion des comptes RADIUS le trafic.
- Syntaxe du trafic IPv6 : [Adresseipv6] : UDPport, où les crochets autour de Adresseipv6 sont requis, Adresseipv6 est l’adresse IPv6 configurée sur la carte réseau sur laquelle vous souhaitez envoyer le trafic RADIUS et UDPport est le numéro de port RADIUS que vous souhaitez à utiliser pour l’authentification RADIUS ou le trafic de gestion des comptes.

Les caractères suivants peuvent être utilisés comme délimiteurs pour configurer l’adresse IP et les informations de port UDP :

- Délimiteur adresse/port : deux-points ( :)
- Délimiteur de port : virgule (,)
- Délimiteur de l’interface : point-virgule (;)

## <a name="configuring-network-access-servers"></a>Configuration des serveurs d’accès réseau

Assurez-vous que vos serveurs d’accès réseau sont configurés avec les mêmes numéros de port UDP RADIUS que ceux que vous configurez sur votre NPSs. Les ports UDP RADIUS standard définis dans les RFC 2865 et 2866 sont 1812 pour l’authentification et 1813 pour la gestion des comptes ; Toutefois, certains serveurs d’accès sont configurés par défaut pour utiliser le port UDP 1645 pour les demandes d’authentification et le port UDP 1646 pour les demandes de gestion des comptes.

>[!IMPORTANT]
>Si vous n’utilisez pas les numéros de port par défaut RADIUS, vous devez configurer des exceptions sur le pare-feu de l’ordinateur local pour autoriser le trafic RADIUS sur les nouveaux ports. Pour plus d’informations, consultez [configurer des pare-feu pour le trafic RADIUS](nps-firewalls-configure.md).

## <a name="configure-the-multihomed-nps"></a>Configurer le serveur NPS multirésident

Vous pouvez utiliser la procédure suivante pour configurer votre serveur NPS multirésident.

L'appartenance au groupe **Admins du domaine**, ou équivalent, est la condition minimale requise pour effectuer cette procédure.

### <a name="to-specify-the-network-adapter-and-udp-ports-that-nps-uses-for-radius-traffic"></a>Pour spécifier la carte réseau et les ports UDP que NPS utilise pour le trafic RADIUS

1. Dans le gestionnaire de serveur, cliquez sur **Outils**, puis sur **serveur de stratégie réseau** pour ouvrir la console NPS.

2. Cliquez avec le bouton droit sur **Serveur NPS (Network Policy Server)** , puis cliquez sur **Propriétés**.

3. Cliquez sur l’onglet **ports** , puis ajoutez l’adresse IP de la carte réseau que vous souhaitez utiliser pour le trafic RADIUS aux numéros de port existants. Par exemple, si vous souhaitez utiliser l’adresse IP 192.168.1.2 et les ports RADIUS 1812 et 1645 pour les demandes d’authentification, modifiez le paramètre de port de **1812, 1645** à **192.168.1.2:1812, 1645**. Si vos ports UDP d’authentification RADIUS et de gestion des comptes RADIUS diffèrent des valeurs par défaut, modifiez les paramètres de port en conséquence.

4. Pour utiliser plusieurs paramètres de port pour les demandes d’authentification ou de gestion des comptes, séparez les numéros de port par des virgules.

Pour plus d’informations sur les ports UDP NPS, consultez [configurer les informations de port UDP NPS](nps-udp-ports-configure.md) .


Pour plus d’informations sur NPS, consultez serveur NPS ( [Network Policy Server](nps-top.md) ).

