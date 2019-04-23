---
title: Nouveautés de DHCP
description: Cette rubrique fournit une vue d’ensemble des nouvelles fonctionnalités pour DHCP Dynamic Host Configuration Protocol () dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dhcp
ms.topic: get-started-article
ms.assetid: c6f36998-5b64-45d1-b1f0-0f0d6604dbe3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 73cc5134f7af5063c912ad578fa7d660b3194aa1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840230"
---
# <a name="whats-new-in-dhcp"></a>Nouveautés de DHCP

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique décrit les fonctionnalités de Configuration protocole DHCP (Dynamic Host) qui sont nouvelles ou modifiées dans Windows Server 2016.
  
DHCP est une norme Internet Engineering Task Force (IETF) qui est conçue pour réduire la charge administrative et la complexité de la configuration des hôtes sur un TCP/IP\-en fonction du réseau, tel qu’un intranet privé. Le recours au service Serveur DHCP permet d’automatiser le processus de configuration de TCP/IP sur les clients DHCP.

Les sections suivantes fournissent des informations sur les nouvelles fonctionnalités et modifications de fonctionnalités pour DHCP.

## <a name="dhcp-subnet-selection-options"></a>Options de sélection de sous-réseau DHCP

DHCP prend désormais en charge les options 118 et 82 \(sous-composant option 5\). Vous pouvez utiliser ces options pour autoriser les clients de proxy DHCP et les agents de relais demander une adresse IP d’un sous-réseau spécifique et d’une plage d’adresses IP et de portée.


Si vous utilisez un agent de relais DHCP qui est configuré avec l’option DHCP 82, sub\-option 5, l’agent de relais peut demander un bail d’adresse IP pour les clients DHCP à partir d’une plage d’adresses IP spécifique.

Pour plus d’informations, consultez [les Options de sélection de sous-réseau DHCP](dhcp-subnet-options.md).

## <a name="new-logging-events-for-dns-registration-failures-by-the-dhcp-server"></a>Nouveaux événements de journalisation pour les échecs d’inscription DNS par le serveur DHCP

DHCP inclut désormais la journalisation d’événements pour les circonstances dans DHCP des enregistrements DNS server échouent sur le serveur DNS.

Pour plus d’informations, consultez [DHCP des événements de journalisation pour les inscriptions d’enregistrement DNS](dhcp-dns-events.md).

## <a name="dhcp-nap-is-not-supported-in-windows-server-2016"></a>NAP par DHCP n’est pas pris en charge dans Windows Server 2016

Protection d’accès réseau \(NAP\) est déconseillée dans Windows Server 2012 R2 et dans le rôle serveur DHCP de Windows Server 2016 ne prend plus en charge NAP. Pour plus d’informations, consultez [fonctionnalités supprimées ou déconseillées dans Windows Server 2012 R2](https://technet.microsoft.com/library/dn303411.aspx).  
  
Prise en charge NAP a été introduit pour le rôle de serveur DHCP avec Windows Server 2008 et ne prend en charge avant Windows 10 et Windows Server 2016 dans les systèmes d’exploitation de client et serveur Windows. Le tableau suivant résume la prise en charge pour la protection NAP dans Windows Server.  
  
|Système d’exploitation|Prise en charge de la protection d’accès réseau|  
|--------------------|---------------|  
| Windows Server 2008 |Prise en charge|  
| Windows Server 2008 R2 |Prise en charge|  
| Windows Server 2012 |Prise en charge|  
| Windows Server 2012 R2 |Prise en charge|  
| Windows Server 2016|Non pris en charge|  
  
Dans un déploiement NAP, un serveur DHCP exécutant un système d’exploitation qui prend en charge NAP peut fonctionner comme un point de mise en conformité NAP pour la méthode de mise en conformité NAP par DHCP. Pour plus d’informations sur DHCP NAP, consultez [liste de vérification : Implémentation d’une conception de mise en conformité DHCP](https://technet.microsoft.com/library/dd314186.aspx).  
  
Dans Windows Server 2016, les serveurs DHCP n’appliquent pas les stratégies NAP et les étendues DHCP ne peut pas être NAP\-activé. Ordinateurs clients DHCP qui sont également les clients NAP envoient une déclaration d’intégrité \(SoH\) avec la demande DHCP. Si le serveur DHCP est en cours d’exécution Windows Server 2016, ces demandes sont traitées comme si aucune déclaration d’intégrité n’est présente. Le serveur DHCP accorde un bail DHCP normal au client. 

Si les serveurs qui exécutent Windows Server 2016 sont des proxys RADIUS qui transfère les demandes d’authentification sur un serveur de stratégie réseau \(NPS\) qui prend en charge NAP, ces clients NAP sont évaluées par NPS en tant que non NAP\-capable, et Échec du traitement de NAP.
  
## <a name="see-also"></a>Voir aussi  
  
-   [Dynamic Host Configuration Protocol (DHCP)](Dynamic-Host-Configuration-Protocol--DHCP-.md)  
  

