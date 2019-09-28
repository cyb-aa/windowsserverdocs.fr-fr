---
title: Utiliser des périphériques USB
description: En savoir plus sur le fonctionnement des périphériques USB avec MultiPoint services
ms.custom: na
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a33f2b83-bbc2-4fc1-8a94-aaa985dfe1f9
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: ce4338eccc5640f8743093649685054718f9ed2a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394776"
---
# <a name="work-with-usb-devices"></a>Utiliser des périphériques USB
Vous pouvez connecter des appareils à l’ordinateur de votre système MultiPoint services ou à un concentrateur de station MultiPoint. Selon son emplacement de connexion et son type, un périphérique est disponible pour l’ensemble ou pour certains des utilisateurs du système, ou n’est disponible pour aucun utilisateur. Voici quelques exemples de types de connexions :  
  
-   Si vous connectez un périphérique directement à l’ordinateur, comme une imprimante ou un périphérique de stockage de masse USB, il est accessible à tous les utilisateurs de la session sur le système MultiPoint Services. Les utilisateurs de station de bureau virtuel ne sont pas en mesure d’accéder aux périphériques connectés directement à l’ordinateur.  
  
-   Si vous connectez un périphérique à un concentrateur de station, par exemple un clavier, une souris, un *périphérique audio* ou un périphérique de stockage de masse, le périphérique est disponible uniquement pour l’utilisateur connecté à cette station MultiPoint Services.  
  
-   Si vous connectez certains types de périphériques à l’ordinateur, comme un clavier ou une souris, les périphériques ne sont disponibles pour aucun utilisateur sur le système.  
  
Le tableau suivant répertorie quelques périphériques et leur fonctionnement selon l’endroit du système où ils sont connectés. Pour plus d’informations sur la connexion de hubs de station, voir [utilisation des concentrateurs de station](#working-with-station-hubs). Pour plus d’informations sur la connexion de moniteurs vidéo à une station, voir [utiliser des périphériques vidéo](Work-with-Video-Devices.md).  
  
|||||  
|-|-|-|-|  
|**Appareil**|**Comportement lorsqu’il est directement connecté à l’ordinateur**|**Comportement lorsqu’il est connecté à une station**|**Notes**|  
|Clavier|Il est déconseillé de connecter un clavier directement à l’ordinateur.|Accessible uniquement à l’utilisateur de la station.|Si le clavier contient un port USB, le concentrateur USB à l’intérieur du clavier peut être le concentrateur de station. Les autres périphériques USB connectés à ce port sont disponibles uniquement pour l’utilisateur de ce clavier.<br /><br />Certains concentrateurs de station sont équipés d’un port souris PS\/2 qui est converti en une connexion USB dans le concentrateur.|  
|Souris|Il est déconseillé de connecter une souris directement à l’ordinateur.|Accessible uniquement à l’utilisateur de la station.|Certains concentrateurs de station sont équipés d’un port souris PS\/2 qui est converti en une connexion USB dans le concentrateur.|  
|Concentrateur USB|Consultez [utilisation des concentrateurs de station](#working-with-station-hubs).|Consultez [utilisation des concentrateurs de station](#working-with-station-hubs).||  
|Moniteur vidéo|Consultez [périphériques vidéo multipoint services](work-with-video-devices.md).|Consultez [périphériques vidéo multipoint services](work-with-video-devices.md).||  
|Périphériques de sortie audio, tels qu’un casque|Il est déconseillé de connecter un périphérique de sortie audio directement à l’ordinateur.|Accessible uniquement à l’utilisateur de la station.|Certains concentrateurs de station sont équipés d’un port audio analogique qui est converti en une connexion audio USB dans le concentrateur.|  
|Périphériques d’entrée audio, tels qu’un micro|Il est déconseillé de connecter un périphérique d’entrée audio directement à l’ordinateur.|Accessible uniquement à l’utilisateur de la station.|Certains concentrateurs de station sont équipés d’un port audio analogique qui est converti en une connexion audio USB dans le concentrateur.|  
|Imprimantes|Accessible à tous les utilisateurs sur le système. *|Accessible uniquement à l’utilisateur de la station.||  
|Périphérique de stockage de masse USB|Accessible à tous les utilisateurs sur le système. \*|Accessible uniquement à l’utilisateur de la station.|Ces périphériques incluent les disques mémoire flash USB, les lecteurs de disques durs externes et les appareils photo numériques.|  
|Webcams|Accessible à tous les utilisateurs sur le système. *|Accessible uniquement à l’utilisateur de la station.|Un seul utilisateur peut se connecter à la webcam à la fois.|  
  
\* Les appareils qui sont connectés à l’ordinateur hôte ne sont pas visibles par les utilisateurs qui sont connectés à des stations de travail virtuelles.  
  
Pour plus d’informations sur l’installation d’une station, voir [Installer une station](Set-Up-a-Station.md).  
  
### <a name="working-with-station-hubs"></a>Utilisation des concentrateurs de station  
Un concentrateur USB peut être utilisé de quatre manières différentes quand il est connecté à un système MultiPoint Services. Chacun des scénarios suivants offre un accès différent aux périphériques qui y sont connectés, en fonction du type de concentrateur et de l’emplacement du système où il est connecté.  
  
-   Un concentrateur de station connecté à l’ordinateur du système MultiPoint Services avec un clavier permet de créer une station MultiPoint Services. Un clavier et une souris sont connectés au concentrateur de station en utilisant les ports disponibles sur le concentrateur. Un moniteur vidéo est connecté à un port vidéo de l’ordinateur ou à une carte vidéo du concentrateur de station, le cas échéant. Le clavier, la souris et le moniteur sont alors *associés* à une station MultiPoint Services.  
  
-   Un concentrateur USB connecté à l’ordinateur du système MultiPoint Services sans clavier permet de connecter d’autres périphériques à l’ordinateur quand ce dernier n’a pas assez de ports pour ces périphériques. Tous les périphériques connectés à ce concentrateur USB sont disponibles pour tous les utilisateurs du système MultiPoint Services. Cela ne constitue toutefois pas un concentrateur de station MultiPoint Services.  
  
-   Un concentrateur USB alimenté connecté à l’ordinateur sur le système MultiPoint Services, également appelé concentrateur intermédiaire, permet de connecter d’autres concentrateurs USB utilisés pour créer des stations MultiPoint.  
  
-   Un concentrateur USB connecté à un concentrateur de station permet de connecter d’autres périphériques au concentrateur de station. Les claviers doivent être connectés directement au concentrateur de station.  
  
Pour plus d’informations sur l’installation d’une station MultiPoint Services, voir [Installer une station](Set-Up-a-Station.md).  
  
## <a name="see-also"></a>Voir aussi  
[Utiliser des périphériques vidéo](Work-with-Video-Devices.md)  
[Gérer le matériel de la station](Manage-Station-Hardware.md)  
[Installer une station](Set-Up-a-Station.md)