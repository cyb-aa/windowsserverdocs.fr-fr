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
ms.openlocfilehash: 6effaf7c2d197c08b4930e09c3ada96462b17d6f
ms.sourcegitcommit: ed27ddbe316d543b7865bc10590b238290a2a1ad
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65476192"
---
# <a name="quota-management"></a>Gestion de quota

> S’applique à : Windows Server 2019, Windows Server 2016, Windows Server (canal semi-annuel), Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Sur le nœud **Gestion de quota** du composant logiciel enfichable Microsoft<sup>®</sup> Management Console (MMC) Gestionnaire de ressources du serveur de fichiers, vous pouvez effectuer les tâches suivantes :

-   Créer des quotas limitant l'espace autorisé pour un volume ou un dossier et générer des notifications lorsque les limites du quota sont presque atteintes ou dépassées.
-   Générer des quotas automatiques qui s’appliquent à tous les sous-dossiers existants dans un volume ou un dossier, ainsi qu'à tous les sous-dossiers créés à l’avenir.
-   Définir des modèles de quota qui peuvent être facilement appliqués aux nouveaux volumes ou dossiers et utilisés dans toute l'organisation.

Vous pouvez par exemple :

-   Placez une limite de 200 mégaoctets (Mo) sur les dossiers serveur personnel des utilisateurs, avec une notification par courrier électronique envoyée à vous et l’utilisateur quand 180 Mo de stockage a été dépassé.
-   Définir un quota de 500 Mo flexible sur le dossier partagé d’un groupe. Lorsque cette limite est atteinte, tous les utilisateurs dans le groupe sont avertis par courrier électronique que le quota de stockage a été temporairement augmenté à 520 Mo afin qu’ils peuvent supprimer les fichiers inutiles et conformes à la stratégie de quota de 500 Mo prédéfini.
-   Recevoir une notification lorsqu’un dossier temporaire atteint 2 gigaoctets (Go) d'utilisation, mais sans limiter le quota du dossier, qui est nécessaire pour un service s'exécutant sur votre serveur.

Cette section comprend les rubriques suivantes :

-   [Créer un quota](create-quota.md)
-   [Créer un quota automatique](create-auto-apply-quota.md)
-   [Créer un modèle de Quota](create-quota-template.md)
-   [Modifier les propriétés du modèle de quota](edit-quota-template-properties.md)
-   [Modifier les propriétés de quota automatique](edit-auto-apply-quota-properties.md)

> [!Note]
> Pour définir des notifications par courrier électronique et des fonctionnalités de création de rapports, vous devez d’abord configurer les options générales du Gestionnaire de ressources du serveur de fichiers.

## <a name="see-also"></a>Voir aussi

-   [Définition des options du Gestionnaire de ressources du serveur de fichiers](setting-file-server-resource-manager-options.md)


