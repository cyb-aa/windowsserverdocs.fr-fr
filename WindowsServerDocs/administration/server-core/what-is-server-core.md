---
title: Quel est le serveur principal?
description: En savoir plus sur l’option d’installation Server Core de Windows Server
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 02/20/2018
ms.openlocfilehash: 08229e458d0aa0c8e8397f0f053f37a207a1aea5
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/01/2018
ms.locfileid: "1718533"
---
# <a name="what-is-the-server-core-installation-option-in-windows-server"></a>Quel est l’option d’installation Server Core de Windows Server?

> S’applique à: Windows Server (Channel annuel séparées) et Windows Server 2016

L’option Server Core est une option d’installation minimale qui est disponible lorsque vous déployez l’Édition Standard ou Datacenter de Windows Server. Server Core inclut la plupart mais pas tous les rôles de serveur. Serveur principal a un encombrement disque et, par conséquent, une surface d’attaque réduite en raison d’une base de code plus petite. 

## <a name="server-core-vs-server-with-desktop-experience"></a>Serveur (principal) vs Server avec la fonctionnalité expérience utilisateur 
Lorsque vous installez Windows Server, vous installez les rôles de serveur que vous choisissez: Cela permet de réduire l’encombrement pour Windows Server. Toutefois, le serveur avec l’option d’installation expérience installe toujours de nombreux services et autres composants n’est pas requis pour un scénario d’utilisation particulière. 

C’est là Server Core en jeu: l’installation Server Core supprime tous les services et d’autres fonctionnalités qui ne sont pas indispensables pour la prise en charge de certains couramment utilisaient des rôles de serveur. Par exemple, un serveur Hyper-V ne doit pas une interface utilisateur graphique (GUI), car vous pouvez gérer pratiquement tous les aspects de Hyper-V à partir de la ligne de commande à l’aide de Windows PowerShell ou à distance à l’aide du Gestionnaire Hyper-V. 

## <a name="the-server-core-difference---core-capabilities-without-the-frills"></a>La différence de Server Core - fonctionnalités principales sans les fioritures
Lorsque vous avez terminé l’installation Server Core sur un système et de la connexion pour la première fois, vous êtes dans un bit de surprenant. La principale différence entre le serveur avec l’option d’installation expérience et Server Core est que Server Core n’inclut pas les packages de shell interface utilisateur suivants:

- Microsoft-Windows-Shell-Package serveur
- Microsoft-Windows-Server-Gui-Mgmt-Package
- Microsoft-Windows-Server-Gui-RSAT-Package
- Microsoft-Windows-Cortana-PAL-Desktop-Package

En d’autres termes, il n’existe **Aucun bureau** dans Server Core, par sa conception. Tout en conservant les fonctionnalités requises pour prendre en charge les applications professionnelles traditionnelles et les charges de travail basé sur un rôle, Server Core ne dispose pas d’une interface de bureau classique. Au lieu de cela, Server Core est conçu pour être géré à distance via la ligne de commande, PowerShell ou un outil GUI (comme [RSAT](../../remote/remote-server-administration-tools.md) ou [Centre d’administration de Windows](../../manage/windows-admin-center/overview.md)).

Aucune interface utilisateur, en plus de Server Core diffère également le serveur avec la fonctionnalité expérience utilisateur comme suit:

- Server Core ne dispose pas des outils d’accessibilité
- Aucun OOBE (out of box experience) pour la configuration minimale
- Aucune prise en charge audio

Le tableau suivant répertorie les applications qui sont disponibles *localement* sur Server Core vs Server avec la fonctionnalité expérience utilisateur. **Important**: dans la plupart des cas, les applications qui sont répertoriées comme «non disponible» ci-dessous peut être exécutée à distance à partir d’un ordinateur client Windows et permet de gérer votre installation Server Core.

> [!NOTE]
> Cette liste est destiné à une référence rapide - il n’est pas destiné à être une liste complète.


| Application                     | ServerCore     | Serveur avec Expérience de bureau |
|------------------------------------|-----------------|--------------------------------|
| Invite de commandes                     | disponible       | disponible                      |
| Windows PowerShell / Microsoft .NET | disponible       | disponible                      |
| Perfmon.exe                        | non disponibles  | disponible                      |
| WinDbg (GUI)                         | pris en charge       | pris en charge                      |
| Resmon.exe                         | non disponibles   | disponible                      |
| Regedit                            | disponible       | disponible                      |
| Fsutil.exe                         | disponible       | disponible                      |
| Disksnapshot.exe                   | non disponibles   | disponible                      |
| DiskPart.exe                       | disponible       | disponible                      |
| Diskmgmt.msc                       | non disponibles   | disponible                      |
| Devmgmt.msc                        | non disponibles   | disponible                      |
| Gestionnaire de serveur                     | non disponibles  | disponible                      |
| MMC.exe                            | non disponibles   | disponible                      |
| Eventvwr                           | non disponibles  | disponible                      |
| Wevtutil (requêtes événement)           | disponible       | disponible                      |
| Services.msc                       | non disponibles   | disponible                      |
| Panneau de configuration,                      | non disponibles   | disponible                      |
| Mise à jour de Windows (GUI)                 | non disponibles | disponible                      |
| Explorateur Windows                   | non disponibles   | disponible                      |
| Barre des tâches                            | non disponibles   | disponible                      |
| Notifications de la barre des tâches              | non disponibles   | disponible                      |
| Taskmgr                            | disponible       | disponible                      |
| Internet Explorer ou Edge          | non disponibles   | disponible                      |
| Système d’aide intégré               | non disponibles   | disponible                      |
| Shell Windows 10                   | non disponibles   | disponible                      |
| Lecteur Windows Media               | non disponibles   | disponible                      |
| PowerShell                         | disponible       | disponible                      |
| PowerShell ISE                     | non disponibles   | disponible                      |
| IME PowerShell                     | disponible       | disponible                      |
| Mstsc.exe                          | non disponibles   | disponible                      |
| Services Bureau à distance            | disponible       | disponible                      |
| Gestionnaire Hyper-V                    | non disponibles  | disponible                      |


Pour plus d’informations sur les *est* inclus dans le serveur principal, voir [rôles, Services de rôle et fonctionnalités incluses dans Windows Server - Server Core](server-core-roles-and-services.md). Et pour savoir quels *n’est pas* inclus dans le serveur principal, voir [rôles, Services de rôle et fonctionnalités non incluses dans le serveur principal](server-core-removed-roles.md)

## <a name="get-started-using-server-core"></a>Commencer à utiliser Server Core
Utilisez les informations suivantes pour installer, configurer et gérer l’option d’installation Server Core de Windows Server.

Installation Server Core: 
- [Rôles, Services de rôle et fonctionnalités incluses dans Server Core](server-core-roles-and-services.md)
- [Rôles, Services de rôle et les fonctionnalités pas de serveur principal](server-core-removed-roles.md)
- [Installer l’option d’installation Server Core](../../get-started/getting-started-with-server-core.md)
- [Configurer le serveur principal avec l’outil SConfig](../../get-started/sconfig-on-ws2016.md)

À l’aide de Server Core:
- [Tâches d’administration Server Core de base à l’aide de Windows PowerShell ou la ligne de commande](server-core-administer.md)
- [Gérer les serveurs principaux](server-core-manage.md)
- [Mise à jour corrective Server Core](server-core-servicing.md)
- [Configurer les fichiers de vidage de mémoire](server-core-memory-dump.md)