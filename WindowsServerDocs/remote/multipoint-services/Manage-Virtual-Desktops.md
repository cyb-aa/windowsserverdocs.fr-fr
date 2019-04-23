---
title: Gérer les bureaux virtuels
description: Découvrez comment gérer des bureaux virtuels (VDI) dans MultiPoint Services
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fa9ac0ed-47cb-4811-91ff-4fcb62d7858b
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: 7afc6d2a65cd5cd3b116db5d65fd97e4cc770690
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861440"
---
# <a name="manage-virtual-desktops"></a>Gérer les bureaux virtuels
VDI d’ordinateur unique vous permet de configurer chaque *local* station MultiPoint Services pour vous connecter à un système de d’exploitation invité Windows 10 entreprise en cours d’exécution sur une machine virtuelle de Hyper-V (VM) sur le même ordinateur MultiPoint Services en tant que le station. Ces stations de bureaux virtuels peuvent être personnalisées avec une application qui ne peut pas être installée sur une version de Windows Server.  
  
## <a name="enable-the-virtual-desktop-feature"></a>Activer la fonctionnalité de bureau virtuelle  
  
1.  Ouvrez le Gestionnaire MultiPoint, puis cliquez sur l’onglet **Bureaux virtuels**.  
  
2.  Sous **Tâches VDI**, cliquez sur **Create virtual desktop** (Créer un bureau virtuel), puis recherchez le fichier .iso ou VHD de Windows 10 Entreprise.  
  
Le système est redémarré, ce qui peut prendre quelques minutes.  
  
## <a name="create-a-virtual-desktop-template"></a>Créer un modèle de bureau virtuel  
  
1.  Ouvrez le Gestionnaire MultiPoint, puis cliquez sur l’onglet **Bureaux virtuels**.  
  
2.  Sous **Tâches VDI**, cliquez sur **Create virtual desktop** (Créer un bureau virtuel), puis recherchez le fichier .iso ou VHD de Windows 10 Entreprise.  
  
    Si vous utilisez le DVD, le programme localise automatiquement le fichier .wim de Windows 10 Entreprise. Sinon, cliquez sur **Parcourir**, puis accédez au fichier .iso ou VHD de Windows 10 Entreprise.  
  
    Modifiez le préfixe, si vous le souhaitez. Il s’agit, par défaut, du nom de l’ordinateur hôte.  
  
    > [!NOTE]  
    > Le préfixe est utilisé pour donner un nom au modèle et aux stations de bureaux virtuels. Le modèle est nommé préfixe \-t. Les stations de bureaux virtuels sont nommées préfixe \-*n*, où *n* est l’ID de la station.  
  
4.  Entrez un nom et un mot de passe pour le compte administrateur local qui sera utilisé pour la connexion à tous les bureaux de station virtuels créés à partir du modèle, puis cliquez sur **OK**.  
  
    La création d’un modèle prend quelques minutes.  
      
    Découvrez ensuite comment personnaliser le modèle de bureau virtuel.  
      
    > [!NOTE]  
    > Si le serveur MultiPoint est joint à un domaine, la boîte de dialogue remplit un champ supplémentaire qui vous permet d’indiquer si les machines virtuelles qui ont été créées à partir du modèle doivent être jointes à un domaine.   
  
## <a name="import-a-virtual-desktop-template"></a>Importer un modèle de bureau virtuel  
Si vous avez créé un modèle de bureau virtuel sur un autre serveur MultiPoint, vous pouvez importer ce modèle en procédant comme suit.  

1.  Ouvrez le Gestionnaire MultiPoint, puis cliquez sur l’onglet **Bureaux virtuels**.  
  
2.  Sous les tâches VDI, cliquez sur **Import Virtual desktop template** (Importer un modèle de bureau virtuel).  
  
3.  Recherchez le modèle et définissez le chemin et le préfixe pour le modèle importé.  
  
## <a name="customize-the-virtual-desktop-template"></a>Personnaliser le modèle de bureau virtuel  
Après avoir créé le modèle de bureau virtuel, vous pouvez le personnaliser avec des applications, des mises à jour logicielles, et configurer les paramètres système.   

1. Ouvrez le Gestionnaire MultiPoint, puis cliquez sur l’onglet **Bureaux virtuels**.  
2. Choisissez le modèle de bureau virtuel, puis cliquez sur **Customize virtual desktop template** (Personnaliser le modèle de bureau virtuel).  
Le modèle s’ouvre dans une fenêtre distincte et des instructions supplémentaires s’affichent, qui mettent en évidence les étapes les plus importantes pour la personnalisation du modèle virtuel. Lisez attentivement ces instructions.  
  
## <a name="create-virtual-desktop-stations"></a>Créer des stations de bureaux virtuels  
  
1.  Ouvrez le Gestionnaire MultiPoint en mode station, puis cliquez sur l’onglet **Bureaux virtuels**.  
  
    > [!NOTE]  
    > Si le système MultiPoint Services n’est pas exécuté en mode station, redémarrez-le avant d’exécuter les étapes suivantes.  
  
2.  Sélectionnez le modèle de bureau virtuel à gauche\-volet. Son nom est <préfixe – t>.  
  
3.  Sous les tâches de modèle, cliquez sur **Créer des stations de bureaux virtuels**, puis sur **OK**.  
  
    Le processus de création de la station de bureau virtuel prend quelques minutes.  
  
    > [!NOTE]  
    > Si une des stations locales est actuellement connectée à une session\-virtuel basé sur bureau, vous devez vous déconnecter cette station pour qu’ils se connecter à une station de bureau virtuelle nouvellement créée.  
  
### <a name="validate-the-newly-created-customized-virtual-station-desktops"></a>Valider les bureaux de station virtuels personnalisés nouvellement créés  
  
Vous pouvez valider vos bureaux de station virtuels personnalisés en vous connectant à un ou plusieurs des stations de bureaux virtuelles à l’aide d’un compte d’administrateur local ou un compte de domaine, puis vérifiez que la nouvelle machine virtuelle\-fonctionnent en fonction des bureaux virtuels correctement.  
  
## <a name="disable-virtual-desktops"></a>Désactiver les bureaux virtuels  
  
Lors de la désactivation des bureaux virtuels, la fonctionnalité Hyper-V est désactivée. Tous les utilisateurs sont déconnectés et le système est redémarré. Toutes les stations virtuelles sont affectées à des sessions locales MultiPoint après le redémarrage du système.  

1. Ouvrez le Gestionnaire MultiPoint en mode station, puis cliquez sur l’onglet **Bureaux virtuels**.  
  
2. Sous les tâches VDI, cliquez sur **Disable virtual desktops** (Désactiver les bureaux virtuels). 