---
title: Monter en charge votre déploiement de services Bureau à distance en ajoutant une batterie de serveurs hôte de Session Bureau à distance
description: Ajouter un second hôte de Session Bureau à distance à votre environnement de services Bureau à distance.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 04/10/2017
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dongill
ms.openlocfilehash: d243994a68c0bf4f0584f68475a185acb9cb73d5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865490"
---
# <a name="scale-out-your-remote-desktop-services-deployment-by-adding-an-rd-session-host-farm"></a>Monter en charge votre déploiement de Services Bureau à distance en ajoutant une batterie de serveurs hôte de Session Bureau à distance

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez améliorer la disponibilité et la mise à l’échelle de votre déploiement des services Bureau à distance en ajoutant une batterie de serveurs hôte de Session de bureau à distance (RDSH).   
  
 
Utilisez les étapes suivantes pour ajouter un autre hôte de session Bureau à distance à votre déploiement :  
  
1. Créer un serveur pour héberger le second hôte de Session Bureau à distance. Si vous utilisez des machines virtuelles, veillez à inclure la nouvelle machine virtuelle dans le même groupe à haute disponibilité qui contient votre premier hôte de Session Bureau à distance.
2. Activer la gestion à distance sur le nouveau serveur ou une machine virtuelle :
   1. Dans le Gestionnaire de serveur, cliquez sur **serveur Local > paramètre actuel de gestion à distance (désactivé)**. 
   2. Sélectionnez **activer la gestion à distance pour ce serveur**, puis cliquez sur **OK**. 
   3. Facultatif : Vous pouvez temporairement définir la mise à jour de Windows pour télécharger et installer les mises à jour pas automatiquement. Cela permet d’éviter les modifications et les redémarrages du système pendant que vous déployez le serveur RDSH. Dans le Gestionnaire de serveur, cliquez sur **serveur Local > paramètre en cours de mise à jour Windows**. Cliquez sur **les options avancées > différer les mises à niveau**. 
3. Ajoutez le serveur ou la machine virtuelle au domaine :
   1. Dans le Gestionnaire de serveur, cliquez sur **serveur Local > paramètre actuel du groupe de travail**. 
   2. Cliquez sur **Modification > domaine**, puis entrez le nom de domaine (par exemple, Contoso.com). 
   3. Entrez les informations d’identification administrateur de domaine. 
   4. Redémarrez la machine virtuelle ou le serveur.
4. Ajoutez le nouvel hôte de Session Bureau à distance à la batterie de serveurs :
>[!NOTE] 
> Étape 1-Création d’une adresse IP publique pour la machine virtuelle de RDM, est uniquement nécessaire si vous utilisez une machine virtuelle pour le SGBDR et si elle n’a pas déjà une adresse IP assignée.
   
   1. Créer une adresse IP publique pour la machine virtuelle exécutant les Services de gestion à distance Bureau (SGBDR). La machine virtuelle de RDM sera généralement la machine virtuelle en cours d’exécution la première instance du rôle Broker pour les connexions Bureau à distance.  
       1. Dans le portail Azure, cliquez sur **Parcourir > groupes de ressources**puis cliquez sur la machine virtuelle de RDM (par exemple, Contoso-Cb1) et cliquez sur le groupe de ressources pour le déploiement.  
       2. Cliquez sur **Paramètres > interfaces réseau**, puis cliquez sur l’interface réseau correspondante.   
       3. Cliquez sur **Paramètres > adresse IP**.
       4. Pour **adresse IP publique**, sélectionnez **activé**, puis cliquez sur **adresse IP**.   
       5. Si vous avez une adresse IP publique existante à utiliser, sélectionnez-le dans la liste. Sinon, cliquez sur **créer**, entrez un nom, puis cliquez sur **OK** , puis **enregistrer**.   
   2. L’authentification sur le SGBDR.
   3. Ajouter le nouveau serveur hôte au Gestionnaire de serveur :   
       1. Lancez le Gestionnaire de serveur, cliquez sur **gérer > ajouter des serveurs**.   
       2. Dans la boîte de dialogue Ajouter des serveurs, cliquez sur **Rechercher maintenant**.   
       3. Sélectionnez le serveur que vous souhaitez utiliser pour l’hôte de Session Bureau à distance ou de la machine virtuelle nouvellement créée (par exemple, Contoso-Sh2) et cliquez sur **OK**.
   4. Ajouter le serveur hôte pour le déploiement
       1. Lancez le Gestionnaire de serveur.  
       2. Cliquez sur **des Services Bureau à distance > vue d’ensemble > serveurs de déploiement > tâches > ajouter des serveurs hôtes de Session Bureau à distance**.   
       3. Sélectionnez le nouveau serveur (par exemple, Contoso-Sh2), puis cliquez sur **suivant**.  
       4. Dans la page de Confirmation, sélectionnez **redémarrer des ordinateurs distants en fonction des besoins**, puis cliquez sur **ajouter**.   
   5. Ajouter le serveur RDSH à la batterie de serveurs de collection :
       1. Lancez le Gestionnaire de serveur.   
       2. Cliquez sur **Services Bureau à distance** puis cliquez sur la collection à laquelle vous souhaitez ajouter le serveur RDSH nouvellement créé (par exemple, ContosoDesktop).   
       3. Sous **serveurs hôtes**, cliquez sur **tâches > ajouter des serveurs d’hôte de Session Bureau à distance**.   
       4. Sélectionnez le serveur nouvellement créé (par exemple, Contoso-Sh2), puis cliquez sur **suivant**.   
       5. Dans la page de Confirmation, cliquez sur **ajouter**.   

