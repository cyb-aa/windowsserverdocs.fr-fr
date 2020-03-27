---
title: Configurer l’inscription automatique de certificat de serveur
description: Cette rubrique fait partie du guide déployer des certificats de serveur pour les déploiements sans fil et câblés 802.1 X.
manager: brianlic
ms.topic: article
ms.assetid: c81e85cb-ecb8-442a-ad27-442c2f9e40df
ms.prod: windows-server
ms.technology: networking
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 9c85c4fd9026155fe1ca880ecb05f4c9358a2309
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318427"
---
# <a name="configure-certificate-auto-enrollment"></a>Configurer l’inscription automatique des certificats

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

> [!NOTE]
> Avant d’effectuer cette procédure, vous devez configurer un modèle de certificat de serveur à l’aide du composant logiciel enfichable modèles de certificats de la console MMC sur une autorité de certification qui exécute les services AD CS.
Pour être autorisé à effectuer cette procédure, vous devez faire partie du groupe **Administrateurs de l'entreprise** et du groupe **Admins du domaine** du domaine racine.

## <a name="configure-server-certificate-auto-enrollment"></a>Configurer l’inscription automatique de certificat de serveur

1. Sur l’ordinateur où est installé AD DS, ouvrez Windows PowerShell&reg;, tapez **MMC**, puis appuyez sur entrée. La console Microsoft Management Console s’ouvre.
2. Dans le menu **Fichier**, cliquez sur **Ajouter/Supprimer un composant logiciel enfichable**. La boîte de dialogue **Ajouter ou supprimer des composants logiciels enfichables** s'ouvre.
3. Dans **composants logiciels enfichables disponibles**, faites défiler la liste jusqu’à, puis double-cliquez sur **éditeur de gestion des stratégies de groupe**. La boîte de dialogue **Sélectionner un objet stratégie de groupe** s’ouvre.

     > [!IMPORTANT]
     > Veillez à sélectionner **éditeur de gestion des stratégies de groupe** et non **stratégie de groupe gestion**. Si vous sélectionnez **stratégie de groupe Management**, votre configuration qui utilise ces instructions échoue et un certificat de serveur n’est pas inscrit automatiquement sur votre NPSs.

4. Dans **Objet de stratégie de groupe**, cliquez sur **Parcourir**. La boîte de dialogue **Rechercher un objet Stratégie de groupe** s'ouvre.
5. Dans **Domaines, unités d'organisation et objets de stratégie de groupe liés**, cliquez sur **Stratégie de domaine par défaut**, puis cliquez sur **OK**.
6. Cliquez sur **Terminer**, puis sur **OK**.
7. Double-cliquez sur **Stratégie de domaine par défaut**. Dans la console, développez le chemin suivant : **Configuration ordinateur**, **stratégies**, **Paramètres Windows**, **paramètres de sécurité**, puis stratégies de **clé publique**.
8. Cliquez sur **Stratégies de clé publique**. Dans le volet d’informations, double-cliquez sur **Client des services de certificats - Inscription automatique**. La boîte de dialogue **Propriétés** s’ouvre. Configurez les éléments suivants, puis cliquez sur **OK** :

     1. Dans **Modèle de configuration**, sélectionnez **Activé**.
     2. Activez la case à cocher **Renouveler les certificats expirés, mettre à jour les certificats en attente et supprimer les certificats révoqués**.
     3. Activez la case à cocher **Mettre à jour les certificats qui utilisent les modèles de certificats**.

9. Cliquez sur **OK**.

## <a name="configure-user-certificate-auto-enrollment"></a>Configurer l’inscription automatique des certificats utilisateur

1. Sur l’ordinateur où est installé AD DS, ouvrez Windows PowerShell&reg;, tapez **MMC**, puis appuyez sur entrée. La console Microsoft Management Console s’ouvre.
2. Dans le menu **Fichier**, cliquez sur **Ajouter/Supprimer un composant logiciel enfichable**. La boîte de dialogue **Ajouter ou supprimer des composants logiciels enfichables** s'ouvre.
3. Dans **composants logiciels enfichables disponibles**, faites défiler la liste jusqu’à, puis double-cliquez sur **éditeur de gestion des stratégies de groupe**. La boîte de dialogue **Sélectionner un objet stratégie de groupe** s’ouvre.

     > [!IMPORTANT]
     > Veillez à sélectionner **éditeur de gestion des stratégies de groupe** et non **stratégie de groupe gestion**. Si vous sélectionnez **stratégie de groupe Management**, votre configuration qui utilise ces instructions échoue et un certificat de serveur n’est pas inscrit automatiquement sur votre NPSs.

4. Dans **Objet de stratégie de groupe**, cliquez sur **Parcourir**. La boîte de dialogue **Rechercher un objet Stratégie de groupe** s'ouvre.
5. Dans **Domaines, unités d'organisation et objets de stratégie de groupe liés**, cliquez sur **Stratégie de domaine par défaut**, puis cliquez sur **OK**.
6. Cliquez sur **Terminer**, puis sur **OK**.
7. Double-cliquez sur **Stratégie de domaine par défaut**. Dans la console, développez le chemin suivant : **Configuration utilisateur**, **stratégies**, **Paramètres Windows**, **paramètres de sécurité**.
8. Cliquez sur **Stratégies de clé publique**. Dans le volet d’informations, double-cliquez sur **Client des services de certificats - Inscription automatique**. La boîte de dialogue **Propriétés** s’ouvre. Configurez les éléments suivants, puis cliquez sur **OK** :

     1. Dans **Modèle de configuration**, sélectionnez **Activé**.
     2. Activez la case à cocher **Renouveler les certificats expirés, mettre à jour les certificats en attente et supprimer les certificats révoqués**.
     3. Activez la case à cocher **Mettre à jour les certificats qui utilisent les modèles de certificats**.

9. Cliquez sur **OK**.

## <a name="next-steps"></a>Étapes suivantes

[Actualiser stratégie de groupe](refresh-group-policy.md)
