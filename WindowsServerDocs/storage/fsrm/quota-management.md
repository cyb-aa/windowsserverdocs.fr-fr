---
title: Gestion de quota
description: Cet article explique comment créer et gérer des quotas
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 5a655e28020d08bb1c10fa862c007f914a8cf566
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403079"
---
# <a name="quota-management"></a>Gestion de quota

> S’applique à : Windows Server 2019, Windows Server 2016, Windows Server (canal semi-annuel), Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Sur le nœud **Gestion de quota** du composant logiciel enfichable Microsoft<sup>®</sup> Management Console (MMC) Gestionnaire de ressources du serveur de fichiers, vous pouvez effectuer les tâches suivantes :

-   Créer des quotas limitant l'espace autorisé pour un volume ou un dossier et générer des notifications lorsque les limites du quota sont presque atteintes ou dépassées.
-   Générer des quotas automatiques qui s’appliquent à tous les sous-dossiers existants dans un volume ou un dossier, ainsi qu'à tous les sous-dossiers créés à l’avenir.
-   Définir des modèles de quota qui peuvent être facilement appliqués aux nouveaux volumes ou dossiers et utilisés dans toute l'organisation.

Vous pouvez par exemple :

-   Placez une limite de 200 mégaoctets (Mo) sur les dossiers du serveur personnel des utilisateurs, une notification par courrier électronique vous est envoyée et l’utilisateur en cas de dépassement de 180 Mo de stockage.
-   Définissez un quota flexible de 500 Mo sur le dossier partagé d’un groupe. Lorsque cette limite de stockage est atteinte, tous les utilisateurs du groupe sont avertis par courrier électronique que le quota de stockage a été temporairement étendu à 520 Mo pour qu’ils puissent supprimer les fichiers inutiles et respecter la stratégie de quota prédéfinie de 500 Mo.
-   Recevoir une notification lorsqu’un dossier temporaire atteint 2 gigaoctets (Go) d'utilisation, mais sans limiter le quota du dossier, qui est nécessaire pour un service s'exécutant sur votre serveur.

Cette section comprend les rubriques suivantes :

-   [Créer un quota](create-quota.md)
-   [Créer un quota automatique](create-auto-apply-quota.md)
-   [Créer un modèle de quota](create-quota-template.md)
-   [Modifier les propriétés du modèle de quota](edit-quota-template-properties.md)
-   [Modifier les propriétés de quota automatique](edit-auto-apply-quota-properties.md)

> [!Note]
> Pour définir des notifications par courrier électronique et des fonctionnalités de création de rapports, vous devez d’abord configurer les options générales du Gestionnaire de ressources du serveur de fichiers.

## <a name="see-also"></a>Voir aussi

-   [Définition des options du Gestionnaire de ressources du serveur de fichiers](setting-file-server-resource-manager-options.md)


