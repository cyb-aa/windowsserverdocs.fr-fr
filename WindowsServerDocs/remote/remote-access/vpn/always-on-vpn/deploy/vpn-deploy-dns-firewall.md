---
title: Configurer DNS et les paramètres de pare-feu
description: Cette rubrique fournit des instructions détaillées pour le déploiement VPN Always On dans Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: d8cf3bae-45bf-4ffa-9205-290d555c59da
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 06/11/2018
ms.openlocfilehash: f57bfa005ef1af2556e4bd1cbb90ef8d3cfd22e3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886250"
---
# <a name="step-5-configure-dns-and-firewall-settings"></a>Étape 5. Configurer les paramètres de pare-feu et de DNS

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows 10

&#171;  [**Précédent :** Étape 4. Installer et configurer le serveur NPS](vpn-deploy-nps.md)<br>
&#187;  [**prochain :** Étape 6. Configurer les clients Windows 10 toujours sur les connexions VPN](vpn-deploy-client-vpn-connections.md)

Dans cette étape, vous configurez DNS et pare-feu paramètres pour la connectivité VPN.

## <a name="configure-dns-name-resolution"></a>Configurer la résolution de nom DNS

Lorsque les clients VPN distants se connectent, ils utilisent les mêmes serveurs DNS par vos clients internes, ce qui leur permet de résoudre les noms de la même manière que le reste de vos postes de travail internes. 

Pour cette raison, vous devez vous assurer que le nom d’ordinateur que les clients externes utilisent pour se connecter au serveur VPN correspond à l’autre nom d’objet défini dans les certificats émis pour le serveur VPN.

Pour vous assurer que les clients distants peuvent se connecter à votre serveur VPN, vous pouvez créer un enregistrement DNS A (hôte) dans votre zone DNS externe. L’enregistrement A doit utiliser le nom alternatif de sujet de certificat pour le serveur VPN.


### <a name="to-add-a-host-a-or-aaaa-resource-record-to-a-zone"></a>Pour ajouter un hôte \(A ou AAAA\) enregistrement de ressource à une zone

1. Sur un serveur DNS, dans le Gestionnaire de serveur, cliquez sur **outils**, puis cliquez sur **DNS**. Le Gestionnaire DNS s’ouvre.
2. Dans l’arborescence de la console Gestionnaire DNS, cliquez sur le serveur que vous souhaitez gérer.
3. Dans le volet de détails, dans **nom**, double\-cliquez sur **Zones de recherche directe** pour développer la vue.
4. Dans **Zones de recherche directe** décrit en détail, droite\-cliquez sur la zone de recherche directe auquel vous souhaitez ajouter un enregistrement, puis cliquez sur **nouvel hôte \(A ou AAAA\)**. Le **nouvel hôte** boîte de dialogue s’ouvre.
5. Dans **nouvel hôte**, dans **nom**, tapez le nom alternatif de sujet de certificat pour le serveur VPN.
6. Dans adresse IP, tapez l’adresse IP du serveur VPN. Vous pouvez taper l’adresse IP version 4 (IPv4) format pour ajouter un hôte \(A\) enregistrement de ressource ou IP version 6 \(IPv6\) format pour ajouter un hôte \(AAAA\) enregistrement de ressource.
7. Si vous avez créé une zone de recherche inversée pour une plage d’adresses IP, y compris l’adresse IP que vous avez tapé, puis sélectionnez le **créer un enregistrement de pointeur (PTR) associé** case à cocher.  Cette option crée un pointeur supplémentaire \(PTR\) enregistrement de ressource dans un ordre inverse de la zone pour cet hôte, en fonction des informations que vous avez entré dans **nom** et **adresse IP**.
8. Cliquez sur **Ajouter un hôte**.

## <a name="configure-the-edge-firewall"></a>Configurer le pare-feu de périmètre

Le pare-feu de périmètre sépare le réseau de périmètre externe à partir de l’Internet Public. Pour obtenir une représentation visuelle de cette séparation, consultez l’illustration dans la rubrique [toujours sur VPN présentation de la technologie](../always-on-vpn-technology-overview.md).

Votre pare-feu de périmètre doit autoriser et renvoyer des ports spécifiques sur votre serveur VPN. Si vous utilisez la traduction d’adresses réseau \(NAT\) sur votre pare-feu de périmètre, vous devrez peut-être activer le transfert de port pour User Datagram Protocol \(UDP\) ports 500 et 4500. Transférer ces ports à l’adresse IP affectée à l’interface externe de votre serveur VPN.

Si vous êtes le routage du trafic entrant et l’exécution de NAT à ou derrière le serveur VPN, puis vous devez ouvrir vos règles de pare-feu pour autoriser UDP ports 500 et 4500 entrant à l’adresse IP externe appliqué à l’interface publique sur le serveur VPN.

Dans les deux cas, si votre pare-feu prend en charge l’inspection approfondie des paquets et que vous rencontrez des difficultés pour l’établissement de connexions de client, vous devez tenter Assouplissez ou désactivez l’inspection approfondie des paquets pour les sessions d’IKE.

Pour plus d’informations sur la façon d’apporter ces modifications de configuration, consultez la documentation de votre pare-feu.

## <a name="configure-the-internal-perimeter-network-firewall"></a>Configurer le pare-feu de réseau de périmètre interne

Le pare-feu de réseau de périmètre interne sépare le réseau de l’organisation / d’entreprise à partir du réseau de périmètre interne. Pour obtenir une représentation visuelle de cette séparation, consultez l’illustration dans la rubrique [toujours sur VPN présentation de la technologie](../always-on-vpn-technology-overview.md).

Dans ce déploiement, le serveur d’accès à distance VPN sur le réseau de périmètre est configuré comme client RADIUS.  Le serveur VPN envoie le trafic RADIUS au serveur NPS sur le réseau d’entreprise et reçoit également le trafic RADIUS à partir du serveur NPS.

Configurer le pare-feu pour autoriser le trafic RADIUS à circuler dans les deux sens.


>[!NOTE]
>Le serveur NPS sur le réseau d’entreprise organisation fonctionne comme un serveur RADIUS pour le serveur VPN, qui est un Client RADIUS. Pour plus d’informations sur l’infrastructure RADIUS, consultez [serveur NPS (Network Policy Server)](../../../../../networking/technologies/nps/nps-top.md).

### <a name="radius-traffic-ports-on-the-vpn-server-and-nps-server"></a>Ports du trafic RADIUS sur le serveur VPN et le serveur NPS

Par défaut, VPN et NPS écoutent le trafic RADIUS sur les ports 1812, 1813, 1645 et 1646 sur toutes les cartes réseau installées. Si vous activez le pare-feu Windows avec fonctions avancées de sécurité lors de l’installation du serveur NPS, les exceptions de pare-feu pour ces ports sont automatiquement créées pendant le processus d’installation pour le trafic IPv6 et IPv4.

>[!IMPORTANT]
>Si vos serveurs d’accès réseau sont configurés pour envoyer le trafic RADIUS sur des ports autres que ces valeurs par défaut, supprimez les exceptions créées dans le pare-feu Windows avec fonctions avancées de sécurité pendant l’installation du serveur NPS et créer des exceptions pour les ports que vous n’utilisez pas pour Trafic RADIUS.

### <a name="use-the-same-radius-ports-for-the-internal-perimeter-network-firewall-configuration"></a>Utiliser les mêmes Ports RADIUS pour la Configuration de pare-feu de réseau de périmètre interne

Si vous utilisez la configuration du port par défaut RADIUS sur le serveur VPN et le serveur NPS, assurez-vous que vous ouvrez les ports suivants sur le pare-feu de réseau de périmètre interne :

- Ports UDP1812, UDP1813, UDP1645 et UDP1646

Si vous n’utilisez pas les ports de rayon par défaut dans votre déploiement de NPS, vous devez configurer le pare-feu pour autoriser le trafic RADIUS sur les ports que vous utilisez. Pour plus d’informations, consultez [configurer le pare-feu pour le trafic RADIUS](../../../../../networking/technologies/nps/nps-firewalls-configure.md).

## <a name="next-step"></a>Étape suivante
[Étape 6. Configurer les clients Windows 10 toujours sur les connexions VPN](vpn-deploy-client-vpn-connections.md): Dans cette étape, vous configurez les ordinateurs clients Windows 10 pour communiquer avec cette infrastructure avec une connexion VPN. Vous pouvez utiliser plusieurs technologies pour configurer les clients VPN Windows 10, y compris Windows PowerShell, System Center Configuration Manager et Intune. Les trois nécessitent un profil VPN XML pour configurer les paramètres VPN appropriés. 

---
