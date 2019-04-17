---
title: "Lire des médias numériques dans WindowsServerEssentials"
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5f570492-ee21-471b-92c1-3fd9bfb84f55
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 8228d0b17a58858ed893181ddceb465715ffdeb5
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="play-digital-media-in-windows-server-essentials"></a>Lire des médias numériques dans WindowsServerEssentials

>S’applique à: Windows Server2012R2 Essentials, Windows Server2012Essentials

Multimédias numériques font référence à audio, vidéo et contenu photo qui ont été compressé numériquement.  WindowsServerEssentials permet aux ordinateurs et certains appareils multimédias numériques lire des fichiers multimédias numériques qui sont stockés sur le serveur en réseau.  
  
 Les rubriques suivantes fournissent des informations sur l’accès et la lecture des fichiers multimédias numériques qui sont stockés sur WindowsServerEssentials:  
  

-   [Vue d’ensemble des médias numériques](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Lire et partager des médias numériques](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Lire des fichiers multimédias numériques partagés à partir d’un emplacement distant](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Ajouter des fichiers multimédias numériques sur le serveur](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [Options de format de téléchargement](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [OUTIL téléchargement facile](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_6)  
  
-   [Afficher et parcourir les supports multimédias numériques partagés](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_7)  

-   [Vue d’ensemble des médias numériques](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Lire et partager des médias numériques](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Lire des fichiers multimédias numériques partagés à partir d’un emplacement distant](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Ajouter des fichiers multimédias numériques sur le serveur](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [Options de format de téléchargement](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [OUTIL téléchargement facile](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_6)  
  
-   [Afficher et parcourir les supports multimédias numériques partagés](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_7)  

  
##  <a name="BKMK_1"></a>Vue d’ensemble des médias numériques  
 Multimédias numériques font référence à audio, vidéo et contenu photo qui ont été encodé (compressés numériquement). Le codage du contenu implique la conversion d’une entrée audio et vidéo dans un fichier multimédia numérique, tel qu’un fichier WindowsMedia. Une fois le média numérique codé, il peut être facilement manipulé, distribué et lu par des ordinateurs et facilement transmis sur des réseaux d’ordinateurs.  
  
 Exemples de types de médias numériques: WindowsMediaAudio (WMA), vidéo WindowsMedia (WMV), MP3, JPEG et AVI. Pour plus d’informations sur les types de médias numériques qui sont pris en charge par le lecteur WindowsMedia, consultez [pris en charge par le lecteur WindowsMedia de types de fichiers](https://support.microsoft.com/kb/316992).  
  
### <a name="why-would-i-want-to-stream-my-digital-media"></a>Pourquoi voudrais-je diffuser en continu mes médias numériques?  
 Nombreuses personnes stockent de musique, vidéos et des images dans des dossiers partagés dans WindowsServerEssentials. Il peut arriver lorsque vous souhaitez effectuer suivant:  
  
-   **Regarder des vidéos**. Votre serveur peut être utilisé pour stocker et diffuser de grandes collections de vidéos et TV enregistrée affiche sur vos ordinateurs ou d’autres périphériques sur votre réseau. Vous pouvez diffuser des vidéos sur une Xbox 360 ou sur un ordinateur à l’aide du lecteur WindowsMedia.  
  
-   **Écouter de la musique**. Lorsque vous activez le partage de médias pour la **musique** dossier partagé, vous pouvez accéder à votre musique à partir d’appareils qui prennent en charge de WindowsMediaConnect. Vous n’avez pas besoin activer ou de configurer les comptes d’utilisateurs en continu à partir du **musique** dossier partagé une fois que le partage est activé.  
  
-   **Présenter des diaporamas de photos**. Vous pouvez stocker des photos numériques dans le **Photos** dossier sur votre serveur partagé et y accéder à partir de n’importe quel ordinateur ou d’une Xbox 360 connectée à une télévision à votre domicile ou au bureau. Vous pouvez regarder des diaporamas de photos, ce qui revient à transformer votre télévision en un grand cadre à image.  
  
### <a name="sharing-copy-protected-media"></a>Partage de médias protégés contre la copie  
  WindowsServerEssentials ne prend pas en charge le partage de médias protégés contre la copie. Cela inclut la musique achetée auprès d’un magasin de musique en ligne.  
  
 Support Copy-protected peut être lues uniquement sur l’ordinateur ou le périphérique que vous avez utilisé pour l’acheter. Protection contre la copie vous empêche de la lecture de médias sur plusieurs ordinateurs ou appareils, même si vous copiez le média sur votre serveur et le lire à partir de là. Toutefois, vous pouvez stocker le média protégé contre la copie sur WindowsServerEssentials et continuer à lire ce média sur l’ordinateur ou le périphérique que vous avez utilisé pour l’acheter.  
  
##  <a name="BKMK_2"></a>Lire et partager des médias numériques  
 Après avoir configuré votre réseau et connecter vos ordinateurs et les appareils multimédias au réseau du serveur, vous pouvez rechercher des fichiers multimédias numériques que vous stockez et partagez sur le serveur.  
  
> [!NOTE]
>  Pour obtenir la liste actuelle des appareils/récepteur multimédia numérique qui sont compatibles avec WindowsServerEssentials, consultez le [centre de compatibilité Windows](https://www.microsoft.com/windows/compatibility/CompatCenter/Home).  
  
 Vous pouvez utiliser une des méthodes suivantes pour rechercher et lire des fichiers multimédias numériques qui sont stockés sur votre serveur:  
  

-   [Rechercher et lire des fichiers multimédias sur WindowsServerEssentials à partir d’un ordinateur ou un lecteur multimédia numérique sur le réseau](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2.1)  
  
-   [Envoyer des fichiers multimédias sur WindowsServerEssentials au lecteur WindowsMedia, Xbox 360, ou à un lecteur multimédia numérique en réseau dans le réseau](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_SendToDevice)  

-   [Rechercher et lire des fichiers multimédias sur WindowsServerEssentials à partir d’un ordinateur ou un lecteur multimédia numérique sur le réseau](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2.1)  
  
-   [Envoyer des fichiers multimédias sur WindowsServerEssentials au lecteur WindowsMedia, Xbox 360, ou à un lecteur multimédia numérique en réseau dans le réseau](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_SendToDevice)  

  
###  <a name="BKMK_2.1"></a>Rechercher et lire des fichiers multimédias sur WindowsServerEssentials à partir d’un ordinateur ou un lecteur multimédia numérique sur le réseau  
 Lorsque votre appareil est joint au réseau WindowsServerEssentials, vous pouvez rechercher et lire des fichiers multimédias numériques d’une des manières suivantes:  
  

-   [Rechercher et lire des fichiers multimédias à partir d’un ordinateur qui exécute WindowsMediaCenter](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_WMC)  
  
-   [Rechercher et lire des fichiers multimédias à partir d’un ordinateur qui exécute Windows à l’aide du lecteur WindowsMedia](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_MWP)  
  
-   [Rechercher et lire des fichiers multimédias à l’aide de Xbox 360](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_Xbox)  
  
-   [Rechercher et lire des fichiers multimédias à l’aide d’autres lecteurs multimédias numériques ou les récepteurs compatibles avec WindowsServerEssentials](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_Other)  
  
-   [Rechercher et lire des fichiers multimédias à l’aide de la fonctionnalité dossiers partagés du Launchpad](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_SharedFolders)  
  
-   [Rechercher et lire des médias partagés à l’aide de l’accès Web à distance](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_RWA2)  

-   [Rechercher et lire des fichiers multimédias à partir d’un ordinateur qui exécute WindowsMediaCenter](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_WMC)  
  
-   [Rechercher et lire des fichiers multimédias à partir d’un ordinateur qui exécute Windows à l’aide du lecteur WindowsMedia](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_MWP)  
  
-   [Rechercher et lire des fichiers multimédias à l’aide de Xbox 360](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_Xbox)  
  
-   [Rechercher et lire des fichiers multimédias à l’aide d’autres lecteurs multimédias numériques ou les récepteurs compatibles avec WindowsServerEssentials](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_Other)  
  
-   [Rechercher et lire des fichiers multimédias à l’aide de la fonctionnalité dossiers partagés du Launchpad](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_SharedFolders)  
  
-   [Rechercher et lire des médias partagés à l’aide de l’accès Web à distance](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_RWA2)  

  
####  <a name="BKMK_WMC"></a>Rechercher et lire des fichiers multimédias à partir d’un ordinateur qui exécute WindowsMediaCenter  
  
1.  Cliquez sur **Démarrer**, cliquez sur **tous les programmes**, puis cliquez sur **WindowsMediaCenter**.  
  
2.  Sur le **WindowsMediaCenter** page, faites défiler vers le type de média que vous recherchez et cliquez sur la bibliothèque multimédia.  
  
3.  Recherchez manuellement le fichier qui vous intéressez, ou cliquez sur **recherche** et tapez le nom du fichier que vous voulez trouver.  
  
4.  Cliquez sur l’image du fichier multimédia pour afficher ou lire le fichier.  
  
####  <a name="BKMK_MWP"></a>Rechercher et lire des fichiers multimédias à partir d’un ordinateur qui exécute Windows à l’aide du lecteur WindowsMedia  
  
-   À partir de l’ordinateur ou appareil multimédia, ouvrez **le lecteur WindowsMedia** et recherchez votre bibliothèque multimédia.  
  
    > [!NOTE]
    >  Les étapes de recherche varient selon la version du lecteur WindowsMedia que vous utilisez. Pour plus d’informations, consultez l’aide de votre version.  
  
####  <a name="BKMK_Xbox"></a>Rechercher et lire des fichiers multimédias à l’aide de Xbox 360  
  
1.  Connectez votre console Xbox 360 à votre réseau domestique à l’aide d’une connexion filaire ou sans fil.  
  
2.  Sur l’ordinateur qui exécute WindowsServerEssentials, activez le partage de médias. Pour plus d’informations, voir la rubrique [activer ou désactiver la diffusion multimédia](../manage/Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_4).  
  
3.  Pour lire des fichiers multimédias numériques à l’aide de votre console Xbox 360:  
  
    1.  Accédez à **ma Xbox**, puis sélectionnez **Vidéothèque**, **médiathèque**, ou **bibliothèque d’images**, selon le type de média que vous souhaitez afficher ou lire.  
  
    2.  Sélectionnez le nom de votre serveur.  
  
        > [!NOTE]
        >  Si le nom de votre serveur n’est pas répertorié, sélectionnez **ordinateur**, puis cliquez sur **tester la connexion**.  
  
    3.  Parcourir la liste de fichiers et sélectionnez l’élément que vous voulez lire.  
  
####  <a name="BKMK_Other"></a>Rechercher et lire des fichiers multimédias à l’aide d’autres lecteurs multimédias numériques ou les récepteurs compatibles avec WindowsServerEssentials  
  
1.  Accédez à la [centre de compatibilité Windows](https://www.microsoft.com/windows/compatibility/CompatCenter/Home) et assurez-vous que votre lecteur multimédia numérique ou un récepteur apparaît dans la liste des appareils compatibles.  
  
2.  Étant donné que les étapes de recherche varient selon le lecteur multimédia numérique que vous utilisez, consultez l’aide de votre appareil pour obtenir des instructions détaillées.  
  
####  <a name="BKMK_SharedFolders"></a>Rechercher et lire des fichiers multimédias à l’aide de la fonctionnalité dossiers partagés du Launchpad  
  
1.  Connectez-vous au Launchpad de WindowsServerEssentials.  
  
2.  À partir du Launchpad, cliquez sur **dossiers partagés**. Une fenêtre de l’Explorateur Windows s’ouvre et affiche les dossiers partagés sur le serveur.  
  
3.  Dans le **recherche**, tapez le nom d’un fichier multimédia. Les résultats de votre requête de recherche sont affichés.  
  
    > [!NOTE]
    >  En option, vous pouvez également double-cliquer sur un dossier partagé pour parcourir le contenu du dossier.  
  
####  <a name="BKMK_RWA2"></a>Rechercher et lire des médias partagés à l’aide de l’accès Web à distance  
  
1.  Ouvrez une session sur l’accès Web à distance.  
  
2.  Cliquez sur **dossiers partagés**. Le **dossiers partagés** section de la page web affiche une liste des dossiers partagés sur le serveur.  
  
3.  Double-cliquez sur un dossier pour afficher le contenu du dossier.  
  
###  <a name="BKMK_SendToDevice"></a>Envoyer des fichiers multimédias sur WindowsServerEssentials au lecteur WindowsMedia, Xbox 360, ou à un lecteur multimédia numérique en réseau dans le réseau  
 Utilisez **le lecteur WindowsMedia** pour rechercher le fichier multimédia que vous souhaitez. Cliquez sur le fichier multimédia, puis cliquez sur **lire sur** pour envoyer le fichier multimédia à un appareil multimédia en réseau.  
  
##  <a name="BKMK_3"></a>Lire des fichiers multimédias numériques partagés à partir d’un emplacement distant  
 Vous pouvez lire vos fichiers multimédias lorsque vous n’êtes pas votre réseau WindowsServerEssentials à l’aide de l’accès Web à distance. Vous pouvez utiliser un téléphone portable, un ordinateur distant ou un lecteur multimédia numérique pour rechercher et lire les fichiers multimédias partagés stockés sur votre serveur.  
  
#### <a name="to-play-shared-media-files-when-you-are-away-from-the-network"></a>Pour lire des fichiers multimédias partagés lorsque vous êtes déconnecté du réseau  
  
1.  Ouvrez un navigateur Internet.  
  
2.  Accédez à votre site Web accès Web à distance. Type **https://<YourDomainName\>/remote** dans la barre d’adresses du navigateur Internet, puis appuyez sur ENTRÉE.  
  
    > [!NOTE]
    >  *< yourDomainName\ >* est un espace réservé. Il sera un nom unique à votre serveur, afin de l’adresse que vous tapez ressemblera **https://contoso.com/remote**. Si vous ne connaissez pas le nom de votre domaine, demandez à l’administrateur qui a choisi le nom de domaine lors de la fonctionnalité d’accès à distance a été définie sur le serveur. Pour plus d’informations, voir [Turn on Remote Web Access](../manage/Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_TurnOnRWA).  
  
3.  Sur la page de connexion accès Web à distance, tapez votre nom de compte d’utilisateur et un mot de passe, puis cliquez sur la flèche.  
  
4.  Utilisez la méthode de votre choix pour rechercher le fichier multimédia que vous voulez lire.  
  
    > [!NOTE]

    >  Pour plus d’informations sur les différentes méthodes de recherche, voir [rechercher et lire des fichiers multimédias sur WindowsServerEssentials à partir d’un ordinateur ou un lecteur multimédia numérique sur le réseau](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2.1).  

    >  Pour plus d’informations sur les différentes méthodes de recherche, voir [rechercher et lire des fichiers multimédias sur WindowsServerEssentials à partir d’un ordinateur ou un lecteur multimédia numérique sur le réseau](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2.1).  

  
5.  Lorsque le nom de fichier s’affiche, cliquez sur le nom de fichier pour lire le média.  
  
##  <a name="BKMK_4"></a>Ajouter des fichiers multimédias numériques sur le serveur  

 L’administrateur du serveur peut ajouter des médias numériques à des dossiers partagés dans la bibliothèque multimédia, en accédant directement au serveur, ou en utilisant le site accès Web à distance pour se connecter au tableau de bord. Autres utilisateurs peuvent ajouter des fichiers multimédias au serveur à l’aide de la **dossiers partagés** connexion sur le Launchpad, à l’aide du site accès Web à distance, ou à l’aide de l’application mon serveur pour Windows Phone. Pour plus d’informations sur la lecture de médias, consultez [lire et partager des médias numériques](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2).  

 L’administrateur du serveur peut ajouter des médias numériques à des dossiers partagés dans la bibliothèque multimédia, en accédant directement au serveur, ou en utilisant le site accès Web à distance pour se connecter au tableau de bord. Autres utilisateurs peuvent ajouter des fichiers multimédias au serveur à l’aide de la **dossiers partagés** connexion sur le Launchpad, à l’aide du site accès Web à distance, ou à l’aide de l’application mon serveur pour Windows Phone. Pour plus d’informations sur la lecture de médias, consultez [lire et partager des médias numériques](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2).  

  
> [!NOTE]
>  Vous pouvez également télécharger des fichiers multimédias au serveur à l’aide de l’application mon serveur pour Windows Phone. Vous pouvez télécharger l’application mon serveur à partir de la [store Windows Phone](http://www.windowsphone.com/store/app/my-server/6c2f98d5-6fcf-4e1d-b8b1-cde62ea1a94a). Pour plus d’informations sur l’application mon serveur pour Windows Phone, consultez le blog [application mon serveur pour WindowsServerEssentials](http://blogs.technet.com/b/sbs/archive/2012/09/18/my-server-phone-app-for-windows-server-2012-essentials.aspx).  
  
#### <a name="to-add-digital-media-files-to-shared-folders-on-the-server"></a>Pour ajouter des fichiers multimédias numériques à des dossiers partagés sur le serveur  
  
1.  Utilisez une des méthodes suivantes pour vous connecter au serveur:  
  

    1.  Pour plus d’informations sur la connexion accès Web à distance, consultez [Use Remote Web Access](Use-Remote-Web-Access-in-Windows-Server-Essentials.md).  

    1.  Pour plus d’informations sur la connexion accès Web à distance, consultez [Use Remote Web Access](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md).  

  
    2.  Pour plus d’informations sur la journalisation du Launchpad, consultez [vue d’ensemble de Launchpad](../manage/Overview-of-the-Launchpad-in-Windows-Server-Essentials.md).  
  
2.  Recherchez et cliquez sur le dossier pour le média que vous ajoutez.  
  
3.  Copiez et collez ou glisser-déplacer les fichiers multimédias que vous souhaitez ajouter au dossier sur le serveur.  
  
##  <a name="BKMK_5"></a>Options de format de téléchargement  
 Il existe deux options pour le téléchargement des fichiers. Ces options sont disponibles uniquement lorsque vous téléchargez plusieurs fichiers ou un dossier sur un ordinateur basé sur Windows.  
  
 Choisissez l’option qui correspond à vos besoins pour les téléchargements:  
  
-   **Compressé ZIP fichier (.zip)**  
  
     Compression d’un fichier crée une version compressée du fichier qui est plus petit que le fichier d’origine. La version du fichier zippée a une extension de nom de fichier .zip. Types de fichiers qui sont réduites le plus à la compression sont les types de fichier textuel (par exemple, les fichiers .txt, .doc et .xls) et les fichiers de graphiques qui utilisent des types de fichier non compressés (comme .bmp). Certains fichiers graphiques, tels que les fichiers .jpg et .gif, utilisent déjà la compression et la taille du fichier est réduit très peu à la compression. En outre, un document Word qui contient un grand nombre de graphiques n’est pas réduit autant qu’un document principalement du texte.  
  
    > [!NOTE]
    >  Cette option fournit la prise en charge limitée des noms de fichier internationaux.  
  
-   **Fichier exécutable à extraction automatique (.exe)**  
  
     Un fichier exécutable à extraction automatique est un fichier que vous pouvez télécharger qui associe le programme (exécutable) de décompression des fichiers compressés. Lorsque vous exécutez le programme exécutable, il décompresse automatiquement les fichiers compressés. Il s’agit d’une méthode courante de distribution des données compressées sans tenir compte si le destinataire dispose de l’utilitaire de décompression approprié.  
  
    > [!NOTE]
    >  Cette option prend en charge les caractères Unicode.  
  
 Avant le début de téléchargement, le fichier .exe ou .zip est créé. Selon le nombre de fichiers et la taille totale des fichiers à télécharger, cela peut prendre plusieurs minutes. Une fois le téléchargement de fichier est créé, téléchargez le fichier se produit en arrière-plan. Cela vous permet de continuer à travailler pendant le processus de téléchargement se termine.  
  
##  <a name="BKMK_6"></a>OUTIL téléchargement facile  
 L’outil téléchargement facile simplifie le processus de téléchargement des fichiers sur votre serveur WindowsServerEssentials. Vous pouvez ajouter autant de fichiers que vous le souhaitez à l’outil de téléchargement simplifié de fichiers et les téléchargez dans les dossiers partagés sur le serveur WindowsServerEssentials dans un seul lot. Pour plus d’informations, consultez le blog [présentation à distance accès partage de fichiers Web](http://blogs.technet.com/b/sbs/archive/2012/04/19/understanding-remote-web-access-file-sharing.aspx).  
  
##  <a name="BKMK_7"></a>Afficher et parcourir les supports multimédias numériques partagés  
 Vous pouvez afficher ou parcourir les ressources à l’aide du tableau de bord, la zone de lancement, le site Web accès Web à distance ou l’application mon serveur pour Windows Phone.  
  
#### <a name="to-view-and-browse-shared-media-from-the-dashboard"></a>Pour afficher et consulter les médias partagés à partir du tableau de bord  
  
1.  Ouvrez le tableau de bord du serveur.  
  
2.  Dans la barre de navigation, cliquez sur **stockage**.  
  
3.  Cliquez sur le **dossiers du serveur** onglet. Une liste des dossiers du serveur s’affiche.  
  
4.  Double-cliquez sur un dossier pour afficher le contenu du dossier.  
  
#### <a name="to-view-and-browse-shared-media-using-the-launchpad-from-a-network-computer"></a>Pour afficher et consulter les médias partagés à l’aide du Launchpad à partir d’un ordinateur réseau  
  
1.  Connectez-vous au Launchpad.  
  
2.  Cliquez sur **dossiers partagés**. L’Explorateur Windows s’ouvre et affiche une liste des dossiers partagés sur le serveur.  
  
3.  Double-cliquez sur un dossier pour afficher le contenu du dossier.  
  
#### <a name="to-view-and-browse-shared-media-using-remote-web-access"></a>Pour afficher et consulter les médias partagés à l’aide d’accès Web à distance  
  
1.  Ouvrez une session sur l’accès Web à distance.  
  
2.  Cliquez sur **dossiers partagés**. Le **dossiers partagés** section de la page web affiche une liste des dossiers partagés sur le serveur.  
  
3.  Double-cliquez sur un dossier pour afficher le contenu du dossier.  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Gérer des médias numériques](../manage/Manage-Digital-Media-in-Windows-Server-Essentials.md)  
  

-   [Se connecter](Get-Connected-in-Windows-Server-Essentials.md)  
  
-   [Utiliser des dossiers partagés](Use-Shared-Folders-in-Windows-Server-Essentials.md)  
  
-   [Travailler à distance](Work-Remotely-in-Windows-Server-Essentials.md)

-   [Se connecter](../use/Get-Connected-in-Windows-Server-Essentials.md)  
  
-   [Utiliser des dossiers partagés](../use/Use-Shared-Folders-in-Windows-Server-Essentials.md)  
  
-   [Travailler à distance](../use/Work-Remotely-in-Windows-Server-Essentials.md)

