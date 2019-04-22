---
title: Configurer des pare-feu pour le trafic RADIUS
description: Cette rubrique fournit une vue d’ensemble de la configuration de pare-feu pour autoriser le trafic RADIUS pour le serveur de stratégie réseau dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 58cca2b2-4ef3-4a09-a614-8bdc08d24f15
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 94b03349f21a40f9bf42508d5a2878a5cf2946cb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819450"
---
# <a name="configure-firewalls-for-radius-traffic"></a>Configurer des pare-feu pour le trafic RADIUS

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Les pare-feu peuvent être configurés pour autoriser ou bloquer des types de trafic IP vers et depuis l’ordinateur ou l’appareil sur lequel le pare-feu est en cours d’exécution. Si le pare-feu ne sont pas correctement configurés pour autoriser le trafic RADIUS entre les clients RADIUS, proxy RADIUS et serveurs RADIUS, authentification d’accès réseau peut échouer, empêchant les utilisateurs d’accéder aux ressources réseau. 

Vous devrez peut-être configurer deux types de pare-feu pour autoriser le trafic RADIUS :

- Pare-feu Windows Defender avec fonctions avancées de sécurité sur le serveur local exécutant le serveur NPS (Network Policy Server).
- Les pare-feu en cours d’exécution sur d’autres ordinateurs ou les périphériques matériels.

## <a name="windows-firewall-on-the-local-nps"></a>Pare-feu Windows sur le serveur NPS local

Par défaut, NPS envoie et reçoit le trafic RADIUS à l’aide de User Datagram Protocol \(UDP\) ports 1812, 1813, 1645 et 1646. Le pare-feu Windows Defender sur le serveur NPS est automatiquement configuré avec des exceptions, pendant l’installation du serveur NPS, pour autoriser ce trafic RADIUS à être envoyés et reçus.

Par conséquent, si vous utilisez les ports UDP par défaut, il est inutile de modifier la configuration du pare-feu Windows Defender pour autoriser le trafic RADIUS vers et depuis NPSs.

Dans certains cas, vous souhaiterez modifier les ports que NPS utilise pour le trafic RADIUS. Si vous configurez le serveur NPS et vos serveurs d’accès réseau pour envoyer et recevoir le trafic RADIUS sur des ports autres que les valeurs par défaut, vous devez procédez comme suit :

- Supprimez les exceptions qui autorisent le trafic RADIUS sur les ports par défaut.
- Créer de nouvelles exceptions qui autorisent le trafic RADIUS sur les nouveaux ports.

Pour plus d’informations, consultez [configurer les informations de Port UDP NPS](nps-udp-ports-configure.md).

## <a name="other-firewalls"></a>Autres pare-feux

Dans la configuration la plus courante, le pare-feu est connecté à Internet et le serveur NPS est une ressource intranet qui est connectée au réseau de périmètre.

Pour atteindre le contrôleur de domaine au sein de l’intranet, le serveur NPS peut avoir :

- Une interface sur le réseau de périmètre et une interface sur l’intranet (le routage IP n’est pas activé). 
- Une interface unique sur le réseau de périmètre. Dans cette configuration, NPS communique avec les contrôleurs de domaine via un autre pare-feu qui relie le réseau de périmètre à l’intranet.

## <a name="configuring-the-internet-firewall"></a>Configuration du pare-feu Internet

Le pare-feu est connecté à Internet doit être configuré avec des filtres d’entrée et de sortie sur son interface Internet \(et, éventuellement, son interface de périphérique réseau\), pour autoriser le transfert des messages RADIUS entre le serveur NPS et clients RADIUS ou des proxys sur Internet. Filtres supplémentaires peuvent être utilisés pour autoriser le trafic vers les serveurs Web, les serveurs VPN et les autres types de serveurs sur le réseau de périmètre.

Les filtres peuvent être configurés sur l’interface Internet et l’interface de réseau de périmètre de paquets d’entrée et de sortie distincts.

### <a name="configure-input-filters-on-the-internet-interface"></a>Configurer des filtres d’entrée sur l’Interface Internet

Configurer les filtres de paquets en entrée suivants sur l’interface Internet du pare-feu pour autoriser les types de trafic suivants :

- Adresse IP de destination de l’interface de réseau de périmètre et le port de destination UDP 1812 (0 x 714) du serveur NPS.  Ce filtre autorise le trafic d’authentification RADIUS à partir de clients RADIUS basés sur Internet au serveur NPS. Il s’agit du port UDP par défaut est utilisé par le serveur NPS, tel que défini dans RFC 2865. Si vous utilisez un port différent, remplacez ce numéro de port par 1812.
- Adresse IP de destination de l’interface de réseau de périmètre et le port de destination UDP 1813 (0 x 715) du serveur NPS. Ce filtre autorise le trafic de gestion des comptes RADIUS à partir de clients RADIUS basés sur Internet au serveur NPS. Il s’agit du port UDP par défaut est utilisé par le serveur NPS, tel que défini dans RFC 2866. Si vous utilisez un port différent, remplacez par ce numéro de port 1813.
- \(Facultatif\) adresse IP de Destination de l’interface de réseau de périmètre et le port de destination UDP 1645 \(0x66D\) de serveur NPS. Ce filtre autorise le trafic d’authentification RADIUS à partir de clients RADIUS basés sur Internet au serveur NPS. Il s’agit du port UDP est utilisé par les anciens clients RADIUS.
- \(Facultatif\) adresse IP de Destination de l’interface de réseau de périmètre et le port de destination UDP 1646 \(0x66E\) de serveur NPS. Ce filtre autorise le trafic de gestion des comptes RADIUS à partir de clients RADIUS basés sur Internet au serveur NPS. Il s’agit du port UDP est utilisé par les anciens clients RADIUS.

### <a name="configure-output-filters-on-the-internet-interface"></a>Configurer des filtres de sortie sur l’Interface Internet

Configurer les filtres de sortie suivants sur l’interface Internet du pare-feu pour autoriser les types de trafic suivants :

- Adresse IP source de l’interface de réseau de périmètre et le port source UDP 1812 (0 x 714) du serveur NPS. Ce filtre autorise le trafic d’authentification RADIUS entre le serveur NPS aux clients RADIUS basés sur Internet. Il s’agit du port UDP par défaut est utilisé par le serveur NPS, tel que défini dans RFC 2865. Si vous utilisez un port différent, remplacez ce numéro de port par 1812.
- Adresse IP source de l’interface de réseau de périmètre et le port source UDP 1813 (0 x 715) du serveur NPS. Ce filtre autorise le trafic de gestion des comptes RADIUS à partir du serveur NPS aux clients RADIUS basés sur Internet. Il s’agit du port UDP par défaut est utilisé par le serveur NPS, tel que défini dans RFC 2866. Si vous utilisez un port différent, remplacez par ce numéro de port 1813.
- \(Facultatif\) adresse IP Source de l’interface de réseau de périmètre et le port source UDP 1645 \(0x66D\) de serveur NPS. Ce filtre autorise le trafic d’authentification RADIUS entre le serveur NPS aux clients RADIUS basés sur Internet. Il s’agit du port UDP est utilisé par les anciens clients RADIUS.
- \(Facultatif\) adresse IP Source de l’interface de réseau de périmètre et le port source UDP 1646 \(0x66E\) de serveur NPS. Ce filtre autorise le trafic de gestion des comptes RADIUS à partir du serveur NPS aux clients RADIUS basés sur Internet. Il s’agit du port UDP est utilisé par les anciens clients RADIUS.

### <a name="configure-input-filters-on-the-perimeter-network-interface"></a>Configurer des filtres d’entrée sur l’Interface de réseau de périmètre

Configurer les filtres d’entrée suivants sur l’interface de réseau de périmètre du pare-feu pour autoriser les types de trafic suivants :

- Adresse IP source de l’interface de réseau de périmètre et le port source UDP 1812 (0 x 714) du serveur NPS. Ce filtre autorise le trafic d’authentification RADIUS entre le serveur NPS aux clients RADIUS basés sur Internet. Il s’agit du port UDP par défaut est utilisé par le serveur NPS, tel que défini dans RFC 2865. Si vous utilisez un port différent, remplacez ce numéro de port par 1812.
- Adresse IP source de l’interface de réseau de périmètre et le port source UDP 1813 (0 x 715) du serveur NPS. Ce filtre autorise le trafic de gestion des comptes RADIUS à partir du serveur NPS aux clients RADIUS basés sur Internet. Il s’agit du port UDP par défaut est utilisé par le serveur NPS, tel que défini dans RFC 2866. Si vous utilisez un port différent, remplacez par ce numéro de port 1813.
- \(Facultatif\) adresse IP Source de l’interface de réseau de périmètre et le port source UDP 1645 \(0x66D\) de serveur NPS. Ce filtre autorise le trafic d’authentification RADIUS entre le serveur NPS aux clients RADIUS basés sur Internet. Il s’agit du port UDP est utilisé par les anciens clients RADIUS.
- \(Facultatif\) adresse IP Source de l’interface de réseau de périmètre et le port source UDP 1646 \(0x66E\) de serveur NPS. Ce filtre autorise le trafic de gestion des comptes RADIUS à partir du serveur NPS aux clients RADIUS basés sur Internet. Il s’agit du port UDP est utilisé par les anciens clients RADIUS.

### <a name="configure-output-filters-on-the-perimeter-network-interface"></a>Configurer des filtres de sortie sur l’Interface de réseau de périmètre

Configurer les filtres de paquets de sortie suivants sur l’interface de réseau de périmètre du pare-feu pour autoriser les types de trafic suivants :

- Adresse IP de destination de l’interface de réseau de périmètre et le port de destination UDP 1812 (0 x 714) du serveur NPS. Ce filtre autorise le trafic d’authentification RADIUS à partir de clients RADIUS basés sur Internet au serveur NPS. Il s’agit du port UDP par défaut est utilisé par le serveur NPS, tel que défini dans RFC 2865. Si vous utilisez un port différent, remplacez ce numéro de port par 1812.
- Adresse IP de destination de l’interface de réseau de périmètre et le port de destination UDP 1813 (0 x 715) du serveur NPS. Ce filtre autorise le trafic de gestion des comptes RADIUS à partir de clients RADIUS basés sur Internet au serveur NPS. Il s’agit du port UDP par défaut est utilisé par le serveur NPS, tel que défini dans RFC 2866. Si vous utilisez un port différent, remplacez par ce numéro de port 1813.
- \(Facultatif\) adresse IP de Destination de l’interface de réseau de périmètre et le port de destination UDP 1645 \(0x66D\) de serveur NPS. Ce filtre autorise le trafic d’authentification RADIUS à partir de clients RADIUS basés sur Internet au serveur NPS. Il s’agit du port UDP est utilisé par les anciens clients RADIUS.
- \(Facultatif\) adresse IP de Destination de l’interface de réseau de périmètre et le port de destination UDP 1646 \(0x66E\) de serveur NPS. Ce filtre autorise le trafic de gestion des comptes RADIUS à partir de clients RADIUS basés sur Internet au serveur NPS. Il s’agit du port UDP est utilisé par les anciens clients RADIUS.

Pour renforcer la sécurité, vous pouvez utiliser les adresses IP de chaque client RADIUS qui envoie les paquets via le pare-feu pour définir des filtres pour le trafic entre le client et l’adresse IP du serveur NPS sur le réseau de périmètre.

### <a name="filters-on-the-perimeter-network-interface"></a>Filtres sur l’interface de réseau de périmètre

Configurer les filtres de paquets en entrée suivants sur l’interface de réseau de périmètre du pare-feu intranet pour autoriser les types de trafic suivants :

- Adresse IP source de l’interface de réseau de périmètre du serveur NPS. Ce filtre autorise le trafic à partir du serveur NPS sur le réseau de périmètre.

Configurer les filtres de sortie suivants sur l’interface de réseau de périmètre du pare-feu intranet pour autoriser les types de trafic suivants :

- Adresse IP de destination de l’interface de réseau de périmètre du serveur NPS. Ce filtre autorise le trafic vers le serveur NPS sur le réseau de périmètre.

### <a name="filters-on-the-intranet-interface"></a>Filtres sur l’interface intranet

Configurer les filtres d’entrée suivants sur l’interface intranet du pare-feu pour autoriser les types de trafic suivants :

- Adresse IP de destination de l’interface de réseau de périmètre du serveur NPS. Ce filtre autorise le trafic vers le serveur NPS sur le réseau de périmètre.

Configurer les filtres de paquets de sortie suivants sur l’interface intranet du pare-feu pour autoriser les types de trafic suivants :

- Adresse IP source de l’interface de réseau de périmètre du serveur NPS. Ce filtre autorise le trafic à partir du serveur NPS sur le réseau de périmètre.


Pour plus d’informations sur la gestion de serveur NPS, consultez [gérer un serveur de stratégie réseau](nps-manage-top.md).

Pour plus d’informations sur le serveur NPS, consultez [serveur NPS (Network Policy Server)](nps-top.md).




