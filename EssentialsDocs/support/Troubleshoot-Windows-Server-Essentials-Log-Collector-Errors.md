---
title: Résoudre les erreurs de Windows Server Essentials Log Collector
description: Décrit comment utiliser Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fa2e1685-31c0-4d4f-a10a-6c8885dfc493
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: a08826a979defa2b7c78b8257726eb35ed07d054
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318627"
---
# <a name="troubleshoot-windows-server-essentials-log-collector-errors"></a>Résoudre les erreurs de Windows Server Essentials Log Collector

>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Quand vous exécutez Log Collector, les erreurs suivantes peuvent se produire. Pour résoudre un problème, suivez les instructions fournies pour l'erreur associée.  
  
> [!NOTE]
> Dans ce document, les ordinateurs sur votre réseau, à l’exception de votre serveur, sont appelés « ordinateurs réseau ».
  
###  <a name="the-destination-folder-is-not-valid"></a><a name="BKMK_TheDestinationFolderIsNotValid"></a>Le dossier de destination n’est pas valide  
 **Cause** : le dossier dans lequel vous tentez de copier les fichiers journaux n’existe pas ou ne dispose pas de suffisamment d’espace pour stocker les fichiers.  
  
 **Solution** :vérifiez que le dossier sélectionné existe et qu’il y a suffisamment d’espace libre disponible sur le lecteur pour stocker les fichiers. Vous devez également vous assurer qu'il reste suffisamment d'espace libre dans le dossier temporaire sur le lecteur.  
  
###  <a name="a-network-error-has-occurred"></a><a name="BKMK_ANetworkErrorHasOccurred"></a>Une erreur réseau s’est produite  
 **Cause** : un problème de réseau empêche peut-être le fonctionnement du serveur ou de l’ordinateur réseau.  
  
 **Solution** : vérifiez que tous les ordinateurs et appareils sur le réseau sont allumés et qu’ils sont connectés au réseau. Si vous ne parvenez pas à résoudre le problème, contactez la personne qui gère votre réseau pour obtenir une assistance.  
  
###  <a name="cannot-collect-log-files-for-the-computer"></a><a name="BKMK_CannotCollectLogFiles"></a>Impossible de collecter les fichiers journaux de l’ordinateur  
 **Cause** : le collecteur de journaux n’a peut-être pas été installé sur l’ordinateur, car ce dernier n’a pas pu se connecter au serveur via l’Assistant Connexion de l’ordinateur au serveur.  
  
 **Solution :** Pour plus d’informations sur la résolution des problèmes relatifs aux connexions à votre serveur, consultez résolution des problèmes de [connexion des ordinateurs au serveur](https://go.microsoft.com/fwlink/p/?LinkID=241492).  
  
 Si vous ne parvenez toujours pas à connecter l'ordinateur au serveur, vous pouvez copier les fichiers journaux manuellement sur un lecteur flash USB, comme suit :  
  
-   Sur les ordinateurs clients qui exécutent Windows 7, Windows 8 ou Windows Multipoint Server, vous pouvez copier le dossier **Journaux** disponible dans **%sysdir%\programdata\Microsoft\Windows Server**.  
  
###  <a name="you-do-not-have-permission-to-save-the-log-files-to-the-selected-folder"></a><a name="BKMK_YouDoNotHavePermission"></a>Vous n’avez pas l’autorisation d’enregistrer les fichiers journaux dans le dossier sélectionné  
 **Cause** : vous n’avez peut-être pas l’autorisation d’accès en écriture pour le dossier où vous souhaitez enregistrer les fichiers journaux.  
  
 **Solution :** Si vous utilisez le chemin d’accès par défaut pour enregistrer les fichiers journaux, vérifiez que vous disposez de l’autorisation en écriture sur le dossier partagé **\\\\< ServerName\>\Logs**. Si vous stockez les journaux sur un ordinateur réseau, vérifiez que vous avez accès en écriture au dossier que vous avez sélectionné pour enregistrer les fichiers journaux.  
  
###  <a name="the-computer-is-not-configured-properly-to-collect-the-log-files"></a><a name="BKMK_TheComputerIsNotConfiguredProperly"></a>L’ordinateur n’est pas configuré correctement pour collecter les fichiers journaux  
 **Cause** : l’ordinateur n’a pas été configuré correctement pour le collecteur de journaux.  
  
 **Solution** : réinstallez le collecteur de journaux. Voir [Réinstallation de Log Collector](Install-the-Windows-Server-Essentials-Log-Collector.md#BKMK_Reinstall).  
  
###  <a name="an-unknown-error-occurred"></a><a name="BKMK_AnUnknownErrorOccurred"></a>Une erreur inconnue s’est produite  
 **Cause** : inconnue.  
  
 **Solution 1** : réexécutez le collecteur de journaux. Si l'erreur persiste, vérifiez qu'il n'y a pas de problème de connectivité. Vous pouvez aussi essayer de réinstaller Log Collector. Voir [Réinstallation de Log Collector](Install-the-Windows-Server-Essentials-Log-Collector.md#BKMK_Reinstall). Si vous ne parvenez pas à résoudre le problème, contactez la personne qui gère votre réseau pour obtenir une assistance.  
  
 **Solution 2** : essayez d’abord d’ouvrir le dossier dans lequel les fichiers journaux sont enregistrés. Si le fichier zip avec le nom de l'ordinateur est déjà généré, ignorez cette erreur et utilisez les fichiers journaux à la place. Si aucun fichier journal n'est généré, relancez Log Collector. Si l'erreur persiste, vérifiez qu'il n'y a pas de problème de connectivité. Vous pouvez aussi essayer de réinstaller Log Collector. Voir [Réinstallation de Log Collector](Install-the-Windows-Server-Essentials-Log-Collector.md#BKMK_Reinstall). Si vous ne parvenez pas à résoudre le problème, contactez la personne qui gère votre réseau pour obtenir une assistance.