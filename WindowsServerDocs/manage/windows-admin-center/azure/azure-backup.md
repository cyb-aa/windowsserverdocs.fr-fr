---
title: Sauvegarder vos serveurs Windows à partir de Windows Admin Center avec sauvegarde Azure
description: Utilisez Windows Admin Center (projet Honolulu) aux serveurs de sauvegarde Windows avec sauvegarde Azure
ms.technology: manage
ms.topic: article
author: saurabhsensharma
ms.author: saurse
ms.date: 03/25/2019
ms.localizationpriority: low
ms.prod: windows-server-threshold
ms.openlocfilehash: 3983d0b65bc69ef9fd40f3c8e196d40534b1b8f9
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296908"
---
# Sauvegarder vos serveurs Windows à partir de Windows Admin Center avec sauvegarde Azure

>S’applique à: Windows Admin Center Preview, Windows Admin Center

[En savoir plus sur l’intégration d’Azure avec Windows Admin Center.](../plan/azure-integration-options.md)

Windows Admin Center simplifie le processus de sauvegarder vos serveurs Windows vers Azure et vous protéger contre des suppressions accidentelles ou malveillantes, la corruption et même ransomware. Pour automatiser l’installation, vous pouvez connecter la passerelle Windows Admin Center à Azure.

Utilisez les informations suivantes pour configurer la sauvegarde pour vous Windows Server et créer une stratégie de sauvegarde pour sauvegarder les Volumes de votre serveur et l’état du système Windows à partir du centre d’administration de Windows.

## Quelle est la sauvegarde Azure et comment il fonctionne avec Windows Admin Center? 

**Sauvegarde Azure** est le service basé sur Azure, vous pouvez utiliser pour sauvegarder (ou protéger) et restaurer vos données dans le cloud de Microsoft. Sauvegarde Azure remplace votre local existant ou une solution de sauvegarde hors site par une solution basée sur le cloud qui est compétitif, sécurisées et fiables.
[En savoir plus sur Azure sauvegarde](https://docs.microsoft.com/azure/backup/backup-overview).

Sauvegarde Azure propose plusieurs composants que vous télécharger et déployez sur l’ordinateur approprié, serveur, ou dans le cloud. Le composant, ou un agent, que vous déployez varie en fonction de ce que vous souhaitez protéger. Tous les composants de sauvegarde Azure (quel que soit l’indique si vous êtes protection des données en local ou dans Azure) peuvent être utilisés pour sauvegarder des données vers un coffre Recovery Services dans Azure.

L’intégration de la sauvegarde Azure dans le centre d’administration de Windows est idéale pour la sauvegarde des volumes et les système Windows état Windows physiques ou virtuels serveurs locaux. Cela permet à un mécanisme complet sauvegarder les serveurs de fichiers, les contrôleurs de domaine et les serveurs Web IIS.

Windows Admin Center expose l’intégration de sauvegarde Azure via l’outil de **sauvegarde** native. L’outil de **sauvegarde** fournit d’installation, gestion et de contrôle expériences afin de démarrer rapidement sauvegarder vos serveurs, effectuer une sauvegarde courants et les opérations de restauration et surveiller la santé sauvegarde générale de vos serveurs Windows.

## Conditions préalables et planification

- Un compte Azure au moins un abonnement actif
- La cible de serveurs Windows Server que vous souhaitez sauvegarde doit avoir accès à Internet pour Azure
- [Connectez votre passerelle Windows Admin Center à Azure](azure-integration.md)

Pour démarrer le flux de travail pour la sauvegarde Windows Server, ouvrir une connexion au serveur, cliquez sur l’outil de **sauvegarde** et suivez les étapes indiqués ci-dessous.

## Configurer la sauvegarde Azure
Lorsque vous cliquez sur l’outil de **sauvegarde** pour une connexion au serveur sur lequel sauvegarde Azure n’est pas encore activé, vous verrez l’écran **d’accueil à la sauvegarde Azure** . Cliquez sur le bouton **configurer la sauvegarde Azure** . Cela s’ouvre l’Assistant d’installation de sauvegarde Azure. Suivez les étapes répertoriées ci-dessous dans l’Assistant pour sauvegarder votre serveur.

Si la sauvegarde Azure est déjà configuré, en cliquant sur l’outil de **sauvegarde** s’ouvre le **Tableau de bord de sauvegarde**. Reportez-vous à la section ([analyse et gestion](#management-and-monitoring)) pour détecter les opérations et les tâches pouvant être effectuées à partir du tableau de bord.

### Étape 1: Connexion à Microsoft Azure
Se connecter à votre compte Azure. 

> [!NOTE]
> Si vous avez connecté votre passerelle Windows Admin Center à Azure, vous devez être automatiquement connecté à Azure. Vous pouvez cliquer sur **déconnexion** pour davantage se connecter en tant qu’autre utilisateur.

### Étape 2: Configurer la sauvegarde de Azure
Sélectionnez les paramètres appropriés pour la sauvegarde Azure comme décrit ci-dessous

 - **Id d’abonnement:** L’abonnement Azure que vous souhaitez utiliser la sauvegarde Windows Server vers Azure. Toutes les ressources Azure comme le groupe de ressources Azure, le coffre Recovery Services sera créé dans l’abonnement sélectionné.
 - **Coffre:** Le coffre Recovery Services où seront stockées les sauvegardes de vos serveurs. Vous pouvez sélectionner à partir de coffres-forts existants ou Windows Admin Center crée un nouveau coffre.  
 - **Groupe de ressources:** Le groupe de ressources Azure est un conteneur pour une collection de ressources. Le coffre Recovery Services est créé ou contenu dans le groupe de ressources spécifié. Vous pouvez sélectionner des groupes de ressource existants ou Windows Admin Center crée un nouveau.
 - **Emplacement:** La région Azure où le coffre Recovery Services sera créé. Il est recommandé de sélectionner la région Azure la plus proche du serveur Windows.

### Étape 3: Sélectionner les éléments de sauvegarde et de planification

- Sélectionnez ce que vous voulez sauvegarder à partir de votre serveur. Windows Admin Center vous permet de sélectionner à partir d’une combinaison de **Volumes** et l' **État du système Windows** tout en vous offrant la taille estimée de données qui sont sélectionnées pour la sauvegarde.

> [!NOTE]
> La première sauvegarde est une sauvegarde complète de toutes les données sélectionnées. Toutefois, les sauvegardes suivantes sont incrémentielles dans la nature et transfèrent uniquement les modifications apportées aux données depuis la sauvegarde précédente.

- Sélectionner à partir de plusieurs prédéfini **Des planifications de sauvegarde** pour vous l’état du système et/ou des Volumes.

### Étape 4: Entrez le mot de passe de chiffrement

- Entrez un **Mot de passe de chiffrement** de votre choix (minimum 16 caractères).  **Sauvegarde Azure** protège vos données de sauvegarde avec un mot de passe de chiffrement configurés par l’utilisateur et gérées par l’utilisateur. Le mot de passe de chiffrement est nécessaire pour récupérer des données à partir de la sauvegarde Azure.

> [!NOTE]
> Le mot de passe doit être stocké dans un emplacement sécurisé hors site par exemple, un autre serveur ou le [Coffre de clés Azure](https://docs.microsoft.com/azure/key-vault/quick-create-portal). Microsoft ne stocker le mot de passe et ne peut pas récupérer ni réinitialiser le mot de passe si elle est perdu ou oublié.

- Passez en revue de tous les paramètres et cliquez sur **Appliquer**

Windows Admin Center puis effectue les opérations suivantes

1. Créer un groupe de ressources Azure s’il n’existe pas déjà
2. Créer un coffre Azure Recovery Services comme spécifié
3. Installer et d’inscrire l’Agent de Services Microsoft Azure récupération dans le coffre
4. Créer la planification de sauvegarde et de la durée de rétention conformément aux options sélectionnées et les associer à Windows Server.

## Gestion et la surveillance

Une fois que vous avez correctement configuré sauvegarde Azure, vous verrez le **Tableau de bord de sauvegarde** lorsque vous ouvrez l’outil de sauvegarde pour une connexion au serveur existant. Vous pouvez effectuer les tâches suivantes à partir de la **Sauvegarde du tableau de bord**

- **Accéder le coffre dans Azure:** Vous pouvez cliquer sur le lien **Coffre Recovery Services** dans l’onglet **vue d’ensemble** du tableau de **Bord de sauvegarde** afin d’être redirigés vers le coffre dans Azure pour effectuer un [ensemble complet des opérations de gestion](https://docs.microsoft.com/azure/backup/backup-azure-manage-windows-server)
- **Effectuer une sauvegarde ad hoc:** Cliquez sur **Sauvegarder maintenant** pour effectuer une sauvegarde ad hoc. 
- **Surveillance des tâches et configurer les notifications d’alerte:** Accédez à l’onglet **travaux** du tableau de bord pour surveiller des cours ou au-delà des tâches et [configurer les notifications d’alerte](https://docs.microsoft.com/azure/backup/backup-azure-manage-windows-server#configuring-notifications-for-alerts) pour recevoir des e-mails d’un n’a pas pu travaux ou autres alertes connexes sauvegarde.
- **Afficher les Points de récupération et restaurer les données:** Cliquez sur l’onglet de **Points de récupération** du tableau de bord pour afficher les Points de récupération, puis cliquez sur **Récupérer des données** pour obtenir la procédure pour récupérer les données à partir d’Azure.