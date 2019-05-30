---
title: Conseils de dépannage et messages WSUS
description: Rubrique de Windows Server Update Service (WSUS) - résoudre les problèmes à l’aide de messages WSUS
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9f6317f7-bfe0-42d9-87ce-d8f038c728ca
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 77a4702ddab987cb3adda7627badb790e3102952
ms.sourcegitcommit: 8eea7aadbe94f5d4635c4ffedc6a831558733cc0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66308548"
---
# <a name="wsus-messages-and-troubleshooting-tips"></a>Conseils de dépannage et messages WSUS

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette rubrique contient des informations sur les messages WSUS suivants :

-   « Ordinateur n'a pas signalé d’état »

-   « Message ID 6703 - échouée de la synchronisation WSUS »

-   « Erreur 0 x 80070643 : Erreur irrécupérable lors de l’installation »

-   « Certains services ne sont pas exécutés. Vérifier les services suivants [...] »

## <a name="computer-has-not-reported-status"></a>Ordinateur n’a pas indiqué son état
Ce message est généré dans la console WSUS lorsqu’un ordinateur de client WSUS n’envoie pas d’informations sur le serveur WSUS pour indiquer son état actuel de la mise à jour. Ce problème est généralement dû à l’ordinateur client WSUS, pas le serveur WSUS.

Les raisons les plus courantes sont :

-   L’ordinateur a perdu la connectivité au réseau :
    -   Le câble réseau est débranché.
    -   Un câble réseau qui interviennent est défectueux.
    -   L’ordinateur possède une carte réseau défectueuse.
    -   Le port de réseau auquel se connecte l’ordinateur a été désactivé.
    -   La carte réseau sans fil ne peut pas associer et se connecter au point d’accès sans fil d’entreprise.
-   L’ordinateur est éteint. (Il a été arrêté ou est en mode veille ou veille prolongée.)

## <a name="message-id-6703---wsus-synchronization-failed"></a>ID de message 6703 - synchronisation WSUS a échoué
> Message : La demande a échoué avec l’état HTTP 503 : Service non disponible.

> Source : Microsoft.UpdateServices.Administration.AdminProxy.createUpdateServer.

Lorsque vous essayez d’ouvrir des Services de mise à jour sur le serveur WSUS, vous recevez l’erreur suivante :

> Erreur : Erreur de connexion

> Une erreur s’est produite lors de la tentative de connexion au serveur WSUS. Cette erreur peut se produire pour plusieurs raisons. Si le problème persiste, contactez votre administrateur réseau. Cliquez sur la réinitialisation du nœud du serveur de vous connecter au serveur.

Outre les précautions ci-dessus, tente d’accéder à l’URL pour le site Web Administration WSUS (par exemple, `http://CM12CAS:8530`) échoue avec l’erreur :

> Erreur HTTP 503. Le service n’est pas disponible

Dans ce cas, la cause la plus probable est que le Pool d’applications WsusPool dans IIS est dans un état arrêté.

En outre, la limite de mémoire privée (Ko) pour le Pool d’applications est probablement défini sur la valeur par défaut de la base de connaissances 1843200. Si vous rencontrez ce problème, augmentez la limite de mémoire privée à 4 Go (4000000 Ko) et redémarrez le Pool d’applications. Pour augmenter la limite de mémoire privée, sélectionnez le Pool d’applications WsusPool et cliquez sur Paramètres avancés sous Modifier le Pool d’applications. Puis définissez la limite de mémoire privée à 4 Go (4000000 Ko). Une fois que le Pool d’applications a été redémarré, surveiller l’état du composant SMS_WSUS_SYNC_MANAGER, wcm.log et wsyncmgr.log pour les échecs. Veuillez noter qu’il peut être nécessaire d’augmenter la limite de mémoire privée supérieure ou égale à 8 Go (8000000 Ko) selon l’environnement.

Pour plus d’informations, consultez : [Synchronisation WSUS dans ConfigMgr 2012 échoue avec des erreurs HTTP 503](http://blogs.technet.com/b/sus/archive/2015/03/23/configmgr-2012-support-tip-wsus-sync-fails-with-http-503-errors.aspx)

## <a name="error-0x80070643-fatal-error-during-installation"></a>Erreur 0 x 80070643 : Erreur irrécupérable pendant l’installation
Le programme d’installation de WSUS utilise Microsoft SQL Server pour effectuer l’installation. Ce problème se produit car l’utilisateur qui exécute le programme d’installation de WSUS ne dispose pas des autorisations d’administrateur système dans SQL Server.

Pour résoudre ce problème, accordez des autorisations d’administrateur système à un compte d’utilisateur ou à un compte de groupe dans SQL Server, puis exécutez à nouveau le programme d’installation de WSUS.

## <a name="some-services-are-not-running-check-the-following-services"></a>Certains services ne sont pas en cours d’exécution. Vérifier les services suivants :

- **SelfUpdate :** Consultez [automatique des mises à jour doivent être mis à jour](https://technet.microsoft.com/library/cc708554(v=ws.10).aspx) pour plus d’informations sur le dépannage du service Selfupdate.

- **WSSUService.exe :** Ce service facilite la synchronisation. Si vous avez des problèmes de synchronisation, accéder WSUSService.exe en cliquant sur **Démarrer**, en pointant sur **outils d’administration**, puis sur **Services**, puis en recherchant **Windows Server Update Service** dans la liste des services. Procédez comme suit :
    
    -   Vérifiez que ce service est en cours d’exécution. Cliquez sur **Démarrer** s’il est arrêté ou **redémarrer** pour actualiser le service.
    
    -   Utilisez l’Observateur d’événements pour vérifier le **Application**, **sécurité**y, et **système** journaux des événements pour voir s’il existe des événements qui peuvent indiquer un problème.
    
    -   Vous pouvez également vérifier SoftwareDistribution.log pour voir s’il existe des événements qui peuvent indiquer un problème.

- **ServicesSQL Web Service :** Services Web sont hébergés dans IIS. S’ils n’exécutent pas, vérifiez que IIS est en cours d’exécution (ou prise en main). Vous pouvez également essayer de réinitialiser le service Web en tapant **iisreset** à une invite de commandes.

- **Service SQL :** Chaque service à l’exception du service selfupdate nécessite que le service SQL est en cours d’exécution. Si les fichiers journaux indiquent des problèmes de connexion SQL, commencez par vérifier le service SQL. Pour accéder au service SQL, cliquez sur **Démarrer**, pointez sur **outils d’administration**, cliquez sur **Services**, puis recherchez une des opérations suivantes :
    
    -   **MSSQLSERver** (si vous utilisez WMSDE ou MSDE, ou si vous utilisez SQL Server et que vous utilisez le nom de l’instance par défaut pour le nom d’instance)
    
    -   **MSSQL$ WSUS** (si vous utilisez une base de données SQL Server et avez nommé votre instance de base de données « WSUS »)
    
    Cliquez sur le service, puis cliquez sur **Démarrer** si le service n’est pas en cours d’exécution, ou **redémarrer** pour actualiser le service s’il s’exécute.
