---
title: Configurer l’inscription automatique des certificats serveur
description: Cette rubrique fait partie du guide des certificats de serveur de déploiement pour les déploiements de sans fil et câblé à 802.1 X
manager: brianlic
ms.topic: article
ms.assetid: c81e85cb-ecb8-442a-ad27-442c2f9e40df
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ea7b7efe01525f4ecfb35200463c3f221d92d62d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888560"
---
# <a name="configure-certificate-auto-enrollment"></a>Configurer l’inscription automatique des certificats

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

> [!NOTE]
> Avant d’effectuer cette procédure, vous devez configurer un modèle de certificat de serveur à l’aide du composant logiciel enfichable Console de gestion de Microsoft de modèles de certificats sur une autorité de certification AD CS est en cours d’exécution.
Pour être autorisé à effectuer cette procédure, vous devez faire partie du groupe **Administrateurs de l'entreprise** et du groupe **Admins du domaine** du domaine racine.

## <a name="configure-server-certificate-auto-enrollment"></a>Configurer l’inscription automatique des certificats serveur

1. Sur l’ordinateur où AD DS est installé, ouvrez Windows PowerShell&reg;, type **mmc**, puis appuyez sur ENTRÉE. La console Microsoft Management Console s’ouvre.
2. Dans le menu **Fichier** , cliquez sur **Ajouter/Supprimer un composant logiciel enfichable**. La boîte de dialogue **Ajouter ou supprimer des composants logiciels enfichables** s'ouvre.
3. Dans **des composants logiciels enfichables disponibles**, faites défiler jusqu'à et double-cliquez sur **éditeur de gestion de stratégie de groupe**. Le **sélectionner un objet de stratégie de groupe** boîte de dialogue s’ouvre.

     > [!IMPORTANT]
     > Vérifiez que vous sélectionnez **éditeur de gestion de stratégie de groupe** et non **Group Policy Management**. Si vous sélectionnez **Group Policy Management**, votre configuration à l’aide de ces instructions échouera et un certificat de serveur ne sera pas inscrit automatiquement pour votre NPSs.

4. Dans **Objet de stratégie de groupe**, cliquez sur **Parcourir**. La boîte de dialogue **Rechercher un objet Stratégie de groupe** s'ouvre.
5. Dans **Domaines, unités d'organisation et objets de stratégie de groupe liés**, cliquez sur **Stratégie de domaine par défaut**, puis cliquez sur **OK**.
6. Cliquez sur **Terminer**, puis sur **OK**.
7. Double-cliquez sur **Stratégie de domaine par défaut**. Dans la console, développez le chemin suivant : **Configuration de l'ordinateur**, **Stratégies**, **Paramètres Windows**, **Paramètres de sécurité**, puis **Stratégies de clé publique**.
8. Cliquez sur **Stratégies de clé publique**. Dans le volet d’informations, double-cliquez sur **Client des services de certificats - Inscription automatique**. Le **propriétés** boîte de dialogue s’ouvre. Configurez les éléments suivants, puis cliquez sur **OK** :

     1. Dans **Modèle de configuration**, sélectionnez **Activé**.
     2. Activez la case à cocher **Renouveler les certificats expirés, mettre à jour les certificats en attente et supprimer les certificats révoqués**.
     3. Activez la case à cocher **Mettre à jour les certificats qui utilisent les modèles de certificats**.

9. Cliquez sur **OK**.

## <a name="configure-user-certificate-auto-enrollment"></a>Configurer l’inscription automatique des certificats utilisateur

1. Sur l’ordinateur où AD DS est installé, ouvrez Windows PowerShell&reg;, type **mmc**, puis appuyez sur ENTRÉE. La console Microsoft Management Console s’ouvre.
2. Dans le menu **Fichier** , cliquez sur **Ajouter/Supprimer un composant logiciel enfichable**. La boîte de dialogue **Ajouter ou supprimer des composants logiciels enfichables** s'ouvre.
3. Dans **des composants logiciels enfichables disponibles**, faites défiler jusqu'à et double-cliquez sur **éditeur de gestion de stratégie de groupe**. Le **sélectionner un objet de stratégie de groupe** boîte de dialogue s’ouvre.

     > [!IMPORTANT]
     > Vérifiez que vous sélectionnez **éditeur de gestion de stratégie de groupe** et non **Group Policy Management**. Si vous sélectionnez **Group Policy Management**, votre configuration à l’aide de ces instructions échouera et un certificat de serveur ne sera pas inscrit automatiquement pour votre NPSs.

4. Dans **Objet de stratégie de groupe**, cliquez sur **Parcourir**. La boîte de dialogue **Rechercher un objet Stratégie de groupe** s'ouvre.
5. Dans **Domaines, unités d'organisation et objets de stratégie de groupe liés**, cliquez sur **Stratégie de domaine par défaut**, puis cliquez sur **OK**.
6. Cliquez sur **Terminer**, puis sur **OK**.
7. Double-cliquez sur **Stratégie de domaine par défaut**. Dans la console, développez le chemin suivant : **Configuration de l’utilisateur**, **stratégies**, **Windows paramètres**, **paramètres de sécurité**.
8. Cliquez sur **Stratégies de clé publique**. Dans le volet d’informations, double-cliquez sur **Client des services de certificats - Inscription automatique**. Le **propriétés** boîte de dialogue s’ouvre. Configurez les éléments suivants, puis cliquez sur **OK** :

     1. Dans **Modèle de configuration**, sélectionnez **Activé**.
     2. Activez la case à cocher **Renouveler les certificats expirés, mettre à jour les certificats en attente et supprimer les certificats révoqués**.
     3. Activez la case à cocher **Mettre à jour les certificats qui utilisent les modèles de certificats**.

9. Cliquez sur **OK**.

## <a name="next-steps"></a>Étapes suivantes

[Stratégie de groupe de l’actualisation](refresh-group-policy.md)
