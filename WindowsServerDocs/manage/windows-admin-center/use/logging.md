---
title: Journalisation des événements
description: Journalisation des événements à partir de Windows Admin Center (projet Honolulu)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: d91b92cb3bba99ae4aa96a96650a251a6df4cea5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849300"
---
# <a name="use-event-logging-in-windows-admin-center-to-gain-insight-into-management-activities-and-track-gateway-usage"></a>Journalisation des événements dans Windows Admin Center permet d’obtenir des informations sur les activités de gestion et de suivre l’utilisation de passerelle

>S'applique à : Windows Admin Center, version préliminaire de Windows Admin Center

Windows Admin Center écrit des journaux des événements pour vous permettre de voir les activités de gestion en cours d’exécution sur les serveurs dans votre environnement, ainsi que pour vous aider à résoudre les problèmes de Windows Admin Center.

## <a name="gain-insight-into-management-activities-in-your-environment-through-user-action-logging"></a>Familiarisez-vous avec les activités de gestion dans votre environnement via la journalisation d’action utilisateur

Windows Admin Center fournit des informations sur les activités de gestion effectuées sur les serveurs dans votre environnement par des actions de journalisation pour le **Microsoft-ServerManagementExperience** canal d’événement dans le journal des événements de managé serveur avec ID d’événement 4000 et SMEGateway de la Source. Windows Admin Center enregistre uniquement les actions sur le serveur géré, donc vous ne verrez les événements consignés si un utilisateur accède à un serveur en lecture seule.

Événements enregistrés incluent les informations suivantes :

| Touche           | Value                                                                                              |
|---------------|----------------------------------------------------------------------------------------------------|
| PowerShell    | Nom du script PowerShell qui a été exécuté sur le serveur, si l’action a été exécuté un script PowerShell |
| CIM           | Appel CIM qui a été exécuté sur le serveur, si l’action a été exécuté un appel CIM                        |
| Module        | Outil (ou module) où l’action a été exécutée                                                     |
| Passerelle       | Nom de l’ordinateur de passerelle Windows Admin Center où l’action a été exécutée                     |
| UserOnGateway | Nom d’utilisateur utilisé pour accéder à la passerelle Windows Admin Center et exécuter l’action                    |
| UserOnTarget  | Nom d’utilisateur utilisé pour accéder au serveur géré cible, s’il diffère de l’userOnGateway (autrement dit, l’utilisateur accédé à l’aide du serveur à l’aide de « Gérer en tant que » les informations d’identification) |
| Délégation    | Valeur booléenne : si la cible géré server approuve la passerelle et informations d’identification sont déléguées à partir de l’ordinateur client de l’utilisateur             |
| LAPS          | Valeur booléenne : si l’utilisateur a accédé le serveur à l’aide [LAPS](https://technet.microsoft.com/mt227395.aspx) informations d’identification                          |
| Fichier          | nom du fichier chargé, si l’action a été un téléchargement de fichier                                |

## <a name="learn-about-windows-admin-center-activity-with-event-logging"></a>En savoir plus sur l’activité Windows Admin Center avec la journalisation des événements

Windows Admin Center consigne les activités de la passerelle pour le canal d’événement sur l’ordinateur de passerelle pour vous aider à résoudre les problèmes et afficher les mesures sur l’utilisation. Ces événements sont consignés dans le **Microsoft-ServerManagementExperience** canal d’événement.

[En savoir plus sur le dépannage de Windows Admin Center.](troubleshooting.md)
