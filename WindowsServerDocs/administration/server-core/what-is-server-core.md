---
title: Nouveautés de Server Core ?
description: En savoir plus sur l’option d’installation Server Core de Windows Server
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 02/20/2018
ms.openlocfilehash: 08229e458d0aa0c8e8397f0f053f37a207a1aea5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885600"
---
# <a name="what-is-the-server-core-installation-option-in-windows-server"></a>Qu’est l’option d’installation Server Core de Windows Server ?

> S’applique à : Windows Server (canal semi-annuel) et Windows Server 2016

L’option Server Core est une option d’installation minimale qui est disponible lorsque vous déployez l’Édition Standard ou Datacenter de Windows Server. Server Core inclut la plupart mais pas tous les rôles de serveur. Server Core possède un encombrement de disque et par conséquent une surface d’attaque réduite en raison d’une base de code plus petits. 

## <a name="server-core-vs-server-with-desktop-experience"></a>Serveur (Core) vs serveur avec expérience utilisateur 
Lorsque vous installez Windows Server, vous installez uniquement les rôles de serveur que vous choisissez : Cela permet de réduire l’encombrement global pour Windows Server. Toutefois, le serveur avec l’option d’installation expérience installe toujours de nombreux services et autres composants qui ne sont généralement pas nécessaires pour un scénario d’utilisation particulier. 

C’est là que Server Core entre en jeu : l’installation de Server Core élimine tous les services et d’autres fonctionnalités qui ne sont pas indispensables pour la prise en charge de certains couramment utilisaient des rôles de serveur. Par exemple, un serveur Hyper-V n’a pas besoin une interface utilisateur graphique (GUI), étant donné que vous pouvez gérer pratiquement tous les aspects d’Hyper-V à partir de la ligne de commande à l’aide de Windows PowerShell ou à distance en utilisant le Gestionnaire Hyper-V. 

## <a name="the-server-core-difference---core-capabilities-without-the-frills"></a>La différence de Server Core - fonctionnalités principales sans les fioritures
Lorsque vous avez terminé l’installation Server Core sur un système et de la connexion pour la première fois, vous l’avez un peu surprise. La principale différence entre le serveur avec l’option d’installation expérience utilisateur et Server Core est que Server Core n’inclut pas les packages d’interpréteur de commandes de l’interface graphique utilisateur suivants :

- Microsoft-Windows-Server-Shell-Package
- Microsoft-Windows-Server-Gui-Mgmt-Package
- Microsoft-Windows-Server-Gui-RSAT-Package
- Microsoft-Windows-Cortana-PAL-Desktop-Package

En d’autres termes, il est **aucun Bureau** dans Server Core, par conception. Tout en conservant les capacités requises pour prendre en charge des applications professionnelles traditionnelles et les charges de travail en fonction du rôle, Server Core n’a pas d’une interface de bureau traditionnelle. Au lieu de cela, Server Core est conçu pour être géré à distance via la ligne de commande, PowerShell ou un outil GUI (comme [RSAT](../../remote/remote-server-administration-tools.md) ou [Windows Admin Center](../../manage/windows-admin-center/overview.md)).

En plus aucune interface utilisateur, Server Core diffère également le serveur avec expérience comme suit :

- Server Core n’a pas d’outils d’accessibilité
- Aucun OOBE (out of box experience) pour la configuration minimale
- Aucune prise en charge audio

Le tableau suivant indique les applications disponibles *localement* sur Server Core et Server avec la fonctionnalité expérience utilisateur. **Important** : Dans la plupart des cas, les applications qui sont répertoriées comme « non disponible » ci-dessous peut être exécuté à distance à partir d’un ordinateur client de Windows et permet de gérer votre installation de Server Core.

> [!NOTE]
> Cette liste est destinée à une référence rapide - il n’est pas destiné à être une liste complète.


| Application                     | Server Core     | Serveur avec Expérience de bureau |
|------------------------------------|-----------------|--------------------------------|
| Invite de commandes                     | disponible       | disponible                      |
| Windows PowerShell / .NET de Microsoft | disponible       | disponible                      |
| Perfmon.exe                        | non disponibles  | disponible                      |
| WinDbg (GUI)                         | prise en charge       | prise en charge                      |
| Resmon.exe                         | non disponibles   | disponible                      |
| Regedit                            | disponible       | disponible                      |
| Fsutil.exe                         | disponible       | disponible                      |
| Disksnapshot.exe                   | non disponibles   | disponible                      |
| DiskPart.exe                       | disponible       | disponible                      |
| Diskmgmt.msc                       | non disponibles   | disponible                      |
| Devmgmt.msc                        | non disponibles   | disponible                      |
| Gestionnaire de serveur                     | non disponibles  | disponible                      |
| Mmc.exe                            | non disponibles   | disponible                      |
| Eventvwr                           | non disponibles  | disponible                      |
| Wevtutil (requêtes d’événements)           | disponible       | disponible                      |
| Services.msc                       | non disponibles   | disponible                      |
| Panneau de configuration                      | non disponibles   | disponible                      |
| Mise à jour de Windows (GUI)                 | non disponibles | disponible                      |
| Explorateur Windows                   | non disponibles   | disponible                      |
| Barre des tâches                            | non disponibles   | disponible                      |
| Notifications de la barre des tâches              | non disponibles   | disponible                      |
| Gestionnaire des tâches                            | disponible       | disponible                      |
| Internet Explorer ou Edge          | non disponibles   | disponible                      |
| Système d’aide intégré               | non disponibles   | disponible                      |
| Shell Windows 10                   | non disponibles   | disponible                      |
| Lecteur Windows Media               | non disponibles   | disponible                      |
| PowerShell                         | disponible       | disponible                      |
| PowerShell ISE                     | non disponibles   | disponible                      |
| PowerShell IME                     | disponible       | disponible                      |
| Mstsc.exe                          | non disponibles   | disponible                      |
| Services Bureau à distance            | disponible       | disponible                      |
| Gestionnaire Hyper-V                    | non disponibles  | disponible                      |


Pour plus d’informations sur les éléments *est* inclus dans Server Core, consultez [rôles, Services de rôle et fonctionnalités incluses dans Windows Server - Server Core](server-core-roles-and-services.md). Et pour plus d’informations sur les éléments *n’est pas* inclus dans Server Core, consultez [rôles, Services de rôle et fonctionnalités non incluses dans Server Core](server-core-removed-roles.md)

## <a name="get-started-using-server-core"></a>Prise en main à l’aide de Server Core
Utilisez les informations suivantes pour installer, configurer et gérer l’option d’installation Server Core de Windows Server.

Installation Server Core : 
- [Rôles, Services de rôle et fonctionnalités incluses dans Server Core](server-core-roles-and-services.md)
- [Rôles, Services de rôle et fonctionnalités pas de Server Core](server-core-removed-roles.md)
- [Installer l’option d’installation Server Core](../../get-started/getting-started-with-server-core.md)
- [Configurer Server Core avec l’outil SConfig](../../get-started/sconfig-on-ws2016.md)

À l’aide de Server Core :
- [Base tâches d’administration de Server Core à l’aide de Windows PowerShell ou la ligne de commande](server-core-administer.md)
- [Gérer Server Core](server-core-manage.md)
- [Mise à jour corrective de Server Core](server-core-servicing.md)
- [Configurer les fichiers de vidage de mémoire](server-core-memory-dump.md)