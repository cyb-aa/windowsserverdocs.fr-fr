---
title: Préparer les machines virtuelles pour les services Bureau à distance
description: Préparer vos machines virtuelles pour les composants Bureau à distance
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 07/21/2017
ms.topic: article
ms.assetid: 2fc39dff-61ca-4eba-81ab-52289081bead
author: lizap
manager: dongill
ms.openlocfilehash: 5aec90275db1e09906e051419929086a40a34800
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2020
ms.locfileid: "80858132"
---
# <a name="create-virtual-machines-for-remote-desktop"></a>Créer des machines virtuelles pour les services Bureau à distance

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2019, Windows Server 2016

Vous pouvez installer les composants Services Bureau à distance sur des serveurs physiques ou des machines virtuelles. 

La première étape consiste à [créer des machines virtuelles Windows Server dans Azure](/azure/virtual-machines/windows/quick-create-portal). Vous allez créer trois machines virtuelles : une pour l’hôte de session Bureau à distance, une pour le service Broker de connexion Bureau à distance et une pour la passerelle Bureau à distance et l’accès web Bureau à distance. Pour garantir la disponibilité de votre déploiement Services Bureau à distance, créez un groupe de disponibilité (sous **High availablility** (Haute disponibilité) dans le processus de création de machine virtuelle) et regroupez plusieurs machines virtuelles dans ce groupe de disponibilité.
 
Après avoir créé vos machines virtuelles, procédez comme suit pour les préparer pour Services Bureau à distance.

1.  Connectez-vous à la machine virtuelle via le client Connexion Bureau à distance :  
    1.  Dans le portail Azure, ouvrez la vue Groupes de ressources, puis cliquez sur le groupe de ressources à utiliser pour le déploiement.  
    2.  Sélectionnez la nouvelle machine virtuelle d’hôte de session Bureau à distance (par exemple, Contoso-Sh1).  
    3.  Cliquez sur **Se connecter > Ouvrir** pour ouvrir le client Bureau à distance.  
    4.  Dans le client, cliquez sur **Se connecter**, puis cliquez sur **Use another user account** (Utiliser un autre compte d’utilisateur). Entrez le nom d’utilisateur et le mot de passe du compte d’administrateur local.  
    5.  Cliquez sur **Oui** une fois l’avertissement concernant le certificat affiché.  
2.  Activer la gestion à distance :  
    1.  Dans le Gestionnaire de serveur, cliquez sur **Serveur local > paramètre actuel de l’Administration à distance (désactivé)** .  
    2.  Sélectionnez **Enable remote management for this server** (Activer la gestion à distance pour ce serveur).  
    3.  Cliquez sur **OK**.  
3.  Facultatif : Vous pouvez temporairement définir Windows Update pour que les mises à jour ne soient ni téléchargées ni installées automatiquement. Cela permet d’éviter les modifications et les redémarrages du système pendant que vous déployez le serveur d’hôte de session Bureau à distance.  
    1.  Dans le Gestionnaire de serveur, cliquez sur **Serveur local > paramètre actuel de Windows Update**.  
    2.  Sélectionnez **Options avancées > Différer les mises à niveau**.   
4.  Ajouter le serveur au domaine :  
    1.  Dans le Gestionnaire de serveur, cliquez sur **Serveur local > paramètre actuel du Groupe de travail**.  
    2.  Cliquez sur **Modifier > Domaine**, puis entrez le nom de domaine (par exemple, Contoso.com).  
    3.  Entrez les informations d’identification de l’administrateur de domaine.  
    4.  Redémarrez l’ordinateur virtuel.  
5.  Répétez les étapes 1 à 4 pour la machine virtuelle de passerelle et d’accès web Bureau à distance.  
6.  Répétez les étapes 1 à 4 pour la machine virtuelle du service Broker de connexion Bureau à distance.  
7.  Initialiser et formater le disque attaché sur la machine virtuelle du service Broker de connexion Bureau à distance :  
    1.  Connectez-vous à la machine virtuelle du service Broker de connexion Bureau à distance (étape 1 ci-dessus).  
    2.  Dans le Gestionnaire de serveur, cliquez sur **Outils > Gestion de l’ordinateur**.  
    3.  Cliquez sur **Gestion des disques**.  
    4.  Sélectionnez le disque attaché, puis **Secteur de démarrage principal**, puis cliquez sur **OK**.  
    5.  Cliquez avec le bouton droit sur le nouveau disque (marqué comme **non alloué**) et cliquez sur **Nouveau volume simple**.  
    6.  Dans l’Assistant **Nouveau volume simple**, acceptez les valeurs par défaut, mais fournissez un **nom de volume** (par exemple, Partages).  
8.  Sur la machine virtuelle du service Broker de connexion Bureau à distance, créez des partages de fichiers pour les certificats et disques de profil utilisateur :   
    1.  Ouvrez l’Explorateur de fichiers, cliquez sur **Ce PC**, puis ouvrez le disque que vous avez ajouté aux partages de fichiers.  
    2.  Cliquez sur **Accueil** et **Nouveau dossier**.  
    3.  Entrez un nom pour le dossier de disques utilisateur, par exemple, **UserDisks**.  
    4.  Cliquez avec le bouton droit sur le nouveau dossier et cliquez sur **Propriétés > Partage > Partage avancé**.  
    5.  Sélectionnez **Partager ce dossier** et cliquez sur **Autorisations**.  
    6.  Sélectionnez **Tout le monde**, puis cliquez sur **Supprimer**. Cliquez maintenant sur **Ajouter**, entrez **Admins du domaine**, puis cliquez sur **OK**.  
    7.  Sélectionnez **Allow Full Control** (Autoriser le contrôle intégral), puis cliquez sur **OK > OK > Fermer**.  
    8.  Répétez les étapes c. à g. pour créer un dossier partagé pour les certificats.   


