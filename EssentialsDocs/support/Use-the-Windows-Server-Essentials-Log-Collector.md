---
title: Utiliser Windows Server Essentials Log Collector
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c6985518-b42d-4cfb-9761-eaa75306b6d7
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: d003c6a45159548f7e34d86ca242f74868659d2f
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="use-the-windows-server-essentials-log-collector"></a>Utiliser Windows Server Essentials Log Collector

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

Lors du dépannage de problèmes de l’ordinateur, un représentant du support technique et Service clientèle Microsoft peut vous demander de collecter les journaux à partir de serveurs, les ordinateurs sur le réseau, ou à l’aide de Windows Server Essentials Log Collector.  
  
 Log Collector copie les journaux des programmes, les journaux des événements réviseur et informations sur l’environnement associés dans un fichier zip unique à un emplacement spécifié. Vous pouvez exécuter Log Collector directement à partir du serveur ou de n’importe quel ordinateur sur le réseau ou à l’aide d’une connexion à distance aux ordinateurs.  
  
> [!NOTE]
>  -   Le collecteur de journaux ne pas analyser les problèmes de réseau ou apporter des modifications à n’importe quel ordinateur serveur ou sur le réseau. Pour plus d’informations sur la façon de résoudre les problèmes de réseau, consultez la documentation de votre produit serveur.  
> -   Dans ce guide, les ordinateurs sur votre réseau, à l’exception de votre serveur, sont appelés ordinateurs du réseau.  
> -   [Télécharger le package d’installation de Windows Server Essentials Log Collector](https://go.microsoft.com/fwlink/?LinkID=266341).  
  
 Pour installer et exécuter Log Collector, effectuez les opérations dans les rubriques suivantes:  
  

1.  [Installer le collecteur de journaux](Install-the-Windows-Server-Essentials-Log-Collector.md)  
  
2.  [Exécuter Log Collector](Run-the-Windows-Server-Essentials-Log-Collector.md)  

1.  [Installer le collecteur de journaux](../support/Install-the-Windows-Server-Essentials-Log-Collector.md)  
  
2.  [Exécuter Log Collector](../support/Run-the-Windows-Server-Essentials-Log-Collector.md)  

  
## <a name="environment-information-collected"></a>Informations collectées sur l’environnement  
 Pour chaque ordinateur du réseau ou le serveur que vous spécifiez, Log Collector collecte l’informations suivantes sur l’environnement et le place dans le fichier de collecte de journaux.  
  
-   Version du système d’exploitation  
  
-   Description et le fabricant du processeur  
  
-   L’allocation et la quantité de mémoire  
  
-   Cartes réseau qui sont liées à TCP/IP  
  
-   Paramètres régionaux  
  
-   Processus  
  
-   Configuration du stockage  
  
-   Informations de fichier hôte  
  
-   Journaux des événements, y compris Application, système, Windows Server et Media Center  
  
-   Messages du Gestionnaire de contrôle de service  
  
-   Événements de redémarrage et événements de mise à jour de Windows  
  
-   Erreurs système et des erreurs d’Application  
  
## <a name="services-information-collected"></a>Les informations collectées des services  
  
### <a name="server-services"></a>Services de serveur  
  
-   Service sauvegarde de l’ordinateur Client de Windows Server  
  
-   Service de fournisseur de sauvegarde ordinateur Client Windows Server  
  
-   Fournisseur de périphériques Windows Server  
  
-   Gestion des noms de domaine Windows Server  
  
-   Registre du fournisseur du Service de serveur Windows  
  
-   Fournisseur de paramètres de Windows Server  
  
-   Service de périphérique UPnP de Windows Server  
  
-   Fournisseur d’Administration accès Web à distance Windows Server  
  
-   Service de contrôle d’intégrité de Windows Server  
  
-   Service de stockage Windows Server  
  
-   Service SQM Windows Server  
  
### <a name="network-computer-services"></a>Services de l’ordinateur réseau  
  
-   Service de fournisseur de sauvegarde ordinateur Client Windows Server  
  
-   Service de contrôle d’intégrité de Windows Server  
  
-   Registre du fournisseur du Service de serveur Windows  
  
-   Service SQM Windows Server  
  
## <a name="logs-and-registry-information-collected"></a>Journaux et les informations du Registre collectées  
 Pour chaque ordinateur du réseau ou le serveur spécifié, Log Collector collecte des informations de journaux et le Registre à partir du serveur et l’ordinateur réseau comme suit.  
  
### <a name="server-logs-and-registry-information"></a>Journaux du serveur et les informations du Registre  
  
-   Journaux de produit du serveur, à partir de < ProgramData\ > \Microsoft\Windows Server\Logs  
  
-   Tâches planifiées  
  
-   Journaux API d’installation  
  
-   Journaux Windows Update  
  
-   Fichier d’alertes d’intégrité  
  
-   Fichier d’informations de périphériques  
  
-   Fichier journal de sauvegarde du serveur  
  
-   Fichier journal Panther  
  
-   Services  
  
-   Clés de Registre, à partir de  
  
    -   \\\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\  
  
    -   \\\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DevicesProviderSvc  
  
    -   \\\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DomainManagerProviderSvc  
  
### <a name="network-computer-logs-and-registry-information"></a>Journaux de l’ordinateur du réseau et les informations du Registre  
  
-   Journaux de produit ordinateur réseau \Microsoft\Windows Server\Logs < ProgramData\ >  
  
-   Fichier d’alertes d’intégrité à < ProgramData\ > Server\Data  
  
-   Journaux Windows Update  
  
-   Journaux API d’installation  
  
-   Informations sur les tâches planifiées  
  
-   Clés de Registre à partir de \\\HKEY_LOCAL_MACHINE \SOFTWARE\Microsoft\Windows Server\  
  
## <a name="logs-for-computers-that-do-not-run-a-version-of-the-windows-operating-system"></a>Journaux pour les ordinateurs qui n’exécutent pas une version du système d’exploitation Windows  
 Log Collector ne collecte pas les fichiers journaux à partir d’ordinateurs qui n’exécutent pas une version du système d’exploitation Windows. Pour les ordinateurs non Windows, vous devez copier manuellement les fichiers journaux suivants vers le même emplacement où vous stockez les fichiers Log Collector.  
  
-   System.log  
  
-   Server.log journaux/Library/Windows  
  
-   Library/Logs/CrashReporter/LaunchPad-< nnn\ > (copier tous les fichiers LaunchPad-< nnn\ > .crash)  
  
-   Library/Logs/DiagnosticReports/LaunchPad-< nnn\ > (copier tous les fichiers LaunchPad-< nnn\ > .crash)  
  
## <a name="see-also"></a>Voir aussi  
  

-   [Résoudre les erreurs de Log Collector](Troubleshoot-Windows-Server-Essentials-Log-Collector-Errors.md)

-   [Résoudre les erreurs de Log Collector](../support/Troubleshoot-Windows-Server-Essentials-Log-Collector-Errors.md)

