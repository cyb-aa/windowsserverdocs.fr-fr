---
title: Gestion du filtrage de fichiers
description: Cet article explique comment créer des filtres de fichiers, générer des notifications, définir des modèles de filtrage de fichiers et créer des exceptions au filtrage de fichiers
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 52a08ae7eaee81c00985d5334f9abeaa84e30879
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814160"
---
# <a name="file-screening-management"></a>Gestion du filtrage de fichiers

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Sur le nœud **Gestion du filtrage de fichiers** du composant logiciel enfichable MMC Gestionnaire de ressources du serveur de fichiers, vous pouvez effectuer les tâches suivantes :

-   Créer des filtres de fichiers pour contrôler les types de fichiers que les utilisateurs peuvent enregistrer et générer des notifications lorsque les utilisateurs tentent d’enregistrer des fichiers non autorisés.
-   Définir des modèles de filtrage de fichiers qui peuvent être appliqués aux nouveaux volumes ou dossiers et utilisés dans une organisation.
-   Créer des exceptions au filtrage de fichiers qui assouplissent les règles de filtrage de fichiers.

Vous pouvez par exemple :

-   Vous assurer qu’aucun fichier de musique n’est stocké dans des dossiers personnels sur un serveur, mais autoriser le stockage de certains types de fichiers multimédias prenant en charge la gestion des droits ou conformes aux stratégies d’entreprise. Dans le même scénario, vous souhaiterez peut-être accorder à un vice-président de l'entreprise des privilèges spéciaux pour qu'il puisse stocker tout type de fichiers dans son dossier personnel.
-   Implémenter un processus de filtrage pour recevoir une notification par courrier électronique si un fichier exécutable est stocké dans un dossier partagé, avec des informations sur l’utilisateur ayant stocké le fichier et l’emplacement du fichier exact, afin que vous puissiez prendre les précautions appropriées.

Cette section comprend les rubriques suivantes :

-   [Définir des groupes de fichiers pour le filtrage](define-file-groups-for-screening.md)
-   [Créer un filtre de fichiers](create-file-screen.md)
-   [Créer une Exception de filtre de fichier](create-file-screen-exception.md)
-   [Créer un modèle d’écran de fichier](create-file-screen-template.md)
-   [Modifier les propriétés de modèle de filtre de fichier](edit-file-screen-template-properties.md)

> [!Note]
> Pour définir des notifications par courrier électronique et certaines fonctionnalités de création de rapports, vous devez d’abord configurer les options générales du Gestionnaire de ressources du serveur de fichiers.

## <a name="see-also"></a>Voir aussi

-   [Options de paramètre File Server Resource Manager](setting-file-server-resource-manager-options.md)


