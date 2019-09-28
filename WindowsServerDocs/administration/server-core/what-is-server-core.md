---
title: Qu’est-ce que Server Core ?
description: En savoir plus sur l’option d’installation Server Core dans Windows Server
ms.prod: windows-server
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 02/20/2018
ms.openlocfilehash: 269be253367ba2bc692a5903e7d519a40f487d8b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383338"
---
# <a name="what-is-the-server-core-installation-option-in-windows-server"></a>Qu’est-ce que l’option d’installation Server Core dans Windows Server ?

> S’applique à : Windows Server 2019, Windows Server 2016 et Windows Server (canal semi-annuel)

L’option Server Core est une option d’installation minimale qui est disponible lors du déploiement de l’édition standard ou Datacenter de Windows Server. Server Core comprend la plupart des rôles de serveur, mais pas tous. Server Core a un encombrement de disque plus faible et, par conséquent, une surface d’attaque plus petite en raison d’une base de code plus petite. 

## <a name="server-core-vs-server-with-desktop-experience"></a>Serveur (Core) vs Server avec expérience utilisateur 
Lorsque vous installez Windows Server, vous ne devez installer que les rôles de serveur de votre choix. cela permet de réduire l’encombrement global de Windows Server. Toutefois, l’option d’installation serveur avec expérience utilisateur installe toujours de nombreux services et d’autres composants qui ne sont généralement pas nécessaires pour un scénario d’utilisation particulier. 

C’est là qu’intervient Server Core : l’installation Server Core élimine tous les services et autres fonctionnalités qui ne sont pas essentiels pour la prise en charge de certains rôles de serveur couramment utilisés. Par exemple, un serveur Hyper-V n’a pas besoin d’une interface utilisateur graphique (GUI), car vous pouvez gérer pratiquement tous les aspects d’Hyper-V à partir de la ligne de commande à l’aide de Windows PowerShell ou à distance à l’aide du Gestionnaire Hyper-V. 

## <a name="the-server-core-difference---core-capabilities-without-the-frills"></a>La différence Server Core-fonctionnalités principales sans base
Lorsque vous avez terminé l’installation de Server Core sur un système et que vous vous connectez pour la première fois, vous avez un peu de surprise. La principale différence entre l’option d’installation Server with Desktop Experience et Server Core est que Server Core n’inclut pas les packages Shell GUI suivants :

- Microsoft-Windows-Server-Shell-package
- Microsoft-Windows-Server-GUI-Mgmt-package
- Microsoft-Windows-Server-GUI-RSAT-package
- Microsoft-Windows-Cortana-PAL-Desktop-package

En d’autres termes, il n’y a **pas de bureau** dans Server Core, par conception. Tout en conservant les fonctionnalités requises pour prendre en charge les applications métier traditionnelles et les charges de travail basées sur les rôles, Server Core ne dispose pas d’une interface de bureau traditionnelle. Au lieu de cela, Server Core est conçu pour être géré à distance par le biais de la ligne de commande, de PowerShell ou d’un outil d’interface graphique utilisateur (par exemple, [RSAT](../../remote/remote-server-administration-tools.md) ou [Centre d’administration Windows](../../manage/windows-admin-center/overview.md)).

Outre l’absence d’interface utilisateur, Server Core diffère également du serveur avec l’expérience utilisateur des manières suivantes :

- Server Core n’a aucun outil d’accessibilité
- Aucun OOBE (prêt à l’emploi) pour la configuration de Server Core
- Aucune prise en charge audio

Le tableau suivant indique les applications qui sont disponibles *localement* sur Server Core vs Server avec expérience utilisateur. **Important** : Dans la plupart des cas, les applications répertoriées comme « non disponibles » ci-dessous peuvent être exécutées à distance à partir d’un ordinateur client Windows et utilisées pour gérer votre installation Server Core.

> [!NOTE]
> Cette liste est destinée à une référence rapide ; elle n’est pas destinée à constituer une liste complète.


| Application                     | Server Core     | Serveur avec Expérience de bureau |
|------------------------------------|-----------------|--------------------------------|
| Invite de commandes                     | disponible       | disponible                      |
| Windows PowerShell/Microsoft .NET | disponible       | disponible                      |
| Perfmon. exe                        | non disponibles  | disponible                      |
| WinDbg (GUI)                         | prise en charge       | prise en charge                      |
| Resmon. exe                         | non disponibles   | disponible                      |
| Regedit                            | disponible       | disponible                      |
| Fsutil. exe                         | disponible       | disponible                      |
| Disksnapshot. exe                   | non disponibles   | disponible                      |
| DiskPart.exe                       | disponible       | disponible                      |
| Diskmgmt. msc                       | non disponibles   | disponible                      |
| Devmgmt. msc                        | non disponibles   | disponible                      |
| Gestionnaire de serveur                     | non disponibles  | disponible                      |
| MMC. exe                            | non disponibles   | disponible                      |
| Eventvwr                           | non disponibles  | disponible                      |
| Wevtutil (requêtes d’événement)           | disponible       | disponible                      |
| Services. msc                       | non disponibles   | disponible                      |
| Panneau de configuration                      | non disponibles   | disponible                      |
| Windows Update (GUI)                 | non disponibles | disponible                      |
| Explorateur Windows                   | non disponibles   | disponible                      |
| Barre des tâches                            | non disponibles   | disponible                      |
| Notifications de la barre des tâches              | non disponibles   | disponible                      |
| Taskmgr                            | disponible       | disponible                      |
| Internet Explorer ou Edge          | non disponibles   | disponible                      |
| Système d’aide intégré               | non disponibles   | disponible                      |
| Shell Windows 10                   | non disponibles   | disponible                      |
| Lecteur Windows Media               | non disponibles   | disponible                      |
| PowerShell                         | disponible       | disponible                      |
| ISE PowerShell                     | non disponibles   | disponible                      |
| IME PowerShell                     | disponible       | disponible                      |
| Mstsc. exe                          | non disponibles   | disponible                      |
| Services Bureau à distance            | disponible       | disponible                      |
| Gestionnaire Hyper-V                    | non disponibles  | disponible                      |


Pour plus d’informations sur ce qui *est* inclus dans Server Core, voir [rôles, services de rôle et fonctionnalités inclus dans Windows Server-Server Core](server-core-roles-and-services.md). Pour plus d’informations sur les éléments qui *ne sont pas* inclus dans Server Core, voir [rôles, services de rôle et fonctionnalités non inclus dans Server Core](server-core-removed-roles.md)

## <a name="get-started-using-server-core"></a>Prise en main de l’utilisation de Server Core
Utilisez les informations suivantes pour installer, configurer et gérer l’option d’installation Server Core de Windows Server.

Installation Server Core : 
- [Rôles, services de rôle et fonctionnalités inclus dans Server Core](server-core-roles-and-services.md)
- [Rôles, services de rôle et fonctionnalités non disponibles dans Server Core](server-core-removed-roles.md)
- [Installer l’option d’installation minimale](../../get-started/getting-started-with-server-core.md)
- [Configurer Server Core avec l’outil SConfig](../../get-started/sconfig-on-ws2016.md)

Utilisation de Server Core :
- [Tâches d’administration Server Core de base à l’aide de Windows PowerShell ou de la ligne de commande](server-core-administer.md)
- [Gérer le Server Core](server-core-manage.md)
- [Mise à jour corrective de Server Core](server-core-servicing.md)
- [Configurer les fichiers de vidage mémoire](server-core-memory-dump.md)