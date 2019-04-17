---
title: Configurer l’inscription automatique des certificats serveur
description: Cette rubrique fait partie du guide de certificats de serveur de déploiement pour 802.1 X câblé et sans fil déploiements
manager: brianlic
ms.topic: article
ms.assetid: c81e85cb-ecb8-442a-ad27-442c2f9e40df
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ddf8a905fdb68bbc474b10f526b32f3d8b83af46
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="configure-certificate-auto-enrollment"></a>Configurer l’inscription automatique de certificat

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

> [!NOTE]
> Avant d’effectuer cette procédure, vous devez configurer un modèle de certificat de serveur à l’aide du composant logiciel enfichable modèles de certificat MicrosoftManagement Console sur une autorité de certification qui exécute les services ADCS.
L’appartenance dans les deux **administrateurs de l’entreprise** et du domaine racine **Admins du domaine** groupe est la condition minimale requise pour effectuer cette procédure.

## <a name="configure-server-certificate-auto-enrollment"></a>Configurer l’inscription automatique des certificats serveur

1. Sur l’ordinateur sur lequel les services ADDS est installé, ouvrez Windows PowerShell&reg;, type **mmc**, puis appuyez sur ENTRÉE. MicrosoftManagement Console s’ouvre.
2. Sur le **fichier** menu, cliquez sur **ajouter/supprimer un composant logiciel enfichable**. Le **ajouter ou supprimer des composants logiciel enfichables** boîte de dialogue s’ouvre.
3. Dans **des composants logiciels enfichables disponibles**, faites défiler vers le bas et double-cliquez sur **éditeur de gestion de stratégie de groupe**. Le **sélectionner un objet de stratégie de groupe** boîte de dialogue s’ouvre.

     > [!IMPORTANT]
     > Veillez à sélectionner **éditeur de gestion de stratégie de groupe** et non **gestion des stratégies de groupe**. Si vous sélectionnez **gestion des stratégies de groupe**, votre configuration à l’aide de ces instructions échoue et un certificat de serveur ne sera pas inscrit automatiquement à vos serveurs NPS.

4. Dans **objet de stratégie de groupe**, cliquez sur **Parcourir**. Le **rechercher un objet de stratégie de groupe** boîte de dialogue s’ouvre.
5. Dans **domaines, unités d’organisation et objets de stratégie de groupe liés,** cliquez sur **stratégie de domaine par défaut**, puis cliquez sur **OK**.
6. Cliquez sur **Terminer**, puis cliquez sur **OK**.
7. Double-cliquez sur **stratégie de domaine par défaut**. Dans la console, développez le chemin suivant: **Configuration ordinateur**, **stratégies**, **paramètres Windows**, **paramètres de sécurité**, puis **stratégies de clé publique**.
8. Cliquez sur **stratégies de clé publique**. Dans le volet d’informations, double-cliquez sur **Client des Services de certificats - inscription automatique**. Le **propriétés** boîte de dialogue s’ouvre. Configurer les éléments suivants, puis cliquez sur **OK**:

     1. Dans **modèle de Configuration**, sélectionnez **activé**.
     2. Sélectionnez le **renouveler les certificats expirés, mettre à jour des certificats en attente et supprimer les certificats révoqués** case à cocher.
     3. Sélectionnez le **mettre à jour les certificats qui utilisent les modèles de certificats** case à cocher.

9. Cliquez sur **OK**.

## <a name="configure-user-certificate-auto-enrollment"></a>Configurer l’inscription automatique des certificats utilisateur

1. Sur l’ordinateur sur lequel les services ADDS est installé, ouvrez Windows PowerShell&reg;, type **mmc**, puis appuyez sur ENTRÉE. MicrosoftManagement Console s’ouvre.
2. Sur le **fichier** menu, cliquez sur **ajouter/supprimer un composant logiciel enfichable**. Le **ajouter ou supprimer des composants logiciel enfichables** boîte de dialogue s’ouvre.
3. Dans **des composants logiciels enfichables disponibles**, faites défiler vers le bas et double-cliquez sur **éditeur de gestion de stratégie de groupe**. Le **sélectionner un objet de stratégie de groupe** boîte de dialogue s’ouvre.

     > [!IMPORTANT]
     > Veillez à sélectionner **éditeur de gestion de stratégie de groupe** et non **gestion des stratégies de groupe**. Si vous sélectionnez **gestion des stratégies de groupe**, votre configuration à l’aide de ces instructions échoue et un certificat de serveur ne sera pas inscrit automatiquement à vos serveurs NPS.

4. Dans **objet de stratégie de groupe**, cliquez sur **Parcourir**. Le **rechercher un objet de stratégie de groupe** boîte de dialogue s’ouvre.
5. Dans **domaines, unités d’organisation et objets de stratégie de groupe liés,** cliquez sur **stratégie de domaine par défaut**, puis cliquez sur **OK**.
6. Cliquez sur **Terminer**, puis cliquez sur **OK**.
7. Double-cliquez sur **stratégie de domaine par défaut**. Dans la console, développez le chemin suivant: **Configuration utilisateur**, **stratégies**, **paramètres Windows**, **paramètres de sécurité**, puis **stratégies de clé publique**.
8. Cliquez sur **stratégies de clé publique**. Dans le volet d’informations, double-cliquez sur **Client des Services de certificats - inscription automatique**. Le **propriétés** boîte de dialogue s’ouvre. Configurer les éléments suivants, puis cliquez sur **OK**:

     1. Dans **modèle de Configuration**, sélectionnez **activé**.
     2. Sélectionnez le **renouveler les certificats expirés, mettre à jour des certificats en attente et supprimer les certificats révoqués** case à cocher.
     3. Sélectionnez le **mettre à jour les certificats qui utilisent les modèles de certificats** case à cocher.

9. Cliquez sur **OK**.

## <a name="next-steps"></a>Étapes suivantes

[Stratégie de groupe d’actualisation](refresh-group-policy.md)