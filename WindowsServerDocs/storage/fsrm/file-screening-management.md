---
title: Gestion du filtrage de fichiers
description: "Cet article explique comment créer des filtres de fichiers, générer des notifications, définir des modèles de filtrage de fichiers et créer des exceptions au filtrage de fichiers"
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 52a08ae7eaee81c00985d5334f9abeaa84e30879
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# <a name="file-screening-management"></a>Gestion du filtrage de fichiers

> S’applique à: WindowsServer (canal semi-annuel), WindowsServer2016, WindowsServer2012R2, WindowsServer2012, WindowsServer2008R2

Sur le nœud **Gestion du filtrage de fichiers** du composant logiciel enfichable MMC Gestionnaire de ressources du serveur de fichiers, vous pouvez effectuer les tâches suivantes:

-   Créer des filtres de fichiers pour contrôler les types de fichiers que les utilisateurs peuvent enregistrer et générer des notifications lorsque les utilisateurs tentent d’enregistrer des fichiers non autorisés.
-   Définir des modèles de filtrage de fichiers qui peuvent être appliqués aux nouveaux volumes ou dossiers et utilisés dans une organisation.
-   Créer des exceptions au filtrage de fichiers qui assouplissent les règles de filtrage de fichiers.

Vous pouvez par exemple:

-   Vous assurer qu’aucun fichier de musique n’est stocké dans des dossiers personnels sur un serveur, mais autoriser le stockage de certains types de fichiers multimédias prenant en charge la gestion des droits ou conformes aux stratégies d’entreprise. Dans le même scénario, vous souhaiterez peut-être accorder à un vice-président de l'entreprise des privilèges spéciaux pour qu'il puisse stocker tout type de fichiers dans son dossier personnel.
-   Implémenter un processus de filtrage pour recevoir une notification par courrier électronique si un fichier exécutable est stocké dans un dossier partagé, avec des informations sur l’utilisateur ayant stocké le fichier et l’emplacement du fichier exact, afin que vous puissiez prendre les précautions appropriées.

Cette section comprend les rubriques suivantes:

-   [Définir des groupes de fichiers pour le filtrage](define-file-groups-for-screening.md)
-   [Créer un filtre de fichiers](create-file-screen.md)
-   [Créer une exception au filtre de fichiers](create-file-screen-exception.md)
-   [Créer un modèle de filtre de fichiers](create-file-screen-template.md)
-   [Modifier les propriétés du modèle de filtre de fichiers](edit-file-screen-template-properties.md)

> [!Note]
> Pour définir des notifications par courrier électronique et certaines fonctionnalités de création de rapports, vous devez d’abord configurer les options générales du Gestionnaire de ressources du serveur de fichiers.

## <a name="see-also"></a>Articles associés

-   [Définition des options du Gestionnaire de ressources du serveur de fichiers](setting-file-server-resource-manager-options.md)


