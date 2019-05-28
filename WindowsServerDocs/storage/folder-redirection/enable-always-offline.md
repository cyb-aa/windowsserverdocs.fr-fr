---
title: Activer le mode toujours hors connexion pour un accès plus rapide aux fichiers
description: Comment utiliser le mode toujours hors connexion des fichiers hors connexion pour fournir un accès plus rapide pour les fichiers mis en cache et les dossiers redirigés.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: 8684926beb0f0c911ac384970d15ba7d25f84079
ms.sourcegitcommit: ed27ddbe316d543b7865bc10590b238290a2a1ad
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65475931"
---
# <a name="enable-always-offline-mode-for-faster-access-to-files"></a>Activer le mode toujours hors connexion pour un accès plus rapide aux fichiers

>S’applique à : Windows 10, Windows 8, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2012 R2 et Windows (canal semi-annuel)

Ce document décrit comment utiliser le mode toujours hors connexion des fichiers hors connexion pour fournir un accès plus rapide pour les fichiers mis en cache et les dossiers redirigés. Toujours hors connexion fournit également l’utilisation de la bande passante inférieure, car les utilisateurs travaillent toujours hors connexion, même lorsqu’ils sont connectés via une connexion réseau haut débit.

## <a name="prerequisites"></a>Prérequis

Pour activer le mode toujours hors connexion, votre environnement doit respecter les conditions préalables suivantes.

- Un domaine de Services de domaine Active Directory (AD DS) avec les ordinateurs clients joints au domaine. Il n’existe aucune exigences de niveau fonctionnel de forêt ou domaine ou les spécifications de schéma.
- Ordinateurs clients exécutant Windows 10, Windows 8.1, Windows 8, Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012. (Les ordinateurs clients exécutant des versions antérieures de Windows peuvent continuer à passer en mode en ligne sur des connexions réseau très haut débit).
- Un ordinateur avec la gestion des stratégies de groupe installée.

## <a name="enable-always-offline-mode"></a>Activer le mode toujours hors connexion

Pour activer le mode toujours hors connexion, utilisez la stratégie de groupe pour activer la **configurer le mode de liaison lente** stratégie de configuration et la valeur est la latence **1** (millisecondes). De cette manière, les ordinateurs clients exécutant Windows 8 ou Windows Server 2012 afin d’utiliser automatiquement le mode toujours hors connexion.

>[!NOTE]
>Les ordinateurs qui exécutent Windows 7, Windows Vista, Windows Server 2008 R2 ou Windows Server 2008 peut continuer de transition vers le mode en ligne si la latence de la connexion réseau est inférieur à une milliseconde.

1. Ouvrez **gestion de stratégie de groupe**.
2. Pour éventuellement créer un nouvel objet (stratégie de groupe) pour les paramètres fichiers hors connexion, cliquez sur le domaine approprié ou l’unité d’organisation (UO), puis sélectionnez **créer un objet GPO dans ce domaine et le lier ici**.
3. Dans l’arborescence de la console, cliquez sur l’objet de stratégie de groupe pour lequel vous souhaitez configurer les paramètres fichiers hors connexion, puis sélectionnez **modifier**. Le **éditeur de gestion de stratégie de groupe** s’affiche.
4. Dans l’arborescence de la console, sous **Configuration ordinateur**, développez **stratégies**, développez **modèles d’administration**, développez **réseau**, Développez **fichiers hors connexion**.
5. Avec le bouton droit **configurer le mode de liaison lente**, puis sélectionnez **modifier**. Le **configurer le mode de liaison lente** fenêtre s’affiche.
6. Sélectionnez **Activé**.
7. Dans le **Options** boîte, sélectionnez **afficher**. Le **fenêtre d’afficher le contenu** s’affiche.
8. Dans le **nom de la valeur** , spécifiez le partage de fichiers pour lequel vous souhaitez activer le mode toujours hors connexion.
9. Pour activer le mode toujours hors connexion sur tous les partages de fichiers, entrez **\***.
10. Dans le **valeur** , entrez **latence = 1** pour définir le seuil de latence à une milliseconde, puis sélectionnez **OK**.

>[!NOTE]
>Par défaut, en mode toujours hors connexion, Windows synchronise les fichiers dans le cache de fichiers hors connexion en arrière-plan toutes les deux heures. Pour modifier cette valeur, utilisez le **Configure Background Sync** paramètre de stratégie.

## <a name="more-information"></a>Informations supplémentaires

* [Vue d’ensemble de la Redirection de dossiers, fichiers hors connexion et les profils utilisateur itinérants](folder-redirection-rup-overview.md)
* [Déployer la Redirection de dossiers des fichiers hors connexion](deploy-folder-redirection.md)