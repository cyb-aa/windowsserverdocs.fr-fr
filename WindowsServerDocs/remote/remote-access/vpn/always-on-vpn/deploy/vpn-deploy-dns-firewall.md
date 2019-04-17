---
title: Configurer DNS et les paramètres de pare-feu
description: Cette rubrique fournit des instructions détaillées pour le déploiement de toujours actif (AlwaysOn) dans Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: d8cf3bae-45bf-4ffa-9205-290d555c59da
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 06/11/2018
ms.openlocfilehash: f57bfa005ef1af2556e4bd1cbb90ef8d3cfd22e3
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067128"
---
# Étape5. Configurer les paramètres de pare-feu et de DNS

>S’applique à: Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows 10

& #171;  [ **Précédente:** étape 4. Installer et configurer le serveur NPS](vpn-deploy-nps.md)<br>
& #187;  [ **Suivant:** étape 6. Configurer le Client Windows 10 toujours sur des connexions VPN](vpn-deploy-client-vpn-connections.md)

Dans cette étape, vous configurez DNS et le pare-feu paramètres pour une connexion VPN.

## Configurer la résolution de nom DNS

Lorsque les clients VPN distants connectés, ils utilisent les mêmes serveurs DNS par vos clients internes, ce qui leur permet de résoudre les noms de la même manière que le reste de vos stations de travail internes. 

Pour cette raison, vous devez vous assurer que le nom d’ordinateur par les clients externes pour se connecter au serveur VPN correspond à l’autre nom du sujet défini dans les certificats émis vers le serveur VPN.

Pour vous assurer que les clients à distance peuvent se connecter à votre serveur VPN, vous pouvez créer un enregistrement DNS A (hôte) dans votre zone DNS externe. L’enregistrement A doit utiliser le certificat autre nom du sujet du serveur VPN.


### Pour ajouter un hôte \ (A ou AAAA\) enregistrement de ressource à une zone

1. Sur un serveur DNS, dans le Gestionnaire de serveur, cliquez sur **Outils**, puis cliquez sur **DNS**. Le Gestionnaire DNS s’ouvre.
2. Dans l’arborescence de la console Gestionnaire DNS, cliquez sur le serveur que vous souhaitez gérer.
3. Dans le volet d’informations, dans la zone **nom**, double\-cliquez sur **Zones de recherche directe** pour développer la vue.
4. Dans les détails des **Zones de recherche directe** , right\-cliquez sur la zone de recherche directe pour laquelle vous souhaitez ajouter un enregistrement, puis cliquez sur **nouvel hôte \(A or AAAA\)**. La boîte de dialogue **Nouvel hôte** s’ouvre.
5. Dans le **Nouvel hôte**, dans la zone **nom**, tapez le nom du sujet autre certificat du serveur VPN.
6. Dans la zone adresse IP, tapez l’adresse IP du serveur VPN. Vous pouvez taper l’adresse IP (IPv4) version 4 pour ajouter un enregistrement de ressource hôte \(A\) ou IP version 6 \(IPv6\) au format pour ajouter un enregistrement de ressource hôte \(AAAA\).
7. Si vous avez créé une zone de recherche inversée pour une plage d’adresses IP, y compris l’adresse IP que vous avez tapé, puis sélectionnez la case à cocher **créer un enregistrement de pointeur (PTR) associé** .  Cette option crée un enregistrement de ressource \(PTR\) pointeur supplémentaire dans une zone de recherche inversée pour cet hôte, basé sur les informations que vous avez entré dans **nom** et **adresse IP**.
8. Cliquez sur **Ajouter un hôte**.

## Configurer le pare-feu de périmètre

Le pare-feu de périmètre sépare le réseau de périmètre externe à partir de l’Internet Public. Pour obtenir une représentation visuelle de cette séparation, voir la figure dans la rubrique [Toujours sur VPN présentation de la technologie](../always-on-vpn-technology-overview.md).

Votre pare-feu de périmètre doit autoriser et transférer des ports spécifiques à votre serveur VPN. Si vous utilisez \(NAT\) traduction d’adresses réseau sur votre pare-feu de périmètre, vous devrez peut-être activer le transfert de port pour User Datagram Protocol \(UDP\) ports500 et 4500. Transmettre ces ports à l’adresse IP qui est attribuée à l’interface externe de votre serveur VPN.

Si vous êtes le routage du trafic entrant et effectuer ces NAT à ou derrière le serveur VPN, puis vous devez ouvrir vos règles de pare-feu pour autoriser ports500 UDP et 4500 entrante à l’adresse IP externe appliqué à l’interface publique sur le serveur VPN.

Dans les deux cas, si votre pare-feu prend en charge approfondie des paquets et que vous avez des difficultés pour établir des connexions client, vous devez essayer d’alléger ou désactivez l’inspection approfondie des paquets pour les sessions IKE.

Pour plus d’informations sur la façon d’apporter les modifications de configuration, voir la documentation du pare-feu.

## Configurer le pare-feu de réseau de périmètre interne

Le pare-feu interne du réseau de périmètre sépare le réseau d’entreprise/entreprise à partir du réseau de périmètre interne. Pour obtenir une représentation visuelle de cette séparation, voir la figure dans la rubrique [Toujours sur VPN présentation de la technologie](../always-on-vpn-technology-overview.md).

Dans ce déploiement, le serveur VPN accès à distance sur le réseau de périmètre est configuré comme un client RADIUS.  Le serveur VPN envoie le trafic RADIUS au serveur NPS sur le réseau d’entreprise et reçoit également le trafic RADIUS dans le serveur NPS.

Configurer le pare-feu pour autoriser le trafic RADIUS dans les deux sens.


>[!NOTE]
>Le serveur NPS sur le réseau d’entreprise/entreprise fonctionne comme un serveur RADIUS pour le serveur VPN, qui est un Client RADIUS. Pour plus d’informations sur l’infrastructure RADIUS, voir le [Serveur NPS (Network Policy Server)](../../../../../networking/technologies/nps/nps-top.md).

### Ports de trafic RADIUS sur le serveur VPN et le serveur NPS

Par défaut, NPS et VPN écoutent pour le trafic RADIUS sur les ports 1812, 1813, 1645 et 1646 sur toutes les cartes réseau installées. Si vous activez le pare-feu Windows avec fonctions avancées de sécurité lors de l’installation de NPS, exceptions de pare-feu pour ces ports sont automatiquement créées au cours du processus d’installation pour le trafic IPv6 et IPv4.

>[!IMPORTANT]
>Si vos serveurs d’accès réseau sont configurés pour envoyer le trafic RADIUS sur les ports autres que ces valeurs par défaut, supprimez les exceptions créées dans le pare-feu Windows avec fonctions avancées de sécurité lors de l’installation de NPS et créer des exceptions pour les ports que vous utilisez pour Trafic RADIUS.

### Utiliser les mêmes Ports de RADIUS pour la Configuration de pare-feu de réseau de périmètre interne

Si vous utilisez la configuration du port RADIUS par défaut sur le serveur VPN et le serveur NPS, assurez-vous que vous ouvrez les ports suivants sur le pare-feu interne du réseau de périmètre:

- Ports UDP1812, UDP1813, UDP1645 et UDP1646

Si vous n’utilisez pas les ports RADIUS par défaut dans votre déploiement de serveur NPS, vous devez configurer le pare-feu pour autoriser le trafic RADIUS sur les ports que vous utilisez. Pour plus d’informations, consultez [Configurer le pare-feu pour le trafic RADIUS](../../../../../networking/technologies/nps/nps-firewalls-configure.md).

## Étape suivante
[Étape 6. Configurer Windows 10 toujours sur les connexions VPN Client](vpn-deploy-client-vpn-connections.md): dans cette étape, vous configurez les ordinateurs clients de Windows 10 pour communiquer avec cette infrastructure avec une connexion VPN. Vous pouvez utiliser plusieurs technologies pour configurer des clients VPN Windows 10, y compris Windows PowerShell, System Center Configuration Manager et Intune. Les trois nécessitent un profil VPN XML pour configurer les paramètres VPN appropriés. 

---
