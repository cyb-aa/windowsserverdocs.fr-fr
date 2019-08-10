---
title: Utiliser l'accès web à distance dans Windows Server Essentials
description: Décrit comment utiliser Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 47ea21a0-5e05-4b4b-8fa4-338c82601276
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: f6a5d6fd42c5cd7e92821e1157748054c741ef04
ms.sourcegitcommit: 0e3c2473a54f915d35687d30d1b4b1ac2bae4068
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68914684"
---
# <a name="use-remote-web-access-in-windows-server-essentials"></a>Utiliser l'accès web à distance dans Windows Server Essentials

>S'applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
  
  Les Accès web distantes sont une fonctionnalité de Windows Servers Essentials qui vous permet d’accéder à des fichiers/dossiers et des ordinateurs sur votre réseau via un navigateur Web, où que vous soyez. 
  
  L'accès web à distance vous permet de rester connecté à votre ordinateur Windows Server Essentials réseau lorsque vous êtes absent. Quand vous vous connectez à un Accès web distant, vous pouvez vous connecter aux ordinateurs de votre réseau Windows Server Essentials, ouvrir le tableau de bord pour gérer votre réseau Windows Server Essentials et accéder à tous les dossiers partagés et fichiers multimédias sur le serveur.  
  
 Cette rubrique contient les sections suivantes :  
  

-   [Se connecter à un Accès web distant](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Connect)  
  
-   [Partager des fichiers et des dossiers](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SharedFolders)  
  
-   [Se connecter à partir d’un appareil mobile](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ConnectMobile)  
  
##  <a name="BKMK_Connect"></a>Se connecter à un Accès web distant  
  
-   [Connexion à un Accès web distant](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Accéder à distance à votre ordinateur](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1.5)  

-   [Se connecter à un Accès web distant](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Connect)  
  
-   [Partager des fichiers et des dossiers](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SharedFolders)  
  
-   [Se connecter à partir d’un appareil mobile](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ConnectMobile)  
  
##  <a name="BKMK_Connect"></a>Se connecter à un Accès web distant  
  
-   [Connexion à un Accès web distant](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Accéder à distance à votre ordinateur](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1.5)  

  
###  <a name="BKMK_1"></a>Connexion à un Accès web distant  
 Lorsque vous vous connectez à une Accès web distante à partir d’un ordinateur local ou distant, vous pouvez accéder aux ressources de votre serveur exécutant Windows Server Essentials et des ordinateurs sur votre réseau.  
  
##### <a name="to-log-on-to-remote-web-access-from-a-network-computer"></a>Pour se connecter à l'accès web à distance à partir d'un ordinateur réseau  
  
1.  Ouvrez un navigateur Web, tapez **https://** _< yourservername\>_ **/Remote** dans la barre d’adresses, puis appuyez sur entrée.  
  
    > [!NOTE]
    >  Veillez à inclure les s dans https.  
  
2.  Dans la page d’ouverture de session Accès web à distance, tapez votre nom d’utilisateur et votre mot de passe dans les zones de texte, puis cliquez sur la flèche.  
  
##### <a name="to-log-on-to-remote-web-access-from-a-remote-computer"></a>Pour se connecter à l'accès web à distance à partir d'un ordinateur à distance  
  
1.  Ouvrez un navigateur Web, tapez **https://** _< VotreNomdeDomaine\>_ **/Remote** dans la barre d’adresses, puis appuyez sur entrée.  
  
    > [!NOTE]
    >  Contactez votre administrateur réseau pour obtenir les informations relatives à votre nom de domaine. Veillez à inclure les s dans https.  
  
2.  Dans la page d’ouverture de session Accès web à distance, tapez votre nom d’utilisateur et votre mot de passe dans les zones de texte, puis cliquez sur la flèche.  
  
###  <a name="BKMK_1.5"></a>Accéder à distance à votre ordinateur  
 Lorsque vous n’êtes pas dans votre bureau, vous pouvez utiliser votre navigateur Web pour ouvrir une session sur le site Accès web distant pour accéder à distance au tableau de bord Windows Server Essentials, aux dossiers partagés et aux ordinateurs de votre réseau.  
  
 Lorsque vous vous connectez au tableau de bord, vous pouvez gérer Windows Server Essentials comme vous le feriez si vous étiez au bureau. Vous pouvez effectuer toutes les tâches d'administration habituelles, comme ajouter des comptes d'utilisateurs, ajouter des dossiers partagés, définir l'accès aux dossiers partagés, etc. Lorsque vous vous connectez aux ordinateurs de votre réseau, vous pouvez accéder à leurs bureaux comme si vous étiez physiquement en face d'eux au bureau.  
  
 La colonne **État** vous indique si vous pouvez vous connecter à un ordinateur de votre réseau et peut comprendre inclure les valeurs suivantes :  
  
-   **Disponible**  
  
     L'ordinateur est allumé et est disponible pour une connexion à distance. Même si vous voyez cet état, il se peut que vous ne soyez pas en mesure de vous connecter à cet ordinateur, si un pare-feu tiers bloque la connexion.  
  
-   **Hors connexion ou en veille**  
  
     L'ordinateur est éteint ou est en mode Veille ou Veille prolongée. Si un ordinateur est hors connexion ou en veille, l'état est mis à jour en temps réel afin que vous puissiez savoir quand l'ordinateur est disponible.  
  
-   **Système d’exploitation non pris en charge**  
  
     Le système d'exploitation de l'ordinateur ne prend pas en charge les services Bureau à distance. La mise à jour de cet état sur le serveur peut prendre jusqu'à 6 heures en cas de modifications.  
  
-   **La connexion est désactivée**  
  
     La connexion de l'ordinateur est bloquée par un pare-feu ou le Bureau à distance est désactivé sur l'ordinateur ou par la stratégie de groupe. La mise à jour de cet état sur le serveur peut prendre jusqu'à 6 heures en cas de modifications.  
  
#### <a name="to-connect-to-a-computer-on-your-network"></a>Pour se connecter à un ordinateur de votre réseau  
 Sous l'onglet **PÉRIPHÉRIQUES**, cliquez sur le nom de l'ordinateur. Vous ne pouvez sélectionner que des ordinateurs avec un état **Disponible**.  
  
#### <a name="to-connect-to-the-server-dashboard"></a>Pour se connecter au tableau de bord du serveur  
 Sous l'onglet **PÉRIPHÉRIQUES**, cliquez sur le nom de votre serveur. Vous ne pouvez sélectionner que des ordinateurs avec un état **Disponible**. Vous devez être en mesure de fournir un compte d'utilisateur et un mot de passe d'administrateur sur votre serveur pour utiliser le tableau de bord.  
  
##  <a name="BKMK_SharedFolders"></a>Partager des fichiers et des dossiers  
  

-   [Charger et télécharger des fichiers dans des Accès web distantes](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_UploadRWA)  
  
-   [Créer, renommer, déplacer, supprimer ou copier des fichiers et des dossiers dans Accès web à distance](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_2)  

-   [Charger et télécharger des fichiers dans des Accès web distantes](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_UploadRWA)  
  
-   [Créer, renommer, déplacer, supprimer ou copier des fichiers et des dossiers dans Accès web à distance](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_2)  

  
###  <a name="BKMK_UploadRWA"></a>Charger et télécharger des fichiers dans des Accès web distantes  
 Sous l'onglet **Dossiers partagés** de l'accès web à distance, vous pouvez procéder comme suit :  
  
-   Téléchargez (envoyez) des fichiers de votre ordinateur vers Windows Server Essentials.  
  
    > [!NOTE]
    >  Vous ne pouvez télécharger que des fichiers, pas des dossiers, vers l'accès web à distance. Si vous souhaitez conserver la même structure de fichiers et de dossiers dans les **Dossiers partagés** sur le serveur que sur votre ordinateur, vous devez créer les dossiers sur le serveur dans l'accès web à distance et puis télécharger les fichiers vers le dossier que vous avez créé. Pour plus d’informations sur la création de dossiers serveur, consultez [Add or move a server folder](../manage/Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_5).  
  
-   Téléchargez (recevez) des fichiers et des dossiers de Windows Server Essentials sur votre ordinateur.  
  
-   Créez un dossier dans un dossier partagé sur Windows Server Essentials.  
  

-   Déplacez, supprimez et renommez des fichiers et des dossiers sur Windows Server Essentials. Pour plus d’informations, consultez [créer, renommer, déplacer, supprimer ou copier des fichiers et des dossiers dans accès Web à distance](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_2).  

-   Déplacez, supprimez et renommez des fichiers et des dossiers sur Windows Server Essentials. Pour plus d’informations, consultez [créer, renommer, déplacer, supprimer ou copier des fichiers et des dossiers dans accès Web à distance](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_2).  

  
#### <a name="upload-files"></a>Charger des fichiers  
  
###### <a name="to-upload-files"></a>Pour télécharger des fichiers  
  
1. Dans l'accès web à distance, cliquez sur l'onglet **Dossiers partagés**, puis cliquez sur un lien de dossier partagé. Une liste des fichiers et des dossiers dans ce dossier partagé s'affiche.  
  
2. Dans la liste des fichiers et dossiers du dossier partagé, cliquez sur le dossier où vous voulez télécharger le fichier, puis cliquez sur **Télécharger**.  
  
3. Si l'outil de téléchargement standard n'est pas déjà chargé, cliquez sur **Utiliser la méthode de téléchargement standard**.  
  
4. Cliquez sur **Parcourir**  pour rechercher un fichier sur votre ordinateur.  
  
5. Parcourez les dossiers de votre ordinateur jusqu'au fichier que vous souhaitez télécharger, puis cliquez sur **Ouvrir**.  
  
6. Répétez les étapes 2 et 3 pour chaque fichier à télécharger.  
  
7. Lorsque vous avez ajouté tous les fichiers que vous souhaitez télécharger, cliquez sur **Télécharger**.  
  
   L'outil de téléchargement simplifié de fichiers simplifie le processus de téléchargement des fichiers sur votre serveur exécutant Windows Server Essentials. Vous pouvez ajouter autant de fichiers que vous voulez dans l'outil de téléchargement simplifié par glissement-déplacement, puis les télécharger dans les dossiers partagés sur le serveur.  
  
> [!NOTE]
>  Le téléchargement de plusieurs fichiers est pris en charge en mode natif dans les navigateurs web qui sont compatibles avec HTML5. Cet outil est nécessaire uniquement lorsque le navigateur web ne prend pas en charge HTML5.  
  
###### <a name="to-upload-files-using-the-easy-file-upload-tool"></a>Pour télécharger les fichiers à l'aide de l'outil de téléchargement simplifié de fichiers  
  
1.  Dans l'accès web à distance, cliquez sur l'onglet **Dossiers partagés**, puis cliquez sur un lien de dossier partagé. Une liste des fichiers et des dossiers dans ce dossier partagé s'affiche.  
  
2.  Dans la liste des fichiers et dossiers du dossier partagé, cliquez sur le dossier où vous voulez télécharger les fichiers, puis cliquez sur **Télécharger**. Si le dossier que vous souhaitez télécharger n'existe ne pas, cliquez sur **Nouveau dossier**, tapez le nom du nouveau dossier dans la boîte de dialogue, puis cliquez sur **OK**.  
  
3.  Vous devrez peut-être exécuter le module complémentaire Solutions Windows Server. Si tel est le cas, cliquez sur la bande jaune en haut de l'écran, cliquez sur **Module complémentaire**, puis sur **Exécuter** dans la boîte de dialogue.  
  
4.  S'il n'est pas déjà chargé, cliquez sur **Utilisez l'outil de téléchargement simplifié de fichiers**.  
  
5.  Vous pouvez glisser-déplacer les fichiers de l'Explorateur Windows vers l'outil de téléchargement simplifié de fichiers, ou cliquer sur **Parcourir pour sélectionner des fichiers**.  
  
6.  Lorsque vous avez terminé d'ajouter les fichiers que vous voulez télécharger dans le dossier sélectionné, cliquez sur **Télécharger**.  
  
7.  Lorsque les fichiers sont téléchargés avec succès, cliquez sur **Fermer**.  
  
#### <a name="download-files-or-folders"></a>Télécharger des fichiers ou des dossiers  
  
###### <a name="to-download-a-single-file"></a>Pour télécharger un seul fichier  
  
1. Dans l'accès web à distance, cliquez sur l'onglet **Dossiers partagés**, puis cliquez sur un lien de dossier partagé. Une liste des fichiers et des dossiers dans ce dossier partagé s'affiche.  
  
2. Dans la liste de fichiers du dossier partagé, cochez sur la case à cocher en regard du fichier que vous voulez télécharger sur votre ordinateur personnel.  
  
3. Cliquez sur **Télécharger** pour commencer le téléchargement.  
  
4. Dans la boîte de dialogue **Téléchargement de fichier** , cliquez sur **Enregistrer** pour enregistrer le fichier sur votre ordinateur.  
  
5. Dans la boîte de dialogue **Enregistrer sous**, sélectionnez l'emplacement d'enregistrement du fichier, puis cliquez sur **Enregistrer**. Un fichier unique n'est pas compressé avant d'être téléchargé.  
  
   Il existe deux options pour le téléchargement de plusieurs fichiers ou dossiers. Choisissez l'option qui correspond à vos besoins :  
  
> [!NOTE]
>  Ces options sont disponibles uniquement lorsque vous téléchargez plusieurs fichiers ou dossiers sur votre ordinateur.  
  
- **Fichier exécutable à extraction automatique (. exe)**  
  
  > [!NOTE]
  >   Cette section s’applique à un serveur exécutant Windows Server Essentials.  
  
   Un fichier exécutable à extraction automatique est un fichier que vous pouvez télécharger qui associe le programme (exécutable) de décompression et les fichiers compressés. Lorsque vous exécutez le programme exécutable, il décompresse automatiquement les fichiers compressés (à extraction automatique). Il s'agit d'une méthode courante de distribution des données compressées qui permet de ne pas se préoccuper de savoir si le destinataire dispose de l'utilitaire de décompression approprié.  
  
  > [!NOTE]
  >  Cette option prend en charge les caractères Unicode.  
  
- **Dossier Windows compressé (. zip)**  
  
   La compression d'un fichier crée une version compressée du fichier qui est plus petite que le fichier d'origine. La version du fichier compressée a une extension de nom de fichier .zip. Les types de fichiers dont la taille se réduit le plus à la compression sont les fichiers à contenu textuel, comme les fichiers .txt, .doc, .xls et les fichiers graphiques qui utilisent des types de fichier non compressés comme .bmp. Certains fichiers graphiques, tels que les fichiers .jpg et .gif, utilisent déjà la compression et la taille du fichier est très peu réduite par la compression. Ainsi, la taille d'un document Word qui contient un grand nombre de graphiques n'est pas autant réduite que celle d'un document qui contient principalement du texte.  
  
  > [!NOTE]
  >  Cette option fournit une prise en charge limitée des noms de fichier internationaux dans Windows Server Essentials.  
  
  Avant le début de téléchargement, le fichier exe ou zip est créé. En fonction du nombre de fichiers et de la taille totale des fichiers à télécharger, cela peut prendre plusieurs minutes. Une fois le fichier de téléchargement créé, le téléchargement du fichier se produit en arrière-plan. Cela vous permet de continuer à travailler pendant le processus de téléchargement.  
  
###### <a name="to-download-multiple-files-or-folders"></a>Pour télécharger plusieurs fichiers ou dossiers  
  
1.  Dans l'accès web à distance, cliquez sur l'onglet **Dossiers partagés**, puis cliquez sur un lien de dossier partagé. Une liste des fichiers et des dossiers dans ce dossier partagé s'affiche.  
  
2.  Dans la liste de fichiers du dossier partagé, cochez sur la case à cocher en regard des fichiers ou dossiers que vous voulez télécharger sur votre ordinateur personnel.  
  
3.  Cliquez sur **Télécharger** pour commencer le téléchargement.  
  
4.  Dans la boîte dialogue **Sélection d'un format de téléchargement**, cliquez pour sélectionner l'option de format de téléchargement que vous préférez, puis cliquez sur **OK**. Le fichier compressé est préparé dans l'option de format que vous avez sélectionné.  
  
5.  Dans la boîte de dialogue **Téléchargement de fichier** , cliquez sur **Enregistrer** pour enregistrer le fichier sur votre ordinateur.  
  
6.  Dans la boîte de dialogue **Enregistrer sous**, sélectionnez l'emplacement d'enregistrement du fichier, puis cliquez sur **Enregistrer**.  
  
#### <a name="retrieve-compressed-files-downloaded-to-your-computer"></a>Récupérer des fichiers compressés téléchargés sur votre ordinateur  
  
> [!NOTE]
>   Cette section s’applique à un serveur exécutant Windows Server Essentials.  
  
 Si vous sélectionnez plusieurs fichiers ou dossiers à télécharger, vous pouvez recevoir un fichier exécutable compressé à extraction automatique (.exe) ou un fichier compressé (.zip).  
  
###### <a name="to-retrieve-a-file-from-the-compressed-exe-file"></a>Pour récupérer un fichier à partir du fichier compressé (.exe)  
  
1.  Sur votre ordinateur, double-cliquez sur le fichier compressé pour l'ouvrir.  
  
2.  Suivez les instructions pour extraire les fichiers dans un dossier sur votre ordinateur.  
  
###### <a name="to-retrieve-a-file-from-the-compressed-zip-file"></a>Pour récupérer un fichier à partir du fichier compressé (.zip)  
  
1.  Sur votre ordinateur, double-cliquez sur le fichier compressé pour l'ouvrir.  
  
2.  Sélectionner les fichiers que vous souhaitez récupérer, puis glissez-déplacez les fichiers vers un dossier sur votre ordinateur où vous voulez les stocker.  
  
    > [!NOTE]
    >  Si vous utilisez un programme de compression tiers, suivez les procédures de ce programme pour extraire vos fichiers du fichier compressé.  
  
###  <a name="BKMK_2"></a>Créer, renommer, déplacer, supprimer ou copier des fichiers et des dossiers dans Accès web à distance  
 Vous pouvez utiliser l'accès web à distance pour créer des dossiers dans un dossier partagé existant, renommer des fichiers et dossiers, déplacer et copier des fichiers et des dossiers et supprimer des fichiers et dossiers sur votre serveur.  
  
> [!NOTE]
>  Pour ajouter de nouveaux dossiers partagés sur un serveur qui exécute Windows Server Essentials, vous devez utiliser le tableau de bord. Pour vous connecter à la console du serveur depuis l'accès web à distance, sous l'onglet **Ordinateurs**, cliquez sur le nom du serveur, cliquez sur **Connexion**, puis suivez les instructions pour ouvrir une session sur le serveur. Pour plus d’informations sur la création de dossiers partagés, consultez [Add or move a server folder](../manage/Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_5).  
  
##### <a name="to-create-a-new-folder"></a>Pour créer un nouveau dossier  
  
1.  Dans l'Accès web à distance, cliquez sur l'onglet **Dossiers partagés** , puis cliquez sur un lien de dossier partagé. Une liste des fichiers et des dossiers dans ce dossier partagé s'affiche.  
  
2.  Dans la barre des tâches, cliquez sur **Nouveau dossier**.  
  
3.  Tapez un nom pour le dossier, puis cliquez sur **OK**.  
  
##### <a name="to-rename-a-file-or-folder"></a>Pour renommer un fichier ou un dossier  
  
1.  Dans l'Accès web à distance, cliquez sur l'onglet **Dossiers partagés** , puis cliquez sur un lien de dossier partagé. Une liste des fichiers et des dossiers dans ce dossier partagé s'affiche.  
  
2.  Cliquez avec le bouton droit sur le fichier ou dossier que vous voulez renommer, puis cliquez sur **Renommer**.  
  
3.  Tapez un nouveau nom dans la zone de texte, puis cliquez sur **OK**.  
  
##### <a name="to-move-files-or-folders"></a>Pour déplacer des fichiers ou des dossiers  
  
1.  Dans l'Accès web à distance, cliquez sur l'onglet **Dossiers partagés** , puis cliquez sur un lien de dossier partagé. Une liste des fichiers et des dossiers dans ce dossier partagé s'affiche.  
  
2.  Cochez la case en regard des fichiers ou dossiers que vous voulez déplacer, cliquez avec le bouton droit sur un des fichiers ou dossiers sélectionnés, puis cliquez sur **Couper**.  
  
3.  Cliquez avec le bouton droit sur le dossier dans lequel vous voulez déplacer les fichiers ou dossiers, puis cliquez sur **Coller**.  
  
##### <a name="to-delete-a-file-or-folder"></a>Pour supprimer un fichier ou un dossier  
  
1.  Dans l'Accès web à distance, cliquez sur l'onglet **Dossiers partagés** , puis cliquez sur un lien de dossier partagé. Une liste des fichiers et des dossiers dans ce dossier partagé s'affiche.  
  
2.  Cochez la case en regard des fichiers ou dossiers que vous voulez supprimer, cliquez avec le bouton droit sur un des fichiers ou dossiers sélectionnés, puis cliquez sur **Supprimer**.  
  
3.  Pour confirmer que vous voulez supprimer les fichiers et dossiers sélectionnés, cliquez sur **Oui**.  
  
##### <a name="to-copy-files-or-folders"></a>Pour copier des fichiers ou des dossiers  
  
1.  Dans l'Accès web à distance, cliquez sur l'onglet **Dossiers partagés** , puis cliquez sur un lien de dossier partagé. Une liste des fichiers et des dossiers dans ce dossier partagé s'affiche.  
  
2.  Cochez la case en regard des fichiers ou dossiers que vous voulez copier, cliquez avec le bouton droit sur un des fichiers ou dossiers sélectionnés, puis cliquez sur **Copier**.  
  
3.  Cliquez avec le bouton droit sur le dossier dans lequel vous voulez copier les fichiers ou dossiers, puis cliquez sur **Coller**.  
  
##  <a name="BKMK_ConnectMobile"></a>Se connecter à partir d’un appareil mobile  
  

-   [Utiliser des Accès web distantes à partir d’un appareil mobile](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_8)  
  
-   [Navigateurs Web pris en charge pour les appareils mobiles](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_9)  

-   [Utiliser des Accès web distantes à partir d’un appareil mobile](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_8)  
  
-   [Navigateurs Web pris en charge pour les appareils mobiles](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_9)  

  
###  <a name="BKMK_8"></a>Utiliser des Accès web distantes à partir d’un appareil mobile  
 Vous pouvez vous connecter à l'accès web à distance à partir de votre smartphone pour consulter les fichiers et dossiers des dossiers partagés du serveur.  
  
> [!NOTE]
>  Vous pouvez également télécharger et utiliser l’application My Server pour Windows Server Essentials à partir de la [Place de marché Windows Phone](http://www.windowsphone.com/apps/6c2f98d5-6fcf-4e1d-b8b1-cde62ea1a94a) pour accéder à vos dossiers partagés et fichiers multimédias stockés sur le serveur.  
  
##### <a name="to-log-on-to-remote-web-access-from-a-mobile-device"></a>Pour se connecter à l'accès web à distance à partir d'un appareil mobile  
  
1.  Ouvrez un navigateur Web et tapez **https://** _< VotreNomdeDomaine\>_ **/Remote** dans la barre d’adresses.  Veillez à inclure les s dans https.  
  
2.  Dans la page d’ouverture de session Accès web à distance, tapez votre nom d’utilisateur et votre mot de passe dans les zones de texte, puis cliquez sur la flèche. Vous êtes connecté à la version mobile de l'accès web à distance.  
  
##### <a name="to-switch-to-the-desktop-version-of-remote-web-access"></a>Pour passer à la version bureau de l'accès web à distance  
  
1.  Ouvrez un navigateur Web et tapez **https://** _< VotreNomdeDomaine\>_ **/Remote** dans la barre d’adresses.  Veillez à inclure les s dans https.  
  
2.  Dans la page d’ouverture de session Accès web à distance, tapez votre nom d’utilisateur et votre mot de passe dans les zones de texte, cliquez sur **afficher la version du Bureau**, puis cliquez sur la flèche. Vous êtes connecté à la version bureau de l'accès web à distance.  
  
##### <a name="to-return-to-the-mobile-version-of-remote-web-access"></a>Pour revenir à la version mobile de l'accès web à distance  
  
1. Fermez la session.  
  
2. Ouvrez un navigateur Web et tapez **https://** _< VotreNomdeDomaine\>_ **/Remote/m** dans la barre d’adresses. Veillez à inclure les s dans https.  
  
3. La version mobile de Accès web à distance s’affiche. Dans la page d’ouverture de session Accès web à distance, tapez votre nom d’utilisateur et votre mot de passe dans les zones de texte, puis cliquez sur la flèche. Vous êtes connecté à la version mobile de Accès web à distance.  
  
   Vous pouvez rechercher des fichiers et des dossiers dans les dossiers partagés sur le serveur.  
  
###  <a name="BKMK_9"></a>Navigateurs Web pris en charge pour les appareils mobiles  
 Les navigateurs web pris en charge pour les appareils mobiles incluent les suivants :  
  
-   Internet Explorer Mobile 6.0 ou version ultérieure  
  
-   Safari  
  
-   Blackberry  
  
-   Symbian 6.0 ou version ultérieure  
  
-   Android  
  
-   Google Chrome  
  
-   Firefox  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Gérer les Accès web distantes](../manage/Manage-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  

-   [Travailler à distance](Work-Remotely-in-Windows-Server-Essentials.md)  
  
-   [Utiliser Windows Server Essentials](Use-Windows-Server-Essentials.md)

-   [Travailler à distance](../use/Work-Remotely-in-Windows-Server-Essentials.md)  
  
-   [Utiliser Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)

