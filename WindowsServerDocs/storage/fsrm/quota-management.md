---
title: Gestion de quota
description: "Cet article explique comment créer et gérer des quotas"
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 60436f12e07b8a3f16312829d53a2885c98f30ed
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# <a name="quota-management"></a>Gestion de quota

> S’applique à: WindowsServer (canal semi-annuel), WindowsServer2016, WindowsServer2012R2, WindowsServer2012, WindowsServer2008R2

Sur le nœud **Gestion de quota** du composant logiciel enfichable Microsoft<sup>®</sup> Management Console (MMC) Gestionnaire de ressources du serveur de fichiers, vous pouvez effectuer les tâches suivantes:

-   Créer des quotas limitant l'espace autorisé pour un volume ou un dossier et générer des notifications lorsque les limites du quota sont presque atteintes ou dépassées.
-   Générer des quotas automatiques qui s’appliquent à tous les sous-dossiers existants dans un volume ou un dossier, ainsi qu'à tous les sous-dossiers créés à l’avenir.
-   Définir des modèles de quota qui peuvent être facilement appliqués aux nouveaux volumes ou dossiers et utilisés dans toute l'organisation.

Vous pouvez par exemple:

-   Placer une limite de 200mégaoctets (Mo) sur les dossiers serveur personnels des utilisateurs et prévoir l'envoi à vous-même et à l’utilisateur d'une notification par courrier électronique lorsque le stockage dépasse le seuil de 180Mo.
-   Définir un quota flexible de 500Mo sur les dossiers partagés d’un groupe. Lorsque cette limite est atteinte, tous les utilisateurs du groupe sont avertis par courrier électronique que le quota de stockage a été temporairement augmenté à 520Mo, pour leur permettre de supprimer les fichiers inutiles et de revenir dans les limites de stratégie de quota initiale de 500Mo.
-   Recevoir une notification lorsqu’un dossier temporaire atteint 2gigaoctets (Go) d'utilisation, mais sans limiter le quota du dossier, qui est nécessaire pour un service s'exécutant sur votre serveur.

Cette section comprend les rubriques suivantes:

-   [Créer un quota](create-quota.md)
-   [Créer un quota automatique](create-auto-apply-quota.md)
-   [Créer un modèle de quota](create-quota-template.md)
-   [Modifier les propriétés du modèle de quota](edit-quota-template-properties.md)
-   [Modifier les propriétés de quota automatique](edit-auto-apply-quota-properties.md)

> [!Note]
> Pour définir des notifications par courrier électronique et des fonctionnalités de création de rapports, vous devez d’abord configurer les options générales du Gestionnaire de ressources du serveur de fichiers.

## <a name="see-also"></a>Articles associés

-   [Définition des options du Gestionnaire de ressources du serveur de fichiers](setting-file-server-resource-manager-options.md)


