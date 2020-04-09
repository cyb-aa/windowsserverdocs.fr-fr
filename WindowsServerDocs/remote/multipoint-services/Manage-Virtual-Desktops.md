---
title: Gérer les bureaux virtuels
description: En savoir plus sur la gestion des bureaux virtuels (VDI) dans MultiPoint services
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: fa9ac0ed-47cb-4811-91ff-4fcb62d7858b
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: 114fde42ca36f9451680066056251bafbe944e56
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853462"
---
# <a name="manage-virtual-desktops"></a>Gérer les bureaux virtuels
L’infrastructure VDI sur un seul ordinateur vous permet de configurer chaque station multipoint services *locale* pour se connecter à un système d’exploitation invité Windows 10 entreprise s’exécutant sur une machine virtuelle Hyper-V sur le même ordinateur multipoint services que la station. Ces stations de bureaux virtuels peuvent être personnalisées avec une application qui ne peut pas être installée sur une version de Windows Server.  
  
## <a name="enable-the-virtual-desktop-feature"></a>Activer la fonctionnalité de bureau virtuel  
  
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

1.    Ouvrez le Gestionnaire MultiPoint, puis cliquez sur l’onglet **Bureaux virtuels**.  
  
2.    Sous les tâches VDI, cliquez sur **Import Virtual desktop template** (Importer un modèle de bureau virtuel).  
  
3.    Recherchez le modèle et définissez le chemin et le préfixe pour le modèle importé.  
  
## <a name="customize-the-virtual-desktop-template"></a>Personnaliser le modèle de bureau virtuel  
Après avoir créé le modèle de bureau virtuel, vous pouvez le personnaliser avec des applications, des mises à jour logicielles, et configurer les paramètres système.   

1. Ouvrez le Gestionnaire MultiPoint, puis cliquez sur l’onglet **Bureaux virtuels**.  
2. Choisissez le modèle de bureau virtuel, puis cliquez sur **Customize virtual desktop template** (Personnaliser le modèle de bureau virtuel).  
Le modèle s’ouvre dans une fenêtre distincte et des instructions supplémentaires s’affichent, qui mettent en évidence les étapes les plus importantes pour la personnalisation du modèle virtuel. Lisez attentivement ces instructions.  
  
## <a name="create-virtual-desktop-stations"></a>Créer des stations de bureaux virtuels  
  
1.  Ouvrez le Gestionnaire MultiPoint en mode station, puis cliquez sur l’onglet **Bureaux virtuels**.  
  
    > [!NOTE]  
    > Si le système MultiPoint Services n’est pas exécuté en mode station, redémarrez-le avant d’exécuter les étapes suivantes.  
  
2.  Sélectionnez le modèle de bureau virtuel dans le volet gauche. Son nom est <préfixe – t>.  
  
3.  Sous les tâches de modèle, cliquez sur **Créer des stations de bureaux virtuels**, puis sur **OK**.  
  
    Le processus de création de la station de bureau virtuel prend quelques minutes.  
  
    > [!NOTE]  
    > Si l’une des stations locales est actuellement connectée à un bureau virtuel basé sur une session, vous devez vous déconnecter de cette station pour qu’elle se connecte à l’une des stations de bureaux virtuels que vous venez de créer.  
  
### <a name="validate-the-newly-created-customized-virtual-station-desktops"></a>Valider les bureaux de station virtuels personnalisés nouvellement créés  
  
Vous pouvez valider les bureaux de station virtuels personnalisés en vous connectant à une ou plusieurs stations de bureaux virtuels à l’aide d’un compte administrateur local ou d’un compte de domaine, puis vérifier que les nouveaux bureaux virtuels basés sur des machines virtuelles fonctionnent correctement.  
  
## <a name="disable-virtual-desktops"></a>Désactiver les bureaux virtuels  
  
Lors de la désactivation des bureaux virtuels, la fonctionnalité Hyper-V est désactivée. Tous les utilisateurs sont déconnectés et le système est redémarré. Toutes les stations virtuelles sont affectées à des sessions locales MultiPoint après le redémarrage du système.  

1. Ouvrez le Gestionnaire MultiPoint en mode station, puis cliquez sur l’onglet **Bureaux virtuels**.  
  
2. Sous les tâches VDI, cliquez sur **Disable virtual desktops** (Désactiver les bureaux virtuels). 