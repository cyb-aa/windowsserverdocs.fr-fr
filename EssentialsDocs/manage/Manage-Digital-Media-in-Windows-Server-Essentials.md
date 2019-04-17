---
title: "Gérer des médias numériques dans WindowsServerEssentials"
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9378bffa-487c-43ca-9ec3-7e7864d2dd9a
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 6906c1dafc6d4131e07c008b9db47ebebe9b7770
ms.sourcegitcommit: 5c8d8acc59d61756377c9c4e459a47a9ab555b39
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/18/2018
---
# <a name="manage-digital-media-in-windows-server-essentials"></a>Gérer des médias numériques dans WindowsServerEssentials

>S’applique à: Windows Server2012R2 Essentials, Windows Server2012Essentials

Les rubriques suivantes abordent la diffusion multimédia en continu les fonctionnalités de votre serveur et expliquent comment configurer et utiliser un support de diffusion en continu sur votre réseau.  
  
> [!NOTE]
>  Par défaut, la fonctionnalité de diffusion multimédia n’est pas disponible dans WindowsServerEssentials et Windows Server2012R2 avec le rôle expérience WindowsServerEssentials installé. Pour ajouter la fonctionnalité de diffusion multimédia dans ces versions, [télécharger et installer le serveur WindowsServerEssentialsMediaPack](https://www.microsoft.com/download/details.aspx?id=40837) à partir du MicrosoftDownload Center.  
  
-   [Vue d’ensemble des médias numériques](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Gérer le serveur multimédia à l’aide du tableau de bord](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Fonctionnement de la diffusion multimédia en continu](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Activer ou désactiver la diffusion multimédia](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [Ajouter des fichiers multimédias numériques sur le serveur](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Autoriser ou limiter l’accès à une bibliothèque multimédia sur le serveur](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_6)  
  
-   [Renommer la bibliothèque multimédia](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_8)  
  
-   [Arrêter le partage de médias numériques](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_9)  
  
-   [Permettre aux appareils multimédias qui utilisent le protocole Server Message Block (SMB) pour accéder aux fichiers partagés sur le serveur](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_10)  
  
-   [Processeurs courants et profils vidéo que pris en charge](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_CommonProcessors)  
  
-   [Problèmes connus avec les types de fichiers multimédias](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_KnownIssues)  
  
##  <a name="BKMK_1"></a>Vue d’ensemble des médias numériques  
 Multimédias numériques font référence à audio, vidéo et contenu photo qui ont été encodé (compressés numériquement). Le codage du contenu implique la conversion d’une entrée audio et vidéo dans un fichier multimédia numérique, tel qu’un fichier WindowsMedia. Une fois le média numérique codé, il peut être facilement manipulé, distribué et lu par des ordinateurs et facilement transmis sur des réseaux d’ordinateurs.  
  
 Exemples de types de médias numériques: WindowsMediaAudio (WMA), vidéo WindowsMedia (WMV), MP3, JPEG et AVI. Pour plus d’informations sur les types de médias numériques qui sont pris en charge par le lecteur WindowsMedia, consultez [pris en charge par le lecteur WindowsMedia de types de fichiers](https://support.microsoft.com/kb/316992).  
  
### <a name="why-would-i-want-to-stream-my-digital-media"></a>Pourquoi voudrais-je diffuser en continu mes médias numériques?  
 Nombreuses personnes stockent de musique, vidéos et des images dans des dossiers partagés dans WindowsServerEssentials. Il peut arriver lorsque vous souhaitez effectuer les opérations suivantes:  
  
-   **Regarder des vidéos**. Votre serveur peut être utilisé pour stocker et diffuser de grandes collections de vidéos et TV enregistrée affiche sur vos ordinateurs ou d’autres périphériques sur votre réseau. Vous pouvez diffuser des vidéos sur une Xbox 360 ou sur un ordinateur à l’aide du lecteur WindowsMedia.  
  
-   **Écouter de la musique**. Lorsque vous activez le partage de médias pour la **musique** dossier partagé, vous pouvez accéder à votre musique à partir d’appareils qui prennent en charge de WindowsMediaConnect. Vous n’avez pas besoin activer ou de configurer les comptes d’utilisateurs en continu à partir du **musique** dossier partagé une fois que le partage est activé.  
  
-   **Présenter des diaporamas de photos**. Vous pouvez stocker des photos numériques dans le **Photos** dossier sur votre serveur partagé et y accéder à partir de n’importe quel ordinateur ou d’une Xbox 360 connectée à une télévision à votre domicile ou au bureau. Vous pouvez regarder des diaporamas de photos, ce qui revient à transformer votre télévision en un grand cadre à image.  
  
###  <a name="BKMK_1.5"></a>Partage de médias protégés contre la copie  
  WindowsServerEssentials ne prend pas en charge le partage de médias protégés contre la copie. Cela inclut la musique achetée auprès d’un magasin de musique en ligne.  
  
 Support Copy-protected peut être lues uniquement sur l’ordinateur ou le périphérique que vous avez utilisé pour l’acheter. Protection contre la copie vous empêche de la lecture de médias sur plusieurs ordinateurs ou appareils, même si vous copiez le média sur votre serveur et le lire à partir de là. Toutefois, vous pouvez stocker le média protégé contre la copie sur WindowsServerEssentials et continuer à lire ce média sur l’ordinateur ou le périphérique que vous avez utilisé pour l’acheter.  
  
##  <a name="BKMK_2"></a>Gérer le serveur multimédia à l’aide du tableau de bord  
  WindowsServerEssentials permet d’effectuer des tâches d’administration courantes à l’aide du tableau de bord WindowsServerEssentials. Le **Media** onglet du serveur **paramètres** page du tableau de bord fournit les éléments suivants:  
  
|Section|Fonctionnalités|  
|-------------|-------------------|  
|Serveur multimédia|Le **activer / désactiver la fonctionnalité** bouton vous permet d’activer ou désactiver la diffusion multimédia.|  
|Qualité de diffusion vidéo|La flèche déroulante vous permet de choisir la qualité de diffusion des vidéos qui sont lues à partir du serveur.|  
|Bibliothèque multimédia|Affiche le nom de la bibliothèque multimédia. Le nom par défaut de la bibliothèque est appelé **bibliothèque multimédia numérique**, qui est créé lorsque vous activez la diffusion multimédia en continu. Pour modifier le nom de la bibliothèque multimédia, voir [renommer la bibliothèque multimédia](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_8). Vous pouvez cliquer sur **personnaliser** pour personnaliser les dossiers qui sont partagés dans la bibliothèque multimédia.|  
  
 Pour plus d’informations, voir [autoriser ou limiter l’accès à une bibliothèque multimédia sur le serveur](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_6) et [partage de médias protégés contre la copie](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_1.5).  
  
##  <a name="BKMK_3"></a>Fonctionnement de la diffusion multimédia en continu  
 La fonctionnalité de diffusion multimédia dans WindowsServerEssentials rend possible pour les ordinateurs en réseau et certains appareils multimédias numériques en réseau de lire des fichiers multimédias numériques qui sont stockés sur le serveur.  
  
 Lorsque vous activez le serveur multimédia, le contenu que vous partagez dans les bibliothèques multimédias sera disponible sur les périphériques sur votre réseau qui sont capables de recevoir la diffusion multimédia en continu à partir de votre serveur. Vous pouvez diffuser la plupart des types de fichiers multimédias numériques. Les types de fichiers que vous pouvez diffuser plus courants sont les suivantes:  
  
-   Formats WindowsMedia (.asf, .wma, .wmv, .wm)  
  
-   Audio Visual Interleave (.avi)  
  
-   Images animées Experts Group (.mpeg, .mpg, .mp3)  
  
-   Audio pour Windows (.wav)  
  
-   CD piste Audio (.cda)  
  
 Pour lire un fichier, il vous suffit de rechercher une chanson, vidéo, ou l’image dans un dossier partagé, double-cliquez sur le fichier et le contenu sera diffuser en continu à partir du serveur sur votre ordinateur et lire. Pour plus d’informations sur la façon de rechercher et lire les fichiers multimédias numériques qui sont stockés sur le serveur, consultez [Play Digital Media](../use/Play-Digital-Media-in-Windows-Server-Essentials.md).  
  
 Pour diffuser vos contenus multimédias, vous avez besoin du matériel suivant:  
  
-   Un réseau privé câblé ou sans fil  
  
-   Un autre ordinateur sur votre réseau ou un périphérique appelé récepteur multimédia numérique (parfois appelé un lecteur multimédia numérique en réseau). Récepteurs multimédias numériques sont des périphériques matériels connectés à votre réseau filaire ou sans fil que vous pouvez contrôler à l’aide de votre ordinateur? même si votre ordinateur se trouve dans une autre pièce.  
  
 Pour plus d’informations, voir [activer ou désactiver la diffusion multimédia](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_4).  
  
##  <a name="BKMK_4"></a>Activer ou désactiver la diffusion multimédia  
 Vous pouvez partager de musique, vidéos et des images à partir de WindowsServerEssentials en diffusant les fichiers vers tout récepteur multimédia numérique pris en charge (DMR) tels que les ordinateurs, téléphones mobiles, téléviseurs, récepteurs numériques, extensions pour WindowsMediaCenter (y compris Xbox 360) et autres appareils personnels électroniques.  
  
 Pour obtenir la liste actuelle des appareils multimédias numériques qui sont compatibles avec WindowsServerEssentials, consultez le [centre de compatibilité Windows](https://www.microsoft.com/windows/compatibility/CompatCenter/Home).  
  
### <a name="enabling-media-sharing"></a>Activation du partage multimédia  
 Pour partager le contenu multimédia stocké dans WindowsServerEssentials, vous devez activer la diffusion multimédia en continu. Diffusion multimédia en continu est désactivée par défaut.  
  
####  <a name="BKMK_2.5"></a>Pour activer ou désactiver la diffusion multimédia  
  
1.  Ouvrez le tableau de bord Windows Server Essentials.  
  
2.  Cliquez sur **paramètres**, cliquez sur **Media**, puis effectuez l’une des opérations suivantes:  
  
    -   Cliquez sur **activer** pour commencer à partager tous les fichiers qui sont stockés dans la bibliothèque multimédia du serveur.  
  
    -   Cliquez sur **désactiver** pour cesser de partager tous les fichiers qui sont stockés dans la bibliothèque multimédia du serveur.  
  
3.  Si vous voulez partager des dossiers supplémentaires dans la bibliothèque multimédia, cliquez sur **personnaliser**, puis sélectionnez **Oui** pour chaque dossier partagé que vous souhaitez inclure dans la bibliothèque multimédia.  
  
4.  Cliquez sur **OK** pour enregistrer vos modifications.  
  
 Pour plus d’informations sur les types de médias numériques pris en charge par le lecteur WindowsMedia, consultez [pris en charge par le lecteur WindowsMedia de types de fichiers](https://support.microsoft.com/kb/316992).  
  
 Pour plus d’informations, voir [autoriser ou limiter l’accès à une bibliothèque multimédia sur le serveur](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_6).  
  
##  <a name="BKMK_5"></a>Ajouter des fichiers multimédias numériques sur le serveur  
 L’administrateur du serveur peut ajouter des médias numériques à des dossiers partagés dans la bibliothèque multimédia, en accédant directement au serveur, ou en utilisant le site accès Web à distance pour se connecter au tableau de bord. Autres utilisateurs peuvent ajouter des fichiers multimédias au serveur à l’aide de la **dossiers partagés** connexion sur le Launchpad, à l’aide du site accès Web à distance, ou à l’aide de l’application mon serveur pour Windows Phone. Pour plus d’informations sur la lecture de médias, consultez [Play Digital Media](../use/Play-Digital-Media-in-Windows-Server-Essentials.md).  
  
> [!NOTE]
>  Vous pouvez également télécharger des fichiers multimédias au serveur à l’aide de l’application mon serveur pour Windows Phone. Vous pouvez télécharger l’application mon serveur à partir de la [store Windows Phone](http://www.windowsphone.com/store/app/my-server/6c2f98d5-6fcf-4e1d-b8b1-cde62ea1a94a). Pour plus d’informations sur l’application mon serveur pour Windows phone, consultez le blog [application mon serveur pour WindowsServerEssentials](http://blogs.technet.com/b/sbs/archive/2012/09/18/my-server-phone-app-for-windows-server-2012-essentials.aspx).  
  
#### <a name="to-add-digital-media-files-to-shared-folders-on-the-server"></a>Pour ajouter des fichiers multimédias numériques à des dossiers partagés sur le serveur  
  
1.  Utilisez une des méthodes suivantes pour vous connecter au serveur:  
  
    1.  Pour plus d’informations sur la connexion accès Web à distance, consultez [ouvrez une session sur l’accès Web à distance](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1).  
  
    2.  Pour plus d’informations sur la journalisation du Launchpad, consultez [vue d’ensemble de Launchpad](Overview-of-the-Launchpad-in-Windows-Server-Essentials.md).  
  
2.  Recherchez et cliquez sur le dossier pour le média que vous ajoutez.  
  
3.  Copiez et collez ou glisser-déplacer les fichiers multimédias que vous souhaitez ajoutent au dossier partagé approprié sur le serveur.  
  
##  <a name="BKMK_6"></a>Autoriser ou limiter l’accès à une bibliothèque multimédia sur le serveur  
  
-   Lorsque vous activez le partage de médias, il crée quatre dossiers prédéfinis: musique, images, vidéos et TV enregistrée. Si un de ces dossiers existe déjà sur le serveur, le dossier existant est réutilisé comme dossier partagé pour le partage de médias. Toutes les les dossier s media utilisateur et contenu autorisations existantes sont conservées, et ils sont partagés avec tous les utilisateurs du réseau.  
  
-   Avant d’activer le partage de bibliothèque multimédia pour un dossier partagé, vous devez savoir que le partage de bibliothèque multimédia ignore tout type d’accès de compte d’utilisateur que vous définissez pour le dossier partagé. Par exemple, supposons que vous activez le partage de bibliothèque multimédia pour le **Photos** dossier partagé, et si vous définissez le **Photos** du dossier partagé **aucun accès** pour un utilisateur compte nommé Bobby. Bobby peut toujours diffuser en continu tout support numérique à partir de la **vidéos** dossier partagé à n’importe quel lecteur multimédia numérique pris en charge ou à DMR. Si vous avez des médias numériques que vous ne souhaitez pas diffuser de cette manière, stockez les fichiers dans un dossier qui n’a pas de partage de bibliothèque multimédia est activée.  
  
-   Si vous activez le partage de bibliothèque multimédia pour un dossier partagé, tout lecteur multimédia numérique pris en charge ou les DMR qui peut accéder à votre réseau WindowsServerEssentials peut également accéder contenus numériques du dossier partagé. Par exemple, si vous disposez d’un réseau sans fil et que vous ne le n'avez pas sécurisé, quiconque à portée de votre réseau sans fil peut potentiellement accéder aux contenus numériques dans ce dossier. Avant d’activer le partage de bibliothèque multimédia, assurez-vous de sécuriser votre réseau sans fil. Pour plus d’informations, consultez la documentation de votre point d’accès sans fil.  
  
##  <a name="BKMK_8"></a>Renommer la bibliothèque multimédia  
 Le nom par défaut de la bibliothèque multimédia est **serveur multimédia numérique**. Il est créé lorsque vous activez la diffusion multimédia en continu dans WindowsServerEssentials. Pour plus d’informations sur l’activation de la diffusion multimédia en continu, consultez [pour activer ou désactiver la diffusion multimédia](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2.5). Vous pouvez modifier le nom de la bibliothèque multimédia à tout moment à l’aide du tableau de bord du serveur.  
  
#### <a name="to-rename-the-media-library"></a>Pour renommer la bibliothèque multimédia  
  
1.  Ouvrez le tableau de bord du serveur. Pour ouvrir le tableau de bord à partir du Launchpad, cliquez sur **tableau de bord**, tapez le mot de passe du serveur, puis cliquez sur la flèche pour vous connecter.  
  
2.  Dans le tableau de bord du serveur, cliquez sur **paramètres**.  
  
3.  Sur le **paramètres**, cliquez sur le **Media** onglet.  
  
4.  Dans le **bibliothèque multimédia** section de la **paramètres multimédias**, cliquez sur le nom de votre bibliothèque multimédia.  
  
5.  Dans le **modifier le nom de la bibliothèque multimédia** boîte de dialogue, tapez un nouveau nom pour la bibliothèque multimédia, puis cliquez sur **OK**.  
  
##  <a name="BKMK_9"></a>Arrêter le partage de médias numériques  
 L’administrateur du serveur peut arrêter de partager des contenus multimédias numériques qui sont stockées dans des dossiers partagés sur un serveur exécutant WindowsServerEssentials.  
  
#### <a name="to-stop-sharing-media-in-shared-folders"></a>Pour arrêter le partage de médias dans les dossiers partagés  
  
1.  Ouvrez le tableau de bord du serveur.  
  
2.  Sur le tableau de bord **accueil**, cliquez sur **le programme d’installation**, cliquez sur **configurer le serveur multimédia**, puis cliquez sur **cliquez pour configurer le serveur multimédia**.  
  
3.  Sur le **Media** page de paramètres, vous pouvez choisir une des options suivantes:  
  
    -   Cliquez sur **désactiver** pour cesser de partager tous les fichiers qui sont stockés sur le serveur.  
  
    -   Cliquez sur **personnaliser**, puis sélectionnez **non** pour les dossiers que vous souhaitez arrêter le partage.  
  
4.  Cliquez sur **appliquer** ou **OK** pour enregistrer vos modifications.  
  
##  <a name="BKMK_10"></a>Permettre aux appareils multimédias qui utilisent le protocole Server Message Block (SMB) pour accéder aux fichiers partagés sur le serveur  
 Les périphériques qui utilisent le bloc de Message serveur (SMB) pour le partage de fichiers et d’accès réseau au lieu de DLNA (pour la diffusion multimédia en continu) nécessitent que le compte invité est activé. Cela permet de n’importe quel périphérique ou un utilisateur sur le réseau pour afficher le contenu des dossiers partagés sans authentification.  
  
> [!CAUTION]
>  Lorsque vous activez le compte invité, tout le monde peut accéder aux ressources partagées sur le serveur par défaut.  
  
##  <a name="BKMK_CommonProcessors"></a>Processeurs courants et profils vidéo que pris en charge  
 Pour diffuser des fichiers multimédias à partir de votre serveur WindowsServerEssentials, vous pouvez utiliser un ordinateur qui exécute le système d’exploitation Windows7 ou Windows8 ou autres appareils en réseau (par exemple, des lecteurs multimédias numériques) ou des unités Media Center (par exemple, Xbox 360). Lorsque vous n’êtes pas votre réseau, utilisez le lecteur Media accès Web à distance pour lire des fichiers qui sont stockés sur votre serveur.  
  
 Vous avez besoin d’un taux de transfert de données entre 200Kbits/s et 10Mbits/s. Vous devez utiliser des formats multimédias que votre ordinateur et les périphériques reconnus et lus. Tous les périphériques prenant en charge les mêmes formats multimédias, il doit y avoir un moyen pour votre ordinateur et les périphériques de lire les fichiers multimédias que vous avez.  
  
  WindowsServerEssentials contient prend en charge le transcodage qui détermine la capacité de l’ordinateur ou le périphérique, puis convertit dynamiquement un fichier vidéo non pris en charge une prise en charge. En règle générale, si le lecteur WindowsMedia 12 peut lire le contenu sur un ordinateur qui exécute Windows7 ou Windows8, le contenu sur le serveur sera lu sur le périphérique connecté au réseau.  
  
 Le taux de format et bits choisi pour le transcodage est dépendent fortement les performances du processeur du serveur. Les performances du processeur sont identifié dans le cadre de l’indice de performance Windows. Pour déterminer les performances de votre serveur, effectuez l’une des opérations suivantes:  
  
-   Sur un ordinateur réseau exécutant Windows7 ou Windows8 qui possède le même processeur que votre serveur, accédez à la **le panneau de configuration**, cliquez sur **outils et les informations relatives aux performances**, puis passez en revue les informations sur la **taux et améliorer les performances de votre ordinateur s** page.  
  
-   Contactez le fabricant du processeur.  
  
 Pour une expérience utilisateur optimale, choisissez une qualité qui convient pour le processeur du serveur de diffusion vidéo. Le serveur ajuste automatiquement la vitesse de transmission à un de ces paramètres:  
  
-   **Faible** si le score du processeur est inférieur à 3.6.  
  
-   **Moyenne** si le score du processeur est compris entre 3.6 et 4.2.  
  
-   **Haute** si le score du processeur est compris entre 4.2 et 6.0.  
  
-   **Meilleures** si le score du processeur est supérieur à 6.0.  
  
 Si vous choisissez une résolution qui nécessite plus de puissance de traitement que votre serveur de diffusion vidéo, vous pouvez rencontrer des mémoires tampons et s’arrête pendant la diffusion à partir du serveur.  
  
> [!NOTE]
>  Pour diffuser en continu haute définition vidéo par le biais d’accès Web à distance, vous devez un processeur avec un score d’au moins 6.0.  
  
##  <a name="BKMK_KnownIssues"></a>Problèmes connus avec les types de fichiers multimédias  
 La fonctionnalité de diffusion multimédia dans accès Web à distance utilise le Service lecteur WindowsMedia 12 réseau partage. Diffusion de contenu multimédia accès Web à distance prend en charge l’audio, vidéo et types de fichier image sont pris en charge par le lecteur WindowsMedia 12 et Silverlight 4.  
  
 Le tableau suivant répertorie les types de fichier (formats) pris en charge par l’accès Web à distance diffusion multimédia en continu. S’il existe sur votre serveur, les types qui ne sont pas inclus dans la table de fichiers multimédias, vous ne pouvez pas les diffuser via l’accès Web à distance diffusion multimédia en continu.  
  
|Type de fichier|Extension de nom de fichier|  
|---------------|-------------------------|  
|Fichiers 3GPP|.3GP, .3gpp, .3g2 et .3gp2|  
|Fichiers audio de flux de Transport de données (ADTS) audio|.ADTS et .adt|  
|Fichiers audio|.au et .snd|  
|Fichiers audio AIFF Interchange File Format () audio|.aif, .aifc et .aiff|  
|Fichiers AVCHD|.m2ts, .m2t et .mts|  
|Disque audio CD|.cda|  
|Disque DVD vidéo|.vob|  
|Fichiers d’image JPEG|.jpg|  
|Fichiers audio MP3|.MP3 et .m3u|  
|Fichiers vidéo MPEG|.MPEG, .mpg, .m1v, .mpa, .mpe, .mp4, .mp4v, .m4v, .m4a et .mov|  
|Fichiers audio et vidéo Windows|.avi et .wav|  
|Fichiers audio et vidéos WindowsMedia|.asf, .asx, .wax, .wm, .wma, .wmd, .wmp, .wmv, .wmx, .wpl et .wvx|  
  
> [!NOTE]
>  Si vous ne pouvez pas utiliser un type de fichier qui est répertorié dans le tableau suivant, le fichier peut également être encodé avec un codec qui n’est pas pris en charge par le lecteur WindowsMedia.  
  
 Pour plus d’informations sur les formats de fichiers pris en charge, voir [pris en charge par le lecteur WindowsMedia de types de fichiers](https://go.microsoft.com/fwlink/p/?LinkID=196118) et [pris en charge les formats multimédias, protocoles et champs de journal](https://go.microsoft.com/fwlink/p/?LinkId=203339) pour Silverlight.  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Lire des médias numériques](../use/Play-Digital-Media-in-Windows-Server-Essentials.md)  
  
-   [Gérer WindowsServerEssentials](Manage-Windows-Server-Essentials.md)
