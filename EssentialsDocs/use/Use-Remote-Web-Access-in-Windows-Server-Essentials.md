---
title: "Utiliser l’accès Web à distance dans Windows Server Essentials"
description: "Décrit comment utiliser WindowsServerEssentials"
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
ms.openlocfilehash: ac384b545fe61b4a832debdc8aedcc81a75c669a
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="use-remote-web-access-in-windows-server-essentials"></a>Utiliser l’accès Web à distance dans Windows Server Essentials

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials
  
  Accès Web à distance vous permet de rester connecté à votre réseau Windows Server Essentials lorsque vous êtes absent. Lorsque vous ouvrez une session accès Web à distance, vous pouvez vous connecter aux ordinateurs sur votre réseau Windows Server Essentials, ouvrez le tableau de bord pour gérer votre réseau Windows Server Essentials et accéder à tous les fichiers de dossiers et les médias partagés sur le serveur.  
  
 Cette rubrique contient les sections suivantes:  
  

-   [Se connecter à accès Web à distance](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Connect)  
  
-   [Partager des fichiers et dossiers](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SharedFolders)  
  
-   [Se connecter à partir d’un périphérique mobile](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ConnectMobile)  
  
##  <a name="BKMK_Connect"></a>Se connecter à accès Web à distance  
  
-   [Ouvrez une session sur l’accès Web à distance](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Accéder à distance à votre ordinateur](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1.5)  

-   [Se connecter à accès Web à distance](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Connect)  
  
-   [Partager des fichiers et dossiers](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SharedFolders)  
  
-   [Se connecter à partir d’un périphérique mobile](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ConnectMobile)  
  
##  <a name="BKMK_Connect"></a>Se connecter à accès Web à distance  
  
-   [Ouvrez une session sur l’accès Web à distance](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Accéder à distance à votre ordinateur](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1.5)  

  
###  <a name="BKMK_1"></a>Ouvrez une session sur l’accès Web à distance  
 Lorsque vous ouvrez une session sur l’accès Web à distance à partir d’un ordinateur local ou distant, vous pouvez accéder aux ressources sur votre serveur exécutant Windows Server Essentials et des ordinateurs sur votre réseau.  
  
##### <a name="to-log-on-to-remote-web-access-from-a-network-computer"></a>Se connecter à accès Web à distance à partir d’un ordinateur réseau  
  
1.  Ouvrez un navigateur Web, tapez **https://***< YourServerName\ >***/remote** dans la barre d’adresses, puis appuyez sur ENTRÉE.  
  
    > [!NOTE]
    >  Assurez-vous que vous incluez le s dans https.  
  
2.  Sur la page d’ouverture de session accès Web à distance, tapez votre nom d’utilisateur et un mot de passe dans les zones de texte, puis cliquez sur la flèche.  
  
##### <a name="to-log-on-to-remote-web-access-from-a-remote-computer"></a>Se connecter à accès Web à distance à partir d’un ordinateur distant  
  
1.  Ouvrez un navigateur Web, tapez **https://***< YourDomainName\ >***/remote** dans la barre d’adresses, puis appuyez sur ENTRÉE.  
  
    > [!NOTE]
    >  Vous pouvez obtenir des informations sur les nom de domaine auprès de votre administrateur réseau. Assurez-vous que vous incluez le s dans https.  
  
2.  Sur la page d’ouverture de session accès Web à distance, tapez votre nom d’utilisateur et un mot de passe dans les zones de texte, puis cliquez sur la flèche.  
  
###  <a name="BKMK_1.5"></a>Accéder à distance à votre ordinateur  
 Lorsque vous êtes en déplacement, vous pouvez utiliser votre navigateur Web pour se connecter au site accès Web à distance pour accéder à distance à votre tableau de bord Windows Server Essentials, les dossiers partagés et les ordinateurs de votre réseau.  
  
 Lorsque vous vous connectez au tableau de bord, vous pouvez gérer Windows Server Essentials comme vous le feriez si vous étiez au bureau. Vous pouvez effectuer toutes les tâches d’administration habituelles, telles que l’ajout de comptes d’utilisateur, ajouter des dossiers partagés, définir l’accès de dossier partagé et ainsi de suite. Lorsque vous vous connectez aux ordinateurs sur votre réseau, vous pouvez accéder leurs bureaux comme si vous étiez assis face d’eux au bureau.  
  
 Le **état** colonne vous indique si vous pouvez vous connecter à un ordinateur sur votre réseau et pouvez inclure les valeurs suivantes:  
  
-   **Disponible**  
  
     L’ordinateur est allumé et est disponible pour une connexion à distance. Même si vous voyez cet état, vous ne pouvez pas être en mesure de se connecter à cet ordinateur, si un pare-feu tiers bloque la connexion.  
  
-   **En mode hors connexion ou en veille**  
  
     L’ordinateur est éteint ou est en mode veille ou veille prolongée. Si un ordinateur est en veille ou en mode hors connexion, l’état est mis à jour en temps réel afin que vous puissiez savoir lorsque l’ordinateur devient disponible.  
  
-   **Système d’exploitation non pris en charge**  
  
     Le système d’exploitation sur l’ordinateur ne prend pas en charge les services Bureau à distance. Il peut prendre jusqu'à 6heures pour cet état de mise à jour sur le serveur s’il existe une modification.  
  
-   **La connexion est désactivée**  
  
     La connexion de l’ordinateur est bloquée par un pare-feu ou le Bureau à distance est désactivé sur l’ordinateur ou par la stratégie de groupe. Il peut prendre jusqu'à 6heures pour cet état de mise à jour sur le serveur s’il existe une modification.  
  
#### <a name="to-connect-to-a-computer-on-your-network"></a>Se connecter à un ordinateur sur votre réseau  
 Sur le **périphériques** , cliquez sur le nom de l’ordinateur. Vous pouvez sélectionner uniquement les ordinateurs avec une **disponible** état.  
  
#### <a name="to-connect-to-the-server-dashboard"></a>Pour se connecter au serveur du tableau de bord  
 Sur le **périphériques** , cliquez sur le nom de votre serveur. Vous pouvez sélectionner uniquement les ordinateurs avec une **disponible** état. Vous devez être en mesure de fournir un compte d’utilisateur administrateur et le mot de passe sur votre serveur à utiliser le tableau de bord.  
  
##  <a name="BKMK_SharedFolders"></a>Partager des fichiers et dossiers  
  

-   [Télécharger des fichiers dans l’accès Web à distance](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_UploadRWA)  
  
-   [Créer, renommer, déplacer, supprimer ou copiez les fichiers et dossiers dans accès Web à distance](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_2)  

-   [Télécharger des fichiers dans l’accès Web à distance](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_UploadRWA)  
  
-   [Créer, renommer, déplacer, supprimer ou copiez les fichiers et dossiers dans accès Web à distance](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_2)  

  
###  <a name="BKMK_UploadRWA"></a>Télécharger des fichiers dans l’accès Web à distance  
 L’accès Web à distance **dossiers partagés** onglet, vous pouvez procédez comme suit:  
  
-   Téléchargez (envoyez) des fichiers à partir de votre ordinateur vers Windows Server Essentials.  
  
    > [!NOTE]
    >  Vous pouvez télécharger uniquement les fichiers et dossiers pas l’accès Web à distance. Si vous souhaitez que la même hiérarchie de fichiers et de dossiers les **dossiers partagés** sur le serveur que sur votre ordinateur, vous devez créer les dossiers sur le serveur Web l’accès à distance et télécharger les fichiers dans le dossier que vous avez créé. Pour plus d’informations sur la création de dossiers de serveur, voir [ajouter ou déplacer un dossier serveur](../manage/Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_5).  
  
-   Téléchargez (recevez) les fichiers et dossiers à partir de Windows Server Essentials sur votre ordinateur.  
  
-   Créer un dossier dans un dossier partagé sur Windows Server Essentials.  
  

-   Déplacer, supprimer et renommer des fichiers et dossiers sur Windows Server Essentials. Pour plus d’informations, voir [créer, renommer, déplacer, supprimer ou copier des fichiers et dossiers dans accès Web à distance](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_2).  

-   Déplacer, supprimer et renommer des fichiers et dossiers sur Windows Server Essentials. Pour plus d’informations, voir [créer, renommer, déplacer, supprimer ou copier des fichiers et dossiers dans accès Web à distance](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_2).  

  
#### <a name="upload-files"></a>Télécharger des fichiers  
  
###### <a name="to-upload-files"></a>Pour télécharger des fichiers  
  
1.  Accès Web à distance, cliquez sur le **dossiers partagés** onglet, puis cliquez sur un lien de dossier partagé. Une liste des fichiers et dossiers dans ce dossier partagé s’affiche.  
  
2.  Dans la liste de dossier partagé des fichiers et dossiers, cliquez sur le dossier dans lequel vous souhaitez télécharger le fichier, puis cliquez sur **télécharger**.  
  
3.  Si l’outil de téléchargement standard n’est pas déjà chargé, cliquez sur **utiliser la méthode de téléchargement standard**.  
  
4.  Cliquez sur **Parcourir** pour rechercher un fichier sur votre ordinateur.  
  
5.  Parcourir les dossiers sur votre ordinateur pour rechercher le fichier que vous souhaitez télécharger, puis cliquez sur **ouvrir**.  
  
6.  Répétez les étapes 2 et 3 pour chaque fichier que vous souhaitez télécharger.  
  
7.  Lorsque vous avez ajouté tous les fichiers que vous souhaitez télécharger, cliquez sur **télécharger**.  
  
 L’outil téléchargement facile simplifie le processus de téléchargement des fichiers sur votre serveur exécutant Windows Server Essentials. Vous pouvez ajouter autant de fichiers que vous le souhaitez à l’outil de téléchargement simplifié de fichiers à l’aide de la fonctionnalité glisser-déplacer et les télécharger dans les dossiers partagés sur le serveur.  
  
> [!NOTE]
>  Le téléchargement de plusieurs fichiers est prise en charge dans les navigateurs web qui sont compatibles avec HTML5. Cet outil est nécessaire uniquement lorsque le navigateur web ne prend pas en charge HTML5.  
  
###### <a name="to-upload-files-using-the-easy-file-upload-tool"></a>Pour télécharger des fichiers à l’aide de l’outil téléchargement facile  
  
1.  Accès Web à distance, cliquez sur le **dossiers partagés** onglet, puis cliquez sur un lien de dossier partagé. Une liste des fichiers et dossiers dans ce dossier partagé s’affiche.  
  
2.  Dans la liste de dossier partagé des fichiers et dossiers, cliquez sur le dossier dans lequel vous souhaitez télécharger les fichiers, puis cliquez sur **télécharger**. Si le dossier que vous souhaitez télécharger n’existe ne pas, cliquez sur **nouveau dossier**, tapez le nom du nouveau dossier dans la boîte de dialogue, puis cliquez sur **OK**.  
  
3.  Vous devrez peut-être exécuter le module complémentaire Solutions Windows Server. Dans ce cas, cliquez sur la bande jaune en haut de l’écran, cliquez sur exécuter **module complémentaire**, puis cliquez sur **exécuter** dans la boîte de dialogue.  
  
4.  Si l’outil de téléchargement simplifié de fichiers n’est pas déjà chargé, cliquez sur **utiliser l’outil de téléchargement simplifié de fichiers**.  
  
5.  Vous pouvez glisser-déplacer des fichiers depuis l’Explorateur Windows à l’outil de téléchargement simplifié de fichiers, ou cliquez sur **Parcourir pour sélectionner les fichiers**.  
  
6.  Lorsque vous avez terminé d’ajouter les fichiers que vous voulez télécharger dans le dossier sélectionné, cliquez sur **télécharger**.  
  
7.  Lorsque les fichiers sont téléchargés avec succès, cliquez sur **fermer**.  
  
#### <a name="download-files-or-folders"></a>Télécharger des fichiers ou des dossiers  
  
###### <a name="to-download-a-single-file"></a>Pour télécharger un fichier unique  
  
1.  Accès Web à distance, cliquez sur le **dossiers partagés** onglet, puis cliquez sur un lien de dossier partagé. Une liste des fichiers et dossiers dans ce dossier partagé s’affiche.  
  
2.  Dans la liste des fichiers du dossier partagé, cliquez sur la case à cocher en regard du fichier que vous voulez télécharger sur votre ordinateur personnel.  
  
3.  Cliquez sur **télécharger** pour commencer le téléchargement.  
  
4.  Sur le **téléchargement de fichier** boîte de dialogue, cliquez sur **enregistrer** pour enregistrer le fichier sur votre ordinateur.  
  
5.  Dans le **Enregistrer sous** boîte de dialogue, sélectionnez l’emplacement pour enregistrer le fichier, puis cliquez sur **enregistrer**. Un fichier unique n’est pas compressé avant d’être téléchargé.  
  
 Il existe deux options pour le téléchargement de plusieurs fichiers ou dossiers. Choisissez l’option qui correspond à vos besoins:  
  
> [!NOTE]
>  Ces options sont disponibles uniquement lorsque vous téléchargez plusieurs fichiers ou dossiers sur votre ordinateur.  
  
-   **Fichier exécutable à extraction automatique (.exe)**  
  
    > [!NOTE]
    >   Cette section s’applique à un serveur exécutant WindowsServerEssentials.  
  
     Un fichier exécutable à extraction automatique est un fichier que vous pouvez télécharger qui associe le programme (exécutable) de décompression des fichiers compressés. Lorsque vous exécutez le programme exécutable, il décompresse automatiquement les fichiers compressés (à extraction automatique). Il s’agit d’une méthode courante de distribution des données compressées sans tenir compte si le destinataire dispose de l’utilitaire de décompression approprié.  
  
    > [!NOTE]
    >  Cette option prend en charge les caractères Unicode.  
  
-   **Dossier Windows compressé (.zip)**  
  
     Compression d’un fichier crée une version compressée du fichier qui est plus petit que le fichier d’origine. La version du fichier zippée a une extension de nom de fichier .zip. Types de fichiers qui sont réduites le plus à la compression sont des types de fichier textuel, comme .txt, .doc, .xls et les fichiers de graphiques qui utilisent des types de fichier non compressés comme .bmp. Certains fichiers graphiques, tels que les fichiers .jpg et .gif, utilisent déjà la compression et la taille du fichier est réduit très peu à la compression. En outre, un document Word qui contient un grand nombre de graphiques n’est pas réduit autant qu’un document principalement du texte.  
  
    > [!NOTE]
    >  Cette option fournit la prise en charge limitée des noms de fichier internationaux dans Windows Server Essentials.  
  
 Avant le début de téléchargement, le fichier exe ou zip est créé. Selon le nombre de fichiers et la taille totale des fichiers à télécharger, cela peut prendre plusieurs minutes. Une fois le téléchargement de fichier est créé, téléchargez le fichier se produit en arrière-plan. Cela vous permet de continuer à travailler pendant le processus de téléchargement se termine.  
  
###### <a name="to-download-multiple-files-or-folders"></a>Pour télécharger plusieurs fichiers ou dossiers  
  
1.  Accès Web à distance, cliquez sur le **dossiers partagés** onglet, puis cliquez sur un lien de dossier partagé. Une liste des fichiers et dossiers dans ce dossier partagé s’affiche.  
  
2.  Dans la liste des fichiers du dossier partagé, cliquez sur la case à cocher en regard des fichiers ou dossiers que vous voulez télécharger sur votre ordinateur personnel.  
  
3.  Cliquez sur **télécharger** pour commencer le téléchargement.  
  
4.  Dans le **choisir un Format de téléchargement** boîte de dialogue, cliquez pour sélectionner l’option de format de téléchargement que vous préférez, puis cliquez sur **OK**. Le fichier compressé est préparé dans l’option de format que vous avez sélectionné.  
  
5.  Dans le **téléchargement de fichier** boîte de dialogue, cliquez sur **enregistrer** pour enregistrer le fichier sur votre ordinateur.  
  
6.  Dans le **Enregistrer sous** boîte de dialogue, sélectionnez l’emplacement pour enregistrer le fichier, puis cliquez sur **enregistrer**.  
  
#### <a name="retrieve-compressed-files-downloaded-to-your-computer"></a>Récupérer des fichiers compressés téléchargés sur votre ordinateur  
  
> [!NOTE]
>   Cette section s’applique à un serveur exécutant WindowsServerEssentials.  
  
 Si vous sélectionnez plusieurs fichiers ou dossiers à télécharger, vous pouvez recevoir un fichier exécutable compressé à extraction automatique (.exe) ou un fichier compressé (.zip).  
  
###### <a name="to-retrieve-a-file-from-the-compressed-exe-file"></a>Pour récupérer un fichier à partir du fichier compressé (.exe)  
  
1.  Sur votre ordinateur, double-cliquez sur le fichier compressé pour l’ouvrir.  
  
2.  Suivez les instructions pour extraire les fichiers vers un dossier sur votre ordinateur.  
  
###### <a name="to-retrieve-a-file-from-the-compressed-zip-file"></a>Pour récupérer un fichier à partir du fichier compressé (.zip)  
  
1.  Sur votre ordinateur, double-cliquez sur le fichier compressé pour l’ouvrir.  
  
2.  Sélectionnez les fichiers que vous souhaitez récupérer, puis faites glisser les fichiers vers un dossier sur votre ordinateur où vous voulez les stocker.  
  
    > [!NOTE]
    >  Si vous utilisez un programme de compression tiers, suivez les procédures de ce programme pour extraire vos fichiers à partir du fichier compressé.  
  
###  <a name="BKMK_2"></a>Créer, renommer, déplacer, supprimer ou copiez les fichiers et dossiers dans accès Web à distance  
 Vous pouvez utiliser l’accès Web à distance pour créer des dossiers dans un dossier partagé existant, renommer des fichiers et dossiers, déplacer et copier des fichiers et dossiers et de supprimer des fichiers et dossiers sur votre serveur.  
  
> [!NOTE]
>  Pour ajouter de nouveaux dossiers partagés sur un serveur qui exécute Windows Server Essentials, vous devez utiliser le tableau de bord. Se connecter à la console du serveur d’accès Web à distance, sur le **ordinateurs** , cliquez sur le nom du serveur, cliquez sur **connexion**, puis suivez les instructions pour ouvrir une session sur le serveur. Pour plus d’informations sur la création de dossiers partagés, consultez [ajouter ou déplacer un dossier serveur](../manage/Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_5).  
  
##### <a name="to-create-a-new-folder"></a>Pour créer un nouveau dossier  
  
1.  Accès Web à distance, cliquez sur le **dossiers partagés** onglet, puis cliquez sur un lien de dossier partagé. Une liste des fichiers et dossiers dans ce dossier partagé s’affiche.  
  
2.  Dans la barre des tâches, cliquez sur **nouveau dossier**.  
  
3.  Tapez un nom pour le dossier, puis cliquez sur **OK**.  
  
##### <a name="to-rename-a-file-or-folder"></a>Pour renommer un fichier ou dossier  
  
1.  Accès Web à distance, cliquez sur le **dossiers partagés** onglet, puis cliquez sur un lien de dossier partagé. Une liste des fichiers et dossiers dans ce dossier partagé s’affiche.  
  
2.  Cliquez sur le fichier ou dossier que vous souhaitez renommer, puis cliquez sur **renommer**.  
  
3.  Tapez un nouveau nom dans la zone de texte, puis cliquez sur **OK**.  
  
##### <a name="to-move-files-or-folders"></a>Pour déplacer des fichiers ou des dossiers  
  
1.  Accès Web à distance, cliquez sur le **dossiers partagés** onglet, puis cliquez sur un lien de dossier partagé. Une liste des fichiers et dossiers dans ce dossier partagé s’affiche.  
  
2.  Activez la case à cocher en regard des fichiers ou dossiers que vous souhaitez déplacer, avec le bouton droit des fichiers ou dossiers sélectionnés, puis cliquez sur **couper**.  
  
3.  Cliquez sur le dossier que vous souhaitez déplacer les fichiers ou dossiers, puis cliquez sur **coller**.  
  
##### <a name="to-delete-a-file-or-folder"></a>Pour supprimer un fichier ou dossier  
  
1.  Accès Web à distance, cliquez sur le **dossiers partagés** onglet, puis cliquez sur un lien de dossier partagé. Une liste des fichiers et dossiers dans ce dossier partagé s’affiche.  
  
2.  Activez la case à cocher en regard des fichiers ou dossiers que vous souhaitez supprimer, avec le bouton droit des fichiers ou dossiers sélectionnés, puis cliquez sur **supprimer**.  
  
3.  Pour confirmer que vous voulez supprimer les fichiers et dossiers sélectionnés, cliquez sur **Oui**.  
  
##### <a name="to-copy-files-or-folders"></a>Pour copier des fichiers ou des dossiers  
  
1.  Accès Web à distance, cliquez sur le **dossiers partagés** onglet, puis cliquez sur un lien de dossier partagé. Une liste des fichiers et dossiers dans ce dossier partagé s’affiche.  
  
2.  Activez la case à cocher en regard des fichiers ou dossiers que vous souhaitez copier, avec le bouton droit des fichiers ou dossiers sélectionnés, puis cliquez sur **copie**.  
  
3.  Cliquez sur le dossier que vous souhaitez copier les fichiers ou dossiers, puis cliquez sur **coller**.  
  
##  <a name="BKMK_ConnectMobile"></a>Se connecter à partir d’un périphérique mobile  
  

-   [Utiliser l’accès Web à distance à partir d’un périphérique mobile](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_8)  
  
-   [Prise en charge de navigateurs Web pour les périphériques mobiles](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_9)  

-   [Utiliser l’accès Web à distance à partir d’un périphérique mobile](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_8)  
  
-   [Prise en charge de navigateurs Web pour les périphériques mobiles](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_9)  

  
###  <a name="BKMK_8"></a>Utiliser l’accès Web à distance à partir d’un périphérique mobile  
 Vous pouvez vous connecter à accès Web à distance à partir de votre Smartphone pour afficher les fichiers et dossiers dans les dossiers partagés sur le serveur.  
  
> [!NOTE]
>  Vous pouvez également télécharger et utiliser l’application My Server pour Windows Server Essentials à partir de la [Windows Phone Marketplace](http://www.windowsphone.com/apps/6c2f98d5-6fcf-4e1d-b8b1-cde62ea1a94a) pour accéder à vos fichiers de dossiers et les médias partagés qui sont stockés sur le serveur.  
  
##### <a name="to-log-on-to-remote-web-access-from-a-mobile-device"></a>Se connecter à accès Web à distance à partir d’un périphérique mobile  
  
1.  Ouvrez un navigateur Web et tapez **https://***< YourDomainName\ >***/remote** dans la barre d’adresses.  Assurez-vous que vous incluez le s dans https.  
  
2.  Sur la page d’ouverture de session accès Web à distance, tapez votre nom d’utilisateur et un mot de passe dans les zones de texte, puis cliquez sur la flèche. Vous êtes connecté à la version mobile de l’accès Web à distance.  
  
##### <a name="to-switch-to-the-desktop-version-of-remote-web-access"></a>Pour basculer vers la version bureau de l’accès Web à distance  
  
1.  Ouvrez un navigateur Web et tapez **https://***< YourDomainName\ >***/remote** dans la barre d’adresses.  Assurez-vous que vous incluez le s dans https.  
  
2.  Sur la page d’ouverture de session accès Web à distance, tapez votre nom d’utilisateur et un mot de passe dans les zones de texte, cliquez sur **afficher la version de bureau**, puis cliquez sur la flèche. Vous êtes connecté à la version bureau de l’accès Web à distance.  
  
##### <a name="to-return-to-the-mobile-version-of-remote-web-access"></a>Pour revenir à la version mobile de l’accès Web à distance  
  
1.  Fermez la session.  
  
2.  Ouvrez un navigateur Web et tapez **https://***< YourDomainName\ >***/remote/m** dans la barre d’adresses. Assurez-vous que vous incluez le s dans https.  
  
3.  La version mobile de l’accès Web à distance s’affiche. Sur la page d’ouverture de session accès Web à distance, tapez votre nom d’utilisateur et un mot de passe dans les zones de texte, puis cliquez sur la flèche. Vous êtes connecté à la version mobile de l’accès Web à distance.  
  
 Vous pouvez rechercher des fichiers et dossiers dans les dossiers partagés sur le serveur.  
  
###  <a name="BKMK_9"></a>Prise en charge de navigateurs Web pour les périphériques mobiles  
 Navigateurs web pris en charge pour les périphériques mobiles sont les suivantes:  
  
-   Internet Explorer Mobile 6.0 ou version ultérieure  
  
-   Safari  
  
-   BlackBerry  
  
-   Symbian 6.0 ou version ultérieure  
  
-   Android  
  
-   Google Chrome  
  
-   Firefox  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Gérer l’accès Web à distance](../manage/Manage-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  

-   [Travailler à distance](Work-Remotely-in-Windows-Server-Essentials.md)  
  
-   [Utiliser Windows Server Essentials](Use-Windows-Server-Essentials.md)

-   [Travailler à distance](../use/Work-Remotely-in-Windows-Server-Essentials.md)  
  
-   [Utiliser Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)

