---
title: Activer le mode toujours hors connexion pour un accès plus rapide aux fichiers
description: Comment utiliser le mode toujours hors connexion de Fichiers hors connexion pour fournir un accès plus rapide aux fichiers mis en cache et aux dossiers redirigés.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: 389fdd26a7e1d9824f1eaf0136a544547f08eb05
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71401955"
---
# <a name="enable-always-offline-mode-for-faster-access-to-files"></a>Activer le mode toujours hors connexion pour un accès plus rapide aux fichiers

>S’applique à : Windows 10, Windows 8, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2012 R2 et Windows (canal semi-annuel)

Ce document explique comment utiliser le mode toujours hors connexion de Fichiers hors connexion pour fournir un accès plus rapide aux fichiers mis en cache et aux dossiers redirigés. Toujours hors connexion offre également une utilisation plus faible de la bande passante, car les utilisateurs travaillent toujours hors connexion, même lorsqu’ils sont connectés via une connexion réseau à haut débit.

## <a name="prerequisites"></a>Prérequis

Pour activer le mode toujours hors connexion, votre environnement doit répondre aux conditions préalables suivantes.

- Un domaine Active Directory Domain Services (AD DS) avec des ordinateurs clients joints au domaine. Il n’existe aucune exigence de niveau fonctionnel de forêt ou de domaine ou de schéma.
- Ordinateurs clients exécutant Windows 10, Windows 8.1, Windows 8, Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012. (Les ordinateurs clients qui exécutent des versions antérieures de Windows peuvent continuer à passer en mode en ligne sur des connexions réseau très rapides.)
- Un ordinateur sur lequel la gestion des stratégie de groupe est installée.

## <a name="enable-always-offline-mode"></a>Activer le mode toujours hors connexion

Pour activer le mode toujours hors connexion, utilisez stratégie de groupe pour activer le paramètre de stratégie **configurer le mode de liaison lente** et définir la latence sur **1** (milliseconde). Cela amène les ordinateurs clients exécutant Windows 8 ou Windows Server 2012 à utiliser automatiquement le mode toujours hors connexion.

>[!NOTE]
>Les ordinateurs exécutant Windows 7, Windows Vista, Windows Server 2008 R2 ou Windows Server 2008 peuvent continuer à passer en mode en ligne si la latence de la connexion réseau tombe en dessous d’une milliseconde.

1. Ouvrez la **gestion des stratégie de groupe**.
2. Pour créer éventuellement un objet de stratégie de groupe pour Fichiers hors connexion paramètres, cliquez avec le bouton droit sur l’unité d’organisation ou le domaine approprié, puis sélectionnez **créer un objet de stratégie de groupe dans ce domaine, et le lier ici**.
3. Dans l’arborescence de la console, cliquez avec le bouton droit sur l’objet de stratégie de groupe pour lequel vous souhaitez configurer les paramètres de Fichiers hors connexion, puis sélectionnez **modifier**. Le **éditeur de gestion des stratégies de groupe** s’affiche.
4. Dans l’arborescence de la console, sous **Configuration ordinateur**, développez successivement **stratégies**, **Modèles d’administration**, **réseau**, puis **fichiers hors connexion**.
5. Cliquez avec le bouton droit sur **configurer le mode de liaison lente**, puis sélectionnez **modifier**. La fenêtre **configurer le mode de liaison lente** s’affiche.
6. Sélectionnez **Activé**.
7. Dans la zone **options** , sélectionnez **Afficher**. La **fenêtre afficher le contenu** s’affiche.
8. Dans la zone nom de la **valeur** , spécifiez le partage de fichiers pour lequel vous souhaitez activer le mode toujours hors connexion.
9. Pour activer le mode toujours hors connexion sur tous les partages de fichiers, entrez **\*** .
10. Dans la zone **valeur** , entrez **latence = 1** pour définir le seuil de latence sur une milliseconde, puis sélectionnez **OK**.

>[!NOTE]
>Par défaut, lorsqu’il est en mode toujours hors connexion, Windows synchronise les fichiers dans le cache de Fichiers hors connexion en arrière-plan toutes les deux heures. Pour modifier cette valeur, utilisez le paramètre de stratégie **configurer synchronisation en arrière-plan** .

## <a name="more-information"></a>Plus d’informations

* [Vue d’ensemble de la redirection de dossiers, des Fichiers hors connexion et des profils utilisateur itinérants](folder-redirection-rup-overview.md)
* [Déployer la redirection de dossiers avec Fichiers hors connexion](deploy-folder-redirection.md)