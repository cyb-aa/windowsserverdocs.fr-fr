---
title: Charger une image système Windows Server 2008/2008 R2 spécialisée dans Azure
description: Windows Server 2008 et 2008 R2 approchent de leur fin de service. Découvrez comment basculer vers Azure en hébergeant Windows Server dans le cloud.
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: mikeblodge
ms.author: mikeblodge
ms.date: 07/11/2018
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.localizationpriority: high
ms.openlocfilehash: af98a219a4a5aa708df9c648f1b245a21e95f016
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827810"
---
# <a name="upload-a-windows-server-20082008-r2-specialized-image-to-azure"></a>Charger une image système Windows Server 2008/2008 R2 spécialisée dans Azure 

![Bannière introduisant la rubrique image système WS08](media/WS08-image-banner-large.png)

Vous pouvez désormais exécuter une machine virtuelle Windows Server 2008/2008 R2 dans le cloud avec Azure. 

## <a name="prep-the-windows-server-20082008-r2-specialized-image"></a>Préparation de l’image système Windows Server 2008/2008 R2 spécialisée
Avant de charger une image système, effectuez les modifications suivantes :

- Téléchargez et installez Windows Server 2008 Service Pack 2 (SP2) si vous ne l’avez pas déjà installé sur votre image système.

- Configurez les paramètres du Bureau à distance (RDP).
   1. Accédez à **Panneau de configuration** > **Paramètres système**.   
   2. Dans le menu de gauche, sélectionnez **Paramètres distants**.

   ![Capture d’écran des paramètres système, avec « Paramètres distants » en surbrillance.](media/1a_remote_settings.png)

   3. Sélectionnez l’onglet **Distant** dans les Propriétés système.   

   ![Capture d’écran de l’onglet distant dans les propriétés système.](media/2c_sysprops.png)

   4. Sélectionnez Autoriser la connexion des ordinateurs exécutant n'importe quelle version de Bureau à distance (moins sûr).   
   5. Cliquez sur **Appliquer**, puis sur **OK**.
- Configurez les paramètres du Pare-feu Windows.   
   1. À l’invite de commandes en mode Administrateur, entrez « **wf.msc** » pour les paramètres de Pare-feu Windows et sécurité avancée.   
   2. Triez les résultats par **Ports**, sélectionnez **port 3389**.   
     ![Règles de trafic entrant de paramètres du pare-feu de capture d’écran de WIndows.](media/3b_inboundrules.png)   
   3. Activer le Bureau à distance (TCP-IN) pour les profils : **Domaine**, **privé**, et **Public** (illustré ci-dessus).

- Enregistrez tous les paramètres et arrêtez l’image.   
- Si vous utilisez Hyper-V, assurez-vous que l’AVHD enfant est fusionné dans le disque dur virtuel parent pour la conservation des modifications.

Un bogue actuel connu fait expirer dans les 24 heures le mot de passe administrateur sur l’image chargée. Procédez comme suit pour contourner ce problème : 

1. Accédez à **Démarrer** > **Exécuter**
2. Tapez **lusrmgr.msc**
3. Sélectionnez **Utilisateurs** sous Utilisateurs et groupes locaux
4. Cliquez avec le bouton droit sur **Administrateur** et sélectionnez **Propriétés**
5. Sélectionnez **mot de passe n’expire jamais** et sélectionnez **OK**
![capture d’écran des propriétés de l’administrateur.](media/6_adminprops.png)

## <a name="uploading-the-image-vhd"></a>Chargement du disque dur virtuel de l’image
Vous pouvez utiliser le script ci-dessous pour charger le disque dur virtuel. Avant cela, vous aurez besoin du fichier de paramètres de publication de votre compte Azure. Obtenez vos [paramètres de fichier Azure](https://azure.microsoft.com/downloads/).

Voici le script :

```powershell
Get-AzurePublishSettingsFile 

Login-AzureRmAccount
 
      # Import publishsettings
      Import-AzurePublishSettingsFile -PublishSettingsFile <LocationOfPublishingFile>
      $subscriptionId = 'xxxx-xxxx-xxxx-xxxx-xxxxx'
 
      # Set NodeFlight subscription as default subscription
      Select-AzureRmSubscription -SubscriptionId $subscriptionId
      Set-AzureRmContext -SubscriptionId $subscriptionId
      $rgName = "<resourcegroupname>"
    
      $urlOfUploadedImageVhd = "<BlobUrl>/<NameForVHD>.vhd"
      Add-AzureRmVhd -ResourceGroupName $rgName -Destination $urlOfUploadedImageVhd -LocalFilePath "<FilePath>"  
```
## <a name="deploy-the-image-in-azure"></a>Déployer l’image dans Azure
Dans cette section, vous déployez le disque dur virtuel de l’image dans Azure. 

> [!IMPORTANT]
> N’utilisez pas d’images prédéfinies par l’utilisateur dans Azure.

1.  Créez un [groupe de ressources](https://docs.microsoft.com/rest/api/resources/resourcegroups/createorupdate). 
2.  Créez un [objet blob de stockage](https://docs.microsoft.com/rest/api/storageservices/put-blob) dans le groupe de ressources.
3.  Créez un [conteneur](https://docs.microsoft.com/rest/api/storageservices/create-container) à l’intérieur de l’objet blob de stockage.
4.  Copiez l’URL du stockage blob à partir des propriétés.
5.  Utilisez le script indiqué ci-dessus pour charger l’image vers le nouvel objet blob de stockage.
6.  Créez un [disque](https://docs.microsoft.com/azure/virtual-machines/windows/prepare-for-upload-vhd-image) pour votre disque dur virtuel.   
     a. Accédez à Disques, cliquez sur **Ajouter**.  
     b. Entrez un nom pour le disque. Sélectionnez l’abonnement que vous souhaitez utiliser, définissez la région et choisissez le type de compte.   
     c. Pour Type de source, sélectionnez stockage. Accédez à l’emplacement du disque dur virtuel blob créé à l’aide du script.  
     d. Sélectionnez le type de système d’exploitation Windows et la taille (par défaut : 1023).   
     e. Cliquez sur **Create (Créer)**.   

7.  Accédez au disque créé et cliquez sur **Créer une machine virtuelle**.   
     a. Donnez un nom à la machine virtuelle.   
     b. Sélectionnez le groupe existant que vous avez créé à l’étape 5 lorsque vous avez chargé le disque.   
     c. Choisissez une taille et un plan de référence pour votre machine virtuelle.   
     d. Sélectionnez une interface réseau dans la page Paramètres. Vérifiez que l’interface réseau présente la règle spécifiée suivante :
 
        PORT:3389 Protocol: TCP Action: Allow Priority: 1000 Name: ‘RDP-Rule’.   
     e. Cliquez sur **Create (Créer)**.




