---
title: Gérer les supports numériques dans Windows Server Essentials
description: Décrit comment utiliser Windows Server Essentials
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
ms.openlocfilehash: 8d7c4f0432e1f840717776ee5df00d64c39b18fd
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75947486"
---
# <a name="manage-digital-media-in-windows-server-essentials"></a>Gérer les supports numériques dans Windows Server Essentials

>S’applique à : Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Les rubriques suivantes abordent les fonctions de diffusion multimédia en continu de votre serveur et expliquent comment les configurer et les utiliser sur votre réseau.  
  
> [!NOTE]
>  Par défaut, la fonctionnalité de diffusion multimédia en continu n’est pas disponible dans Windows Server Essentials et Windows Server 2012 R2 avec le rôle expérience Windows Server Essentials installé. Pour ajouter la fonctionnalité de diffusion multimédia en continu dans ces versions, [téléchargez et installez Windows Server Essentials Media Pack](https://www.microsoft.com/download/details.aspx?id=40837) dans le Centre de téléchargement Microsoft.  
  
-   [Présentation des médias numériques](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Gérer le serveur multimédia à l’aide du tableau de bord](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Fonctionnement de la diffusion multimédia en continu](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Activer ou désactiver la diffusion multimédia en continu](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [Ajouter des fichiers multimédias numériques au serveur](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Autoriser ou restreindre l’accès à une bibliothèque multimédia sur le serveur](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_6)  
  
-   [Renommer la bibliothèque multimédia](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_8)  
  
-   [Arrêter le partage de médias numériques](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_9)  
  
-   [Activer les appareils multimédias qui utilisent le protocole SMB (Server Message Block) pour accéder aux fichiers partagés sur le serveur](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_10)  
  
-   [Processeurs courants et profils vidéo qu’ils prennent en charge](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_CommonProcessors)  
  
-   [Problèmes connus avec les types de fichiers multimédias](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_KnownIssues)  
  
##  <a name="BKMK_1"></a>Présentation des médias numériques  
 Les éléments multimédias numériques font référence à du contenu audio, vidéo et à des photos qui ont été encodés (compressés numériquement). Le codage du contenu implique la conversion de l'entrée audio et vidéo en un fichier multimédia, comme un fichier Windows Media. Une fois le média numérique codé, il peut être facilement manipulé, distribué et lu par des ordinateurs. Il peut aussi être facilement transmis sur des réseaux d'ordinateurs.  
  
 Voici des exemples de types de médias numériques : WMA (Windows Media Audio), WMV (Windows Media Video), MP3, JPEG et AVI. Pour plus d’informations sur les types de médias numériques qui sont pris en charge par le Lecteur Windows Media, consultez la rubrique [Types de fichiers pris en charge par le Lecteur Windows Media](https://support.microsoft.com/kb/316992).  
  
### <a name="why-would-i-want-to-stream-my-digital-media"></a>Pourquoi voudrais-je diffuser en continu mes médias numériques ?  
 De nombreuses personnes stockent de la musique, des vidéos et des images dans des dossiers partagés dans Windows Server Essentials. Vous pouvez avoir envie de faire ce qui suit :  
  
-   **Regarder des vidéos**. Votre serveur peut être utilisé pour stocker et diffuser de grandes collections de vidéos et des programmes de télévision enregistrés sur vos ordinateurs ou sur d'autres appareils de votre réseau. Vous pouvez diffuser en continu des vidéos sur une Xbox 360 ou sur un ordinateur à l'aide du Lecteur Windows Media.  
  
-   **Écouter de la musique**. Quand vous activez le partage de fichiers multimédias pour le dossier partagé **Musique** , vous pouvez accéder à votre musique à partir d'appareils qui prennent en charge Windows Media Connect. Vous n'avez pas besoin d'activer ou de configurer des comptes d'utilisateur pour diffuser en continu à partir du dossier partagé **Musique** une fois que le partage est activé.  
  
-   **Présenter des diaporamas de photos**. Vous pouvez stocker des photos numériques dans le dossier partagé **Photos** sur votre serveur, puis y accéder à partir de n'importe quel ordinateur ou d'une Xbox 360 connectée à une télévision à votre domicile ou au bureau. Vous pouvez regarder des diaporamas de photos, ce qui revient à transformer votre télévision en un grand cadre à image.  
  
###  <a name="BKMK_1.5"></a>Partage de médias protégés contre la copie  
  Windows Server Essentials ne prend pas en charge le partage de médias protégés contre la copie. Ceci inclut la musique achetée auprès d'un magasin de musique en ligne.  
  
 Un média protégé contre la copie peut être lu seulement sur l'ordinateur ou l'appareil que vous avez utilisé pour l'acheter. La protection contre la copie vous empêche de lire un média sur plusieurs ordinateurs ou appareils, même si vous copiez le média sur votre serveur et que vous le lisez à partir de là. Toutefois, vous pouvez stocker le média protégé contre la copie sur Windows Server Essentials et continuer à lire le média sur l’ordinateur ou le périphérique que vous avez utilisé pour l’acheter.  
  
##  <a name="BKMK_2"></a>Gérer le serveur multimédia à l’aide du tableau de bord  
  Windows Server Essentials permet d’effectuer des tâches d’administration courantes à l’aide du tableau de bord Windows Server Essentials. L'onglet **Multimédia** de la page **Paramètres** du serveur du tableau de bord propose les éléments suivants :  
  
|Section|Fonctionnalités|  
|-------------|-------------------|  
|Serveur multimédia|Le bouton **Activer/Désactiver** vous permet d'activer ou de désactiver la diffusion multimédia en continu.|  
|Qualité de diffusion vidéo|La flèche déroulante vers le bas vous permet de choisir la qualité de diffusion des vidéos qui sont lues à partir du serveur.|  
|Bibliothèque multimédia|Affiche le nom de la bibliothèque multimédia. Le nom par défaut de la bibliothèque est **Bibliothèque multimédia numérique**, qui est créé lorsque vous activez la diffusion multimédia en continu. Pour modifier le nom de la bibliothèque multimédia, consultez la rubrique [Rename the media library](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_8). Vous pouvez cliquer sur **Personnaliser** pour personnaliser les dossiers qui sont partagés dans la bibliothèque multimédia.|  
  
 Pour plus d'informations, consultez [Allow or restrict access to a media library on the server](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_6) et [Sharing copy-protected media](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_1.5).  
  
##  <a name="BKMK_3"></a>Fonctionnement de la diffusion multimédia en continu  
 La fonctionnalité de diffusion multimédia en continu de Windows Server Essentials permet aux ordinateurs en réseau et à certains appareils multimédias numériques en réseau de lire des fichiers multimédias numériques stockés sur le serveur.  
  
 Lorsque vous activez le serveur multimédia, le contenu que vous partagez dans les bibliothèques multimédias sera disponible sur les périphériques de votre réseau qui sont capables de recevoir des contenus multimédias diffusés en continu depuis votre serveur. Vous pouvez diffuser la plupart des types de fichiers multimédias. Certains des types de fichiers les plus communs que vous pouvez diffuser sont les suivants :  
  
- Formats Windows Media (.asf, .wma, .wmv, .wm)  
  
- Audio Visual Interleave (.avi)  
  
- Moving Pictures Experts Group (.mpeg, .mpg, .mp3)  
  
- Audio pour Windows (.wav)  
  
- Piste CD Audio (.cda)  
  
  Pour lire un fichier, il vous suffit de rechercher une chanson, une vidéo ou une image dans un dossier partagé, puis de double-cliquer sur le fichier pour diffuser et lire le contenu depuis le serveur sur votre ordinateur. Pour plus d’informations sur la façon de rechercher et de lire les fichiers multimédias numériques stockés sur le serveur, consultez [lire des médias numériques](../use/Play-Digital-Media-in-Windows-Server-Essentials.md).  
  
  Pour diffuser vos contenus multimédias, il vous faut le matériel suivant :  
  
- Un réseau privé câblé ou sans fil  
  
- Un autre ordinateur sur votre réseau ou un périphérique appelé récepteur multimédia numérique (parfois appelé lecteur multimédia numérique en réseau). Les récepteurs multimédias numériques sont des périphériques matériels connectés à votre réseau câblé ou sans fil que vous pouvez contrôler à l’aide de votre ordinateur, même si votre ordinateur se trouve dans une autre pièce.  
  
  Pour plus d’informations, consultez [activer ou désactiver la diffusion multimédia en continu](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_4).  
  
##  <a name="BKMK_4"></a>Activer ou désactiver la diffusion multimédia en continu  
 Vous pouvez partager de la musique, des vidéos et des images à partir de Windows Server Essentials en diffusant des fichiers vers tout récepteur multimédia numérique (DMR) pris en charge, tel que les ordinateurs, les téléphones mobiles, les télévisions, les récepteurs numériques, les extendeurs pour Windows Media Center (y compris Xbox 360) et d’autres appareils électroniques personnels.  
  
 Pour obtenir la liste actuelle des appareils multimédias numériques compatibles avec Windows Server Essentials, consultez le [Centre de compatibilité Windows](https://www.microsoft.com/windows/compatibility/CompatCenter/Home).  
  
### <a name="enabling-media-sharing"></a>Activation du partage multimédia  
 Pour partager le média qui est stocké dans Windows Server Essentials, vous devez activer la diffusion multimédia en continu. La diffusion multimédia en continu est désactivée par défaut.  
  
####  <a name="BKMK_2.5"></a>Pour activer ou désactiver la diffusion multimédia en continu  
  
1. Ouvrez le tableau de bord Windows Server Essentials.  
  
2. Cliquez sur **Paramètres**, sur **Multimédia**, puis effectuez une des opérations suivantes :  
  
   -   Cliquez sur **Activer** pour commencer à partager tous les fichiers qui sont stockés dans la bibliothèque multimédia du serveur.  
  
   -   Cliquez sur **Désactiver** pour cesser de partager tous les fichiers qui sont stockés dans la bibliothèque multimédia du serveur.  
  
3. Si vous voulez partager des dossiers supplémentaires de la bibliothèque multimédia, cliquez sur **Personnaliser**, puis sélectionnez **Oui** pour chaque dossier partagé que vous souhaitez inclure dans la bibliothèque multimédia.  
  
4. Cliquez sur **OK** pour enregistrer vos modifications.  
  
   Pour plus d’informations sur les types de médias numériques pris en charge par le Lecteur Windows Media, consultez la rubrique [Types de fichiers pris en charge par le Lecteur Windows Media](https://support.microsoft.com/kb/316992).  
  
   Pour plus d'informations, voir [Allow or restrict access to a media library on the server](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_6).  
  
##  <a name="BKMK_5"></a>Ajouter des fichiers multimédias numériques au serveur  
 L’administrateur du serveur peut ajouter des médias numériques à des dossiers partagés dans la bibliothèque multimédia en accédant directement au serveur ou en utilisant le site de Accès web à distance pour se connecter au tableau de bord. D’autres utilisateurs peuvent ajouter des fichiers multimédias au serveur à l’aide de la connexion **dossiers partagés** sur le Launchpad, à l’aide du site de accès Web à distance ou à l’aide de l’application my server pour Windows Phone. Pour plus d’informations sur la lecture de médias, consultez [lire des médias numériques](../use/Play-Digital-Media-in-Windows-Server-Essentials.md).  
  
> [!NOTE]
>  Vous pouvez également télécharger des fichiers multimédias sur le serveur à l'aide de l'application Mon Serveur pour Windows Phone. Vous pouvez télécharger l’application My Server à partir du [Windows Phone Store](https://www.windowsphone.com/store/app/my-server/6c2f98d5-6fcf-4e1d-b8b1-cde62ea1a94a). Pour plus d’informations sur l’application my Server pour Windows Phone, consultez le billet de blog [My Server Phone App for Windows Server Essentials](https://blogs.technet.com/b/sbs/archive/2012/09/18/my-server-phone-app-for-windows-server-2012-essentials.aspx).  
  
#### <a name="to-add-digital-media-files-to-shared-folders-on-the-server"></a>Pour ajouter des fichiers multimédias numériques à des dossiers partagés sur le serveur  
  
1.  Utilisez une des méthodes suivantes pour vous connecter au serveur :  
  
    1.  Pour plus d’informations sur la connexion à l’accès web à distance, consultez la rubrique [Log on to Remote Web Access](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1).  
  
    2.  Pour plus d’informations sur la connexion avec Launchpad, consultez la [vue d’ensemble de Launchpad](Overview-of-the-Launchpad-in-Windows-Server-Essentials.md).  
  
2.  Recherchez et cliquez sur le dossier pour le média que vous ajoutez.  
  
3.  Copiez et collez ou glissez-déplacez les fichiers multimédias que vous souhaitez ajouter au dossier partagé approprié sur le serveur.  
  
##  <a name="BKMK_6"></a>Autoriser ou restreindre l’accès à une bibliothèque multimédia sur le serveur  
  
-   Lorsque vous activez le partage de médias, vous créez quatre dossiers prédéfinis : Musique, Images, Vidéos et TV enregistrée. Si l'un de ces dossiers existe déjà sur le serveur, le dossier existant est réutilisé comme dossier partagé pour le partage de fichiers multimédias. Tout le contenu multimédia et les autorisations utilisateur du dossier existant sont conservés et partagés avec tous les utilisateurs du réseau.  
  
-   Avant d'activer le partage de la bibliothèque multimédia pour un dossier partagé, vous devez savoir que ce paramétrage a priorité sur tout autre type d'accès de compte d'utilisateur que vous définissez pour le dossier partagé. Par exemple, si vous activez le partage de bibliothèque multimédia pour le dossier partagé **Photos** et que vous définissez le dossier partagé **Photos** en **Aucun accès** pour un compte d'utilisateur nommé Bobby. Bobby peut continuer à lire en diffusion continue les contenus multimédias du dossier partagé **Vidéos** sur n'importe quel lecteur multimédia ou DMR pris en charge. Si vous avez des contenus multimédias numériques que vous ne souhaitez pas diffuser de cette manière, stockez-les dans un dossier sur lequel le partage de bibliothèque multimédia n'est pas activé.  
  
-   Si vous activez le partage de bibliothèque multimédia pour un dossier partagé, tout lecteur multimédia numérique ou DMR pris en charge pouvant accéder à votre réseau Windows Server Essentials peut également accéder à vos médias numériques dans ce dossier partagé. Par exemple, si vous disposez d'un réseau sans fil et que vous ne l'avez pas sécurisé, quiconque à portée de votre réseau sans fil peut potentiellement accéder aux contenus numériques de ce dossier. Avant d'activer le partage de la bibliothèque multimédia, assurez-vous de sécuriser votre réseau sans fil. Pour plus d'informations, consultez la documentation de votre point d'accès sans fil.  
  
##  <a name="BKMK_8"></a>Renommer la bibliothèque multimédia  
 Le nom par défaut de la bibliothèque multimédia est **Serveur multimédia numérique**. Il est créé lorsque vous activez la diffusion multimédia en continu dans Windows Server Essentials. Pour plus d’informations sur l’activation de la diffusion multimédia en continu, consultez [pour activer ou désactiver la diffusion multimédia en continu](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2.5). Vous pouvez modifier le nom de la bibliothèque multimédia à tout moment à l'aide du tableau de bord du serveur.  
  
#### <a name="to-rename-the-media-library"></a>Pour renommer la bibliothèque multimédia  
  
1.  Ouvrez le tableau de bord du serveur. Pour ouvrir le tableau de bord à partir du Launchpad, cliquez sur **Tableau de bord**, tapez le mot de passe du serveur, puis cliquez sur la flèche pour vous connecter.  
  
2.  Sur le tableau de bord du serveur, cliquez sur **Paramètres**.  
  
3.  Dans la page **Paramètres** , cliquez sur l'onglet **Multimédia** .  
  
4.  Dans la section **Bibliothèque multimédia** de la page **Paramètres multimédias** , cliquez sur le nom de votre bibliothèque multimédia.  
  
5.  Dans la boîte de dialogue **Modifier le nom de la bibliothèque multimédia** , tapez un nouveau nom pour la bibliothèque multimédia, puis cliquez sur **OK**.  
  
##  <a name="BKMK_9"></a>Arrêter le partage de médias numériques  
 L’administrateur du serveur peut cesser de partager des contenus multimédias numériques stockés dans des dossiers partagés sur un serveur exécutant Windows Server Essentials.  
  
#### <a name="to-stop-sharing-media-in-shared-folders"></a>Pour arrêter le partage de contenus multimédias de dossiers partagés  
  
1.  Ouvrez le tableau de bord du serveur.  
  
2.  Dans la page **Accueil** du tableau de bord, cliquez sur **Configuration**, sur **Configurer le serveur multimédia**, puis sur **Cliquez pour configurer le serveur multimédia**.  
  
3.  Dans la page des paramètres **Multimédia** , vous pouvez choisir une des options suivantes :  
  
    -   Cliquez sur **Désactiver** pour cesser de partager tous les fichiers stockés sur le serveur.  
  
    -   Cliquez sur **Personnaliser**, puis sélectionnez **Non** pour les dossiers que vous voulez plus partager.  
  
4.  Cliquez sur **Appliquer** ou sur **OK** pour enregistrer vos modifications.  
  
##  <a name="BKMK_10"></a>Activer les appareils multimédias qui utilisent le protocole SMB (Server Message Block) pour accéder aux fichiers partagés sur le serveur  
 Les périphériques qui utilisent le protocole SMB pour l'accès aux fichiers réseau partagés au lieu de DLNA (pour la diffusion multimédia en continu) nécessitent l'activation du compte d'invité. Cela permet à tout appareil ou utilisateur du réseau de consulter le contenu des dossiers partagés sans authentification.  
  
> [!CAUTION]
>  Lorsque vous activez le compte d'invité, tous les utilisateurs peuvent accéder aux ressources partagées sur le serveur par défaut.  
  
##  <a name="BKMK_CommonProcessors"></a>Processeurs courants et profils vidéo qu’ils prennent en charge  
 Pour diffuser du contenu multimédia à partir de votre serveur Windows Server Essentials, vous pouvez utiliser un ordinateur qui exécute le système d’exploitation Windows 7 ou Windows 8 ou d’autres appareils en réseau (tels que des lecteurs multimédias numériques) ou des unités Media Center Extender (par exemple, Xbox 360). Lorsque vous n'êtes pas à votre réseau, utilisez le lecteur multimédia de l'accès web à distance pour lire les fichiers qui sont stockés sur votre serveur.  
  
 Il vous faut un taux de transfert de données compris entre 200 Kbits/s et 10 Mbits/s. Vous devez utiliser des formats de contenus multimédias reconnus et lus par vos ordinateur et périphériques. Tous les périphériques ne prenant pas en charge les mêmes formats multimédias, il doit y avoir un moyen pour votre ordinateur et les périphériques de lire vos fichiers multimédias.  
  
  Windows Server Essentials contient une prise en charge du transcodage qui détermine la capacité de l’ordinateur ou de l’appareil, puis convertit dynamiquement un fichier vidéo non pris en charge en un fichier pris en charge. En règle générale, si le lecteur Windows Media 12 peut lire le contenu sur un ordinateur qui exécute Windows 7 ou Windows 8, le contenu présent sur le serveur sera lu sur le périphérique connecté au réseau.  
  
 Le format et la vitesse de transmission choisis pour le transcodage dépendent largement des performances du processeur du serveur. Les performances du processeur font partie de l'Indice de performance Windows. Pour déterminer les performances de votre serveur, effectuez l'une des opérations suivantes :  
  
- Sur un ordinateur réseau exécutant Windows 7 ou Windows 8 qui possède le même processeur que votre serveur, accédez au **panneau de configuration**, cliquez sur **informations et outils de performance**, puis passez en revue les informations de la page **fréquence et amélioration des performances de votre ordinateur** .  
  
- Contactez le fabricant du processeur.  
  
  Pour une expérience utilisateur optimale, choisissez une qualité de diffusion vidéo appropriée au processeur de votre serveur. Le serveur ajuste automatiquement la vitesse de transmission à l'un de ces paramètres :  
  
- **Faible** si le score du processeur est inférieur à 3.6.  
  
- **Moyenne** si le score du processeur est compris entre 3.6 et 4.2.  
  
- **Élevée** si le score du processeur est compris entre 4.2 et 6.0.  
  
- **Meilleure** si le score du processeur est supérieur à 6.0.  
  
  Si vous choisissez une résolution de diffusion vidéo qui nécessite plus de puissance de traitement que votre serveur, vous pouvez rencontrer mises en mémoire tampon et des arrêts de lecture pendant la diffusion à partir du serveur.  
  
> [!NOTE]
>  Pour diffuser une vidéo en haute définition via l'accès web à distance, vous devez disposer d'un processeur dont le score est d'au moins 6.0.  
  
##  <a name="BKMK_KnownIssues"></a>Problèmes connus avec les types de fichiers multimédias  
 La fonctionnalité de diffusion multimédia par accès web à distance utilise le Service Partage réseau du Lecteur Windows Media 12. La diffusion multimédia en continu de l'accès web à distance prend en charge les types de fichiers audio, vidéo et d'image qui sont pris en charge par le lecteur Windows Media 12 et Silverlight 4.  
  
 Le tableau suivant répertorie les types de fichier (formats) qui sont pris en charge par la diffusion multimédia en continu de l'accès web à distance. S'il y a sur votre serveur des types de fichier qui ne sont pas inclus dans le tableau, vous ne pouvez pas les diffuser via la diffusion multimédia en continu de l'accès web à distance.  
  
|Type de fichier|Extension de nom de fichier|  
|---------------|-------------------------|  
|Fichiers 3GPP|.3GP, .3gpp, .3g2 et .3gp2|  
|Fichiers audio ADTS (Audio Data Transport Stream)|.adts et .adt|  
|Fichiers audio AU|.au et .snd|  
|Fichiers audio AIFF (Audio Interchange File Format)|.aif, aifc et .aiff|  
|Fichiers AVCHD|.m2ts, .m2t et .mts|  
|Disque audio CD|.cda|  
|Disque DVD vidéo|.vob|  
|Fichiers d'image JPEG|.jpg|  
|Fichiers audio MP3|.mp3 et .m3u|  
|Fichiers vidéo MPEG|.mpeg, .mpg, .m1v, .mpa, .mpe, .mp4, .mp4v, .m4v, .m4a et .mov|  
|Fichiers audio et vidéo Windows|.avi et .wav|  
|Fichiers audio et vidéo Windows Media|.asf, .asx, .wax, .wm, .wma, .wmd, .wmp, .wmv, .wmx, .wpl et .wvx|  
  
> [!NOTE]
>  Si vous ne pouvez pas utiliser un type de fichier répertorié dans ce tableau, le fichier peut également être encodé avec un codec qui n'est pas pris en charge par le lecteur Windows Media.  
  
 Pour plus d’informations sur les formats de fichiers pris en charge, consultez les rubriques [Types de fichiers pris en charge par le lecteur Windows Media](https://go.microsoft.com/fwlink/p/?LinkID=196118) et [Formats, protocoles et champs de journal multimédias pris en charge](https://go.microsoft.com/fwlink/p/?LinkId=203339) pour Silverlight.  
  
## <a name="see-also"></a>Articles associés  
  
-   [Lire des médias numériques](../use/Play-Digital-Media-in-Windows-Server-Essentials.md)  
  
-   [Gérer Windows Server Essentials](Manage-Windows-Server-Essentials.md)
