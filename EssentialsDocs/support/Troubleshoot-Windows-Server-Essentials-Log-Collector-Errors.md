---
title: Résoudre les erreurs de Windows Server Essentials Log Collector
description: Décrit comment utiliser Windows Server Essentials
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836660"
---
# <a name="troubleshoot-windows-server-essentials-log-collector-errors"></a>Résoudre les erreurs de Windows Server Essentials Log Collector

>S'applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Quand vous exécutez Log Collector, les erreurs suivantes peuvent se produire. Pour résoudre un problème, suivez les instructions fournies pour l'erreur associée.  
  
> [!NOTE]
>  Dans ce document, les ordinateurs sur votre réseau, à l’exception de votre serveur, sont appelés ordinateurs réseau. ?  
  
###  <a name="BKMK_TheDestinationFolderIsNotValid"></a> Le dossier de destination n’est pas valide  
 **Cause :** Le dossier dans lequel vous tentez de copier les fichiers journaux n'existe pas ou ne dispose pas de suffisamment d'espace pour stocker les fichiers.  
  
 **Solution :** Vérifiez que le dossier sélectionné existe et qu'il y a suffisamment d'espace libre disponible sur le lecteur pour stocker les fichiers. Vous devez également vous assurer qu'il reste suffisamment d'espace libre dans le dossier temporaire sur le lecteur.  
  
###  <a name="BKMK_ANetworkErrorHasOccurred"></a> Une erreur réseau s’est produite  
 **Cause :** un problème lié au réseau peut affecter le fonctionnement du serveur ou de l'ordinateur réseau.  
  
 **Solution :** Vérifiez que tous les ordinateurs et appareils sur le réseau sont sous tension et qu'ils sont connectés au réseau. Si vous ne parvenez pas à résoudre le problème, contactez la personne qui gère votre réseau pour obtenir une assistance.  
  
###  <a name="BKMK_CannotCollectLogFiles"></a> Impossible de collecter les fichiers journaux pour l’ordinateur  
 **Cause :** Log Collector peut ne pas être installé sur l'ordinateur, car ce dernier n'est pas parvenu à se connecter au serveur à l'aide de l'Assistant Connexion de l'ordinateur au serveur.  
  
 **Solution :** Pour plus d’informations sur la résolution des problèmes de connexion à votre serveur, consultez [résoudre les problèmes de connexion des ordinateurs au serveur](https://go.microsoft.com/fwlink/p/?LinkID=241492)? (https://go.microsoft.com/fwlink/p/?LinkID=241492) sur le site Web Microsoft.  
  
 Si vous ne parvenez toujours pas à connecter l'ordinateur au serveur, vous pouvez copier les fichiers journaux manuellement sur un lecteur flash USB, comme suit :  
  
-   Sur les ordinateurs clients qui exécutent Windows 7, Windows 8 ou Windows Multipoint Server, vous pouvez copier le dossier **Journaux** disponible dans **%sysdir%\programdata\Microsoft\Windows Server**.  
  
###  <a name="BKMK_YouDoNotHavePermission"></a> Vous n’êtes pas autorisé à enregistrer les fichiers journaux dans le dossier sélectionné  
 **Cause :** Vous n'êtes peut-être pas autorisé à accéder en écriture au dossier que vous avez sélectionné pour enregistrer les fichiers journaux.  
  
 **Solution :** Si vous utilisez le chemin d’accès par défaut pour enregistrer les fichiers journaux, assurez-vous d’avoir l’autorisation d’écriture pour le dossier partagé  **\\ \\< nom_serveur\>\Logs**. Si vous stockez les journaux sur un ordinateur réseau, vérifiez que vous avez accès en écriture au dossier que vous avez sélectionné pour enregistrer les fichiers journaux.  
  
###  <a name="BKMK_TheComputerIsNotConfiguredProperly"></a> L’ordinateur n’est pas correctement configuré pour collecter les fichiers journaux  
 **Cause :** L'ordinateur n'a pas été configuré correctement pour Log Collector.  
  
 **Solution :** Réinstallez Log Collector. Consultez [Reinstalling the Log Collector](Install-the-Windows-Server-Essentials-Log-Collector.md#BKMK_Reinstall).  
  
###  <a name="BKMK_AnUnknownErrorOccurred"></a> Une erreur inconnue s’est produite  
 **Cause :** Inconnue.  
  
 **Solution 1 :** Relancez Log Collector. Si l'erreur persiste, vérifiez qu'il n'y a pas de problème de connectivité. Vous pouvez aussi essayer de réinstaller Log Collector. Consultez [Reinstalling the Log Collector](Install-the-Windows-Server-Essentials-Log-Collector.md#BKMK_Reinstall). Si vous ne parvenez pas à résoudre le problème, contactez la personne qui gère votre réseau pour obtenir une assistance.  
  
 **Solution 2 :** Essayez tout d'abord d'ouvrir le dossier dans lequel les fichiers journaux sont enregistrés. Si le fichier zip avec le nom de l'ordinateur est déjà généré, ignorez cette erreur et utilisez les fichiers journaux à la place. Si aucun fichier journal n'est généré, relancez Log Collector. Si l'erreur persiste, vérifiez qu'il n'y a pas de problème de connectivité. Vous pouvez aussi essayer de réinstaller Log Collector. Consultez [Reinstalling the Log Collector](Install-the-Windows-Server-Essentials-Log-Collector.md#BKMK_Reinstall). Si vous ne parvenez pas à résoudre le problème, contactez la personne qui gère votre réseau pour obtenir une assistance.