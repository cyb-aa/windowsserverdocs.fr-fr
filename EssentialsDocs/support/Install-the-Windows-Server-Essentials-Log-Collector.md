---
title: Installer Windows Server Essentials Log Collector
description: Décrit comment utiliser Windows Server Essentials
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837030"
---
# <a name="install-the-windows-server-essentials-log-collector"></a>Installer Windows Server Essentials Log Collector

>S'applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

L’Assistant d’installation de Windows Server Essentials Log Collector installe Log Collector en tant qu’un Launchpad Add-in. Vous pouvez installer et utiliser Log Collector sur les ordinateurs réseau ou le serveur, ou les deux. Une fois l'installation terminée, Log Collector apparaît dans le tableau de bord.  
  
###  <a name="BKMK_ToInstall"></a> Pour installer le collecteur de journaux  
  
1.  Téléchargez le package d'installation de Log Collector sur un serveur ou un ordinateur du réseau.  
  
    > [!NOTE]
    >  Vous pouvez [télécharger le package d'installation de Log Collector](https://go.microsoft.com/fwlink/p/?LinkId=255470) à partir du site Microsoft.  
  
2.  Double-cliquez sur l'icône Log Collector.  
  
3.  Si vous exécutez l'installation à partir d'un ordinateur réseau, entrez vos informations d'identification d'administrateur de serveur quand vous y êtes invité.  
  
4.  Acceptez les termes du contrat de licence logiciel Microsoft.  
  
5.  Pour installer Log Collector uniquement sur le serveur, cochez la case **Uniquement sur le serveur** . Pour installer Log Collector sur tous les ordinateurs réseau, cochez la case **Sur le serveur et sur tous les ordinateurs du réseau** .  
  
6.  Cliquez sur **Installer le complément**.  
  
###  <a name="BKMK_Reinstall"></a> Réinstallation de Log Collector  
 S'il est nécessaire de réinstaller Log Collector, vous devez le désinstaller et le réinstaller sur le serveur et les ordinateurs du réseau. Quand vous désinstallez Log Collector du serveur à l'aide du tableau de bord, il est désinstallé automatiquement de tous les ordinateurs réseau.  
  
##### <a name="to-uninstall-and-reinstall-the-log-collector"></a>Pour désinstaller et réinstaller Log Collector  
  
1.  Ouvrez le tableau de bord.  
  
2.  Cliquez sur l'onglet **Complément**, sélectionnez **Log Collector** dans la liste, puis cliquez sur **Désinstaller**.  
  

3.  Téléchargez et installez Log Collector en suivant les étapes décrites de la procédure précédente, [To install the Log Collector](Install-the-Windows-Server-Essentials-Log-Collector.md#BKMK_ToInstall).  

3.  Téléchargez et installez Log Collector en suivant les étapes décrites de la procédure précédente, [To install the Log Collector](../support/Install-the-Windows-Server-Essentials-Log-Collector.md#BKMK_ToInstall).  

  
### <a name="manually-install-the-log-collector"></a>Installer manuellement Log Collector  
 En cas d'échec de l'Assistant d'installation, vous pouvez installer Log Collector sur un seul ordinateur à l'aide de la procédure suivante.  
  
##### <a name="to-manually-install-the-log-collector"></a>Pour installer manuellement Log Collector  
  
1.  Renommez l’extension du fichier d’installation téléchargé à partir de .wssx en .cab.  
  
2.  Double-cliquez sur le nom du fichier d'installation.  
  
3.  Cliquez sur **OK** si vous y êtes invité.  
  
4.  Double-cliquez sur le nom de fichier se terminant par ˜.msi et sélectionnez un dossier dans lequel l’extraire.  
  
5.  Accédez au dossier dans lequel vous avez extrait le fichier, puis double-cliquez dessus pour terminer l'installation à l'aide de l'Assistant.
