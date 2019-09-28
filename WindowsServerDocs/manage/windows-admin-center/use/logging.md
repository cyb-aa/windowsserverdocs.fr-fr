---
title: Journalisation des événements
description: Journalisation des événements à partir du centre d’administration Windows (projet Honolulu)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 012c2229fb29aa711d9887f28859e09bcf71c14a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356868"
---
# <a name="use-event-logging-in-windows-admin-center-to-gain-insight-into-management-activities-and-track-gateway-usage"></a>Utilisez la journalisation des événements dans le centre d’administration Windows pour mieux comprendre les activités de gestion et suivre l’utilisation de la passerelle

>S'applique à : Windows Admin Center, Windows Admin Center Preview

Le centre d’administration Windows écrit les journaux des événements pour vous permettre de voir les activités de gestion exécutées sur les serveurs de votre environnement, ainsi que pour vous aider à résoudre les problèmes liés au centre d’administration Windows.

## <a name="gain-insight-into-management-activities-in-your-environment-through-user-action-logging"></a>Familiarisez-vous avec les activités de gestion dans votre environnement grâce à la journalisation des actions utilisateur

Le centre d’administration Windows fournit des informations sur les activités de gestion effectuées sur les serveurs de votre environnement en enregistrant des actions dans le canal d’événement **Microsoft-ServerManagementExperience** dans le journal des événements du serveur géré, avec l’ID d’événement 4000 et la source SMEGateway. Le centre d’administration Windows consigne uniquement les actions sur le serveur géré. vous ne verrez donc pas les événements consignés si un utilisateur accède à un serveur à des fins de lecture seule.

Les événements journalisés incluent les informations suivantes :

| Touche           | Value                                                                                              |
|---------------|----------------------------------------------------------------------------------------------------|
| PowerShell    | Nom du script PowerShell qui a été exécuté sur le serveur, si l’action a exécuté un script PowerShell |
| MODÈLE           | Appel CIM exécuté sur le serveur, si l’action a exécuté un appel CIM                        |
| Module        | Outil (ou module) dans lequel l’action a été exécutée                                                     |
| Passerelle       | Nom de l’ordinateur passerelle du centre d’administration Windows sur lequel l’action a été exécutée                     |
| UserOnGateway | Nom d’utilisateur utilisé pour accéder à la passerelle du centre d’administration Windows et exécuter l’action                    |
| UserOnTarget  | Nom d’utilisateur utilisé pour accéder au serveur géré cible, s’il est différent de userOnGateway (c’est-à-dire l’utilisateur auquel l’utilisateur a accédé en utilisant le serveur à l’aide des informations d’identification « gérer en tant que ») |
| Délégation    | Booléen : si le serveur géré cible approuve la passerelle et que les informations d’identification sont déléguées à partir de l’ordinateur client de l’utilisateur             |
| PIRE          | Booléen : si l’utilisateur a accédé au serveur [à l'](https://technet.microsoft.com/mt227395.aspx) aide des informations d’identification                          |
| Fichier          | nom du fichier chargé, si l’action était un chargement de fichier                                |

## <a name="learn-about-windows-admin-center-activity-with-event-logging"></a>En savoir plus sur l’activité du centre d’administration Windows avec la journalisation des événements

Le centre d’administration Windows consigne l’activité de la passerelle dans le canal d’événement sur l’ordinateur de la passerelle pour vous aider à résoudre les problèmes et à afficher les mesures sur l’utilisation. Ces événements sont enregistrés dans le canal d’événement **Microsoft-ServerManagementExperience** .

[En savoir plus sur le dépannage du centre d’administration Windows.](troubleshooting.md)
