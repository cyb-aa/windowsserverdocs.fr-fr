---
title: Référence des commandes des services Bureau à distance (services Terminal Server)
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2f371848-5c48-470c-908c-afbc95d3a805
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 630b2274e96e446af07f7630d95056b40858209f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722440"
---
# <a name="remote-desktop-services-terminal-services-command-reference"></a>Référence des commandes des services Bureau à distance (services Terminal Server)

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La liste suivante répertorie les Services Bureau à distance outils en ligne de commande.
> [!NOTE]
> Dans Windows Server 2008 R2, les services Terminal Server ont été renommés services Bureau à distance. Pour découvrir les nouveautés de la dernière version, consultez les nouveautés [de services Bureau à distance dans Windows server 2012](https://technet.microsoft.com/library/hh831527) dans la bibliothèque TechNet de Windows Server.
> 
> |                 Commande                 |                                                      Description                                                       |
> |-----------------------------------------|------------------------------------------------------------------------------------------------------------------------|
> |           [change](change.md)           | modifications Bureau à distance paramètres de serveur de l’hôte de session (hôte de session Bureau à distance) pour les ouvertures de session, les mappages de port COM et le mode d’installation. |
> |     [change logon](change-logon.md)     |    Active ou désactive les ouvertures de session des sessions clientes sur un serveur hôte de session Bureau à distance ou affiche l’état actuel de l’ouverture de session.     |
> |      [change port](change-port.md)      |                   répertorie ou modifie les mappages de port COM pour qu’ils soient compatibles avec les applications MS-DOS.                    |
> |      [change user](change-user.md)      |                                modifie le mode d’installation du serveur hôte de session Bureau à distance.                                |
> |         [chglogon](chglogon.md)         |    Active ou désactive les ouvertures de session des sessions clientes sur un serveur hôte de session Bureau à distance ou affiche l’état actuel de l’ouverture de session.     |
> |          [chgport](chgport.md)          |                   répertorie ou modifie les mappages de port COM pour qu’ils soient compatibles avec les applications MS-DOS.                    |
> |           [chgusr](chgusr.md)           |                                modifie le mode d’installation du serveur hôte de session Bureau à distance.                                |
> |         [flattemp](flattemp.md)         |                                      Active ou désactive les dossiers temporaires plats.                                       |
> |           [logoff](logoff.md)           |          Déconnecte un utilisateur d’une session sur un serveur hôte de session Bureau à distance et supprime la session du serveur.          |
> |              [msg](msg.md)              |                                Envoie un message à un utilisateur sur un serveur hôte de session Bureau à distance.                                 |
> |            [mstsc](mstsc.md)            |                       crée des connexions aux serveurs hôtes de session Bureau à distance ou à d’autres ordinateurs distants.                        |
> |          [qappsrv](qappsrv.md)          |                             Affiche la liste de tous les serveurs hôtes de session Bureau à distance sur le réseau.                             |
> |         [qprocess](qprocess.md)         |                  Affiche des informations sur les processus qui s’exécutent sur un serveur hôte de session Bureau à distance.                   |
> |            [requête](query.md)            |                      Affiche des informations sur les processus, les sessions et les serveurs hôtes de session Bureau à distance.                      |
> |    [query process](query-process.md)    |                  Affiche des informations sur les processus qui s’exécutent sur un serveur hôte de session Bureau à distance.                   |
> |    [session de requête](query-session.md)    |                           Affiche des informations sur les sessions sur un serveur hôte de session Bureau à distance.                            |
> | [requête termserver](query-termserver.md) |                             Affiche la liste de tous les serveurs hôtes de session Bureau à distance sur le réseau.                             |
> |       [interroger l’utilisateur](query-user.md)       |                         Affiche des informations sur les sessions utilisateur sur un serveur hôte de session Bureau à distance.                         |
> |            [quser](quser.md)            |                         Affiche des informations sur les sessions utilisateur sur un serveur hôte de session Bureau à distance.                         |
> |          [qwinsta](qwinsta.md)          |                           Affiche des informations sur les sessions sur un serveur hôte de session Bureau à distance.                            |
> |          [rdpsign](rdpsign.md)          |                          Vous permet de signer numériquement un fichier protocole RDP (Remote Desktop Protocol) (. RDP).                          |
> |    [reset session](reset-session.md)    |                         Vous permet de réinitialiser (supprimer) une session sur un serveur hôte de session Bureau à distance.                          |
> |          [rwinsta](rwinsta.md)          |                         Vous permet de réinitialiser (supprimer) une session sur un serveur hôte de session Bureau à distance.                          |
> |           [shadow](shadow.md)           |            Vous permet de contrôler à distance une session active d’un autre utilisateur sur un serveur hôte de session Bureau à distance.             |
> |            [tscon](tscon.md)            |                               Se connecte à une autre session sur un serveur hôte de session Bureau à distance.                                |
> |         [tsdiscon](tsdiscon.md)         |                                 Déconnecte une session d’un serveur hôte de session Bureau à distance.                                  |
> |           [tskill](tskill.md)           |                           Met fin à un processus en cours d’exécution dans une session sur un serveur hôte de session Bureau à distance.                            |
> |           [tsprof](tsprof.md)           |              Copie les informations de configuration d’utilisateur Services Bureau à distance d’un utilisateur vers un autre.               |
