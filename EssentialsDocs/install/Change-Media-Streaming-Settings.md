---
title: "Modifier les paramètres de diffusion multimédia en continu"
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dec690d2-f80c-4b09-99d6-3bba41331972
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: d34d60e792fcda4d71a4509fe3d1b95fc6e74d0e
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="change-media-streaming-settings"></a>Modifier les paramètres de diffusion multimédia en continu

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

Plusieurs options sont disponibles pour modifier les paramètres de diffusion multimédia en continu. Les options suivantes sont disponibles:  
  
-   [Masquer à distance diffusion multimédia en continu complément](Change-Media-Streaming-Settings.md#BKMK_DisableRemote)  
  
-   [Définir le nom de la bibliothèque multimédia](Change-Media-Streaming-Settings.md#BKMK_LibraryName)  
  
-   [Définir la qualité de diffusion vidéo](Change-Media-Streaming-Settings.md#BKMK_StreamingQuality)  
  
-   [Activer ou désactiver la diffusion multimédia en continu par programme](Change-Media-Streaming-Settings.md#BKMK_Program)  
  
##  <a name="BKMK_DisableRemote"></a>Masquer à distance diffusion multimédia en continu complément  
 Vous pouvez masquer le support à distance diffusion complément en ajoutant une entrée dans le Registre.  
  
#### <a name="to-hide-the-remote-media-streaming-add-in"></a>Pour masquer le support à distance en continu dans  
  
1.  Sur le serveur, déplacez votre souris vers le coin supérieur droit de l’écran, puis cliquez sur **recherche**.  
  
2.  Dans le **recherche**, tapez **regedit**, puis cliquez sur le **Regedit** application.  
  
3.  Dans le volet gauche, développez l’entrée de Registre suivante:  
  
     **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\RemoteAccess\DisabledAddIns**  
  
4.  Avec le bouton droit **DisabledAddIns**, pointez sur **New**, puis cliquez sur **valeur DWORD**.  
  
5.  Pour la nouvelle valeur, tapez **497796c6-9cc7-43be-aa26-4e6b5695370d**, qui est l’identificateur pour le support à distance en continu dans et appuyez sur **entrée**.  
  
6.  Cliquez avec le bouton droit sur la valeur, puis cliquez sur **modifier**.  
  
7.  Type **1** pour les données de la valeur, puis cliquez sur **OK**.  
  
##  <a name="BKMK_LibraryName"></a>Définir le nom de la bibliothèque multimédia  
 Vous pouvez utiliser une classe dans le SDK WindowsServerSolutions pour définir le nom de la bibliothèque multimédia. Pour définir le nom de la bibliothèque multimédia, vous pouvez utiliser le **SetMediaLibraryName** méthode du **MediaStreamingManager** classe dans le **Microsoft.WindowsServerSolutions.MediaStreaming** espace de noms. L’exemple suivant montre comment définir le nom de la bibliothèque multimédia:  
  
```c#  
  
MediaStreamingManager mediaStreamingManager = new MediaStreamingManager();  
string mediaLibraryName = Guid.NewGuid().ToString("B");   
mediaStreamingManager.SetMediaLibraryName(mediaLibraryName);  
  
```  
  
 Pour plus d’informations, voir [SDK WindowsServerSolutions](https://go.microsoft.com/fwlink/?LinkID=248648).  
  
##  <a name="BKMK_StreamingQuality"></a>Définir la qualité de diffusion vidéo  
 Vous définissez la vidéo de qualité de diffusion par l’obtention du score WinSAT du processeur et de création et de l’installation du fichier .xml contenant les informations du score WinSAT. Si le fichier .xml contenant les informations du score WinSAT est installé avant l’exécution de la Configuration initiale, le client n’apparaît pas l’interface utilisateur pour la qualité vidéo de paramètre. Pour plus d’informations, voir [définition du Score WinSAT sur le serveur](Set-the-WinSAT-Score-on-the-Server.md).  
  
##  <a name="BKMK_Program"></a>Activer ou désactiver la diffusion multimédia en continu par programme  
 Vous pouvez utiliser une classe dans le SDK WindowsServerSolutions pour activer ou désactiver la diffusion multimédia en continu par programme. Pour activer ou désactiver la diffusion multimédia en continu, vous pouvez utiliser le **SetMediaStreamingEnabled** méthode du **MediaStreamingManager** classe dans le **Microsoft.WindowsServerSolutions.MediaStreaming** espace de noms. L’exemple de code suivant montre comment activer la diffusion multimédia en continu:  
  
```c#  
  
MediaStreamingManager mediaStreamingManager = new MediaStreamingManager();  
mediaStreamingManager.SetMediaStreamingEnabled(true);  
  
```  
  
 L’exemple de code suivant montre comment désactiver la diffusion multimédia en continu:  
  
```c#  
  
MediaStreamingManager mediaStreamingManager = new MediaStreamingManager();  
mediaStreamingManager.SetMediaStreamingEnabled(false);  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Création et personnalisation de l’Image](Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](Additional-Customizations.md)   
 [Préparation de l’Image pour le déploiement](Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience client](Testing-the-Customer-Experience.md)