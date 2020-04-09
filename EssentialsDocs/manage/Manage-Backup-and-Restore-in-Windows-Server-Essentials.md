---
title: Gérer la sauvegarde et la restauration dans Windows Server Essentials
description: Décrit comment utiliser Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 41000915-f6ff-4dbb-b7be-629ef36386d4
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: f81e4cfe5a18219608932f5b819b00d87322efe7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852842"
---
# <a name="manage-backup-and-restore-in-windows-server-essentials"></a>Gérer la sauvegarde et la restauration dans Windows Server Essentials

>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
 
 Windows Server Essentials offre des moyens fiables pour effectuer des sauvegardes régulières de votre serveur et des ordinateurs de votre réseau. En cas de perte de données, vous pouvez restaurer des données à partir d'une sauvegarde réussie sur le serveur sans avoir à restaurer l'ensemble de l'ordinateur. Si nécessaire, vous pouvez effectuer une restauration complète du système sur votre serveur ou les ordinateurs clients du réseau. Le tableau suivant décrit les différentes options de sauvegarde disponibles ainsi que leurs avantages.  
  
|Fonctionnalité de sauvegarde|Description|Avantages|  
|--------------------|-----------------|----------------|  
|Sauvegarde du serveur|Sauvegarde votre serveur exécutant Windows Server Essentials. Les données sont sauvegardées sur un lecteur USB externe.<br /><br /> Pour plus d’informations, consultez [gérer la sauvegarde](Manage-Server-Backup-in-Windows-Server-Essentials.md) et [la restauration du serveur ou réparer votre serveur](Restore-or-repair-your-server-running-Windows-Server-Essentials.md).|-Peut restaurer des fichiers et des dossiers sur votre serveur.<br /><br /> -Peut effectuer une restauration complète du système de votre serveur.|  
|Sauvegarde d'ordinateurs clients|Sauvegarde vos ordinateurs clients du réseau. Les données sont sauvegardées sur le serveur exécutant Windows Server Essentials.<br /><br /> Pour plus d’informations, consultez [gérer la sauvegarde du client](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md) et [restaurer un système complet à partir d’une sauvegarde d’ordinateur client existante](Restore-a-full-system-from-an-existing-client-computer-backup.md).|-Peut restaurer des fichiers et des dossiers à partir de votre serveur.<br /><br /> -Peut effectuer une restauration complète du système de votre ordinateur client.|  
| Sauvegarde Microsoft Azure|Effectue une sauvegarde en ligne des fichiers ou des dossiers sur votre serveur. Lorsque vous utilisez la sauvegarde Azure pour sauvegarder les données du serveur, les informations sont chiffrées à l’aide de votre phrase secrète avant d’être téléchargées vers un centre de données sécurisé sur Internet.<br /><br /> Pour plus d’informations, consultez [gérer la sauvegarde en ligne](Manage-Online-Backup-in-Windows-Server-Essentials.md).|-Peut restaurer des fichiers et des dossiers à partir de votre serveur.<br /><br /> -Avec des sauvegardes incrémentielles, seules les modifications apportées aux fichiers sont transférées vers le Cloud.<br /><br /> -Les sauvegardes sont stockées dans Microsoft Azure et sont hors site, ce qui réduit la nécessité de sécuriser et de protéger les supports de sauvegarde sur site.|  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Gérer Windows Server Essentials](Manage-Windows-Server-Essentials.md)  
  
-   [Utiliser Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
