---
title: Configurer DNS et les paramètres de pare-feu
description: Cette rubrique fournit des instructions détaillées sur le déploiement de Always On VPN dans Windows Server 2016.
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: d8cf3bae-45bf-4ffa-9205-290d555c59da
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 06/11/2018
ms.openlocfilehash: aa7658587b8434bfbaa6874498215a6b2c9213be
ms.sourcegitcommit: 07c9d4ea72528401314e2789e3bc2e688fc96001
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76822662"
---
# <a name="step-5-configure-dns-and-firewall-settings"></a>Étape 5. Configurer les paramètres DNS et de pare-feu

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Précédent :** Étape 4. Installer et configurer le serveur NPS](vpn-deploy-nps.md)
- [**Ensuite :** Étape 6. Configurer le client Windows 10 Always On les connexions VPN](vpn-deploy-client-vpn-connections.md)

Dans cette étape, vous configurez les paramètres DNS et de pare-feu pour la connectivité VPN.

## <a name="configure-dns-name-resolution"></a>Configurer la résolution de noms DNS

Lorsque les clients VPN distants se connectent, ils utilisent les mêmes serveurs DNS que ceux utilisés par vos clients internes, ce qui leur permet de résoudre les noms de la même manière que les autres postes de travail internes.

Pour cette raison, vous devez vous assurer que le nom de l’ordinateur que les clients externes utilisent pour se connecter au serveur VPN correspond à l’autre nom de l’objet défini dans certificats émis pour le serveur VPN.

Pour vous assurer que les clients distants peuvent se connecter à votre serveur VPN, vous pouvez créer un enregistrement DNS A (hôte) dans votre zone DNS externe. L’enregistrement A doit utiliser l’autre nom de l’objet du certificat pour le serveur VPN.

### <a name="to-add-a-host-a-or-aaaa-resource-record-to-a-zone"></a>Pour ajouter un enregistrement de ressource hôte (A ou AAAA) à une zone

1. Sur un serveur DNS, dans Gestionnaire de serveur, sélectionnez **Outils**, puis **DNS**. Le Gestionnaire DNS s’ouvre.
2. Dans l’arborescence de la console du Gestionnaire DNS, sélectionnez le serveur que vous souhaitez gérer.
3. Dans le volet d’informations, dans **nom**, double-cliquez sur **zones de recherche directe** pour développer la vue.
4. Dans détails des **zones de recherche directe** , cliquez avec le bouton droit sur la zone de recherche directe à laquelle vous souhaitez ajouter un enregistrement, puis sélectionnez **nouvel hôte (a ou AAAA)** . La boîte **de dialogue nouvel hôte** s’ouvre.
5. Dans **nouvel hôte**, dans **nom**, entrez l’autre nom de l’objet du certificat pour le serveur VPN.
6. Dans adresse IP, entrez l’adresse IP du serveur VPN. Vous pouvez entrer l’adresse au format IP version 4 (IPv4) pour ajouter un enregistrement de ressource hôte (A) ou le format IP version 6 (IPv6) pour ajouter un enregistrement de ressource hôte (AAAA).
7. Si vous avez créé une zone de recherche inversée pour une plage d’adresses IP, y compris l’adresse IP que vous avez entrée, activez la case à cocher **créer un enregistrement de pointeur (PTR) associé** .  La sélection de cette option crée un enregistrement de ressource pointeur (PTR) supplémentaire dans une zone inversée pour cet hôte, en fonction des informations que vous avez entrées dans le **nom** et l' **adresse IP**.
8. Sélectionnez **Ajouter un ordinateur hôte**.

## <a name="configure-the-edge-firewall"></a>Configurer le pare-feu de périmètre

Le pare-feu de périmètre sépare le réseau de périmètre externe de l’Internet public. Pour obtenir une représentation visuelle de cette séparation, reportez-vous à l’illustration de la rubrique [Always on vue d’ensemble de la technologie VPN](../always-on-vpn-technology-overview.md).

Votre pare-feu de périmètre doit autoriser et transférer des ports spécifiques à votre serveur VPN. Si vous utilisez la traduction d’adresses réseau (NAT) sur votre pare-feu de périmètre, vous devrez peut-être activer le réacheminement de port pour les ports UDP (User Datagram Protocol) 500 et 4500. Transférez ces ports à l’adresse IP affectée à l’interface externe de votre serveur VPN.

Si vous routez le trafic entrant et que vous exécutez NAT au niveau ou derrière le serveur VPN, vous devez ouvrir vos règles de pare-feu pour autoriser les ports UDP 500 et 4500 entrants sur l’adresse IP externe appliquée à l’interface publique sur le serveur VPN.

Dans les deux cas, si votre pare-feu prend en charge l’inspection Poussée des paquets et que vous avez des difficultés à établir des connexions clientes, vous devez tenter de détendre ou désactiver l’inspection profonde des paquets pour les sessions IKE.

Pour plus d’informations sur l’utilisation de ces modifications de configuration, consultez la documentation de votre pare-feu.

## <a name="configure-the-internal-perimeter-network-firewall"></a>Configurer le pare-feu de réseau de périmètre interne

Le pare-feu de réseau de périmètre interne sépare le réseau de l’organisation/de l’entreprise du réseau de périmètre interne. Pour obtenir une représentation visuelle de cette séparation, reportez-vous à l’illustration de la rubrique [Always on vue d’ensemble de la technologie VPN](../always-on-vpn-technology-overview.md).

Dans ce déploiement, le serveur VPN d’accès à distance sur le réseau de périmètre est configuré en tant que client RADIUS.  Le serveur VPN envoie le trafic RADIUS au serveur NPS sur le réseau d’entreprise et reçoit également le trafic RADIUS du serveur NPS.

Configurez le pare-feu pour permettre au trafic RADIUS de circuler dans les deux sens.

>[!NOTE]
>Le serveur NPS sur le réseau de l’organisation/de l’entreprise fonctionne comme un serveur RADIUS pour le serveur VPN, qui est un client RADIUS. Pour plus d’informations sur l’infrastructure RADIUS, consultez [serveur NPS (Network Policy Server)](../../../../../networking/technologies/nps/nps-top.md).

### <a name="radius-traffic-ports-on-the-vpn-server-and-nps-server"></a>Ports de trafic RADIUS sur le serveur VPN et le serveur NPS

Par défaut, NPS et VPN écoutent le trafic RADIUS sur les ports 1812, 1813, 1645 et 1646 sur toutes les cartes réseau installées. Si vous activez le pare-feu Windows avec fonctions avancées de sécurité lors de l’installation du serveur NPS, des exceptions de pare-feu pour ces ports sont créées automatiquement au cours du processus d’installation pour le trafic IPv6 et IPv4.

>[!IMPORTANT]
>Si vos serveurs d’accès réseau sont configurés pour envoyer le trafic RADIUS sur des ports autres que ces paramètres par défaut, supprimez les exceptions créées dans le pare-feu Windows avec fonctions avancées de sécurité lors de l’installation du serveur NPS, et créez des exceptions pour les ports que vous utilisez pour Trafic RADIUS.

### <a name="use-the-same-radius-ports-for-the-internal-perimeter-network-firewall-configuration"></a>Utiliser les mêmes ports RADIUS pour la configuration du pare-feu du réseau de périmètre interne

Si vous utilisez la configuration de port RADIUS par défaut sur le serveur VPN et le serveur NPS, veillez à ouvrir les ports suivants sur le pare-feu de réseau de périmètre interne :

- Ports UDP1812, UDP1813, UDP1645 et UDP1646

Si vous n’utilisez pas les ports RADIUS par défaut dans votre déploiement NPS, vous devez configurer le pare-feu pour autoriser le trafic RADIUS sur les ports que vous utilisez. Pour plus d’informations, consultez [configurer des pare-feu pour le trafic RADIUS](../../../../../networking/technologies/nps/nps-firewalls-configure.md).

## <a name="next-steps"></a>Étapes suivantes

[Étape 6. Configurer le client Windows 10 Always On les connexions VPN](vpn-deploy-client-vpn-connections.md): au cours de cette étape, vous configurez les ordinateurs clients Windows 10 pour qu’ils communiquent avec cette infrastructure avec une connexion VPN. Vous pouvez utiliser plusieurs technologies pour configurer les clients VPN Windows 10, notamment Windows PowerShell, le point de terminaison Microsoft Configuration Manager et Intune. Les trois nécessitent un profil VPN XML pour configurer les paramètres VPN appropriés.