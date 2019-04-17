---
title: Configuration système requise pour WindowsServerEssentials
description: Décrit comment utiliser WindowsServerEssentials
ms.custom: na
ms.date: 10/31/2013
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0951a67d-492f-41ad-9ae5-8e4cd25e3041
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 26bfcc77a066cdacc2d9677a79ed636061c8ceca
ms.sourcegitcommit: e88e4fffa9b9790eb5c826b95e3dc2c92b4c4626
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/16/2018
---
# <a name="system-requirements-for-windows-server-essentials"></a>Configuration système requise pour WindowsServerEssentials

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials 
  
  Logiciel de serveur WindowsServerEssentials est un système d’exploitation uniquement 64bits. Tableau1 indique la configuration matérielle minimale recommandée pour WindowsServerEssentials. Tableau2 indique la configuration matérielle et logicielle supplémentaire requise pour le serveur.  
    
  
## <a name="table-1-system-requirements-for-windows-server-essentials"></a>Le tableau1. Configuration système requise pour WindowsServerEssentials  
  
|Composant|Au minimum|Recommandé *|Durée maximale|  
|---------------|-------------|-------------------|-------------|  
|Socket CPU|1,4 GHz (processeur 64 bits) ou plus rapide pour simple cœur<br /><br /> 1,3 GHz (processeur 64 bits) ou plus rapide pour multi-core|3,1 GHz (processeur 64 bits) ou plus rapide multicœur|2 sockets|  
|Mémoire (RAM)|2GO<br /><br /> 4Go si vous déployez WindowsServerEssentials en tant qu’un ordinateur virtuel|16 GO|64 GO|  
|Les disques durs et espace de stockage disponible|Disque dur de 160Go avec une partition de système de 60Go||Aucune limite|  
  
 * La configuration matérielle recommandée prend en charge maximal d’utilisateurs et les limites de l’appareil.  
  
## <a name="table-2-additional-hardware-and-software-requirements-for-windows-server-essentials"></a>Le tableau2. Configuration matérielle et logicielle supplémentaire requise pour WindowsServerEssentials  
  
|Composant|Description|  
|---------------|-----------------|  
|Carte réseau|Carte Gigabit Ethernet (10/100/1000baseT PHY/MAC)|  
|Internet|Certaines fonctionnalités peuvent nécessiter un accès à Internet (des frais peuvent s’appliquer) ou un compte Microsoft|  
|Systèmes d’exploitation clients pris en charge|Windows8.1, Windows8, Windows7, Macintosh OS X versions 10.5 à 10.8.<br /><br /> **Remarque:** certaines fonctionnalités requièrent des éditions professionnelles ou ultérieures.<br /><br /> 1 Go d’espace disque disponible (une partie de ce disque sera libérée après l’installation)|  
|Routeur|Un routeur ou un pare-feu qui prend en charge IPv4 NAT ou IPv6|  
|Configuration supplémentaire requise|Lecteur de DVD-ROM|  
  
 La configuration requise réelle dépend de la configuration de votre système et des applications et fonctionnalités que vous choisissez d’installer. Performances du processeur dépendent non seulement de la fréquence d’horloge du processeur, mais également du nombre de cœurs et la taille du cache du processeur. Espace de stockage requis pour la partition système est approximatif. Espace de stockage disponible supplémentaire peut être requis si vous installez sur un réseau.  
  
 Pour plus d’informations sur la configuration matérielle requise, consultez le [catalogue Windows Server](http://www.windowsservercatalog.com/).  
  
 Tout le matériel serveur doit répondre aux spécifications établies dans le programme de Logo Windows Server2012R2 pour les systèmes. Pour plus d’informations, voir [programme WindowsLogoProgram](https://msdn.microsoft.com/windows/hardware/gg487403.aspx).  

> [!IMPORTANT]
> Les disques dynamiques ne sont pas pris en charge sur WindowsServerEssentials.

## <a name="see-also"></a>Voir aussi  
 
-   [Installer WindowsServerEssentials](../install/Install-Windows-Server-Essentials.md)  
  
-   [Configuration système requise pour WindowsServerEssentials](system-requirements.md)


