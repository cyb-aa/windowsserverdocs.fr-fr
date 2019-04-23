---
title: Préparer les machines virtuelles pour les services Bureau à distance
description: Préparer vos machines virtuelles pour les composants de bureau à distance
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 07/21/2017
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2fc39dff-61ca-4eba-81ab-52289081bead
author: lizap
manager: dongill
ms.openlocfilehash: cb06963c3ae51fb9337827c7b29b93b8c2736c16
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871880"
---
# <a name="create-virtual-machines-for-remote-desktop"></a>Créer des machines virtuelles pour les services Bureau à distance

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez installer les composants des Services Bureau à distance sur des serveurs physiques ou des ordinateurs virtuels. 

La première étape consiste à [créer des machines virtuelles Windows Server dans Azure](/azure/virtual-machines/windows/quick-create-portal). Vous voudrez créer trois machines virtuelles : un pour l’hôte de Session Bureau à distance, un pour le service Broker de connexion et un pour le Web de bureau à distance et de la passerelle Bureau à distance. Pour garantir la disponibilité de votre déploiement des services Bureau à distance, créez un groupe de disponibilité défini (sous **haute disponibilité** dans le processus de création de machine virtuelle) et de regrouper plusieurs machines virtuelles dans la mesure à haute disponibilité.
 
Après avoir créé vos machines virtuelles, procédez comme suit pour les préparer pour RDS.

1.  Se connecter à la machine virtuelle via le client Connexion Bureau à distance (RDC) :  
    1.  Dans le portail Azure ouvrir l’affichage de groupes de ressources, puis cliquez sur le groupe de ressources à utiliser pour le déploiement.  
    2.  Sélectionnez la nouvelle machine virtuelle RDSH (par exemple, Contoso-Sh1).  
    3.  Cliquez sur **Connect > Ouvrez** pour ouvrir le client Bureau à distance.  
    4.  Dans le client, cliquez sur **Connect**, puis cliquez sur **utiliser un autre compte d’utilisateur**. Entrez le nom d’utilisateur et le mot de passe pour le compte d’administrateur local.  
    5.  Cliquez sur **Oui** lorsque l’avertissement concernant le certificat.  
2.  Activer la gestion à distance :  
    1.  Dans le Gestionnaire de serveur, cliquez sur **serveur Local > paramètre actuel de gestion à distance (désactivé)**.  
    2.  Sélectionnez **activer la gestion à distance pour ce serveur**.  
    3.  Cliquez sur **OK**.  
3.  Facultatif : Vous pouvez temporairement définir la mise à jour de Windows pour télécharger et installer les mises à jour pas automatiquement. Cela permet d’éviter les modifications et les redémarrages du système pendant que vous déployez le serveur RDSH.  
    1.  Dans le Gestionnaire de serveur, cliquez sur **serveur Local > paramètre en cours de mise à jour Windows**.  
    2.  Sélectionnez **les options avancées > différer les mises à niveau**.   
4.  Ajoutez le serveur au domaine :  
    1.  Dans le Gestionnaire de serveur, cliquez sur **serveur Local > paramètre actuel du groupe de travail**.  
    2.  Cliquez sur **Modification > domaine**, puis entrez le nom de domaine (par exemple, Contoso.com).  
    3.  Entrez les informations d’identification administrateur de domaine.  
    4.  Redémarrez l’ordinateur virtuel.  
5.  Répétez les étapes 1 à 4 pour l’ordinateur virtuel Web de bureau à distance et passerelle.  
6.  Répétez les étapes 1 à 4 pour la machine virtuelle de service Broker pour les connexions Bureau à distance.  
7.  Initialiser et formater le disque attaché sur la machine virtuelle de service Broker pour les connexions Bureau à distance :  
    1.  Se connecter à la machine virtuelle de service Broker pour les connexions Bureau à distance (étape 1 ci-dessus).  
    2.  Dans le Gestionnaire de serveur, cliquez sur **Outils > Gestion de l’ordinateur**.  
    3.  Cliquez sur **gestion des disques**.  
    4.  Sélectionnez le disque attaché, puis **MBR (Master Boot Record)**, puis cliquez sur **OK**.  
    5.  Cliquez sur le nouveau disque (marqué comme **non alloué**) et cliquez sur **nouveau Volume Simple**.  
    6.  Dans le **nouveau Volume Simple** Assistant, acceptez les valeurs par défaut, mais fournir un nom pour le **nom de Volume** (telles que les partages).  
8.  Créer des partages de fichiers pour les disques de profil utilisateur et les certificats sur la machine virtuelle de service Broker pour les connexions Bureau à distance :   
    1.  Ouvrez l’Explorateur de fichiers, cliquez sur **ce PC**, puis ouvrez le disque que vous avez ajouté de partages de fichiers.  
    2.  Cliquez sur **accueil** et **nouveau dossier**.  
    3.  Entrez un nom pour le dossier de disques utilisateur, par exemple, **UserDisks**.  
    4.  Cliquez sur le nouveau dossier et cliquez sur **Propriétés > partage > Partage avancé**.  
    5.  Sélectionnez **partager ce dossier** et cliquez sur **autorisations**.  
    6.  Sélectionnez **tout le monde**, puis cliquez sur **supprimer**. Cliquez maintenant sur **ajouter**, entrez **Admins du domaine**, puis cliquez sur **OK**.  
    7.  Sélectionnez **permettent un contrôle total**, puis cliquez sur **OK > OK > Fermer**.  
    8.  Répétez les étapes c. g. Pour créer un dossier partagé pour les certificats.   


