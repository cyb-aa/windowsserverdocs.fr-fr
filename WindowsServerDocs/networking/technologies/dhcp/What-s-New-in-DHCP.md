---
title: Quelles sont les nouveautés dans DHCP
description: Cette rubrique fournit une vue d’ensemble des nouvelles fonctionnalités pour DHCP Dynamic Host Configuration Protocol () dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dhcp
ms.topic: get-started-article
ms.assetid: c6f36998-5b64-45d1-b1f0-0f0d6604dbe3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f67e5dfd8aefcf408a41f6fc919665419f0ff1c9
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="whats-new-in-dhcp"></a>Quelles sont les nouveautés dans DHCP

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Cette rubrique décrit les fonctionnalités de Dynamic Host Configuration Protocol (DHCP) qui sont nouvelles ou modifiées dans Windows Server 2016.
  
DHCP est une norme Internet Engineering Task Force (IETF) qui est conçue pour réduire la charge administrative et la complexité de la configuration des hôtes sur un réseau basé sur TCP/IP\, tel qu’un intranet privé. En utilisant le service serveur DHCP, le processus de configuration TCP/IP sur les clients DHCP est automatique.

Les sections suivantes fournissent des informations sur les nouvelles fonctionnalités et modifications de fonctionnalités pour DHCP.

## <a name="dhcp-subnet-selection-options"></a>Options de sélection de sous-réseau DHCP

DHCP prend désormais en charge les options 118 et 82 \(sub-option 5\). Vous pouvez utiliser ces options pour autoriser les clients du proxy DHCP et les agents de relais demander une adresse IP d’un sous-réseau spécifique et à partir d’une plage d’adresses IP et l’étendue.

Si vous utilisez un client du proxy DHCP configuré avec l’option DHCP 118, tel qu’un serveur \(VPN\) de réseau privé virtuel qui exécute Windows Server 2016 et le rôle de serveur d’accès à distance, le serveur VPN peut demander un bail d’adresse IP pour les clients VPN à partir d’une plage d’adresses IP spécifique.

Si vous utilisez un agent de relais DHCP qui est configuré avec l’option DHCP 82, sub\-option 5, l’agent de relais peut demander un bail d’adresse IP pour les clients DHCP à partir d’une plage d’adresses IP spécifique.

Pour plus d’informations, voir [les Options de sélection de sous-réseau DHCP](dhcp-subnet-options.md).

## <a name="new-logging-events-for-dns-registration-failures-by-the-dhcp-server"></a>Nouveaux événements de journalisation pour les échecs d’inscription DNS par le serveur DHCP

DHCP inclut désormais la journalisation des événements pour les circonstances dans DHCP des enregistrements DNS server échouent sur le serveur DNS.

Pour plus d’informations, voir [des événements de journalisation DHCP pour les inscriptions d’enregistrement DNS](dhcp-dns-events.md).

## <a name="dhcp-nap-is-not-supported-in-windows-server-2016"></a>NAP par DHCP n’est pas pris en charge dans Windows Server 2016

Réseau la Protection d’accès \(NAP\) est obsolète dans Windows Server 2012 R2 et Windows Server 2016 le rôle serveur DHCP n’est plus prend en charge NAP. Pour plus d’informations, voir [fonctionnalités supprimées ou déconseillées dans Windows Server 2012 R2](https://technet.microsoft.com/library/dn303411.aspx).  
  
Prise en charge de la protection d’accès réseau a été introduit pour le rôle de serveur DHCP dans Windows Server 2008 et est pris en charge dans Windows client et serveur de systèmes d’exploitation antérieurs à Windows 10 et Windows Server 2016. Le tableau suivant résume la prise en charge de la protection d’accès réseau dans Windows Server.  
  
|Système d'exploitation|Prise en charge de la protection d’accès réseau|  
|--------------------|---------------|  
| Windows Server2008 |Prise en charge|  
| Windows Server2008R2 |Prise en charge|  
| Windows Server2012 |Prise en charge|  
| Windows Server2012R2 |Prise en charge|  
| Windows Server2016|Non pris en charge|  
  
Dans un déploiement NAP, un serveur DHCP exécutant un système d’exploitation qui prend en charge la protection d’accès réseau peut fonctionner comme un point de mise en œuvre NAP pour la méthode de mise en œuvre NAP DHCP. Pour plus d’informations sur le protocole DHCP dans la protection d’accès réseau, voir [liste de vérification: implémentation d’une conception de contrainte DHCP](https://technet.microsoft.com/library/dd314186.aspx).  
  
Dans Windows Server 2016, les serveurs DHCP n’appliquent pas les stratégies NAP et étendues DHCP ne peut pas être activé pour NAP\. Les ordinateurs clients DHCP qui sont également des clients NAP envoient une déclaration de \(SoH\) d’intégrité avec la requête DHCP. Si le serveur DHCP s’exécute Windows Server 2016, ces demandes sont traitées comme si aucune déclaration d’intégrité n’est présente. Le serveur DHCP accorde un bail DHCP normal pour le client. 

Si les serveurs qui exécutent Windows Server 2016 sont les proxys RADIUS à transférer les demandes d’authentification à un serveur de stratégie réseau \(NPS\) qui prend en charge la protection d’accès réseau, ces clients NAP sont évaluées par NPS en tant que compatibles non NAP\ et NAP Échec du traitement.
  
## <a name="see-also"></a>Voir aussi  
  
-   [Dynamic Host Configuration Protocol (DHCP)](Dynamic-Host-Configuration-Protocol--DHCP-.md)  
  

