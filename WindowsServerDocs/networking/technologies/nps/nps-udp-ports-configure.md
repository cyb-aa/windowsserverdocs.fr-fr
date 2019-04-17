---
title: Configurer les informations de Port UDP NPS
description: Vous pouvez utiliser cette rubrique pour configurer les ports utilisés par serveur NPS (Network Policy) pour l’authentification Authentication Dial-In utilisateur RADIUS (Remote Service) et le trafic de gestion dans Windows Server2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 70569958-d7a7-474e-a817-6b7b5134784a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f0e703dc6f9083f1e79091a6cee6d1ac58753d12
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="configure-nps-udp-port-information"></a>Configurer les informations de Port UDP NPS

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser la procédure suivante pour configurer les ports utilisés par serveur NPS (Network Policy) pour l’authentification \(RADIUS\) Remote Authentication Dial-In User Service et le trafic de gestion.

Par défaut, le serveur NPS écoute le trafic RADIUS sur les ports 1812, 1813, 1645 et 1646 pour Internet Protocol version6 \(IPv6\) et IPv4 pour toutes les cartes réseau installées.

>[!NOTE]
>Si vous désinstallez IPv4 ou IPv6 sur une carte réseau, NPS ne surveille pas le trafic RADIUS pour le protocole désinstallé.

Les valeurs de port 1812 pour l’authentification et 1813 pour la gestion des comptes sont définies par la Internet Engineering Task Force \(IETF\) dans les RFC2865 et 2866des ports standard RADIUS. Toutefois, par défaut, de nombreux serveurs d’accès utilisent les ports 1645 pour les demandes d’authentification et 1646 pour les demandes de gestion. Quel que soit les numéros de port que vous décidez d’utiliser, assurez-vous que le serveur NPS et votre serveur d’accès sont configurés pour utiliser les mêmes que.

>[IMPORTANT] Si vous n’utilisez pas les numéros de port par défaut RADIUS, vous devez configurer des exceptions sur le pare-feu de l’ordinateur local Autoriser le trafic RADIUS sur les nouveaux ports. Pour plus d’informations, voir [configurer le pare-feu pour le trafic RADIUS](nps-firewalls-configure.md).

L’appartenance au groupe **Admins du domaine**, ou équivalente, est la condition minimale requise pour effectuer cette procédure.

## <a name="to-configure-nps-udp-port-information"></a>Pour configurer les informations de port UDP NPS 

1. Ouvrez la console NPS.
2. Avec le bouton droit **serveur NPS**, puis cliquez sur **propriétés**.
3. Cliquez sur le **Ports** onglet, puis examinez les paramètres pour les ports. Si votre authentification RADIUS et les ports UDP de gestion de comptes RADIUS diffèrent des valeurs par défaut fournies (1812 et 1645 pour l’authentification et 1813 et 1646 pour la gestion des comptes), tapez vos paramètres de port dans **authentification** et **comptabilité**.
4. Pour utiliser plusieurs paramètres de port pour l’authentification ou les demandes de gestion, séparez les numéros de port par des virgules.

Pour plus d’informations sur la gestion du serveur NPS, voir [gérer un serveur de stratégie réseau](nps-manage-top.md).

Pour plus d’informations sur le serveur NPS, voir [serveur NPS (Network Policy)](nps-top.md).
