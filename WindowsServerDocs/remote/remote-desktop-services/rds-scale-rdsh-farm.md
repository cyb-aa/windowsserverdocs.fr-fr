---
title: Effectuer un scale-out de votre déploiement de services Bureau à distance en ajoutant une batterie de serveurs Hôte de session Bureau à distance
description: Ajoutez un second hôte de session Bureau à distance à votre environnement de services Bureau à distance.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 04/10/2017
ms.topic: article
author: lizap
manager: dongill
ms.openlocfilehash: 61d5569c44d7c7ea300b85bf635fccf86275423d
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2020
ms.locfileid: "80859052"
---
# <a name="scale-out-your-remote-desktop-services-deployment-by-adding-an-rd-session-host-farm"></a>Effectuer un scale-out de votre déploiement de services Bureau à distance en ajoutant une batterie de serveurs Hôte de session Bureau à distance

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2019, Windows Server 2016

Vous pouvez améliorer la disponibilité et la mise à l’échelle de votre déploiement de services Bureau à distance en ajoutant une batterie de serveurs Hôte de session Bureau à distance (RDSH).   
  
 
Utilisez les étapes suivantes pour ajouter un nouvel hôte de session Bureau à distance à votre déploiement :  
  
1. Créez un serveur pour héberger le second hôte de session Bureau à distance. Si vous utilisez des machines virtuelles Azure, veillez à inclure la nouvelle machine virtuelle dans le même groupe à haute disponibilité que celui qui contient votre premier hôte de session Bureau à distance.
2. Activez la gestion à distance sur le nouveau serveur ou la nouvelle machine virtuelle :
   1. Dans le Gestionnaire de serveur, cliquez sur **Serveur local > paramètre actuel de l’Administration à distance (désactivé)** . 
   2. Sélectionnez **Activer l’administration à distance pour ce serveur**, puis cliquez sur **OK**. 
   3. Facultatif : Vous pouvez temporairement définir Windows Update pour que les mises à jour ne soient ni téléchargées ni installées automatiquement. Cela permet d’éviter les modifications et les redémarrages du système pendant que vous déployez le serveur d’hôte de session Bureau à distance. Dans le Gestionnaire de serveur, cliquez sur **Serveur local > paramètre actuel de Windows Update**. Cliquez sur **Options avancées > Différer les mises à niveau**. 
3. Ajoutez le serveur ou la machine virtuelle au domaine :
   1. Dans le Gestionnaire de serveur, cliquez sur **Serveur local > paramètre actuel du Groupe de travail**. 
   2. Cliquez sur **Modifier > Domaine**, puis entrez le nom de domaine (par exemple, Contoso.com). 
   3. Entrez les informations d’identification de l’administrateur de domaine. 
   4. Redémarrez le serveur ou la machine virtuelle.
4. Ajoutez le nouvel hôte de session Bureau à distance à la batterie de serveurs :
   >[!NOTE] 
   > L’étape 1 de création d’une adresse IP publique pour la machine virtuelle RDMS n’est nécessaire que si vous utilisez une machine virtuelle pour les services de gestion Bureau à distance (RDMS), et si aucune adresse IP ne lui a été affectée auparavant.
   
   1. Créez une adresse IP publique pour la machine virtuelle exécutant les services de gestion Bureau à distance. La machine virtuelle RDMS est généralement la machine virtuelle qui exécute la première instance du rôle Service Broker pour les connexions Bureau à distance.  
       1. Dans le portail Azure, cliquez sur **Parcourir > Groupes de ressources**, puis cliquez sur le groupe de ressources pour le déploiement, et sur la machine virtuelle RDMS (par exemple, Contoso-Cb1).  
       2. Cliquez sur **Paramètres > Interfaces réseau**, puis cliquez sur l’interface réseau correspondante.   
       3. Cliquez sur **Paramètres > Adresse IP**.
       4. Pour **Adresse IP publique**, sélectionnez **Activé**, puis cliquez sur **Adresse IP**.   
       5. Si vous souhaitez utiliser une adresse IP publique existante, sélectionnez-la dans la liste. Sinon, cliquez sur **Créer**, entrez un nom, puis cliquez sur **OK** et ensuite sur **Enregistrer**.   
   2. Connectez-vous aux services de gestion Bureau à distance (RDMS).
   3. Ajoutez le nouveau serveur Hôte de session Bureau à distance au Gestionnaire de serveur :   
       1. Lancez le Gestionnaire de serveur, cliquez sur **Gérer > Ajouter des serveurs**.   
       2. Dans la boîte de dialogue Ajouter des serveurs, cliquez sur **Rechercher maintenant**.   
       3. Sélectionnez le serveur que vous souhaitez utiliser pour l’hôte de session Bureau à distance, ou la machine virtuelle nouvellement créée (par exemple, Contoso-Sh2), puis cliquez sur **OK**.
   4. Ajouter le serveur Hôte de session Bureau à distance au déploiement
       1. Lancez le Gestionnaire de serveur.  
       2. Cliquez sur **Services Bureau à distance > Vue d’ensemble > Serveurs de déploiement > Tâches > Ajouter des serveurs Hôte de session Bureau à distance**.   
       3. Sélectionnez le nouveau serveur (par exemple, Contoso-Sh2), puis cliquez sur **Suivant**.  
       4. Dans la page de confirmation, sélectionnez **Redémarrer les ordinateurs distants si nécessaire**, puis cliquez sur **Ajouter**.   
   5. Ajoutez le serveur Hôte de session Bureau à distance à la batterie de serveurs de collection :
       1. Lancez le Gestionnaire de serveur.   
       2. Cliquez sur **Services Bureau à distance**, puis sur la collection à laquelle vous souhaitez ajouter le serveur Hôte de session Bureau à distance nouvellement créé (par exemple, ContosoDesktop).   
       3. Sous **Serveurs hôtes**, cliquez sur **Tâches > Ajouter des serveurs Hôte de session Bureau à distance**.   
       4. Sélectionnez le serveur nouvellement créé (par exemple, Contoso-Sh2), puis cliquez sur **Suivant**.   
       5. Dans la page de confirmation, cliquez sur **Ajouter**.   

