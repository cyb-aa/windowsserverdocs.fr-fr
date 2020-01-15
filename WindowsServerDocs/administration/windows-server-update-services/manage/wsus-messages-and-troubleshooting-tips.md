---
title: Astuces de résolution des problèmes et messages WSUS
description: Rubrique Windows Server Update Service (WSUS)-résolution des problèmes à l’aide de messages WSUS
ms.prod: windows-server
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
ms.openlocfilehash: 0c66e655ea6b6c44ee3ba375f75e6532fab74bfb
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75948484"
---
# <a name="wsus-messages-and-troubleshooting-tips"></a>Astuces de résolution des problèmes et messages WSUS

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette rubrique contient des informations sur les messages WSUS suivants :

-   « L’ordinateur n’a pas signalé l’État »

-   « ID de message 6703-échec de la synchronisation WSUS »

-   « Erreur 0x80070643 : erreur irrécupérable lors de l’installation »

-   «Certains services ne sont pas en cours d’exécution. Vérifiez les services [...] suivants.

## <a name="computer-has-not-reported-status"></a>L’ordinateur n’a pas signalé l’État
Ce message est généré dans la console WSUS lorsqu’un ordinateur client WSUS n’envoie pas d’informations au serveur WSUS pour indiquer son état de mise à jour actuel. Ce problème est généralement dû au fait que l’ordinateur client WSUS n’est pas le serveur WSUS.

Les raisons les plus courantes sont les suivantes :

-   L’ordinateur a perdu la connexion au réseau :
    -   Le câble réseau est débranché.
    -   Un câble réseau intermédiaire est défectueux.
    -   L’ordinateur possède une carte réseau défectueuse.
    -   Le port réseau auquel l’ordinateur se connecte a été désactivé.
    -   La carte sans fil ne peut pas être associée à et se connecter au point d’accès sans fil de l’entreprise.
-   L’ordinateur est éteint. (Il a été arrêté ou est en mode veille ou veille prolongée.)

## <a name="message-id-6703---wsus-synchronization-failed"></a>ID de message 6703-échec de la synchronisation WSUS
> Message : la demande a échoué avec l’état HTTP 503 : service non disponible.
> 
> Source : Microsoft. UpdateServices. Administration. AdminProxy. createUpdateServer.

Lorsque vous essayez d’ouvrir Update Services sur le serveur WSUS, vous recevez l’erreur suivante :

> Erreur : erreur de connexion
> 
> Une erreur s’est produite lors de la tentative de connexion au serveur WSUS. Cette erreur peut se produire pour plusieurs raisons. Si le problème persiste, contactez votre administrateur réseau. Cliquez sur le nœud réinitialiser le serveur pour vous reconnecter au serveur.

En plus de ce qui précède, les tentatives d’accès à l’URL du site Web d’administration WSUS (par exemple, `http://CM12CAS:8530`) échouent avec l’erreur suivante :

> Erreur HTTP 503. Le service n’est pas disponible

Dans ce cas, la cause la plus probable est que le pool d’applications WsusPool dans IIS est à l’état arrêté.

En outre, la limite de mémoire privée (Ko) pour le pool d’applications est probablement définie sur la valeur par défaut de 1843200 Ko. Si vous rencontrez ce problème, augmentez la limite de la mémoire privée à 4 Go (4 millions Ko) et redémarrez le pool d’applications. Pour augmenter la limite de la mémoire privée, sélectionnez le pool d’applications WsusPool, puis cliquez sur Paramètres avancés sous modifier le pool d’applications. Définissez ensuite la limite de la mémoire privée sur 4 Go (4 millions Ko). Une fois le pool d’applications redémarré, surveillez l’état du composant SMS_WSUS_SYNC_MANAGER, WCM. log et fichier wsyncmgr. log pour les échecs. Notez qu’il peut être nécessaire d’augmenter la limite de la mémoire privée à 8 Go (8 millions Ko) ou plus en fonction de l’environnement.

Pour plus d’informations, consultez : la [synchronisation WSUS dans ConfigMgr 2012 échoue avec les erreurs HTTP 503](https://blogs.technet.com/b/sus/archive/2015/03/23/configmgr-2012-support-tip-wsus-sync-fails-with-http-503-errors.aspx)

## <a name="error-0x80070643-fatal-error-during-installation"></a>Erreur 0x80070643 : erreur irrécupérable lors de l’installation
Le programme d’installation de WSUS utilise Microsoft SQL Server pour effectuer l’installation. Ce problème se produit parce que l’utilisateur qui exécute le programme d’installation de WSUS ne dispose pas des autorisations d’administrateur système dans SQL Server.

Pour résoudre ce problème, accordez des autorisations d’administrateur système à un compte d’utilisateur ou à un compte de groupe dans SQL Server, puis réexécutez le programme d’installation de WSUS.

## <a name="some-services-are-not-running-check-the-following-services"></a>Certains services ne sont pas en cours d’exécution. Vérifiez les services suivants :

- **Selfupdate :** Pour plus d’informations sur la résolution des problèmes liés au service selfupdate, consultez [mises à jour automatiques](https://technet.microsoft.com/library/cc708554(v=ws.10).aspx) .

- **WSSUService. exe :** Ce service facilite la synchronisation. Si vous rencontrez des problèmes de synchronisation, accédez à WSUSService. exe en cliquant sur **Démarrer**, pointez sur **Outils d’administration**, cliquez sur **services**, puis recherchez **service de mise à jour Windows Server** dans la liste des services. Procédez comme suit :
    
    -   Vérifiez que ce service est en cours d’exécution. Cliquez sur **Démarrer** s’il est arrêté ou sur **redémarrer** pour actualiser le service.
    
    -   Utilisez observateur d’événements pour vérifier l' **application**, **securit**y et les journaux des événements **système** pour voir s’il existe des événements susceptibles d’indiquer un problème.
    
    -   Vous pouvez également consulter le fichier SoftwareDistribution. log pour voir s’il existe des événements susceptibles d’indiquer un problème.

- **Service ServicesSQL Web :** Les services Web sont hébergés dans IIS. S’ils ne sont pas en cours d’exécution, assurez-vous qu’IIS est en cours d’exécution (ou démarré). Vous pouvez également essayer de réinitialiser le service Web en tapant **IISReset** dans une invite de commandes.

- **Service SQL :** Chaque service, à l’exception du service selfupdate, requiert que le service SQL soit en cours d’exécution. Si l’un des fichiers journaux indique des problèmes de connexion SQL, vérifiez d’abord le service SQL. Pour accéder au service SQL, cliquez sur **Démarrer**, pointez sur **Outils d’administration**, cliquez sur **services**, puis recherchez l’un des éléments suivants :
    
  - **MSSQLSERver** (si vous utilisez WMSDE ou MSDE, si vous utilisez SQL Server et que vous utilisez le nom d’instance par défaut pour le nom de l’instance)
    
  - **MSSQL $ WSUS** (si vous utilisez une base de données SQL Server et que vous avez nommé votre instance de base de données « WSUS »)
    
    Cliquez avec le bouton droit sur le service, puis cliquez sur **Démarrer** si le service n’est pas en cours d’exécution ou sur **redémarrer** pour actualiser le service s’il est en cours d’exécution.
