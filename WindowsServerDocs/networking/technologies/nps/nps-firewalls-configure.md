---
title: Configurer des pare-feu pour le trafic RADIUS
description: Cette rubrique fournit une vue d’ensemble de la configuration de pare-feu pour autoriser le trafic RADIUS pour le serveur NPS dans Windows Server2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 58cca2b2-4ef3-4a09-a614-8bdc08d24f15
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 140111e10eabbece098ae9b7c36746cc663c9cce
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="configure-firewalls-for-radius-traffic"></a>Configurer des pare-feu pour le trafic RADIUS

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Pare-feu peuvent être configurés pour autoriser ou bloquer des types de trafic IP vers et depuis l’ordinateur ou le périphérique sur lequel le pare-feu est en cours d’exécution. Si le pare-feu ne sont pas correctement configurés pour autoriser le trafic RADIUS entre les clients RADIUS, proxy RADIUS et les serveurs RADIUS, authentification d’accès réseau peut échouer, empêchant les utilisateurs d’accéder aux ressources réseau. 

Vous devrez peut-être configurer deux types de pare-feu pour autoriser le trafic RADIUS:

- Pare-feu Windows Defender avec fonctions avancées de sécurité sur le serveur local exécutant le serveur NPS (Network Policy).
- Pare-feu en cours d’exécution sur d’autres ordinateurs ou les périphériques matériels.

## <a name="windows-firewall-on-the-local-nps-server"></a>Pare-feu Windows sur le serveur NPS local

Par défaut, NPS envoie et reçoit le trafic RADIUS à l’aide de \(UDP\) User Datagram Protocol ports 1812, 1813, 1645 et 1646. Le pare-feu Windows Defender sur le serveur NPS est automatiquement configuré avec des exceptions, pendant l’installation du serveur NPS, pour permettre à ce trafic RADIUS à être envoyées et reçues.

Par conséquent, si vous utilisez des ports UDP par défaut, il est inutile de modifier la configuration du pare-feu de Windows Defender pour autoriser le trafic RADIUS vers et depuis les serveurs NPS.

Dans certains cas, vous souhaiterez peut-être modifier les ports que NPS utilise pour le trafic RADIUS. Si vous configurez le serveur NPS et vos serveurs d’accès réseau pour envoyer et recevoir le trafic RADIUS sur des ports autres que les valeurs par défaut, vous devez procédez comme suit:

- Supprimez les exceptions qui autorisent le trafic RADIUS sur les ports par défaut.
- Créez des exceptions autorisent le trafic RADIUS sur les nouveaux ports.

Pour plus d’informations, voir [configuration des informations de Port UDP NPS](nps-udp-ports-configure.md).

## <a name="other-firewalls"></a>Autres pare-feux

Dans la configuration la plus courante, le pare-feu est connecté à Internet et le serveur NPS est une ressource intranet qui est connectée au réseau de périmètre.

Pour atteindre le contrôleur de domaine au sein de l’intranet, le serveur NPS peut avoir:

- Une interface sur le réseau de périmètre et une interface sur l’intranet (le routage IP n’est pas activé). 
- Une interface unique sur le réseau de périmètre. Dans cette configuration, NPS communique avec les contrôleurs de domaine par le biais d’un autre pare-feu qui relie le réseau de périmètre à l’intranet.

## <a name="configuring-the-internet-firewall"></a>La configuration du pare-feu Internet

Le pare-feu est connecté à Internet doit être configuré avec des filtres d’entrée et de sortie sur son interface Internet \ (et, éventuellement, son interface\ du périmètre de réseau), pour autoriser le transfert des messages RADIUS entre le serveur NPS et les clients RADIUS ou les serveurs proxy de fédération sur Internet. Filtres supplémentaires peuvent être utilisés pour autoriser le trafic vers les serveurs Web, les serveurs VPN et les autres types de serveurs sur le réseau de périmètre.

Les filtres peuvent être configurés sur l’interface Internet et l’interface de réseau de périmètre de paquets d’entrée et de sortie distincts.

### <a name="configure-input-filters-on-the-internet-interface"></a>Configurer des filtres d’entrée sur l’Interface Internet

Configurer les filtres de paquets d’entrée suivants sur l’interface Internet du pare-feu pour autoriser les types de trafic suivants:

- Adresse IP de destination de l’interface réseau de périmètre et le port UDP de destination 1812 (0 x 714) du serveur NPS.  Ce filtre autorise le trafic de l’authentification RADIUS à partir de clients RADIUS basés sur Internet sur le serveur NPS. Il s’agit du port UDP par défaut est utilisé par le serveur NPS, comme défini dans RFC2865. Si vous utilisez un autre port, remplacez ce numéro de port par 1812.
- Adresse IP de destination de l’interface réseau de périmètre et le port UDP de destination 1813 (0 x 715) du serveur NPS. Ce filtre autorise le trafic de gestion de comptes RADIUS à partir de clients RADIUS basés sur Internet sur le serveur NPS. Il s’agit du port UDP par défaut est utilisé par le serveur NPS, comme défini dans RFC2866. Si vous utilisez un autre port, remplacez par ce numéro de port 1813.
- \(Optional\) adresse IP de destination de l’interface réseau de périmètre et port UDP de destination de \(0x66D\) 1645 du serveur NPS. Ce filtre autorise le trafic de l’authentification RADIUS à partir de clients RADIUS basés sur Internet sur le serveur NPS. Il s’agit du port UDP est utilisé par les anciens clients RADIUS.
- \(Optional\) adresse IP de destination de l’interface réseau de périmètre et port UDP de destination de \(0x66E\) 1646 du serveur NPS. Ce filtre autorise le trafic de gestion de comptes RADIUS à partir de clients RADIUS basés sur Internet sur le serveur NPS. Il s’agit du port UDP est utilisé par les anciens clients RADIUS.

### <a name="configure-output-filters-on-the-internet-interface"></a>Configurer des filtres de sortie sur l’Interface Internet

Configurer les filtres de sortie suivants sur l’interface Internet du pare-feu pour autoriser les types de trafic suivants:

- Adresse IP source de l’interface réseau de périmètre et port UDP source 1812 (0 x 714) du serveur NPS. Ce filtre autorise le trafic de l’authentification RADIUS à partir du serveur NPS aux clients RADIUS basés sur Internet. Il s’agit du port UDP par défaut est utilisé par le serveur NPS, comme défini dans RFC2865. Si vous utilisez un autre port, remplacez ce numéro de port par 1812.
- Adresse IP source de l’interface réseau de périmètre et port UDP source 1813 (0 x 715) du serveur NPS. Ce filtre autorise le trafic de gestion RADIUS à partir du serveur NPS aux clients RADIUS basés sur Internet. Il s’agit du port UDP par défaut est utilisé par le serveur NPS, comme défini dans RFC2866. Si vous utilisez un autre port, remplacez par ce numéro de port 1813.
- \(Optional\) adresse IP source de l’interface réseau de périmètre et port UDP source \(0x66D\) 1645 du serveur NPS. Ce filtre autorise le trafic de l’authentification RADIUS à partir du serveur NPS aux clients RADIUS basés sur Internet. Il s’agit du port UDP est utilisé par les anciens clients RADIUS.
- \(Optional\) adresse IP source de l’interface réseau de périmètre et port UDP source \(0x66E\) 1646 du serveur NPS. Ce filtre autorise le trafic de gestion RADIUS à partir du serveur NPS aux clients RADIUS basés sur Internet. Il s’agit du port UDP est utilisé par les anciens clients RADIUS.

### <a name="configure-input-filters-on-the-perimeter-network-interface"></a>Configurer des filtres d’entrée sur l’Interface réseau de périmètre

Configurer les filtres d’entrée suivants sur l’interface réseau de périmètre du pare-feu pour autoriser les types de trafic suivants:

- Adresse IP source de l’interface réseau de périmètre et port UDP source 1812 (0 x 714) du serveur NPS. Ce filtre autorise le trafic de l’authentification RADIUS à partir du serveur NPS aux clients RADIUS basés sur Internet. Il s’agit du port UDP par défaut est utilisé par le serveur NPS, comme défini dans RFC2865. Si vous utilisez un autre port, remplacez ce numéro de port par 1812.
- Adresse IP source de l’interface réseau de périmètre et port UDP source 1813 (0 x 715) du serveur NPS. Ce filtre autorise le trafic de gestion RADIUS à partir du serveur NPS aux clients RADIUS basés sur Internet. Il s’agit du port UDP par défaut est utilisé par le serveur NPS, comme défini dans RFC2866. Si vous utilisez un autre port, remplacez par ce numéro de port 1813.
- \(Optional\) adresse IP source de l’interface réseau de périmètre et port UDP source \(0x66D\) 1645 du serveur NPS. Ce filtre autorise le trafic de l’authentification RADIUS à partir du serveur NPS aux clients RADIUS basés sur Internet. Il s’agit du port UDP est utilisé par les anciens clients RADIUS.
- \(Optional\) adresse IP source de l’interface réseau de périmètre et port UDP source \(0x66E\) 1646 du serveur NPS. Ce filtre autorise le trafic de gestion RADIUS à partir du serveur NPS aux clients RADIUS basés sur Internet. Il s’agit du port UDP est utilisé par les anciens clients RADIUS.

### <a name="configure-output-filters-on-the-perimeter-network-interface"></a>Configurer des filtres de sortie sur l’Interface réseau de périmètre

Configurer les filtres de paquets de sortie suivants sur l’interface réseau de périmètre du pare-feu pour autoriser les types de trafic suivants:

- Adresse IP de destination de l’interface réseau de périmètre et le port UDP de destination 1812 (0 x 714) du serveur NPS. Ce filtre autorise le trafic de l’authentification RADIUS à partir de clients RADIUS basés sur Internet sur le serveur NPS. Il s’agit du port UDP par défaut est utilisé par le serveur NPS, comme défini dans RFC2865. Si vous utilisez un autre port, remplacez ce numéro de port par 1812.
- Adresse IP de destination de l’interface réseau de périmètre et le port UDP de destination 1813 (0 x 715) du serveur NPS. Ce filtre autorise le trafic de gestion de comptes RADIUS à partir de clients RADIUS basés sur Internet sur le serveur NPS. Il s’agit du port UDP par défaut est utilisé par le serveur NPS, comme défini dans RFC2866. Si vous utilisez un autre port, remplacez par ce numéro de port 1813.
- \(Optional\) adresse IP de destination de l’interface réseau de périmètre et port UDP de destination de \(0x66D\) 1645 du serveur NPS. Ce filtre autorise le trafic de l’authentification RADIUS à partir de clients RADIUS basés sur Internet sur le serveur NPS. Il s’agit du port UDP est utilisé par les anciens clients RADIUS.
- \(Optional\) adresse IP de destination de l’interface réseau de périmètre et port UDP de destination de \(0x66E\) 1646 du serveur NPS. Ce filtre autorise le trafic de gestion de comptes RADIUS à partir de clients RADIUS basés sur Internet sur le serveur NPS. Il s’agit du port UDP est utilisé par les anciens clients RADIUS.

Pour renforcer la sécurité, vous pouvez utiliser les adresses IP de chaque client RADIUS qui envoie les paquets via le pare-feu pour définir des filtres pour le trafic entre le client et l’adresse IP du serveur NPS sur le réseau de périmètre.

### <a name="filters-on-the-perimeter-network-interface"></a>Filtres sur l’interface réseau de périmètre

Configurer les filtres de paquets d’entrée suivants sur l’interface réseau de périmètre du pare-feu d’intranet pour autoriser les types de trafic suivants:

- Adresse IP source de l’interface de réseau de périmètre du serveur NPS. Ce filtre autorise le trafic à partir du serveur NPS sur le réseau de périmètre.

Configurer les filtres de sortie suivants sur l’interface réseau de périmètre du pare-feu d’intranet pour autoriser les types de trafic suivants:

- Adresse IP de destination de l’interface de réseau de périmètre du serveur NPS. Ce filtre autorise le trafic sur le serveur NPS sur le réseau de périmètre.

### <a name="filters-on-the-intranet-interface"></a>Filtres sur l’interface intranet

Configurer les filtres d’entrée suivants sur l’interface intranet du pare-feu pour autoriser les types de trafic suivants:

- Adresse IP de destination de l’interface de réseau de périmètre du serveur NPS. Ce filtre autorise le trafic sur le serveur NPS sur le réseau de périmètre.

Configurer les filtres de paquets de sortie suivants sur l’interface intranet du pare-feu pour autoriser les types de trafic suivants:

- Adresse IP source de l’interface de réseau de périmètre du serveur NPS. Ce filtre autorise le trafic à partir du serveur NPS sur le réseau de périmètre.


Pour plus d’informations sur la gestion du serveur NPS, voir [gérer un serveur de stratégie réseau](nps-manage-top.md).

Pour plus d’informations sur le serveur NPS, voir [serveur NPS (Network Policy)](nps-top.md).




