---
title: Modification du support de diffusion de contenu
description: Décrit comment utiliser Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dec690d2-f80c-4b09-99d6-3bba41331972
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 217bcbeda8f50ee1f17bcc1da6bd5b982dcf5106
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80310074"
---
# <a name="change-media-streaming-settings"></a>Modification du support de diffusion de contenu

>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Voici les différents moyens dont vous disposez pour modifier les paramètres de diffusion de contenus multimédia. Les options suivantes sont disponibles :  
  
-   [Masquer le complément de diffusion de contenu multimédia à distance](Change-Media-Streaming-Settings.md#BKMK_DisableRemote)  
  
-   [Définir le nom de la bibliothèque multimédia](Change-Media-Streaming-Settings.md#BKMK_LibraryName)  
  
-   [Définir la qualité de diffusion vidéo](Change-Media-Streaming-Settings.md#BKMK_StreamingQuality)  
  
-   [Activer ou désactiver la diffusion multimédia en continu par programmation](Change-Media-Streaming-Settings.md#BKMK_Program)  
  
##  <a name="hide-remote-media-streaming-add-in"></a><a name="BKMK_DisableRemote"></a>Masquer le complément de diffusion de contenu multimédia à distance  
 Il est possible de masquer la diffusion de contenus multimédia à distance en ajoutant une entrée au Registre.  
  
#### <a name="to-hide-the-remote-media-streaming-add-in"></a>Pour masquer le complément de diffusion de contenus multimédias à distance  
  
1.  Sur le serveur, déplacez votre souris sur le coin supérieur droit de l’écran, puis cliquez sur **Rechercher**.  
  
2.  Dans la zone **Rechercher**, tapez **regedit**, puis cliquez sur l'application **Regedit**.  
  
3.  Dans le volet gauche, développez l'entrée de Registre suivante :  
  
     **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\RemoteAccess\DisabledAddIns**  
  
4.  Cliquez avec le bouton droit sur **DisabledAddIns**, pointez sur **Nouveau**, puis cliquez sur **Valeur DWORD**.  
  
5.  Tapez **497796c6-9cc7-43be-aa26-4e6b5695370d** comme nouvelle valeur. Il s'agit de l'identificateur pour le complément de diffusion de contenus multimédias à distance. Appuyez ensuite sur **Enter**.  
  
6.  Cliquez avec le bouton droit sur la valeur, puis cliquez sur **Modifier**.  
  
7.  Tapez **1** dans le champ Données de la valeur, puis cliquez sur **OK**.  
  
##  <a name="set-the-media-library-name"></a><a name="BKMK_LibraryName"></a>Définir le nom de la bibliothèque multimédia  
 Vous pouvez utiliser une classe du Kit de développement logiciel (SDK) des solutions Windows Server pour définir le nom de la bibliothèque multimédia. Pour ce faire, servez-vous de la méthode **SetMediaLibraryName** dans la classe **MediaStreamingManager** de l'espace de noms **Microsoft.WindowsServerSolutions.MediaStreaming**. L'exemple suivant montre comment définir le nom de la bibliothèque multimédia :  
  
```c#  
  
MediaStreamingManager mediaStreamingManager = new MediaStreamingManager();  
string mediaLibraryName = Guid.NewGuid().ToString("B");   
mediaStreamingManager.SetMediaLibraryName(mediaLibraryName);  
  
```  
  
 Pour plus d’informations, consultez [SDK Windows Server Solutions](https://go.microsoft.com/fwlink/?LinkID=248648).  
  
##  <a name="set-video-streaming-quality"></a><a name="BKMK_StreamingQuality"></a>Définir la qualité de diffusion vidéo  
 Pour définir la qualité de diffusion vidéo, il convient de relever la note WinSAT donnée au processeur, puis de créer et d'installer le fichier .xml contenant les indices de performance WinSAT. Si le fichier .xml en question est installé avant d'exécuter la configuration initiale, le client n'aura pas accès à l'interface utilisateur permettant de définir la qualité de diffusion vidéo. Pour plus d'informations, voir [Définition du score WinSAT sur le serveur](Set-the-WinSAT-Score-on-the-Server.md).  
  
##  <a name="programmatically-enable-or-disable-media-streaming"></a><a name="BKMK_Program"></a>Activer ou désactiver la diffusion multimédia en continu par programmation  
 Vous pouvez utiliser une classe du Kit de développement logiciel (SDK) des solutions Windows Server pour activer ou désactiver la diffusion de contenu multimédia par programme. Pour ce faire, servez-vous de la méthode **SetMediaStreamingEnabled** dans la classe **MediaStreamingManager** de l'espace de noms **Microsoft.WindowsServerSolutions.MediaStreaming**. L'exemple de code suivant montre comment activer la diffusion de contenu multimédia :  
  
```c#  
  
MediaStreamingManager mediaStreamingManager = new MediaStreamingManager();  
mediaStreamingManager.SetMediaStreamingEnabled(true);  
  
```  
  
 L'exemple de code suivant montre comment désactiver la diffusion de contenu multimédia :  
  
```c#  
  
MediaStreamingManager mediaStreamingManager = new MediaStreamingManager();  
mediaStreamingManager.SetMediaStreamingEnabled(false);  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Création et personnalisation de l’Image](Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](Additional-Customizations.md)   
 [Préparation de l’image pour le déploiement](Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience utilisateur](Testing-the-Customer-Experience.md)