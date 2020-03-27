---
title: Installer Windows Server Essentials Log Collector
description: Décrit comment utiliser Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d271c54f-1ffa-464e-afa5-27b8df61854e
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 75faeb3fe1c9ced32a37f2ac96ffedbadb2db76f
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318685"
---
# <a name="install-the-windows-server-essentials-log-collector"></a>Installer Windows Server Essentials Log Collector

>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

L’Assistant Installation du collecteur de journaux Windows Server Essentials installe le collecteur de journaux en tant que complément launchpad. Vous pouvez installer et utiliser Log Collector sur les ordinateurs réseau ou le serveur, ou les deux. Une fois l'installation terminée, Log Collector apparaît dans le tableau de bord.  
  
###  <a name="to-install-the-log-collector"></a><a name="BKMK_ToInstall"></a>Pour installer log Collector  
  
1.  Téléchargez le package d'installation de Log Collector sur un serveur ou un ordinateur du réseau.  
  
    > [!NOTE]
    > [Téléchargez le package d’installation du collecteur de journaux Windows Server Essentials](https://www.microsoft.com/download/details.aspx?id=34821).  
  
2.  Double-cliquez sur l'icône Log Collector.  
  
3.  Si vous exécutez l'installation à partir d'un ordinateur réseau, entrez vos informations d'identification d'administrateur de serveur quand vous y êtes invité.  
  
4.  Acceptez les termes du contrat de licence logiciel Microsoft.  
  
5.  Pour installer Log Collector uniquement sur le serveur, cochez la case **Uniquement sur le serveur**. Pour installer Log Collector sur tous les ordinateurs réseau, cochez la case **Sur le serveur et sur tous les ordinateurs du réseau**.  
  
6.  Cliquez sur **Installer le complément**.  
  
###  <a name="reinstalling-the-log-collector"></a><a name="BKMK_Reinstall"></a>Réinstallation du collecteur de journaux  
 S'il est nécessaire de réinstaller Log Collector, vous devez le désinstaller et le réinstaller sur le serveur et les ordinateurs du réseau. Quand vous désinstallez Log Collector du serveur à l'aide du tableau de bord, il est désinstallé automatiquement de tous les ordinateurs réseau.  
  
##### <a name="to-uninstall-and-reinstall-the-log-collector"></a>Pour désinstaller et réinstaller Log Collector  
  
1.  Ouvrez le Tableau de bord.  
  
2.  Cliquez sur l'onglet **Complément**, sélectionnez **Log Collector** dans la liste, puis cliquez sur **Désinstaller**.  
  

3.  Téléchargez et installez Log Collector en suivant les étapes décrites de la procédure précédente ([Pour installer Log Collector](Install-the-Windows-Server-Essentials-Log-Collector.md#BKMK_ToInstall)).  

3.  Téléchargez et installez Log Collector en suivant les étapes décrites de la procédure précédente ([Pour installer Log Collector](../support/Install-the-Windows-Server-Essentials-Log-Collector.md#BKMK_ToInstall)).  

  
### <a name="manually-install-the-log-collector"></a>Installer manuellement Log Collector  
 En cas d'échec de l'Assistant d'installation, vous pouvez installer Log Collector sur un seul ordinateur à l'aide de la procédure suivante.  
  
##### <a name="to-manually-install-the-log-collector"></a>Pour installer manuellement Log Collector  
  
1.  Renommez l’extension du fichier d’installation téléchargé de. wssx en. cab.  
  
2.  Double-cliquez sur le nom du fichier d'installation.  
  
3.  Cliquez sur **OK** si vous y êtes invité.  
  
4.  Double-cliquez sur le nom de fichier se terminant par « . msi » et sélectionnez un dossier dans lequel l’extraire.  
  
5.  Accédez au dossier dans lequel vous avez extrait le fichier, puis double-cliquez dessus pour terminer l'installation à l'aide de l'Assistant.
