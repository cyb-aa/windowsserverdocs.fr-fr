---
title: Gestion de quota
description: Cet article explique comment créer et gérer des quotas
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: febcd6ab0744a7fddd024e1f0afdb93711e8939a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829540"
---
# <a name="quota-management"></a>Gestion de quota

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Sur le nœud **Gestion de quota** du composant logiciel enfichable Microsoft<sup>®</sup> Management Console (MMC) Gestionnaire de ressources du serveur de fichiers, vous pouvez effectuer les tâches suivantes :

-   Créer des quotas limitant l'espace autorisé pour un volume ou un dossier et générer des notifications lorsque les limites du quota sont presque atteintes ou dépassées.
-   Générer des quotas automatiques qui s’appliquent à tous les sous-dossiers existants dans un volume ou un dossier, ainsi qu'à tous les sous-dossiers créés à l’avenir.
-   Définir des modèles de quota qui peuvent être facilement appliqués aux nouveaux volumes ou dossiers et utilisés dans toute l'organisation.

Vous pouvez par exemple :

-   Placez une limite de 200 mégaoctets (Mo) sur les dossiers serveur personnel des utilisateurs, avec une notification par courrier électronique envoyée à vous et l’utilisateur quand 180 Mo de stockage a été dépassé.
-   Définir un quota de 500 Mo flexible sur le dossier partagé d’un groupe. Lorsque cette limite est atteinte, tous les utilisateurs dans le groupe sont avertis par courrier électronique que le quota de stockage a été temporairement augmenté à 520 Mo afin qu’ils peuvent supprimer les fichiers inutiles et conformes à la stratégie de quota de 500 Mo prédéfini.
-   Recevoir une notification lorsqu’un dossier temporaire atteint 2 gigaoctets (Go) d'utilisation, mais sans limiter le quota du dossier, qui est nécessaire pour un service s'exécutant sur votre serveur.

Cette section comprend les rubriques suivantes :

-   [Créer un Quota](create-quota.md)
-   [Créer une fonction automatique d’un quota](create-auto-apply-quota.md)
-   [Créer un modèle de Quota](create-quota-template.md)
-   [Modifier les propriétés de modèle de Quota](edit-quota-template-properties.md)
-   [Modifier automatiquement appliquer des propriétés de Quota](edit-auto-apply-quota-properties.md)

> [!Note]
> Pour définir des notifications par courrier électronique et des fonctionnalités de création de rapports, vous devez d’abord configurer les options générales du Gestionnaire de ressources du serveur de fichiers.

## <a name="see-also"></a>Voir aussi

-   [Options de paramètre File Server Resource Manager](setting-file-server-resource-manager-options.md)


