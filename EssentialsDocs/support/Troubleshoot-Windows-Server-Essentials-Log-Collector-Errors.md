---
title: "Résoudre les erreurs de WindowsServerEssentialsLogCollector"
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fa2e1685-31c0-4d4f-a10a-6c8885dfc493
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: e92236c8e5d956b50f657ebcbe1a942b5841fded
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="troubleshoot-windows-server-essentials-log-collector-errors"></a>Résoudre les erreurs de WindowsServerEssentialsLogCollector

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

Lorsque vous exécutez Log Collector, vous pouvez rencontrer les erreurs suivantes. Pour résoudre un problème, suivez les instructions fournies pour l’erreur associée.  
  
> [!NOTE]
>  Dans ce document, les ordinateurs sur votre réseau, à l’exception de votre serveur, sont appelés ordinateurs du réseau.?  
  
###  <a name="BKMK_TheDestinationFolderIsNotValid"></a>Le dossier de destination n’est pas valide  
 **Cause:** le dossier dans lequel vous tentez de copier les fichiers journaux n’existe pas ou est peut-être pas suffisamment d’espace pour stocker les fichiers.  
  
 **Solution:** Vérifiez que le dossier sélectionné existe et qu’il est suffisamment d’espace libre disponible sur le lecteur pour les fichiers. Vous devez également vous assurer qu’il existe suffisamment d’espace libre restant dans le dossier temporaire sur le lecteur.  
  
###  <a name="BKMK_ANetworkErrorHasOccurred"></a>Une erreur réseau s’est produite  
 **Cause:** peut-être un problème de réseau sur le serveur ou un ordinateur réseau.  
  
 **Solution:** Vérifiez que tous les ordinateurs et périphériques réseau sont sous tension et qu’ils sont connectés au réseau. Si vous ne pouvez pas résoudre le problème, contactez la personne qui gère votre réseau pour obtenir une assistance.  
  
###  <a name="BKMK_CannotCollectLogFiles"></a>Impossible de collecter les fichiers journaux de l’ordinateur  
 **Cause:** le collecteur de journaux ne peut pas être installé sur l’ordinateur, car l’ordinateur n’est pas correctement connecté au serveur à l’aide de la connexion de l’ordinateur à l’Assistant de serveur.  
  
 **Solution:** pour plus d’informations sur la résolution des problèmes de connexion à votre serveur, consultez [résoudre les problèmes de connexion des ordinateurs au serveur](https://go.microsoft.com/fwlink/p/?LinkID=241492)? (https://go.microsoft.com/fwlink/p/?LinkID=241492) sur le site Web Microsoft.  
  
 Si vous ne parvenez toujours pas à se connecter l’ordinateur au serveur, vous pouvez copier les fichiers journaux manuellement sur un lecteur flash USB comme suit:  
  
-   Pour les ordinateurs clients qui exécutent Windows7, Windows8 ou WindowsMultipointServer, vous pouvez copier le **journaux** dossier situé à **%sysdir%\programdata\Microsoft\Windows Server**.  
  
###  <a name="BKMK_YouDoNotHavePermission"></a>Vous n’êtes pas autorisé à enregistrer les fichiers journaux dans le dossier sélectionné  
 **Cause:** vous ne peut-être pas autorisé à écrire dans le dossier que vous avez sélectionné pour enregistrer les fichiers journaux.  
  
 **Solution:** si vous utilisez le chemin d’accès par défaut pour enregistrer les fichiers journaux, vérifiez que vous avez accès en écriture au dossier partagé **\\\ < serverName\ > \Logs**. Si vous stockez les journaux sur un ordinateur réseau, assurez-vous que vous avez accès en écriture pour le dossier que vous avez sélectionné pour enregistrer les fichiers journaux.  
  
###  <a name="BKMK_TheComputerIsNotConfiguredProperly"></a>L’ordinateur n’est pas configuré correctement pour collecter les fichiers journaux  
 **Cause:** l’ordinateur n’a pas été configuré correctement pour le collecteur de journaux.  
  
 **Solution:** réinstaller Log Collector. Voir [réinstaller Log Collector](Install-the-Windows-Server-Essentials-Log-Collector.md#BKMK_Reinstall).  
  
###  <a name="BKMK_AnUnknownErrorOccurred"></a>Une erreur inconnue s’est produite  
 **Cause:** inconnu.  
  
 **Solution 1:** exécuter à nouveau le collecteur de journaux. Si l’erreur persiste, ne vérifiez qu’aucun problème de connectivité. Vous pouvez également essayer de réinstaller Log Collector. Voir [réinstaller Log Collector](Install-the-Windows-Server-Essentials-Log-Collector.md#BKMK_Reinstall). Si vous ne pouvez pas résoudre le problème, contactez la personne qui gère votre réseau pour obtenir une assistance.  
  
 **Solution 2:** tout d’abord, essayez d’ouvrir le dossier dans lequel les fichiers journaux sont enregistrés. Si le fichier zip avec le nom de l’ordinateur est déjà généré, ignorez cette erreur et utiliser les fichiers journaux à la place. S’il n’existe aucun fichier journal généré, relancez Log Collector. Si l’erreur persiste, ne vérifiez qu’aucun problème de connectivité. Vous pouvez également essayer de réinstaller Log Collector. Voir [réinstaller Log Collector](Install-the-Windows-Server-Essentials-Log-Collector.md#BKMK_Reinstall). Si vous ne pouvez pas résoudre le problème, contactez la personne qui gère votre réseau pour obtenir une assistance.