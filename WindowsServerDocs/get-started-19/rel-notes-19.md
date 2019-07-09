---
title: 'Notes de publication : problèmes importants dans Windows Server 2019'
description: Résume les problèmes critiques nécessitant des solutions de contournement pour éviter une panne, des blocages, un échec d’installation ou une perte de données
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
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/17/2019
ms.locfileid: "66810739"
---
# <a name="release-notes---important-issues-in-windows-server-2019"></a>Notes de publication : problèmes importants dans Windows Server 2019

>S'applique à : Windows Server 2019

Ces notes de publication résument les problèmes les plus critiques du système d’exploitation Windows Server 2019 et expliquent, le cas échéant, comment les éviter ou les résoudre. Pour plus sur d’informations sur les modifications de l’interface, les nouvelles fonctionnalités et les correctifs de cette version, consultez [What’s New in Windows Server 2019](whats-new-19.md) (Nouveautés de Windows Server 2019) et les annonces publiées par les équipes en charge des différentes fonctionnalités. Sauf mention contraire, chaque problème signalé s’applique à toutes les éditions et options d’installation de Windows Server 2019.  

Ce document est continuellement mis à jour. Les problèmes critiques nécessitant une solution de contournement sont ajoutés dès qu’ils sont identifiés, tout comme les solutions de contournement et les correctifs.  

## <a name="release-notes"></a>Notes de publication

Les problèmes connus suivants sont présents dans Windows Server 2019.

| Titre         | Description                            |
| -----         | -----------                            |
| Le texte en allemand du menu d’options d’installation pendant l’installation du serveur est tronqué | Lorsque vous exécutez le programme d’installation à partir d’un support de serveur allemand, dans la fenêtre de sélection du système d’exploitation intitulée « Sélectionner le système d’exploitation que vous souhaitez installer », la description des options d’installation Expérience utilisateur contient des caractères incorrects ou manquants à la fin de la phrase. Voici le texte allemand complet tel qu’il doit apparaître.<br/>      <br/>`Durch diese Option wird die vollständige grafische Umgebung von Windows installiert, wodurch zusätzlicher Speicherplatz verbraucht wird. Sie kann hilfreich sein, wenn Sie den Windows-Desktop verwenden möchten oder über eine App verfügen, die die grafische Umgebung benötigt.` <br><br>Cela affecte uniquement le support allemand en disponibilité publique de Windows Server 2019, Windows Server, version 1809 et Microsoft Hyper-V Server 2019.|
| Image de marque de Windows Server incorrecte lors de l’installation de Windows Server, version 1809 | Au cours de l’expérience d’installation pour Windows Server, version 1809, l’image d’arrière-plan sur certains écrans initiaux montre &quot;Windows Server 2019&quot;.  Comme avec Windows Server, versions 1709 et 1803, cela devrait simplement dire &quot;Windows Server&quot;.  Il n’y a aucun autre impact au sein du produit, ni sur le produit Windows Server 2019.  Le problème est limité à cette image pendant l’installation de Windows Server, version 1809, disponible uniquement pour les clients de licence en volume accédant au centre de service de licence en volume.<br/> |

### <a name="copyright"></a>Copyright

Ce document est fourni « en l'état ». Les informations contenues dans ce document, y compris les URL et autres références de sites Web, pourront faire l’objet de modifications sans préavis.  

Ce document ne vous fournit aucun droit légal de propriété intellectuelle de tout produit Microsoft. Vous pouvez copier le présent document pour une utilisation interne à des fins de référence.

&copy; 2019 Microsoft Corporation. Tous droits réservés.  

Microsoft, Active Directory, Hyper-V, Windows et Windows Server sont soit des marques, soit des marques déposées de Microsoft Corporation aux États-Unis d’Amérique et/ou dans d’autres pays.  

Ce produit contient un logiciel de filtrage graphique qui est basé en partie sur le travail du groupe IndependentJPEGGroup.  


1.0  
