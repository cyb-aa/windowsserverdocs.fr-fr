---
title: 'Notes de publication : problèmes importants dans Windows Server 2019'
description: Résume les problèmes critiques nécessitant des solutions de contournement pour éviter les blocages, mise en suspens, installation échec et pertes de données
ms.prod: windows-server-threshold
ms.technology: server-general
ms.topic: article
ms.assetid: 134aab85-664f-4d55-87ef-9e5fd098371f
author: jasongerend
ms.author: jgerend
manager: jasgroce
ms.localizationpriority: medium
ms.date: 06/07/2019
ms.openlocfilehash: 515255c301d343aa1b83bcfb506f2e3baa6ca969
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66810739"
---
# <a name="release-notes---important-issues-in-windows-server-2019"></a>Notes de publication : problèmes importants dans Windows Server 2019

>S'applique à : Windows Server 2019

Ces notes de publication résument les problèmes les plus critiques dans le système d’exploitation Windows Server 2019, y compris les façons d’éviter ou résoudre les problèmes, s’il est connu. Pour plus d’informations sur les modifications apportées par la conception, les nouvelles fonctionnalités et les correctifs dans cette version, consultez [What ' s New in Windows Server 2019](whats-new-19.md) et annonces publiées par les équipes de fonctionnalité spécifique. Sauf indication contraire, chaque problème signalé s’applique à toutes les éditions et options d’installation de Windows Server 2019.  

Ce document est continuellement mis à jour. Les problèmes critiques nécessitant une solution de contournement sont ajoutés dès qu’ils sont identifiés, tout comme les solutions de contournement et les correctifs.  

## <a name="release-notes"></a>Notes de publication

Les problèmes connus suivants sont présents dans Windows Server 2019.

| Titre         | Description                            |
| -----         | -----------                            |
| Menu d’option d’installation pendant l’installation du serveur a tronqué le texte en allemand | Lorsque vous exécutez le programme d’installation à partir du support de serveur allemand, dans la fenêtre de sélection système d’exploitation intitulée, « Sélectionner le système d’exploitation que vous souhaitez installer, » la description pour les options d’installation expérience ont les caractères manquantes ou incorrectes à la fin de la phrase. Voici le texte complet allemand tel qu’il doit apparaître.<br/>      <br/>`Durch diese Option wird die vollständige grafische Umgebung von Windows installiert, wodurch zusätzlicher Speicherplatz verbraucht wird. Sie kann hilfreich sein, wenn Sie den Windows-Desktop verwenden möchten oder über eine App verfügen, die die grafische Umgebung benötigt.` <br><br>Cela affecte uniquement le support allemand libéré à la disponibilité publique de Windows Server 2019, Windows Server, version 1809 et Microsoft Hyper-V Server 2019.|
| Image de personnalisation de Windows Server incorrect lors de l’installation de Windows Server, version 1809 | Au cours de l’expérience d’installation pour Windows Server, version 1809, les écrans de l’image d’arrière-plan sur certains initiale montre &quot;Windows Server 2019&quot;.  Comme avec Windows Server, versions 1709 et 1803, il vous suffit de dire &quot;Windows Server&quot;.  Il n’y a aucune autres impacts sur n’importe où dans le produit et n’a aucun impact sur le produit Windows Server 2019.  Le problème est limité à cette une image pendant l’installation de Windows Server, version 1809, disponible uniquement pour les clients de licence de volume l’accès à du centre de Service de licence de Volume.<br/> |

### <a name="copyright"></a>Copyright

Ce document est fourni «en l'état». Les informations et points de vue contenus dans ce document, y compris les URL et autres références à des sites web, peuvent faire l'objet de modifications sans préavis.  

Ce document ne vous fournit aucun droit légal de propriété intellectuelle de tout produit Microsoft. Vous pouvez copier le présent document pour une utilisation interne à des fins de référence.

&copy; 2019 Microsoft Corporation. Tous droits réservés.  

Microsoft, Active Directory, Hyper-V, Windows et WindowsServer sont soit des marques, soit des marques déposées de Microsoft Corporation aux États-Unis d’Amérique et/ou dans d’autres pays.  

Ce produit contient un logiciel de filtrage graphique qui est basé en partie sur le travail du groupe IndependentJPEGGroup.  


1.0  
