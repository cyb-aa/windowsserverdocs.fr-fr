---
title: Utiliser Windows Server Essentials Log Collector
description: Décrit comment utiliser Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c6985518-b42d-4cfb-9761-eaa75306b6d7
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: d163195343b67ca38e565a0249363e7d1cec21f8
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318563"
---
# <a name="use-the-windows-server-essentials-log-collector"></a>Utiliser Windows Server Essentials Log Collector

>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Lorsque vous résolvez des problèmes d’ordinateur, un représentant du service clientèle et du support technique Microsoft peut vous demander de collecter des journaux à partir de serveurs, d’ordinateurs sur le réseau, ou les deux, à l’aide du collecteur de journaux Windows Server Essentials.  
  
 Log Collector copie les journaux des programmes, les journaux de l'observateur d'événements et d'autres informations relatives à votre environnement dans un fichier zip unique à un emplacement spécifié. Vous pouvez exécuter Log Collector directement à partir du serveur ou d'un ordinateur réseau quelconque, ou bien à l'aide d'une connexion à distance aux ordinateurs.  
  
> [!NOTE]
>Log Collector n'analyse pas les problèmes réseau et n'apporte aucune modification aux serveurs ou ordinateurs sur le réseau. Pour plus d'informations sur la résolution des problèmes réseau, voir la documentation d'aide accompagnant votre produit serveur.  
>Dans ce guide, les ordinateurs sur votre réseau, à l’exception de votre serveur, sont appelés « ordinateurs réseau ».  
>
>[Téléchargez le package d’installation du collecteur de journaux Windows Server Essentials](https://www.microsoft.com/download/details.aspx?id=34821).  
  
 Pour installer et exécuter Log Collector, effectuez les étapes répertoriées dans les rubriques suivantes :  
  

1. [Installer log Collector](Install-the-Windows-Server-Essentials-Log-Collector.md)  
  
2. [Exécuter log Collector](Run-the-Windows-Server-Essentials-Log-Collector.md)  

3. [Installer log Collector](../support/Install-the-Windows-Server-Essentials-Log-Collector.md)  
  
4. [Exécuter log Collector](../support/Run-the-Windows-Server-Essentials-Log-Collector.md)  


## <a name="environment-information-collected"></a>Informations collectées sur l'environnement  
 Pour chaque ordinateur réseau ou serveur que vous spécifiez, Log Collector collecte les informations suivantes sur l'environnement et les enregistre dans le fichier de collecte des journaux.  
  
-   Version du système d'exploitation  
  
-   Fabricant de l'unité centrale et description  
  
-   Quantité de mémoire et allocation  
  
-   Cartes réseau liées à TCP/IP  
  
-   Paramètres régionaux  
  
-   Processus  
  
-   Configuration du stockage  
  
-   Informations sur les fichiers de l'hôte  
  
-   Journaux des événements, y compris Application, Système, Windows Server et Media Center  
  
-   Messages du Gestionnaire de contrôle des services  
  
-   Événements de redémarrage et événements Windows Update  
  
-   Erreurs système et erreurs d'application  
  
## <a name="services-information-collected"></a>Informations collectées sur les services  
  
### <a name="server-services"></a>Services du serveur  
  
-   Service Sauvegarde d'ordinateur client Windows Server  
  
-   Service fournisseur de sauvegarde Windows Server pour ordinateurs clients  
  
-   Fournisseur de périphériques Windows Server  
  
-   Gestion des noms de domaine de Windows Server  
  
-   Registre du fournisseur de services de Windows Server  
  
-   Fournisseur de paramètres de Windows Server  
  
-   Service de périphérique UPnP de Windows Server  
  
-   Fournisseur d'administration Windows Server pour l'accès web à distance  
  
-   Service de contrôle d'intégrité de Windows Server  
  
-   Service de Windows Storage Server  
  
-   Service SQM Windows Server  
  
### <a name="network-computer-services"></a>Services de l'ordinateur réseau  
  
-   Service fournisseur de sauvegarde Windows Server pour ordinateurs clients  
  
-   Service de contrôle d'intégrité de Windows Server  
  
-   Registre du fournisseur de services de Windows Server  
  
-   Service SQM Windows Server  
  
## <a name="logs-and-registry-information-collected"></a>Informations collectées sur les journaux et le Registre  
 Pour chaque ordinateur réseau ou serveur spécifié, Log Collector collecte des informations sur les journaux et le Registre à partir du serveur et de l'ordinateur réseau comme suit.  
  
### <a name="server-logs-and-registry-information"></a>Informations sur les journaux et le Registre du serveur  
  
-   Journaux de produit du serveur, à partir de < ProgramData\>\Microsoft\Windows Server\Logs  
  
-   Tâches planifiées  
  
-   Journaux API d'installation  
  
-   Journaux Windows Update  
  
-   Fichier d'alertes d'intégrité  
  
-   Fichier d'informations sur les périphériques  
  
-   Fichier journal de sauvegarde du serveur  
  
-   Fichier journal Panther  
  
-   Services  
  
-   Clés de Registre situées aux emplacements suivants :  
  
    -   \\\ HKEY_LOCAL_MACHINE serveur \SOFTWARE\Microsoft\Windows \  
  
    -   \\\ HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Services\DevicesProviderSvc  
  
    -   \\\ HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Services\DomainManagerProviderSvc  
  
### <a name="network-computer-logs-and-registry-information"></a>Informations sur les journaux et le Registre de l'ordinateur réseau  
  
-   Journaux de produit de l’ordinateur réseau à < ProgramData\>\Microsoft\Windows Server\Logs  
  
-   Fichier d’alertes d’intégrité sur < ProgramData\>\Microsoft\Windows Server\Data  
  
-   Journaux Windows Update  
  
-   Journaux API d'installation  
  
-   Informations sur les tâches planifiées  
  
-   Clés de registre de \\\ HKEY_LOCAL_MACHINE serveur \SOFTWARE\Microsoft\Windows \  
  
## <a name="logs-for-computers-that-do-not-run-a-version-of-the-windows-operating-system"></a>Journaux pour les ordinateurs qui n'exécutent pas une version du système d'exploitation Windows  
 Log Collector ne collecte pas les fichiers journaux des ordinateurs qui n'exécutent pas une version du système d'exploitation Windows. Pour les ordinateurs non-Windows, vous devez copier manuellement les fichiers journaux suivants à l'emplacement où vous stockez les fichiers Log Collector.  
  
-   System.log  
  
-   Library/Logs/Windows Server.log  
  
-   Library/Logs/CrashReporter/LaunchPad-< nnn\> (copier tous les fichiers LaunchPad-< nnn\>. crash)  
  
-   Library/logs/DiagnosticReports/LaunchPad-< nnn\> (copier tous les fichiers LaunchPad-< nnn\>. crash)  
  
## <a name="see-also"></a>Voir aussi  
  

-   [Résoudre les erreurs du collecteur de journaux](Troubleshoot-Windows-Server-Essentials-Log-Collector-Errors.md)

-   [Résoudre les erreurs du collecteur de journaux](../support/Troubleshoot-Windows-Server-Essentials-Log-Collector-Errors.md)

