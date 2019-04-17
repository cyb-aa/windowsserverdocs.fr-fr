---
title: Journalisation des événements
description: Journalisation des événements à partir du centre d’administration de Windows (Honolulu Project)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: d91b92cb3bba99ae4aa96a96650a251a6df4cea5
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/22/2018
ms.locfileid: "2074340"
---
# <a name="use-event-logging-in-windows-admin-center-to-gain-insight-into-management-activities-and-track-gateway-usage"></a>Journalisation des événements dans le centre d’administration de Windows permet d’obtenir des informations sur les activités de gestion et l’utilisation de passerelle de suivi

>S’applique à: Windows Admin Center, aperçu du centre d’administration de Windows

Centre d’administration de Windows écrit les journaux des événements pour vous permettre de voir les activités de gestion en cours d’exécution sur les serveurs de votre environnement, ainsi que pour vous aider à résoudre les problèmes de centre d’administration de Windows.

## <a name="gain-insight-into-management-activities-in-your-environment-through-user-action-logging"></a>Obtenir un aperçu des activités de gestion de votre environnement via l’enregistrement de l’action utilisateur

Centre d’administration de Windows fournit des informations sur les activités de gestion effectuées sur les serveurs de votre environnement par des actions de journalisation pour le canal d’événements **Microsoft-ServerManagementExperience** dans le journal des événements du serveur géré, avec l’ID d’événement 4000 et SMEGateway de Source. Centre d’administration de Windows enregistre uniquement les actions sur le serveur géré, afin que vous ne verrez pas les événements enregistrés si un utilisateur accède à un serveur en lecture seule.

Événements enregistrés incluent les informations suivantes:

| Clé           | Valeur                                                                                              |
|---------------|----------------------------------------------------------------------------------------------------|
| PowerShell    | Nom du script PowerShell qui a été exécutée sur le serveur, si l’action a exécuté un script PowerShell |
| CIM           | Appel CIM qui a été exécutée sur le serveur, si l’action a exécuté un appel CIM                        |
| Module        | Outil (ou un module) dans lequel l’action a été exécutée                                                     |
| Passerelle       | Nom de l’ordinateur de passerelle de centre d’administration de Windows où l’action a été exécutée                     |
| UserOnGateway | Nom d’utilisateur utilisé pour accéder à la passerelle du centre d’administration de Windows et exécuter l’action                    |
| UserOnTarget  | Nom d’utilisateur utilisé pour accéder au serveur géré cible, s’il diffère de l’userOnGateway (c'est-à-dire l’utilisateur accédé via le serveur à l’aide des informations d’identification «Gérer en tant que») |
| Délégation    | Valeur booléenne: si la cible est géré serveur approuve la passerelle et délégation des informations d’identification à partir de l’ordinateur client de l’utilisateur             |
| LAPS          | Valeur booléenne: si le serveur à l’aide des informations d’identification [LAPS](https://technet.microsoft.com/mt227395.aspx) accédé à l’utilisateur                          |
| File          | nom du fichier téléchargé, si l’action était un téléchargement de fichier                                |

## <a name="learn-about-windows-admin-center-activity-with-event-logging"></a>En savoir plus sur l’activité du centre d’administration de Windows avec la journalisation des événements

Centre d’administration de Windows enregistre l’activité passerelle dans le canal d’événements sur l’ordinateur de passerelle pour vous aider à résoudre les problèmes et de visualiser les mesures d’utilisation. Ces événements sont consignés dans le canal d’événements **Microsoft-ServerManagementExperience** .

[Pour plus d’informations sur la résolution des problèmes du centre d’administration de Windows.](troubleshooting.md)
