---
title: Configurer des informations de port UDP NPS
description: Vous pouvez utiliser cette rubrique pour configurer les ports que le serveur NPS (Network Policy Server) utilise pour le trafic d’authentification et de gestion des comptes protocole RADIUS (Remote Authentication Dial-In User Service) (RADIUS) dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 70569958-d7a7-474e-a817-6b7b5134784a
ms.author: lizross
author: eross-msft
ms.openlocfilehash: f874469666ab9b68d9eb970cf7fcb6a89ef27f0c
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80315570"
---
# <a name="configure-nps-udp-port-information"></a>Configurer des informations de port UDP NPS

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser la procédure suivante pour configurer les ports utilisés par le serveur NPS (Network Policy Server) pour protocole RADIUS (Remote Authentication Dial-In User Service) \(RADIUS\) le trafic d’authentification et de gestion des comptes.

Par défaut, NPS écoute le trafic RADIUS sur les ports 1812, 1813, 1645 et 1646 pour le protocole Internet version 6 \(IPv6\) et IPv4 pour toutes les cartes réseau installées.

>[!NOTE]
>Si vous désinstallez IPv4 ou IPv6 sur une carte réseau, NPS n’analyse pas le trafic RADIUS pour le protocole désinstallé.

Les valeurs de port 1812 pour l’authentification et 1813 pour la comptabilité sont les ports standard RADIUS définis par l’IETF (Internet Engineering Task Force \(IETF\) dans les RFC 2865 et 2866. Toutefois, par défaut, de nombreux serveurs d’accès utilisent les ports 1645 pour les demandes d’authentification et 1646 pour les demandes de gestion des comptes. Quel que soit le numéro de port que vous décidez d’utiliser, assurez-vous que NPS et votre serveur d’accès sont configurés pour utiliser les mêmes.

>PRÉCIEUSE Si vous n’utilisez pas les numéros de port par défaut RADIUS, vous devez configurer des exceptions sur le pare-feu de l’ordinateur local pour autoriser le trafic RADIUS sur les nouveaux ports. Pour plus d’informations, consultez [configurer des pare-feu pour le trafic RADIUS](nps-firewalls-configure.md).

L'appartenance au groupe **Admins du domaine**, ou équivalent, est la condition minimale requise pour effectuer cette procédure.

## <a name="to-configure-nps-udp-port-information"></a>Pour configurer les informations de port UDP NPS 

1. Ouvrez la console NPS.
2. Cliquez avec le bouton droit sur **Serveur NPS (Network Policy Server)** , puis cliquez sur **Propriétés**.
3. Cliquez sur l’onglet **ports** , puis examinez les paramètres des ports. Si vos ports UDP d’authentification RADIUS et de gestion des comptes RADIUS diffèrent des valeurs par défaut fournies (1812 et 1645 pour l’authentification, et 1813 et 1646 pour la gestion des comptes), tapez vos paramètres de port dans **l’authentification** et la **gestion des comptes**.
4. Pour utiliser plusieurs paramètres de port pour les demandes d’authentification ou de gestion des comptes, séparez les numéros de port par des virgules.

Pour plus d’informations sur la gestion de NPS, consultez [gérer un serveur de stratégie réseau](nps-manage-top.md).

Pour plus d’informations sur NPS, consultez [serveur NPS (Network Policy Server)](nps-top.md).
