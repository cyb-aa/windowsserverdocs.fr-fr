---
title: Installer WindowsServerEssentialsLogCollector
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d271c54f-1ffa-464e-afa5-27b8df61854e
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: ade18cec590392f35e7ad6b30d9a22ccdce44dcd
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="install-the-windows-server-essentials-log-collector"></a>Installer WindowsServerEssentialsLogCollector

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

L’Assistant installation de WindowsServerEssentialsLogCollector installe Log Collector en tant qu’un Launchpad Add-in. Vous pouvez installer et utiliser Log Collector sur les ordinateurs du réseau ou le serveur. Après l’installation, le collecteur de journaux s’affiche sur le tableau de bord.  
  
###  <a name="BKMK_ToInstall"></a>Pour installer le collecteur de journaux  
  
1.  Télécharger le package d’installation du collecteur de journaux à tout serveur ou l’ordinateur sur le réseau.  
  
    > [!NOTE]
    >  Vous pouvez [télécharger le package d’installation du collecteur de journaux](https://go.microsoft.com/fwlink/p/?LinkId=255470) de Microsoft.  
  
2.  Double-cliquez sur l’icône Log Collector.  
  
3.  Si vous exécutez l’installation à partir d’un ordinateur réseau, entrez vos informations d’identification serveur administrateur lorsque vous y êtes invité.  
  
4.  Choisir d’accepter les termes du contrat de licence logiciel Microsoft.  
  
5.  Pour installer Log Collector uniquement sur le serveur, sélectionnez le **uniquement sur le serveur** case à cocher. Pour installer Log Collector sur tous les ordinateurs du réseau, sélectionnez le **sur le serveur et sur tous les ordinateurs sur le réseau** case à cocher.  
  
6.  Cliquez sur **installer le complément**.  
  
###  <a name="BKMK_Reinstall"></a>Réinstallation de Log Collector  
 S’il est nécessaire de réinstaller Log Collector, vous devez désinstaller et réinstaller Log Collector sur le serveur et les ordinateurs du réseau sur le réseau. En désinstallant le collecteur de journaux sur le serveur à partir du tableau de bord, tous les ordinateurs du réseau désinstalle automatiquement le collecteur de journaux.  
  
##### <a name="to-uninstall-and-reinstall-the-log-collector"></a>Pour désinstaller et réinstaller Log Collector  
  
1.  Ouvrez le tableau de bord.  
  
2.  Cliquez sur le **complément** onglet, sélectionnez **Log Collector** dans la liste, puis cliquez sur **désinstaller**.  
  

3.  Téléchargez et installez Log Collector en suivant les étapes de la procédure précédente, [pour installer le collecteur de journaux](Install-the-Windows-Server-Essentials-Log-Collector.md#BKMK_ToInstall).  

3.  Téléchargez et installez Log Collector en suivant les étapes de la procédure précédente, [pour installer le collecteur de journaux](../support/Install-the-Windows-Server-Essentials-Log-Collector.md#BKMK_ToInstall).  

  
### <a name="manually-install-the-log-collector"></a>Installer manuellement Log Collector  
 Si l’Assistant d’installation n’a pas pu installer Log Collector, vous pouvez installer le collecteur de journaux sur un seul ordinateur à l’aide de la procédure suivante.  
  
##### <a name="to-manually-install-the-log-collector"></a>Pour installer manuellement Log Collector  
  
1.  Renommez l’extension du fichier d’installation téléchargé à partir de .wssx pour .cab.  
  
2.  Double-cliquez sur le nom de fichier d’installation.  
  
3.  Cliquez sur **OK** si vous êtes invité.  
  
4.  Double-cliquez sur le nom de fichier se terminant par ˜.msi et sélectionnez un dossier dans lequel l’extraire.  
  
5.  Accédez au dossier avec le fichier extrait et double-cliquez sur le fichier d’installation pour utiliser l’Assistant pour terminer l’installation.
