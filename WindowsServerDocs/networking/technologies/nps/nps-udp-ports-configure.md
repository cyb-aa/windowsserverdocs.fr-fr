---
title: Configurer des informations de port UDP NPS
description: Vous pouvez utiliser cette rubrique pour configurer les ports de serveur NPS (Network Policy Server) utilise pour l’authentification de l’authentification Dial-In Service RADIUS (Remote User) et le trafic de gestion dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 70569958-d7a7-474e-a817-6b7b5134784a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 44c20092180c47e97f1505271203f4491606bbcf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851970"
---
# <a name="configure-nps-udp-port-information"></a>Configurer des informations de port UDP NPS

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser la procédure suivante pour configurer les ports de serveur NPS (Network Policy Server) utilise pour Remote Authentication Dial-In User Service \(RADIUS\) l’authentification et le trafic de gestion.

Par défaut, le serveur NPS écoute le trafic RADIUS sur les ports 1812, 1813, 1645 et 1646 pour Internet Protocol version 6 \(IPv6\) et IPv4 pour toutes les cartes réseau installées.

>[!NOTE]
>Si vous désinstallez IPv4 ou IPv6 sur une carte réseau, NPS ne surveille pas le trafic RADIUS pour le protocole désinstallé.

Les valeurs de port 1812 pour l’authentification et 1813 pour la gestion des comptes sont les ports RADIUS standard définis par l’Internet Engineering Task Force \(IETF\) dans les RFC 2865 et 2866. Toutefois, par défaut, plusieurs serveurs d’accès utilisent les ports 1645 pour les demandes d’authentification et 1646 pour les demandes de gestion. Quel que soit les numéros de port que vous décidez d’utiliser, assurez-vous que le serveur NPS et votre serveur d’accès sont configurés pour utiliser les mêmes que celles.

>[IMPORTANT] Si vous n’utilisez pas les numéros de port par défaut RADIUS, vous devez configurer des exceptions sur le pare-feu pour l’ordinateur local Autoriser le trafic RADIUS sur les nouveaux ports. Pour plus d’informations, consultez [configurer le pare-feu pour le trafic RADIUS](nps-firewalls-configure.md).

L'appartenance au groupe **Admins du domaine**, ou équivalent, est la condition minimale requise pour effectuer cette procédure.

## <a name="to-configure-nps-udp-port-information"></a>Pour configurer les informations de port UDP de NPS 

1. Ouvrez la console NPS.
2. Cliquez avec le bouton droit sur **Serveur NPS (Network Policy Server)**, puis cliquez sur **Propriétés**.
3. Cliquez sur le **Ports** onglet et examinez les paramètres des ports. Si votre authentification RADIUS et les ports UDP de gestion des comptes RADIUS diffèrent des valeurs par défaut fournies (1812 et 1645 pour l’authentification et 1813 et 1646 pour la gestion des comptes), tapez vos paramètres de port dans **authentification** et  **Comptabilité**.
4. Pour utiliser plusieurs paramètres de port pour l’authentification ou les demandes de gestion, séparez les numéros de port par des virgules.

Pour plus d’informations sur la gestion de serveur NPS, consultez [gérer un serveur de stratégie réseau](nps-manage-top.md).

Pour plus d’informations sur le serveur NPS, consultez [serveur NPS (Network Policy Server)](nps-top.md).
