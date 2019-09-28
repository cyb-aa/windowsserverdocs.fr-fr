---
title: Sauvegardez vos serveurs Windows à partir du centre d’administration Windows avec sauvegarde Azure
description: Utiliser le centre d’administration Windows (projet Honolulu) pour sauvegarder des serveurs Windows avec sauvegarde Azure
ms.technology: manage
ms.topic: article
author: saurabhsensharma
ms.author: saurse
ms.date: 03/25/2019
ms.localizationpriority: low
ms.prod: windows-server
ms.openlocfilehash: 77171e638fa1eb8c612bdc168868d8286ed23bb6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357403"
---
# <a name="backup-your-windows-servers-from-windows-admin-center-with-azure-backup"></a>Sauvegardez vos serveurs Windows à partir du centre d’administration Windows avec sauvegarde Azure

>S'applique à : Centre d’administration Windows-version préliminaire, Centre d’administration Windows

[En savoir plus sur l’intégration d’Azure avec le centre d’administration Windows.](../plan/azure-integration-options.md)

Le centre d’administration Windows simplifie le processus de sauvegarde de vos serveurs Windows dans Azure et vous protège contre les suppressions accidentelles ou malveillantes, l’endommagement et même les ransomware. Pour automatiser l’installation, vous pouvez connecter la passerelle Windows Admin Center à Azure.

Utilisez les informations suivantes pour configurer la sauvegarde de Windows Server et créer une stratégie de sauvegarde pour sauvegarder les volumes de votre serveur et l’état du système Windows à partir du centre d’administration Windows.

## <a name="what-is-azure-backup-and-how-does-it-work-with-windows-admin-center"></a>Qu’est-ce que la sauvegarde Azure et comment fonctionne-t-elle avec le centre d’administration Windows ? 

**Sauvegarde Azure** est le service Azure que vous pouvez utiliser pour sauvegarder (ou protéger) et restaurer vos données dans le Cloud Microsoft. Azure Backup remplace votre solution de sauvegarde locale ou hors site existante par une solution basée sur le Cloud fiable, sécurisée et économique.
[En savoir plus sur sauvegarde Azure](https://docs.microsoft.com/azure/backup/backup-overview).

Azure Backup propose plusieurs composants que vous téléchargez et déployez sur l’ordinateur, le serveur ou le Cloud approprié. Le composant ou l’agent que vous déployez dépend de ce que vous souhaitez protéger. Tous les composants de sauvegarde Azure (peu importe si vous protégez des données localement ou dans Azure) peuvent être utilisés pour sauvegarder des données dans un coffre Recovery Services dans Azure.

L’intégration d’Azure Backup dans le centre d’administration Windows est idéale pour sauvegarder des volumes et l’état du système Windows des serveurs physiques ou virtuels Windows locaux. Cela permet d’obtenir un mécanisme complet pour sauvegarder les serveurs de fichiers, les contrôleurs de domaine et les serveurs Web IIS.

Le centre d’administration Windows expose l’intégration de la sauvegarde Azure via l’outil de **sauvegarde** natif. L’outil de **sauvegarde** fournit des expériences d’installation, de gestion et de surveillance pour démarrer rapidement la sauvegarde de vos serveurs, effectuer des opérations de sauvegarde et de restauration courantes et surveiller l’intégrité globale de la sauvegarde de vos serveurs Windows.

## <a name="prerequisites-and-planning"></a>Conditions préalables et planification

- Un compte Azure avec au moins un abonnement actif
- Les serveurs Windows cibles que vous souhaitez sauvegarder doivent disposer d’un accès Internet à Azure
- [Connecter votre passerelle du centre d’administration Windows à Azure](azure-integration.md)

Pour démarrer le flux de travail afin de sauvegarder votre serveur Windows, ouvrez une connexion au serveur, cliquez sur l’outil de **sauvegarde** et suivez les étapes indiquées ci-dessous.

## <a name="setup-azure-backup"></a>Configurer la sauvegarde Azure
Lorsque vous cliquez sur l’outil de **sauvegarde** pour une connexion au serveur sur laquelle la sauvegarde Azure n’est pas encore activée, l’écran **Bienvenue dans la sauvegarde Azure** s’affiche. Cliquez sur le bouton **configurer la sauvegarde Azure** . L’Assistant Installation de la sauvegarde Azure s’ouvre. Suivez les étapes décrites ci-dessous dans l’Assistant pour sauvegarder votre serveur.

Si la sauvegarde Azure est déjà configurée, le fait de cliquer sur l’outil de **sauvegarde** ouvre le **tableau de bord de sauvegarde**. Reportez-vous à la section ([gestion et surveillance](#management-and-monitoring)) pour découvrir les opérations et les tâches qui peuvent être effectuées à partir du tableau de bord.

### <a name="step-1-login-to-microsoft-azure"></a>Étape 1 : Connectez-vous à Microsoft Azure
Connectez-vous à votre compte Azure. 

> [!NOTE]
> Si vous avez connecté votre passerelle du centre d’administration Windows à Azure, vous devez être connecté automatiquement à Azure. Vous pouvez cliquer sur **se déconnecter** pour vous connecter avec un autre nom d’utilisateur.

### <a name="step-2-set-up-azure-backup"></a>Étape 2 : Configurer la sauvegarde Azure
Sélectionnez les paramètres appropriés pour la sauvegarde Azure, comme décrit ci-dessous

 - **ID d’abonnement :** L’abonnement Azure que vous souhaitez utiliser pour sauvegarder votre serveur Windows Server dans Azure. Toutes les ressources Azure comme le groupe de ressources Azure, le coffre de Recovery Services est créé dans l’abonnement sélectionné.
 - **Coffre** Le coffre Recovery Services dans lequel les sauvegardes de vos serveurs seront stockées. Vous pouvez effectuer votre choix parmi les coffres existants ou le centre d’administration Windows crée un coffre.  
 - **Groupe de ressources :** Le groupe de ressources Azure est un conteneur pour une collection de ressources. Le coffre de Recovery Services est créé ou contenu dans le groupe de ressources spécifié. Vous pouvez effectuer votre choix parmi les groupes de ressources existants ou le centre d’administration Windows en créera un nouveau.
 - **Emplacement :** Région Azure dans laquelle le coffre de Recovery Services sera créé. Il est recommandé de sélectionner la région Azure la plus proche du serveur Windows.

### <a name="step-3-select-backup-items-and-schedule"></a>Étape 3 : Sélectionner les éléments et la planification de sauvegarde

- Sélectionnez ce que vous souhaitez sauvegarder à partir de votre serveur. Le centre d’administration Windows vous permet de choisir parmi une combinaison de **volumes** et l' **État du système Windows** tout en vous donnant la taille estimée des données sélectionnées pour la sauvegarde.

> [!NOTE]
> La première sauvegarde est une sauvegarde complète de toutes les données sélectionnées. Toutefois, les sauvegardes suivantes sont incrémentielles par nature et ne transfèrent que les modifications apportées aux données depuis la sauvegarde précédente.

- Sélectionnez l’une des **planifications de sauvegarde** prédéfinies pour l’État et/ou les volumes du système.

### <a name="step-4-enter-encryption-passphrase"></a>Étape 4 : Entrer la phrase secrète de chiffrement

- Entrez une **phrase secrète de chiffrement** de votre choix (16 caractères minimum).  **Sauvegarde Azure** sécurise vos données de sauvegarde avec une phrase secrète de chiffrement configurée par l’utilisateur et gérée par l’utilisateur. La phrase secrète de chiffrement est requise pour récupérer des données à partir de sauvegarde Azure.

> [!NOTE]
> La phrase secrète doit être stockée dans un emplacement hors site sécurisé, tel qu’un autre serveur ou le [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/quick-create-portal). Microsoft ne stocke pas la phrase secrète et ne peut pas récupérer ou réinitialiser la phrase secrète en cas de perte ou d’oubli.

- Passez en revue tous les paramètres, puis cliquez sur **appliquer** .

Le centre d’administration Windows effectue ensuite les opérations suivantes :

1. Créer un groupe de ressources Azure s’il n’existe pas déjà
2. Créer un coffre de Recovery Services Azure comme spécifié
3. Installer et inscrire l’agent de Microsoft Azure Recovery Services dans le coffre
4. Créez la planification de la sauvegarde et de la rétention en fonction des options sélectionnées et associez-les au serveur Windows.

## <a name="management-and-monitoring"></a>Gestion et surveillance

Une fois que vous avez correctement configuré la sauvegarde Azure, le **tableau de bord de sauvegarde** s’affiche lorsque vous ouvrez l’outil de sauvegarde pour une connexion au serveur existante. Vous pouvez effectuer les tâches suivantes à partir du **tableau de bord de sauvegarde**

- **Accédez au coffre dans Azure :** Vous pouvez cliquer sur le lien **Recovery Services Vault** sous l’onglet **vue d’ensemble** du tableau de bord de **sauvegarde** à utiliser dans le coffre dans Azure pour effectuer un [ensemble complet d’opérations de gestion](https://docs.microsoft.com/azure/backup/backup-azure-manage-windows-server) .
- **Effectuez une sauvegarde ad hoc :** Cliquez sur **sauvegarder maintenant** pour effectuer une sauvegarde ad hoc. 
- **Surveiller les travaux et configurer les notifications d’alerte :** Accédez à l’onglet **tâches** du tableau de bord pour surveiller les travaux en cours ou passés et [configurez les notifications d’alerte](https://docs.microsoft.com/azure/backup/backup-azure-manage-windows-server#configuring-notifications-for-alerts) pour recevoir des courriers électroniques pour les tâches ayant échoué ou les autres alertes liées à la sauvegarde.
- **Afficher les points de récupération et récupérer les données :** Cliquez sur l’onglet **points de récupération** du tableau de bord pour afficher les points de récupération, puis cliquez sur récupérer les **données** pour connaître les étapes de récupération de vos données à partir d’Azure.