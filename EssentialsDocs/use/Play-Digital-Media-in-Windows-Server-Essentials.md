---
title: Lire des médias numériques dans Windows Server Essentials
description: Décrit comment utiliser Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5f570492-ee21-471b-92c1-3fd9bfb84f55
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 168569fc6ce7937090a45bf9e7c68353f8b62714
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80310977"
---
# <a name="play-digital-media-in-windows-server-essentials"></a>Lire des médias numériques dans Windows Server Essentials

>S’applique à : Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Les éléments multimédias numériques font référence à des contenus audio, vidéo et photo qui ont été compressés numériquement.  Windows Server Essentials permet aux ordinateurs en réseau et à certains appareils multimédias numériques en réseau de lire des fichiers multimédias numériques stockés sur le serveur.  
  
 Les rubriques suivantes fournissent des informations sur l’accès aux fichiers multimédias numériques qui sont stockés sur Windows Server Essentials, ainsi que sur leur utilisation :  
  

-   [Présentation des médias numériques](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Lire et partager des contenus multimédias numériques](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Lire des fichiers multimédias numériques partagés à partir d’un emplacement distant](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Ajouter des fichiers multimédias numériques au serveur](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [Télécharger les options de format](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Outil Easy file upload](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_6)  
  
-   [Afficher et parcourir des médias numériques partagés](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_7)  

-   [Présentation des médias numériques](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Lire et partager des contenus multimédias numériques](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Lire des fichiers multimédias numériques partagés à partir d’un emplacement distant](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Ajouter des fichiers multimédias numériques au serveur](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [Télécharger les options de format](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Outil Easy file upload](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_6)  
  
-   [Afficher et parcourir des médias numériques partagés](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_7)  

  
##  <a name="digital-media-overview"></a><a name="BKMK_1"></a>Présentation des médias numériques  
 Les éléments multimédias numériques font référence à du contenu audio, vidéo et à des photos qui ont été encodés (compressés numériquement). Le codage du contenu implique la conversion de l'entrée audio et vidéo en un fichier multimédia, comme un fichier Windows Media. Une fois le média numérique codé, il peut être facilement manipulé, distribué et lu par des ordinateurs. Il peut aussi être facilement transmis sur des réseaux d'ordinateurs.  
  
 Voici des exemples de types de médias numériques : WMA (Windows Media Audio), WMV (Windows Media Video), MP3, JPEG et AVI. Pour plus d’informations sur les types de médias numériques qui sont pris en charge par le Lecteur Windows Media, consultez la rubrique [Types de fichiers pris en charge par le Lecteur Windows Media](https://support.microsoft.com/kb/316992).  
  
### <a name="why-would-i-want-to-stream-my-digital-media"></a>Pourquoi voudrais-je diffuser en continu mes médias numériques ?  
 De nombreuses personnes stockent de la musique, des vidéos et des images dans des dossiers partagés dans Windows Server Essentials. Vous pouvez avoir envie de faire ce qui suit :  
  
-   **Regarder des vidéos**. Votre serveur peut être utilisé pour stocker et diffuser de grandes collections de vidéos et des programmes de télévision enregistrés sur vos ordinateurs ou sur d'autres appareils de votre réseau. Vous pouvez diffuser en continu des vidéos sur une Xbox 360 ou sur un ordinateur à l'aide du Lecteur Windows Media.  
  
-   **Écouter de la musique**. Quand vous activez le partage de fichiers multimédias pour le dossier partagé **Musique**, vous pouvez accéder à votre musique à partir d'appareils qui prennent en charge Windows Media Connect. Vous n'avez pas besoin d'activer ou de configurer des comptes d'utilisateur pour diffuser en continu à partir du dossier partagé **Musique** une fois que le partage est activé.  
  
-   **Présenter des diaporamas de photos**. Vous pouvez stocker des photos numériques dans le dossier partagé **Photos** sur votre serveur, puis y accéder à partir de n'importe quel ordinateur ou d'une Xbox 360 connectée à une télévision à votre domicile ou au bureau. Vous pouvez regarder des diaporamas de photos, ce qui revient à transformer votre télévision en un grand cadre à image.  
  
### <a name="sharing-copy-protected-media"></a>Partage de médias protégés contre la copie  
  Windows Server Essentials ne prend pas en charge le partage de médias protégés contre la copie. Ceci inclut la musique achetée auprès d'un magasin de musique en ligne.  
  
 Un média protégé contre la copie peut être lu seulement sur l'ordinateur ou l'appareil que vous avez utilisé pour l'acheter. La protection contre la copie vous empêche de lire un média sur plusieurs ordinateurs ou appareils, même si vous copiez le média sur votre serveur et que vous le lisez à partir de là. Toutefois, vous pouvez stocker le média protégé contre la copie sur Windows Server Essentials et continuer à lire le média sur l’ordinateur ou le périphérique que vous avez utilisé pour l’acheter.  
  
##  <a name="play-and-share-digital-media"></a><a name="BKMK_2"></a>Lire et partager des contenus multimédias numériques  
 Après avoir configuré votre réseau et connecté vos ordinateurs et appareils multimédias au réseau du serveur, vous pouvez rechercher des fichiers multimédias numériques que vous stockez et partagez sur le serveur.  
  
> [!NOTE]
>  Pour obtenir la liste actuelle des appareils multimédias/lecteurs multimédias numériques compatibles avec Windows Server Essentials, consultez le [Centre de compatibilité Windows](https://www.microsoft.com/windows/compatibility/CompatCenter/Home).  
  
 Vous pouvez utiliser l'une ou l'autre des méthodes suivantes pour rechercher et lire des fichiers multimédias numériques qui sont stockés sur votre serveur :  
  

-   [Rechercher et lire des fichiers multimédias sur Windows Server Essentials à partir d’un ordinateur ou d’un lecteur multimédia numérique sur le réseau](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2.1)  
  
-   [Envoyer des fichiers multimédias sur Windows Server Essentials vers le lecteur Windows Media, Xbox 360 ou un lecteur multimédia numérique en réseau sur le réseau](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_SendToDevice)  

-   [Rechercher et lire des fichiers multimédias sur Windows Server Essentials à partir d’un ordinateur ou d’un lecteur multimédia numérique sur le réseau](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2.1)  
  
-   [Envoyer des fichiers multimédias sur Windows Server Essentials vers le lecteur Windows Media, Xbox 360 ou un lecteur multimédia numérique en réseau sur le réseau](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_SendToDevice)  

  
###  <a name="search-for-and-play-media-files-on-windows-server-essentials-from-a-computer-or-digital-media-player-on-the-network"></a><a name="BKMK_2.1"></a>Rechercher et lire des fichiers multimédias sur Windows Server Essentials à partir d’un ordinateur ou d’un lecteur multimédia numérique sur le réseau  
 Lorsque votre appareil est joint au réseau Windows Server Essentials, vous pouvez rechercher et lire des fichiers multimédias numériques de l’une des manières suivantes :  
  

-   [Rechercher et lire des fichiers multimédias à partir d’un ordinateur qui exécute Windows Media Center](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_WMC)  
  
-   [Rechercher et lire des fichiers multimédias à partir d’un ordinateur qui exécute Windows à l’aide du lecteur Windows Media](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_MWP)  
  
-   [Rechercher et lire des fichiers multimédias à l’aide de Xbox 360](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_Xbox)  
  
-   [Rechercher et lire des fichiers multimédias à l’aide d’autres lecteurs multimédias numériques ou de récepteurs compatibles avec Windows Server Essentials](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_Other)  
  
-   [Rechercher et lire des fichiers multimédias à l’aide de la fonctionnalité dossiers partagés du Launchpad](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_SharedFolders)  
  
-   [Rechercher et lire des médias partagés à l’aide de l’Accès web à distance](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_RWA2)  

-   [Rechercher et lire des fichiers multimédias à partir d’un ordinateur qui exécute Windows Media Center](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_WMC)  
  
-   [Rechercher et lire des fichiers multimédias à partir d’un ordinateur qui exécute Windows à l’aide du lecteur Windows Media](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_MWP)  
  
-   [Rechercher et lire des fichiers multimédias à l’aide de Xbox 360](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_Xbox)  
  
-   [Rechercher et lire des fichiers multimédias à l’aide d’autres lecteurs multimédias numériques ou de récepteurs compatibles avec Windows Server Essentials](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_Other)  
  
-   [Rechercher et lire des fichiers multimédias à l’aide de la fonctionnalité dossiers partagés du Launchpad](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_SharedFolders)  
  
-   [Rechercher et lire des médias partagés à l’aide de l’Accès web à distance](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_RWA2)  

  
####  <a name="search-for-and-play-media-files-from-a-computer-that-is-running-windows-media-center"></a><a name="BKMK_WMC"></a>Rechercher et lire des fichiers multimédias à partir d’un ordinateur qui exécute Windows Media Center  
  
1.  Cliquez sur **Démarrer**, sur **Tous les programmes**, puis sur **Windows Media Center**.  
  
2.  Dans la page **Windows Media Center**, descendez jusqu'au type de média que vous recherchez, puis cliquez sur la bibliothèque multimédia.  
  
3.  Recherchez manuellement le fichier qui vous intéresse, ou cliquez sur **Recherche** et tapez le nom du fichier à rechercher.  
  
4.  Cliquez sur l'image du fichier multimédia pour afficher ou lire le fichier.  
  
####  <a name="search-for-and-play-media-files-from-a-computer-that-is-running-windows-by-using-windows-media-player"></a><a name="BKMK_MWP"></a>Rechercher et lire des fichiers multimédias à partir d’un ordinateur qui exécute Windows à l’aide du lecteur Windows Media  
  
-   À partir de l'ordinateur ou de l'appareil multimédia, ouvrez le **Lecteur Windows Media**, puis recherchez votre bibliothèque multimédia.  
  
    > [!NOTE]
    >  Les étapes de recherche varient suivant la version du Lecteur Windows Media que vous utilisez. Pour plus d'informations, consultez l'aide de votre version.  
  
####  <a name="search-for-and-play-media-files-by-using-xbox-360"></a><a name="BKMK_Xbox"></a>Rechercher et lire des fichiers multimédias à l’aide de Xbox 360  
  
1.  Connectez votre console Xbox 360 à votre réseau domestique à l'aide d'une connexion câblée ou sans fil.  
  
2.  Sur l’ordinateur qui exécute Windows Server Essentials, activez le partage de fichiers multimédias. Pour plus d’informations, consultez la rubrique [activer ou désactiver la diffusion multimédia en continu](../manage/Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_4).  
  
3.  Pour lire les fichiers multimédias à l'aide de la console Xbox 360 :  
  
    1.  Accédez à **Ma Xbox**, puis sélectionnez **Vidéothèque**, **Médiathèque**, ou **Bibliothèque d'images**, selon le type de média que vous souhaitez afficher ou lire.  
  
    2.  Sélectionnez le nom de votre serveur.  
  
        > [!NOTE]
        >  Si le nom de votre serveur n'est pas répertorié, sélectionnez **Ordinateur**, puis cliquez sur **Tester la connexion**.  
  
    3.  Parcourez la liste de fichiers et sélectionnez l'élément que vous souhaitez lire.  
  
####  <a name="search-for-and-play-media-files-by-using-other-digital-media-players-or-receivers-that-are-compatible-with-windows-server-essentials"></a><a name="BKMK_Other"></a>Rechercher et lire des fichiers multimédias à l’aide d’autres lecteurs multimédias numériques ou de récepteurs compatibles avec Windows Server Essentials  
  
1.  Accédez au [Centre de compatibilité Windows](https://www.microsoft.com/windows/compatibility/CompatCenter/Home) et assurez-vous que votre lecteur ou récepteur multimédia numérique apparaît dans la liste des appareils compatibles.  
  
2.  Étant donné que les étapes de recherche varient selon le lecteur multimédia que vous utilisez, consultez l'aide de votre appareil pour obtenir des instructions détaillées.  
  
####  <a name="search-for-and-play-media-files-by-using-the-shared-folders-feature-of-the-launchpad"></a><a name="BKMK_SharedFolders"></a>Rechercher et lire des fichiers multimédias à l’aide de la fonctionnalité dossiers partagés du Launchpad  
  
1.  Connectez-vous au Launchpad Windows Server Essentials.  
  
2.  Depuis le Launchpad, cliquez sur **Dossiers partagés**. Une fenêtre de l'Explorateur Windows s'ouvre et affiche les dossiers partagés sur le serveur.  
  
3.  Dans la zone **Recherche**, tapez le nom d'un fichier multimédia. Les résultats de l'objet de votre recherche sont affichés.  
  
    > [!NOTE]
    >  En option, vous pouvez également double-cliquer sur un dossier partagé pour parcourir le contenu du dossier.  
  
####  <a name="search-for-and-play-shared-media-by-using-remote-web-access"></a><a name="BKMK_RWA2"></a>Rechercher et lire des médias partagés à l’aide de l’Accès web à distance  
  
1.  Connectez-vous à l'accès web à distance.  
  
2.  Cliquez sur **Dossiers partagés**. La section **Dossiers partagés** de la page web affiche une liste des dossiers partagés sur le serveur.  
  
3.  Double-cliquez sur un dossier pour afficher le contenu de ce dossier.  
  
###  <a name="send-media-files-on-windows-server-essentials-to-windows-media-player-xbox-360-or-to-a-networked-digital-media-player-in-the-network"></a><a name="BKMK_SendToDevice"></a>Envoyer des fichiers multimédias sur Windows Server Essentials vers le lecteur Windows Media, Xbox 360 ou un lecteur multimédia numérique en réseau sur le réseau  
 Utilisez le **Lecteur Windows Media** pour rechercher le fichier multimédia que vous souhaitez. Cliquez avec le bouton droit sur le fichier multimédia, puis cliquez sur **Lire sur** pour envoyer le fichier multimédia à un appareil multimédia en réseau.  
  
##  <a name="play-shared-digital-media-files-from-a-remote-location"></a><a name="BKMK_3"></a>Lire des fichiers multimédias numériques partagés à partir d’un emplacement distant  
 Vous pouvez lire vos fichiers multimédias lorsque vous êtes hors de votre réseau Windows Server Essentials à l’aide d’Accès web à distance. Vous pouvez utiliser un téléphone portable, un ordinateur distant ou un lecteur multimédia numérique pour rechercher et lire les fichiers multimédias partagés stockés sur votre serveur.  
  
#### <a name="to-play-shared-media-files-when-you-are-away-from-the-network"></a>Pour lire les fichiers multimédias partagés lorsque vous êtes déconnecté du réseau  
  
1. Ouvrez un navigateur Internet.  
  
2. Accédez à votre site de l'accès web à distance. Tapez **https://< votrenomdedomaine\>/Remote** dans la barre d’adresses du navigateur Internet, puis appuyez sur entrée.  
  
   > [!NOTE]
   >  *< votrenomdedomaine\>* est un espace réservé. Il s’agit d’un nom qui est unique à votre serveur, donc l’adresse que vous tapez doit ressembler à **https://contoso.com/remote** . Si vous ne connaissez pas le nom de votre domaine, demandez à l'administrateur qui a choisi le nom de domaine lorsque la fonctionnalité d'accès à distance a été définie sur le serveur. Pour plus d'informations, voir [Activer l'accès web à distance](../manage/Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_TurnOnRWA).  
  
3. Dans la page de connexion de l'accès web à distance, tapez votre nom de compte d'utilisateur et un mot de passe, puis cliquez sur la flèche.  
  
4. Utilisez la méthode de votre choix pour rechercher le fichier multimédia que vous souhaitez lire.  
  
   > [!NOTE]
   > 
   >  Pour plus d’informations sur les différentes méthodes de recherche, consultez [Rechercher et lire des fichiers multimédias sur Windows Server Essentials à partir d’un ordinateur ou d’un lecteur multimédia numérique sur le réseau](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2.1).  
   > 
   >  Pour plus d’informations sur les différentes méthodes de recherche, consultez [Rechercher et lire des fichiers multimédias sur Windows Server Essentials à partir d’un ordinateur ou d’un lecteur multimédia numérique sur le réseau](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2.1).  

  
5. Lorsqu'il s'affiche, cliquez sur le nom du fichier multimédia pour le lire.  
  
##  <a name="add-digital-media-files-to-the-server"></a><a name="BKMK_4"></a>Ajouter des fichiers multimédias numériques au serveur  

 L’administrateur du serveur peut ajouter des médias numériques à des dossiers partagés dans la bibliothèque multimédia en accédant directement au serveur ou en utilisant le site de Accès web à distance pour se connecter au tableau de bord. D’autres utilisateurs peuvent ajouter des fichiers multimédias au serveur à l’aide de la connexion **dossiers partagés** sur le Launchpad, à l’aide du site de accès Web à distance ou à l’aide de l’application my server pour Windows Phone. Pour plus d'informations sur la lecture de fichiers multimédias, voir [Lire et partager les médias numériques](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2).  

 L’administrateur du serveur peut ajouter des médias numériques à des dossiers partagés dans la bibliothèque multimédia en accédant directement au serveur ou en utilisant le site de Accès web à distance pour se connecter au tableau de bord. D’autres utilisateurs peuvent ajouter des fichiers multimédias au serveur à l’aide de la connexion **dossiers partagés** sur le Launchpad, à l’aide du site de accès Web à distance ou à l’aide de l’application my server pour Windows Phone. Pour plus d'informations sur la lecture de fichiers multimédias, voir [Lire et partager les médias numériques](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2).  

  
> [!NOTE]
>  Vous pouvez également télécharger des fichiers multimédias sur le serveur à l'aide de l'application Mon Serveur pour Windows Phone. Vous pouvez télécharger l’application My Server à partir du [Windows Phone Store](http://www.windowsphone.com/store/app/my-server/6c2f98d5-6fcf-4e1d-b8b1-cde62ea1a94a). Pour plus d’informations sur l’application my Server pour Windows Phone, consultez le billet de blog [My Server Phone App for Windows Server Essentials](https://blogs.technet.com/b/sbs/archive/2012/09/18/my-server-phone-app-for-windows-server-2012-essentials.aspx).  
  
#### <a name="to-add-digital-media-files-to-shared-folders-on-the-server"></a>Pour ajouter des fichiers multimédias numériques à des dossiers partagés sur le serveur  
  
1.  Utilisez une des méthodes suivantes pour vous connecter au serveur :  
  

    1.  Pour plus d’informations sur la connexion à des Accès web distantes, consultez [utiliser des accès Web distantes](Use-Remote-Web-Access-in-Windows-Server-Essentials.md).  

    1.  Pour plus d’informations sur la connexion à des Accès web distantes, consultez [utiliser des accès Web distantes](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md).  

  
    2.  Pour plus d’informations sur la connexion avec Launchpad, consultez la [vue d’ensemble de Launchpad](../manage/Overview-of-the-Launchpad-in-Windows-Server-Essentials.md).  
  
2.  Recherchez et cliquez sur le dossier pour le média que vous ajoutez.  
  
3.  Copiez et collez ou glissez-déplacez les fichiers multimédias que vous souhaitez ajouter au dossier partagé approprié sur le serveur.  
  
##  <a name="download-format-options"></a><a name="BKMK_5"></a>Télécharger les options de format  
 Il existe deux options pour le téléchargement des fichiers. Ces options sont disponibles uniquement lorsque vous téléchargez plusieurs fichiers ou un dossier sur un ordinateur Windows.  
  
 Choisissez l'option qui correspond à vos besoins pour les téléchargements :  
  
- **Fichier ZIP compressé (. zip)**  
  
   La compression d'un fichier crée une version compressée du fichier qui est plus petite que le fichier d'origine. La version du fichier compressée a une extension de nom de fichier .zip. Les types de fichiers dont la taille se réduit le plus à la compression sont les fichiers à contenu textuel (comme les fichiers .txt, .doc et .xls) et les fichiers graphiques qui utilisent des types de fichier non compressés (comme .bmp). Certains fichiers graphiques, tels que les fichiers .jpg et .gif, utilisent déjà la compression et la taille du fichier est très peu réduite par la compression. Ainsi, la taille d'un document Word qui contient un grand nombre de graphiques n'est pas autant réduite que celle d'un document qui contient principalement du texte.  
  
  > [!NOTE]
  >  Cette option fournit une prise en charge limitée des noms de fichier internationaux.  
  
- **Fichier exécutable à extraction automatique (. exe)**  
  
   Un fichier exécutable à extraction automatique est un fichier que vous pouvez télécharger qui associe le programme (exécutable) de décompression et les fichiers compressés. Lorsque vous exécutez le programme exécutable, il décompresse automatiquement les fichiers compressés. Il s'agit d'une méthode courante de distribution des données compressées qui permet de ne pas se préoccuper de savoir si le destinataire dispose de l'utilitaire de décompression approprié.  
  
  > [!NOTE]
  >  Cette option prend en charge les caractères Unicode.  
  
  Avant le début de téléchargement, le fichier .exe ou .zip est créé. En fonction du nombre de fichiers et de la taille totale des fichiers à télécharger, cela peut prendre plusieurs minutes. Une fois le fichier de téléchargement créé, le téléchargement du fichier se produit en arrière-plan. Cela vous permet de continuer à travailler pendant le processus de téléchargement.  
  
##  <a name="easy-file-upload-tool"></a><a name="BKMK_6"></a>Outil Easy file upload  
 L’outil Easy file upload rationalise le processus de chargement des fichiers sur votre serveur Windows Server Essentials. Vous pouvez ajouter autant de fichiers que vous le souhaitez à l’outil de téléchargement de fichiers facile, puis les charger dans des dossiers partagés sur le serveur Windows Server Essentials en un seul lot. Pour plus d’informations, consultez le billet de blog [Présentation du partage de fichiers d’accès web à distance](https://blogs.technet.com/b/sbs/archive/2012/04/19/understanding-remote-web-access-file-sharing.aspx).  
  
##  <a name="view-and-browse-shared-digital-media"></a><a name="BKMK_7"></a>Afficher et parcourir des médias numériques partagés  
 Vous pouvez afficher ou parcourir les ressources à l'aide du tableau de bord, du Launchpad, du site de l'accès web à distance ou de l'application Mon Serveur pour Windows Phone.  
  
#### <a name="to-view-and-browse-shared-media-from-the-dashboard"></a>Pour afficher et consulter les médias partagés à partir du tableau de bord  
  
1.  Ouvrez le tableau de bord du serveur.  
  
2.  Dans la barre de navigation, cliquez sur **Stockage**.  
  
3.  Cliquez sur l’onglet **dossiers du serveur** . Une liste de dossiers de serveur s’affiche.  
  
4.  Double-cliquez sur un dossier pour afficher le contenu de ce dossier.  
  
#### <a name="to-view-and-browse-shared-media-using-the-launchpad-from-a-network-computer"></a>Pour afficher et consulter les médias partagés à l'aide du Launchpad à partir d'un ordinateur réseau  
  
1.  Connectez-vous au Launchpad.  
  
2.  Cliquez sur **Dossiers partagés**. L'Explorateur Windows s'ouvre et affiche une liste des dossiers partagés sur le serveur.  
  
3.  Double-cliquez sur un dossier pour afficher le contenu de ce dossier.  
  
#### <a name="to-view-and-browse-shared-media-using-remote-web-access"></a>Pour afficher et consulter les médias partagés à l'aide de l'accès web à distance  
  
1.  Connectez-vous à l'accès web à distance.  
  
2.  Cliquez sur **Dossiers partagés**. La section **Dossiers partagés** de la page web affiche une liste des dossiers partagés sur le serveur.  
  
3.  Double-cliquez sur un dossier pour afficher le contenu de ce dossier.  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Gérer des médias numériques](../manage/Manage-Digital-Media-in-Windows-Server-Essentials.md)  
  

-   [Connectez-vous](Get-Connected-in-Windows-Server-Essentials.md)  
  
-   [Utiliser des dossiers partagés](Use-Shared-Folders-in-Windows-Server-Essentials.md)  
  
-   [Travailler à distance](Work-Remotely-in-Windows-Server-Essentials.md)

-   [Connectez-vous](../use/Get-Connected-in-Windows-Server-Essentials.md)  
  
-   [Utiliser des dossiers partagés](../use/Use-Shared-Folders-in-Windows-Server-Essentials.md)  
  
-   [Travailler à distance](../use/Work-Remotely-in-Windows-Server-Essentials.md)

