---
title: Charger une image Windows Server 2008/2008 R2 spécialisée dans Azure
description: Windows Server 2008 et 2008 R2 approchent de leur fin de service. Découvrez comment basculer vers Azure en hébergeant Windows Server dans le cloud.
ms.prod: windows-server
ms.mktglfcycl: manage
ms.sitesec: library
author: mikeblodge
ms.author: mikeblodge
ms.date: 07/11/2018
ms.topic: get-started-article
ms.localizationpriority: high
ms.openlocfilehash: de9233e31c5530abd207a1bbba0e1e16a07d1561
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2020
ms.locfileid: "80826122"
---
# <a name="upload-a-windows-server-20082008-r2-specialized-image-to-azure"></a>Charger une image Windows Server 2008/2008 R2 spécialisée dans Azure 

![Bannière introduisant la rubrique sur l’image WS08](media/WS08-image-banner-large.png)

Vous pouvez désormais exécuter une machine virtuelle Windows Server 2008/2008 R2 dans le cloud avec Azure. 

## <a name="prep-the-windows-server-20082008-r2-specialized-image"></a>Préparer l’image Windows Server 2008/2008 R2 spécialisée
Avant de charger une image, effectuez les modifications suivantes :

- Téléchargez et installez Windows Server 2008 Service Pack 2 (SP2) si vous ne l’avez pas déjà installé sur votre image.

- Configurez les paramètres du Bureau à distance (RDP).
  1. Accédez à **Panneau de configuration** > **Paramètres système**.   
  2. Dans le menu de gauche, sélectionnez **Paramètres distants**.

     ![Capture d’écran des paramètres système, avec « Paramètres distants » en surbrillance.](media/1a_remote_settings.png)

  3. Sélectionnez l’onglet **Distant** dans Propriétés système.   

     ![Capture d’écran de l’onglet Distant dans Propriétés système.](media/2c_sysprops.png)

  4. Sélectionnez Autoriser la connexion des ordinateurs exécutant n’importe quelle version de Bureau à distance (moins sûr).   
  5. Cliquez sur **Appliquer**, puis sur **OK**.
- Configurez les paramètres du Pare-feu Windows.   
   1. À l’invite de commandes en mode Administrateur, entrez « **wf.msc** » pour les paramètres de Pare-feu Windows et de sécurité avancée.   
   2. Triez les résultats par **Ports**, puis sélectionnez le **port 3389**.   
     ![Capture d’écran des règles de trafic entrant des paramètres du Pare-feu Windows.](media/3b_inboundrules.png)   
   3. Activez le Bureau à distance (TCP-IN) pour les profils : **Domaine**, **Privé** et **Public** (illustrés ci-dessus).

- Enregistrez tous les paramètres et arrêtez l’image.   
- Si vous utilisez Hyper-V, vérifiez que l’AVHD enfant est fusionné dans le disque dur virtuel parent pour la conservation des changements.

Un bogue actuel connu fait expirer dans les 24 heures le mot de passe administrateur sur l’image chargée. Pour éviter ce problème, effectuez les étapes suivantes : 

1. Accédez à **Démarrer** > **Exécuter**.
2. Tapez **lusrmgr.msc**.
3. Sélectionnez **Utilisateurs** sous Utilisateurs et groupes locaux.
4. Cliquez avec le bouton droit sur **Administrateur** et sélectionnez **Propriétés**.
5. Sélectionnez **Le mot de passe n’expire jamais**, puis **OK**.
![Capture d’écran des propriétés de l’administrateur.](media/6_adminprops.png)

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
Dans cette section, vous allez déployer le disque dur virtuel de l’image dans Azure. 

> [!IMPORTANT]
> N’utilisez pas d’images prédéfinies par l’utilisateur dans Azure.

1.    Créez un [groupe de ressources](https://docs.microsoft.com/rest/api/resources/resourcegroups/createorupdate). 
2.    Créez un [objet blob de stockage](https://docs.microsoft.com/rest/api/storageservices/put-blob) dans le groupe de ressources.
3.    Créez un [conteneur](https://docs.microsoft.com/rest/api/storageservices/create-container) à l’intérieur de l’objet blob de stockage.
4.    Copiez l’URL du stockage blob à partir des propriétés.
5.    Utilisez le script indiqué ci-dessus pour charger l’image vers le nouvel objet blob de stockage.
6.    Créez un [disque](https://docs.microsoft.com/azure/virtual-machines/windows/prepare-for-upload-vhd-image) pour votre disque dur virtuel.   
     a.    Accédez à Disques, puis cliquez sur **Ajouter**.  
     b.    Entrez un nom pour le disque. Sélectionnez l’abonnement que vous souhaitez utiliser, définissez la région et choisissez le type de compte.   
     c. Pour Type de source, sélectionnez stockage. Accédez à l’emplacement du disque dur virtuel blob créé à l’aide du script.  
     d. Sélectionnez le type de système d’exploitation Windows et la taille (par défaut : 1023).   
     e. Cliquez sur **Créer**.   

7.    Accédez au disque créé et cliquez sur **Créer une machine virtuelle**.   
     a.    Donnez un nom à la machine virtuelle.   
     b.    Sélectionnez le groupe existant que vous avez créé à l’étape 5 quand vous avez chargé le disque.   
     c.    Choisissez une taille et un plan de référence SKU pour votre machine virtuelle.   
     d.    Sélectionnez une interface réseau dans la page Paramètres. Vérifiez que l’interface réseau présente la règle spécifiée suivante :
 
        PORT:3389 Protocole : TCP Action : Autoriser Priorité : 1000 Nom : 'RDP-Rule'.   
     e.    Cliquez sur **Créer**.




