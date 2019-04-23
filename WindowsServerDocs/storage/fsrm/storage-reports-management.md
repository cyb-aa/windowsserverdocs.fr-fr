---
title: Gestion des rapports de stockage
description: Cet article explique comment générer, planifier et surveiller des rapports de stockage
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: c0d750fd139865daa92319c1d1926dc5d36669b2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885770"
---
# <a name="storage-reports-management"></a>Gestion des rapports de stockage

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Sur le nœud **Gestion des rapports de stockage** du composant logiciel enfichable Microsoft<sup>®</sup> Management Console (MMC) Gestionnaire de ressources du serveur de fichiers, vous pouvez effectuer les tâches suivantes :

-   Planifier des rapports de stockage périodiques qui vous permettent d’identifier les tendances d’utilisation des disques.
-   Surveiller les tentatives d’enregistrement de fichiers non autorisés pour tous les utilisateurs ou un groupe sélectionné d’utilisateurs.
-   Générer des rapports de stockage instantanément.

Vous pouvez par exemple :

-   planifier un rapport qui s’exécutera tous les dimanches à minuit et qui générera une liste des derniers fichiers utilisés au cours des deux jours précédents. À l'aide de ces informations, vous pouvez surveiller l’activité de stockage du week-end et planifier les temps d’arrêt du serveur qui auront le moins d’impact sur les utilisateurs qui se connectent de leur domicile pendant le week-end.
-   Exécuter un rapport à tout moment pour identifier tous les fichiers en double dans un volume du serveur, afin que l’espace disque puisse être récupéré rapidement sans perte de données.
-   Exécuter un rapport Fichiers par groupe de fichiers pour identifier la manière dont les ressources de stockage sont réparties entre différents groupes de fichiers 
-   Exécuter un rapport Fichiers par propriétaire pour analyser la manière dont chaque utilisateur utilise les ressources de stockage partagé.

Cette section comprend les rubriques suivantes :

-   [Planifier un ensemble de rapports](schedule-set-of-reports.md)
-   [Générer des rapports à la demande](generate-reports-on-demand.md)

> [!Note]
> Pour définir des notifications par courrier électronique et certaines fonctionnalités de création de rapports, vous devez d’abord configurer les options générales du Gestionnaire de ressources du serveur de fichiers.

## <a name="see-also"></a>Voir aussi

-   [Options de paramètre File Server Resource Manager](setting-file-server-resource-manager-options.md)


