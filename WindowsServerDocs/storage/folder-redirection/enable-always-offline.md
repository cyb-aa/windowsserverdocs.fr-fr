---
title: Activer le mode Toujours hors connexion pour accéder plus rapidement aux fichiers
description: Découvrez comment utiliser le mode Toujours hors connexion dans Fichiers hors connexion pour fournir un accès plus rapide aux fichiers mis en cache et aux dossiers redirigés.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: 389fdd26a7e1d9824f1eaf0136a544547f08eb05
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2020
ms.locfileid: "71401955"
---
# <a name="enable-always-offline-mode-for-faster-access-to-files"></a>Activer le mode Toujours hors connexion pour accéder plus rapidement aux fichiers

>S'applique à : Windows 10, Windows 8, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2012 R2 et Windows (canal semi-annuel)

Ce document décrit comment utiliser le mode Toujours hors connexion dans Fichiers hors connexion pour fournir un accès plus rapide aux fichiers mis en cache et aux dossiers redirigés. Le mode Toujours hors connexion assure également une utilisation réduite de la bande passante dans la mesure où les utilisateurs travaillent toujours hors connexion, même s’ils sont connectés avec une connexion réseau haut débit.

## <a name="prerequisites"></a>Prérequis

L’activation du mode Toujours hors connexion nécessite un environnement respectant les prérequis suivants.

- Un domaine AD DS (Active Directory Domain Services) avec des ordinateurs clients joints au domaine. Aucune condition n’est requise au niveau fonctionnel de la forêt ou du domaine, ni en ce qui concerne le schéma.
- Des ordinateurs clients Windows 10, Windows 8.1, Windows 8, Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012. (Les ordinateurs clients exécutant des versions antérieures de Windows peuvent continuer à passer en mode En ligne sur des connexions réseau à très haut débit.)
- Un ordinateur sur lequel la Gestion des stratégies de groupe est installée.

## <a name="enable-always-offline-mode"></a>Activer le mode Toujours hors connexion

Pour activer le mode Toujours hors connexion, utilisez la stratégie de groupe afin d’activer le paramètre de stratégie **Configurer le mode de liaison lente** et définir une latence de **1** (milliseconde). Les ordinateurs clients Windows Server 8 ou Windows Server 2012 utilisent alors automatiquement le mode Toujours hors connexion.

>[!NOTE]
>Les ordinateurs Windows 7, Windows Vista, Windows Server 2008 R2 ou Windows Server 2008 peuvent continuer à passer en mode En ligne si la latence de la connexion réseau est inférieure à une milliseconde.

1. Ouvrez **Gestion des stratégies de groupe**.
2. Vous pouvez éventuellement créer un objet de stratégie de groupe (GPO) pour les paramètres Fichiers hors connexion. Pour cela, cliquez avec le bouton droit sur l’unité d’organisation ou le domaine approprié, puis sélectionnez **Créer un objet GPO dans ce domaine, et le lier ici**.
3. Dans l’arborescence de la console, cliquez sur l’objet de stratégie de groupe pour lequel vous voulez configurer les paramètres Fichiers hors connexion, puis sélectionnez **Modifier**. L’**Éditeur de gestion des stratégies de groupe** s’ouvre.
4. Dans l’arborescence de la console, sous **Configuration ordinateur**, développez **Stratégies**, **Modèles d’administration**, **Réseau** et **Fichiers hors connexion**.
5. Cliquez avec le bouton droit sur **Configurer le mode de liaison lente**, puis sélectionnez **Modifier**. La fenêtre **Configurer le mode de liaison lente** s’affiche.
6. Sélectionnez **Activé**.
7. Dans la zone **Options**, sélectionnez **Afficher**. La fenêtre **Afficher le contenu** s’affiche.
8. Dans la zone **Nom de la valeur**, spécifiez le partage de fichiers pour lequel vous souhaitez activer le mode Toujours hors connexion.
9. Pour activer le mode Toujours hors connexion sur tous les partages de fichiers, entrez **\*** .
10. Dans la zone **Valeur**, entrez **Latency=1** pour définir un seuil de latence d’une milliseconde, puis sélectionnez **OK**.

>[!NOTE]
>Par défaut, quand Windows est en mode Toujours hors connexion, les fichiers du cache Fichiers hors connexion sont synchronisés en arrière-plan toutes les deux heures. Pour changer cette valeur, utilisez le paramètre de stratégie **Configurer la synchronisation en arrière-plan**.

## <a name="more-information"></a>Autres informations

* [Vue d’ensemble de la redirection de dossiers, des fichiers hors connexion et des profils utilisateur itinérants](folder-redirection-rup-overview.md)
* [Déployer la redirection de dossiers avec Fichiers hors connexion](deploy-folder-redirection.md)