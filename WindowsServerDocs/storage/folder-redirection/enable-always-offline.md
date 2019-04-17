---
title: Activer le mode d’accès plus rapide aux fichiers toujours en mode hors connexion
description: Découvrez comment utiliser le mode toujours en mode hors connexion des fichiers hors connexion pour fournir un accès plus rapide aux fichiers mis en cache et aux dossiers redirigés.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: bc54b1e33d09e7f2b9eea01e4f09fb83f13dc1af
ms.sourcegitcommit: 505505b7ec99506e7ff50eddbdd6aa94a602abe6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/11/2018
ms.locfileid: "3831389"
---
# Activer le mode d’accès plus rapide aux fichiers toujours en mode hors connexion

>S’applique à: Windows 10, Windows 8, Windows 8.1, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016

Ce document décrit comment utiliser le mode toujours en mode hors connexion des fichiers hors connexion pour fournir un accès plus rapide aux fichiers mis en cache et aux dossiers redirigés. Toujours en mode hors connexion fournit également moindre utilisation de la bande passante dans la mesure où les utilisateurs sont toujours en mode hors connexion, même lorsqu’ils sont connectés via une connexion réseau à haut débit.

## Conditions préalables

Pour activer le mode toujours en mode hors connexion, votre environnement doit satisfaire les conditions préalables suivantes.

- Un domaine Active Directory Domain Services (AD DS) avec des ordinateurs clients joints au domaine. Il n’existe aucune exigences de niveau fonctionnel de forêt ou un domaine ou les spécifications de schéma.
- Ordinateurs clients exécutant Windows 10, Windows 8.1, Windows 8, Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012. (Les ordinateurs clients exécutant des versions antérieures de Windows peuvent continuer à la transition vers le mode en ligne sur des connexions réseau très haut débit).
- Un ordinateur avec la gestion de stratégie de groupe installée.

## Activer le mode toujours en mode hors connexion

Pour activer le mode toujours en mode hors connexion, utilisez la stratégie de groupe pour activer le paramètre de stratégie de **mode de liaison lente de configurer** et de la latence de la valeur **1** (milliseconde). De cette manière, les ordinateurs clients exécutant Windows 8 ou Windows Server 2012 à utiliser automatiquement le mode toujours en mode hors connexion.

>[!NOTE]
>Les ordinateurs exécutant Windows 7, Windows Vista, Windows Server 2008 R2 ou Windows Server 2008 peut continuer à effectuer la transition vers le mode en ligne si la latence de la connexion réseau est inférieur à une milliseconde.

1. Ouvrez **Gestion des stratégies de groupe**.
2. Pour créer éventuellement une nouvelle stratégie de groupe objet (GPO) pour les paramètres des fichiers hors connexion, cliquez sur l’unité d’organisation (UO) ou du domaine approprié et sélectionnez **créer un objet GPO dans ce domaine et le lier ici**.
3. Dans l’arborescence de la console, cliquez sur l’objet de stratégie de groupe pour lequel vous souhaitez configurer les paramètres de fichiers hors connexion et sélectionnez **Modifier**. L' **Éditeur de gestion des stratégies de groupe** s’affiche.
4. Dans l’arborescence de la console, sous **Configuration de l’ordinateur**, développez des **stratégies**, développez **Modèles d’administration**, développez **réseau**, puis **Fichiers hors connexion**.
5. **Configurer le mode de liaison lente**d’avec le bouton droit et sélectionnez **Modifier**. La fenêtre de **configurer le mode de liaison lente** s’affiche.
6. Sélectionnez **Activé**.
7. Dans la zone **Options** , sélectionnez **Afficher**. La **fenêtre d’afficher le contenu** s’affiche.
8. Dans la zone **nom de la valeur** , spécifiez le partage de fichiers pour laquelle vous souhaitez activer le mode toujours en mode hors connexion.
9. Pour activer le mode toujours en mode hors connexion sur tous les partages de fichiers, entrez **\***.
10. Dans la zone de **valeur** , entrez **latence = 1** pour définir le seuil de latence à une milliseconde, puis sélectionnez **OK**.

>[!NOTE]
>Par défaut, en mode hors connexion toujours, Windows synchronise les fichiers dans le cache de fichiers hors connexion en arrière-plan toutes les deux heures. Pour modifier cette valeur, utilisez le paramètre de stratégie de **Configurer la synchronisation en arrière-plan** .

## Informations supplémentaires

* [Vue d’ensemble de la Redirection de dossiers, fichiers hors connexion et les profils utilisateur itinérants](folder-redirection-rup-overview.md)
* [Déployer la Redirection de dossiers avec des fichiers hors connexion](deploy-folder-redirection.md)