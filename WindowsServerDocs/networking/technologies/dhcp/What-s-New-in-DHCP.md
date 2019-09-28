---
title: Nouveautés de DHCP
description: Cette rubrique fournit une vue d’ensemble des nouvelles fonctionnalités du protocole DHCP (Dynamic Host Configuration Protocol) dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dhcp
ms.topic: get-started-article
ms.assetid: c6f36998-5b64-45d1-b1f0-0f0d6604dbe3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8032b7c8e78170d57b0367775672577d9fd900e2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355453"
---
# <a name="whats-new-in-dhcp"></a>Nouveautés de DHCP

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Cette rubrique décrit les nouveautés et les modifications apportées à la fonctionnalité DHCP (Dynamic Host Configuration Protocol) dans Windows Server 2016.
  
DHCP est une norme IETF (Internet Engineering Task Force) conçue pour réduire la charge et la complexité d’administration de la configuration des hôtes sur un réseau TCP/IP @ no__t-0based, tel qu’un intranet privé. Le recours au service Serveur DHCP permet d’automatiser le processus de configuration de TCP/IP sur les clients DHCP.

Les sections suivantes fournissent des informations sur les nouvelles fonctionnalités et les modifications apportées aux fonctionnalités de DHCP.

## <a name="dhcp-subnet-selection-options"></a>Options de sélection du sous-réseau DHCP

DHCP prend désormais en charge les options 118 et 82 \(sub-option 5 @ no__t-1. Vous pouvez utiliser ces options pour autoriser les clients de proxy et les agents de relais DHCP à demander une adresse IP pour un sous-réseau spécifique, et à partir d’une plage d’adresses IP et d’une étendue spécifiques.


Si vous utilisez un agent de relais DHCP configuré avec l’option DHCP 82, Sub @ no__t-0option 5, l’agent de relais peut demander un bail d’adresse IP pour les clients DHCP à partir d’une plage d’adresses IP spécifique.

Pour plus d’informations, consultez [options de sélection du sous-réseau DHCP](dhcp-subnet-options.md).

## <a name="new-logging-events-for-dns-registration-failures-by-the-dhcp-server"></a>Nouveaux événements de journalisation pour les échecs d’inscription DNS par le serveur DHCP

DHCP comprend désormais des événements de journalisation dans les cas où les inscriptions d’enregistrements DNS du serveur DHCP échouent sur le serveur DNS.

Pour plus d’informations, consultez [événements de journalisation DHCP pour les inscriptions d’enregistrements DNS](dhcp-dns-events.md).

## <a name="dhcp-nap-is-not-supported-in-windows-server-2016"></a>La protection d’accès réseau DHCP n’est pas prise en charge dans Windows Server 2016

La protection d’accès réseau \(NAP @ no__t-1 est déconseillée dans Windows Server 2012 R2 et dans Windows Server 2016 le rôle de serveur DHCP ne prend plus en charge la protection d’accès réseau. Pour plus d’informations, voir [fonctionnalités supprimées ou déconseillées dans Windows Server 2012 R2](https://technet.microsoft.com/library/dn303411.aspx).  
  
La prise en charge de la protection d’accès réseau a été introduite pour le rôle serveur DHCP avec Windows Server 2008 et est prise en charge dans les systèmes d’exploitation client et serveur Windows antérieurs à Windows 10 et Windows Server 2016. Le tableau suivant résume la prise en charge de la protection d’accès réseau dans Windows Server.  
  
|Système d'exploitation|Prise en charge NAP|  
|--------------------|---------------|  
| Windows Server 2008 |Prise en charge|  
| Windows Server 2008 R2 |Prise en charge|  
| Windows Server 2012 |Prise en charge|  
| Windows Server 2012 R2 |Prise en charge|  
| Windows Server 2016|Non pris en charge|  
  
Dans un déploiement NAP, un serveur DHCP exécutant un système d’exploitation prenant en charge la protection d’accès réseau peut fonctionner comme point de contrainte de mise en conformité NAP pour la méthode de contrainte de mise en conformité DHCP NAP. Pour plus d’informations sur DHCP dans NAP, voir [Checklist : Implémentation d’une conception d’application DHCP @ no__t-0.  
  
Dans Windows Server 2016, les serveurs DHCP n’appliquent pas les stratégies NAP, et les étendues DHCP ne peuvent pas être NAP @ no__t-0enabled. Les ordinateurs clients DHCP qui sont également des clients NAP envoient une déclaration d’intégrité \(SoH @ no__t-1 avec la requête DHCP. Si le serveur DHCP exécute Windows Server 2016, ces demandes sont traitées comme si aucune SoH n’est présente. Le serveur DHCP accorde un bail DHCP normal au client. 

Si les serveurs qui exécutent Windows Server 2016 sont des proxys RADIUS qui transfèrent les demandes d’authentification à un serveur de stratégie réseau \(NPS @ no__t-1 qui prend en charge la protection d’accès réseau, ces clients NAP sont évalués par NPS comme non NAP @ no__t-2capable et le traitement NAP échoue.
  
## <a name="see-also"></a>Voir aussi  
  
-   [Protocole DHCP (Dynamic Host Configuration Protocol)](Dynamic-Host-Configuration-Protocol--DHCP-.md)  
  

