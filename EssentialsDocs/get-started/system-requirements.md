---
title: Configuration requise pour Windows Server Essentials
description: Décrit comment utiliser Windows Server Essentials
ms.custom: na
ms.date: 10/31/2013
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0951a67d-492f-41ad-9ae5-8e4cd25e3041
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 1b1c34646050c3459c88b10f608d8e003e72fdef
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80310438"
---
# <a name="system-requirements-for-windows-server-essentials"></a>Configuration requise pour Windows Server Essentials

>S’applique à : Windows Server 2019 Essentials, Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials 
  
  Le logiciel serveur Windows Server Essentials est un système d’exploitation 64 bits uniquement. Le tableau 1 définit la configuration matérielle minimale recommandée pour Windows Server Essentials. Le tableau 2 indique la configuration matérielle et logicielle requise supplémentaire pour le serveur.  
    
  
## <a name="table-1-system-requirements-for-windows-server-essentials"></a>Tableau 1. Configuration système requise pour Windows Server Essentials  
  
|Component|Minimum|Recommandé*|Maximum|  
|---------------|-------------|-------------------|-------------|  
|Socket CPU|1,4 GHz (processeur 64 bits) ou plus pour une unité centrale simple cœur<br /><br /> 1,3 GHz (processeur 64 bits) ou plus pour une unité centrale multicœur|3,1 GHz (processeur 64 bits) ou plus pour une unité centrale multicœur|2 sockets|  
|Mémoire (RAM)|2 Go<br /><br /> 4 Go si vous déployez Windows Server Essentials en tant qu'ordinateur virtuel|16 Go|64 Go|  
|Disques durs et espace de stockage disponible|Disque dur de 160 Go avec une partition système de 60 Go||Pas de limite|  
  
 \* La configuration matérielle recommandée prend en charge les limites d'utilisateurs et d'appareils maximales.  
  
## <a name="table-2-additional-hardware-and-software-requirements-for-windows-server-essentials"></a>Tableau 2. Configuration matérielle et logicielle requise supplémentaire pour Windows Server Essentials  
  
|Component|Description|  
|---------------|-----------------|  
|Carte réseau|Carte Gigabit Ethernet (10/100/1000baseT PHY/MAC)|  
|Internet|Certaines fonctionnalités peuvent nécessiter un accès à Internet (frais éventuels en sus) et un compte Microsoft.|  
|Systèmes d'exploitation clients compatibles|Windows 8.1, Windows 8, Windows 7, les versions Macintosh OS X 10.5 à 10.8.<br /><br /> **Remarque :** Certaines fonctionnalités requièrent des éditions Professional ou ultérieures.<br /><br /> 1 Go d'espace disponible sur le disque dur (une petite partie sera libérée à l'issue de l'installation)|  
|Routeur|Routeur ou pare-feu prenant en charge la traduction d'adresses réseau (NAT) IPv4 ou IPv6.|  
|Configuration supplémentaire|Lecteur de CD-ROM ou DVD-ROM|  
  
 Les critères réels varient en fonction de la configuration de votre système et des applications et fonctionnalités que vous installez. Les performances du processeur dépendent non seulement de la fréquence d'horloge du processeur, mais également du nombre de cœurs et de la taille du cache de processeur. L'espace de stockage requis pour la partition système est approximatif. Il peut être nécessaire de prévoir de l'espace en plus si vous effectuez l'installation via un réseau.  
  
 Pour plus d’informations sur la configuration matérielle requise, consultez le [Catalogue Windows Server](https://www.windowsservercatalog.com/).  
  
 Tout le matériel serveur doit répondre aux exigences établies pour le programme de logo Windows Server 2012 R2 pour les systèmes. Pour plus d’informations, consultez le [Programme Logo Windows](https://msdn.microsoft.com/windows/hardware/gg487403.aspx).  

> [!IMPORTANT]
> Les disques dynamiques ne sont pas pris en charge sur Windows Server Essentials.

## <a name="see-also"></a>Voir aussi  
 
-   [Installer Windows Server Essentials](../install/Install-Windows-Server-Essentials.md)  
  
-   [Configuration requise pour Windows Server Essentials](system-requirements.md)


