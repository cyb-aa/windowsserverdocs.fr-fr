---
title: "Gérer la sauvegarde et restauration dans WindowsServerEssentials"
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 41000915-f6ff-4dbb-b7be-629ef36386d4
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 6f6f0d27472664cd1cc538897d3d525fad506282
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="manage-backup-and-restore-in-windows-server-essentials"></a>Gérer la sauvegarde et restauration dans WindowsServerEssentials

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials
 
 WindowsServerEssentials offre des moyens fiables pour effectuer des sauvegardes régulières de votre serveur et les sauvegardes d’ordinateurs de votre réseau. En cas de perte de données, vous pouvez restaurer des données à partir d’une sauvegarde réussie sur le serveur sans restauration complète de l’ordinateur. Si nécessaire, vous pouvez effectuer une restauration complète du système sur votre serveur ou les ordinateurs clients du réseau. Le tableau suivant décrit les différentes options de sauvegarde disponibles ainsi que leurs avantages.  
  
|Fonctionnalité de sauvegarde|Description|Avantages|  
|--------------------|-----------------|----------------|  
|Sauvegarde du serveur|Sauvegarde de votre serveur exécutant WindowsServerEssentials. Les données sont sauvegardées sur un lecteur USB externe.<br /><br /> Pour plus d’informations, voir [Manage Server Backup](Manage-Server-Backup-in-Windows-Server-Essentials.md) et [restaurer ou réparer votre serveur](Restore-or-repair-your-server-running-Windows-Server-Essentials.md).|-Peut restaurer des fichiers et dossiers sur votre serveur.<br /><br /> -Peut effectuer la restauration complète du système de votre serveur.|  
|Sauvegarde de l’ordinateur client|Sauvegarde de vos ordinateurs clients dans le réseau. Les données sont sauvegardées sur le serveur exécutant WindowsServerEssentials.<br /><br /> Pour plus d’informations, voir [gérer la sauvegarde Client](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md) et [restaurer un système complet à partir d’une sauvegarde d’ordinateur client existante](Restore-a-full-system-from-an-existing-client-computer-backup.md).|-Peut restaurer des fichiers et dossiers à partir de votre serveur.<br /><br /> -Peut effectuer la restauration complète du système de votre ordinateur client.|  
| Sauvegarde MicrosoftAzure|Effectue une sauvegarde en ligne des fichiers ou dossiers sur votre serveur. Lorsque vous utilisez Azure Backup pour sauvegarder les données de serveur, les informations sont chiffrées à l’aide de votre phrase secrète avant d’être téléchargée sur un centre de données sécurisé sur Internet.<br /><br /> Pour plus d’informations, voir [gérer la sauvegarde en ligne](Manage-Online-Backup-in-Windows-Server-Essentials.md).|-Peut restaurer des fichiers et dossiers à partir de votre serveur.<br /><br /> -Avec les sauvegardes incrémentielles, uniquement les modifications apportées aux fichiers sont transférées vers le cloud.<br /><br /> -Sauvegardes sont stockées dans MicrosoftAzure et sont hors de site, ce qui réduit la nécessité de sécuriser et de protéger les supports de sauvegarde.|  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Gérer WindowsServerEssentials](Manage-Windows-Server-Essentials.md)  
  
-   [Utiliser Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
