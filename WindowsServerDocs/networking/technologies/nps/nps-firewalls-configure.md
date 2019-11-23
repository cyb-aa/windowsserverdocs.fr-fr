---
title: Configurer des pare-feu pour le trafic RADIUS
description: Cette rubrique fournit une vue d’ensemble de la configuration des pare-feu pour autoriser le trafic RADIUS pour le serveur NPS (Network Policy Server) dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 58cca2b2-4ef3-4a09-a614-8bdc08d24f15
ms.author: pashort
author: shortpatti
ms.openlocfilehash: eb7f5c495fa09aed584ef37d668d1fa232d7a497
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405446"
---
# <a name="configure-firewalls-for-radius-traffic"></a>Configurer des pare-feu pour le trafic RADIUS

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Les pare-feu peuvent être configurés pour autoriser ou bloquer les types de trafic IP vers et à partir de l’ordinateur ou du périphérique sur lequel le pare-feu est en cours d’exécution. Si les pare-feu ne sont pas configurés correctement pour autoriser le trafic RADIUS entre les clients RADIUS, les proxys RADIUS et les serveurs RADIUS, l’authentification d’accès réseau peut échouer, ce qui empêche les utilisateurs d’accéder aux ressources réseau. 

Vous devrez peut-être configurer deux types de pare-feu pour autoriser le trafic RADIUS :

- Pare-feu Windows Defender avec fonctions avancées de sécurité sur le serveur local exécutant le serveur NPS (Network Policy Server).
- Les pare-feu s’exécutant sur d’autres ordinateurs ou périphériques matériels.

## <a name="windows-firewall-on-the-local-nps"></a>Pare-feu Windows sur le serveur NPS local

Par défaut, NPS envoie et reçoit le trafic RADIUS en utilisant le protocole UDP (User Datagram Protocol) \(les ports UDP\) 1812, 1813, 1645 et 1646. Le pare-feu Windows Defender sur le serveur NPS est automatiquement configuré avec des exceptions, pendant l’installation du serveur NPS, pour autoriser l’envoi et la réception de ce trafic RADIUS.

Par conséquent, si vous utilisez les ports UDP par défaut, vous n’avez pas besoin de modifier la configuration du pare-feu Windows Defender pour autoriser le trafic RADIUS vers et depuis NPSs.

Dans certains cas, vous souhaiterez peut-être modifier les ports que NPS utilise pour le trafic RADIUS. Si vous configurez NPS et vos serveurs d’accès réseau pour envoyer et recevoir le trafic RADIUS sur des ports autres que les valeurs par défaut, vous devez effectuer les opérations suivantes :

- Supprimez les exceptions qui autorisent le trafic RADIUS sur les ports par défaut.
- Créez de nouvelles exceptions qui autorisent le trafic RADIUS sur les nouveaux ports.

Pour plus d’informations, consultez [configurer les informations de port UDP NPS](nps-udp-ports-configure.md).

## <a name="other-firewalls"></a>Autres pare-feu

Dans la configuration la plus courante, le pare-feu est connecté à Internet et le serveur NPS est une ressource intranet qui est connectée au réseau de périmètre.

Pour atteindre le contrôleur de domaine au sein de l’intranet, le serveur NPS peut avoir :

- Une interface sur le réseau de périmètre et une interface sur l’intranet (le routage IP n’est pas activé). 
- Une seule interface sur le réseau de périmètre. Dans cette configuration, NPS communique avec les contrôleurs de domaine via un autre pare-feu qui connecte le réseau de périmètre à l’intranet.

## <a name="configuring-the-internet-firewall"></a>Configuration du pare-feu Internet

Le pare-feu connecté à Internet doit être configuré avec des filtres d’entrée et de sortie sur son interface Internet \(et, éventuellement, son interface de périmètre du réseau\), pour autoriser le transfert des messages RADIUS entre les clients NPS et RADIUS ou les proxys sur Internet. Des filtres supplémentaires peuvent être utilisés pour autoriser le passage du trafic vers les serveurs Web, les serveurs VPN et d’autres types de serveurs sur le réseau de périmètre.

Des filtres de paquets d’entrée et de sortie distincts peuvent être configurés sur l’interface Internet et l’interface du réseau de périmètre.

### <a name="configure-input-filters-on-the-internet-interface"></a>Configurer des filtres d’entrée sur l’interface Internet

Configurez les filtres de paquets en entrée suivants sur l’interface Internet du pare-feu pour autoriser les types de trafic suivants :

- Adresse IP de destination de l’interface du réseau de périmètre et port de destination UDP 1812 (0x714) du serveur NPS.  Ce filtre autorise le trafic d’authentification RADIUS à partir des clients RADIUS basés sur Internet vers le serveur NPS. Il s’agit du port UDP par défaut qui est utilisé par NPS, comme défini dans la norme RFC 2865. Si vous utilisez un port différent, remplacez le numéro de port 1812.
- Adresse IP de destination de l’interface du réseau de périmètre et port de destination UDP 1813 (0x715) du serveur NPS. Ce filtre autorise le trafic de gestion de comptes RADIUS des clients RADIUS basés sur Internet vers le serveur NPS. Il s’agit du port UDP par défaut qui est utilisé par NPS, comme défini dans la norme RFC 2866. Si vous utilisez un port différent, remplacez le numéro de port 1813.
- \(\) adresse IP de destination facultative de l’interface du réseau de périmètre et le port de destination UDP 1645 \(0x66D\) du serveur NPS. Ce filtre autorise le trafic d’authentification RADIUS à partir des clients RADIUS basés sur Internet vers le serveur NPS. Il s’agit du port UDP qui est utilisé par les anciens clients RADIUS.
- \(\) adresse IP de destination facultative de l’interface du réseau de périmètre et le port de destination UDP 1646 \(0x66E\) du serveur NPS. Ce filtre autorise le trafic de gestion de comptes RADIUS des clients RADIUS basés sur Internet vers le serveur NPS. Il s’agit du port UDP qui est utilisé par les anciens clients RADIUS.

### <a name="configure-output-filters-on-the-internet-interface"></a>Configurer des filtres de sortie sur l’interface Internet

Configurez les filtres de sortie suivants sur l’interface Internet du pare-feu pour autoriser les types de trafic suivants :

- Adresse IP source de l’interface du réseau de périmètre et port source UDP 1812 (0x714) du serveur NPS. Ce filtre autorise le trafic d’authentification RADIUS du serveur NPS vers les clients RADIUS basés sur Internet. Il s’agit du port UDP par défaut qui est utilisé par NPS, comme défini dans la norme RFC 2865. Si vous utilisez un port différent, remplacez le numéro de port 1812.
- Adresse IP source de l’interface du réseau de périmètre et port source UDP 1813 (0x715) du serveur NPS. Ce filtre autorise le trafic de gestion de comptes RADIUS entre le serveur NPS et les clients RADIUS basés sur Internet. Il s’agit du port UDP par défaut qui est utilisé par NPS, comme défini dans la norme RFC 2866. Si vous utilisez un port différent, remplacez le numéro de port 1813.
- \(adresse IP source\) facultative de l’interface du réseau de périmètre et le port source UDP 1645 \(0x66D\) du serveur NPS. Ce filtre autorise le trafic d’authentification RADIUS du serveur NPS vers les clients RADIUS basés sur Internet. Il s’agit du port UDP qui est utilisé par les anciens clients RADIUS.
- \(adresse IP source\) facultative de l’interface du réseau de périmètre et le port source UDP 1646 \(0x66E\) du serveur NPS. Ce filtre autorise le trafic de gestion de comptes RADIUS entre le serveur NPS et les clients RADIUS basés sur Internet. Il s’agit du port UDP qui est utilisé par les anciens clients RADIUS.

### <a name="configure-input-filters-on-the-perimeter-network-interface"></a>Configurer des filtres d’entrée sur l’interface du réseau de périmètre

Configurez les filtres d’entrée suivants sur l’interface du réseau de périmètre du pare-feu pour autoriser les types de trafic suivants :

- Adresse IP source de l’interface du réseau de périmètre et port source UDP 1812 (0x714) du serveur NPS. Ce filtre autorise le trafic d’authentification RADIUS du serveur NPS vers les clients RADIUS basés sur Internet. Il s’agit du port UDP par défaut qui est utilisé par NPS, comme défini dans la norme RFC 2865. Si vous utilisez un port différent, remplacez le numéro de port 1812.
- Adresse IP source de l’interface du réseau de périmètre et port source UDP 1813 (0x715) du serveur NPS. Ce filtre autorise le trafic de gestion de comptes RADIUS entre le serveur NPS et les clients RADIUS basés sur Internet. Il s’agit du port UDP par défaut qui est utilisé par NPS, comme défini dans la norme RFC 2866. Si vous utilisez un port différent, remplacez le numéro de port 1813.
- \(adresse IP source\) facultative de l’interface du réseau de périmètre et le port source UDP 1645 \(0x66D\) du serveur NPS. Ce filtre autorise le trafic d’authentification RADIUS du serveur NPS vers les clients RADIUS basés sur Internet. Il s’agit du port UDP qui est utilisé par les anciens clients RADIUS.
- \(adresse IP source\) facultative de l’interface du réseau de périmètre et le port source UDP 1646 \(0x66E\) du serveur NPS. Ce filtre autorise le trafic de gestion de comptes RADIUS entre le serveur NPS et les clients RADIUS basés sur Internet. Il s’agit du port UDP qui est utilisé par les anciens clients RADIUS.

### <a name="configure-output-filters-on-the-perimeter-network-interface"></a>Configurer des filtres de sortie sur l’interface du réseau de périmètre

Configurez les filtres de paquets de sortie suivants sur l’interface du réseau de périmètre du pare-feu pour autoriser les types de trafic suivants :

- Adresse IP de destination de l’interface du réseau de périmètre et port de destination UDP 1812 (0x714) du serveur NPS. Ce filtre autorise le trafic d’authentification RADIUS à partir des clients RADIUS basés sur Internet vers le serveur NPS. Il s’agit du port UDP par défaut qui est utilisé par NPS, comme défini dans la norme RFC 2865. Si vous utilisez un port différent, remplacez le numéro de port 1812.
- Adresse IP de destination de l’interface du réseau de périmètre et port de destination UDP 1813 (0x715) du serveur NPS. Ce filtre autorise le trafic de gestion de comptes RADIUS des clients RADIUS basés sur Internet vers le serveur NPS. Il s’agit du port UDP par défaut qui est utilisé par NPS, comme défini dans la norme RFC 2866. Si vous utilisez un port différent, remplacez le numéro de port 1813.
- \(\) adresse IP de destination facultative de l’interface du réseau de périmètre et le port de destination UDP 1645 \(0x66D\) du serveur NPS. Ce filtre autorise le trafic d’authentification RADIUS à partir des clients RADIUS basés sur Internet vers le serveur NPS. Il s’agit du port UDP qui est utilisé par les anciens clients RADIUS.
- \(\) adresse IP de destination facultative de l’interface du réseau de périmètre et le port de destination UDP 1646 \(0x66E\) du serveur NPS. Ce filtre autorise le trafic de gestion de comptes RADIUS des clients RADIUS basés sur Internet vers le serveur NPS. Il s’agit du port UDP qui est utilisé par les anciens clients RADIUS.

Pour renforcer la sécurité, vous pouvez utiliser les adresses IP de chaque client RADIUS qui envoie les paquets via le pare-feu pour définir des filtres pour le trafic entre le client et l’adresse IP du serveur NPS sur le réseau de périmètre.

### <a name="filters-on-the-perimeter-network-interface"></a>Filtres sur l’interface du réseau de périmètre

Configurez les filtres de paquets en entrée suivants sur l’interface du réseau de périmètre du pare-feu intranet pour autoriser les types de trafic suivants :

- Adresse IP source de l’interface du réseau de périmètre du serveur NPS. Ce filtre autorise le trafic à partir du serveur NPS sur le réseau de périmètre.

Configurez les filtres de sortie suivants sur l’interface du réseau de périmètre du pare-feu intranet pour autoriser les types de trafic suivants :

- Adresse IP de destination de l’interface du réseau de périmètre du serveur NPS. Ce filtre autorise le trafic vers le serveur NPS sur le réseau de périmètre.

### <a name="filters-on-the-intranet-interface"></a>Filtres sur l’interface intranet

Configurez les filtres d’entrée suivants sur l’interface intranet du pare-feu pour autoriser les types de trafic suivants :

- Adresse IP de destination de l’interface du réseau de périmètre du serveur NPS. Ce filtre autorise le trafic vers le serveur NPS sur le réseau de périmètre.

Configurez les filtres de paquets de sortie suivants sur l’interface intranet du pare-feu pour autoriser les types de trafic suivants :

- Adresse IP source de l’interface du réseau de périmètre du serveur NPS. Ce filtre autorise le trafic à partir du serveur NPS sur le réseau de périmètre.


Pour plus d’informations sur la gestion de NPS, consultez [gérer un serveur de stratégie réseau](nps-manage-top.md).

Pour plus d’informations sur NPS, consultez [serveur NPS (Network Policy Server)](nps-top.md).




