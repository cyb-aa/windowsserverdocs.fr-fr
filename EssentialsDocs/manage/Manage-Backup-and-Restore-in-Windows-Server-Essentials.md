---
title: Gérer la sauvegarde et la restauration dans Windows Server Essentials
description: Décrit comment utiliser Windows Server Essentials
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59828520"
---
# <a name="manage-backup-and-restore-in-windows-server-essentials"></a>Gérer la sauvegarde et la restauration dans Windows Server Essentials

>S'applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
 
 Windows Server Essentials offre des moyens fiables pour effectuer des sauvegardes régulières de votre serveur et des ordinateurs de votre réseau. En cas de perte de données, vous pouvez restaurer des données à partir d'une sauvegarde réussie sur le serveur sans avoir à restaurer l'ensemble de l'ordinateur. Si nécessaire, vous pouvez effectuer une restauration complète du système sur votre serveur ou les ordinateurs clients du réseau. Le tableau suivant décrit les différentes options de sauvegarde disponibles ainsi que leurs avantages.  
  
|Fonctionnalité de sauvegarde|Description|Avantages|  
|--------------------|-----------------|----------------|  
|Sauvegarde du serveur|Sauvegarde votre serveur exécutant Windows Server Essentials. Les données sont sauvegardées sur un lecteur USB externe.<br /><br /> Pour plus d’informations, consultez [Manage Server Backup](Manage-Server-Backup-in-Windows-Server-Essentials.md) et [restaurer ou réparer votre serveur](Restore-or-repair-your-server-running-Windows-Server-Essentials.md).|-Vous pouvez restaurer des fichiers et dossiers sur votre serveur.<br /><br /> -Vous pouvez effectuer la restauration complète du système de votre serveur.|  
|Sauvegarde d'ordinateurs clients|Sauvegarde vos ordinateurs clients du réseau. Les données sont sauvegardées sur le serveur exécutant Windows Server Essentials.<br /><br /> Pour plus d’informations, consultez [gérer la sauvegarde Client](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md) et [restaurer un système complet à partir d’une sauvegarde d’ordinateur client existante](Restore-a-full-system-from-an-existing-client-computer-backup.md).|-Vous pouvez restaurer des fichiers et dossiers à partir de votre serveur.<br /><br /> -Vous pouvez effectuer la restauration complète du système de votre ordinateur client.|  
| Sauvegarde Microsoft Azure|Effectue une sauvegarde en ligne des fichiers ou des dossiers sur votre serveur. Lorsque vous utilisez Azure Backup pour sauvegarder les données de serveur, les informations sont chiffrées à l’aide de votre phrase secrète avant d’être téléchargée vers un centre de données sécurisé sur Internet.<br /><br /> Pour plus d’informations, consultez [gérer la sauvegarde en ligne](Manage-Online-Backup-in-Windows-Server-Essentials.md).|-Vous pouvez restaurer des fichiers et dossiers à partir de votre serveur.<br /><br /> -Avec les sauvegardes incrémentielles, uniquement les modifications apportées aux fichiers sont transférées vers le cloud.<br /><br /> -Sauvegardes sont stockées dans Microsoft Azure et sont hors de site, ce qui réduit la nécessité de sécuriser et protéger les supports de sauvegarde sur site.|  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Gérer Windows Server Essentials](Manage-Windows-Server-Essentials.md)  
  
-   [Utiliser Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
