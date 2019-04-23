---
title: Créer des bureaux virtuels Windows 10 Entreprise pour les stations
description: Découvrez comment créer des postes de travail Windows Server 2016 pour station
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 63f08b5b-c735-41f4-b6c8-411eff85a4ab
author: evaseydl
ms.author: evas
manager: scottman
ms.openlocfilehash: befd784f4a2179c121992057e298d4ea9068c11b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862080"
---
# <a name="create-windows-10-enterprise-virtual-desktops-for-stations"></a>Créer des bureaux virtuels Windows 10 Entreprise pour les stations
Cette configuration facultative dans MultiPoint Services est conçue pour les situations où une application essentielle nécessite sa propre instance d’un système d’exploitation de client pour chaque utilisateur. Exemples : applications qui ne peut pas être installées sur Windows Server et les applications qui s’exécuteront pas plusieurs instances sur le même ordinateur hôte.  
  
> [!NOTE]  
> Ces bureaux virtuels, également appelé infrastructure VDI, sont beaucoup plus de ressources que les sessions de bureau de MultiPoint Services par défaut, donc nous vous recommandons d’utiliser les sessions de MultiPoint Services par défaut lorsque cela est possible.  
  
## <a name="prerequisites"></a>Prérequis  
Pour vous préparer à créer des bureaux virtuels station, vérifiez que vos Services MultiPoint système remplit les conditions suivantes :      
  
|Matériel|Configuration requise|         |
|------------|----------------|----------------| 
|Processeur (multimédia)|1 core ou thread par machine virtuelle|  
|Solid State Drive (SSD)|Capacité > = 20 Go par station + 40 Go pour les Services MultiPoint de système d’exploitation hôte<br /><br />Lecture aléatoire\/écrire les e/s > = 3 K par station|  
|RAM|2 Go par station + 2 Go pour le système de d’exploitation hôte Windows MultiPoint Server|  
|Graphismes|DX11|  
|BIOS|Paramètres de l’UC de BIOS configuré pour activer la virtualisation – adresse traduction (second niveau)|  
  
-   **Stations** -configurer les stations pour votre système MultiPoint Services. Pour plus d’informations, consultez [attacher des stations supplémentaires à MultiPoint Services](Attach-additional-stations-to-your-MultiPoint-services-computer.md).  
  
-   **Domaine** : dans un environnement de domaine, l’ordinateur Windows MultiPoint Server a été ajouté au domaine, et un utilisateur de domaine a été ajouté au groupe Administrateurs local sur le système d’exploitation hôte MultiPoint Services.  
  
## <a name="procedures"></a>Procédures  
Utilisez les procédures suivantes pour :  
  
-   [Créer un modèle pour les bureaux virtuels](#a-namebkmkcreateatemplateacreate-a-template-for-virtual-desktops)  
  
-   [Créer des bureaux virtuels à partir du modèle](#BKMK_CreateVirtualDesktopsfromTemplate)  
  
-   [Copier un modèle de bureau virtuel existant](#BKMK_CopyExiistingVirtualDesktopTemplate)  
  
### <a name="BKMK_CreateaTemplate"></a>Créer un modèle pour les bureaux virtuels  
Avant de pouvoir créer un modèle pour vos bureaux virtuels, vous devez activer la fonctionnalité de bureau virtuel dans MultiPoint Server.  
  
##### <a name="to-enable-the-virtual-desktop-feature"></a>Pour activer la fonctionnalité de bureau virtuel  
  
1.  Connectez-vous au système d’exploitation hôte MultiPoint Server avec un compte d’administrateur local ou, dans un domaine, avec un compte de domaine qui est membre du groupe Administrateurs local.  
  
2.  À partir de la **Démarrer** écran, ouvrez le gestionnaire MultiPoint.  
  
3.  Cliquez sur le **bureaux virtuels** , cliquez sur **activer les bureaux virtuels**, puis cliquez sur **OK**et attendez que le redémarrage du système.  
  
L’étape suivante consiste à créer un modèle de bureau virtuel. Vous créez un fichier de disque dur virtuel (VHD) que vous pouvez utiliser comme modèle pour créer des bureaux de station virtuels pour le gestionnaire MultiPoint littéralement. Vous pouvez utiliser le support d’installation physique pour Windows ou un. Fichier image ISO à en tant que source pour le modèle. Vous pouvez également utiliser un. Disque dur virtuel de l’installation de Windows. Notez que pour utiliser un disque d’installation physique, vous devez insérer le disque avant de démarrer l’Assistant.  
  
##### <a name="to-create-a-virtual-desktop-template"></a>Pour créer un modèle de bureau virtuel  
  
1.  Connectez-vous au système d’exploitation hôte MultiPoint Server avec un compte d’administrateur local ou, dans le domaine, un compte de domaine qui est membre du groupe Administrateurs local.  
  
2.  À partir de la **Démarrer** écran, ouvrez le gestionnaire MultiPoint.  
  
3.  Cliquez sur le **bureaux virtuels** onglet.  
  
4.   Copier un fichier .iso de Windows 10 entreprise vers le disque SSD local.  
  
5.  Sous l’onglet de bureaux virtuels, cliquez sur **créer de modèle de bureau virtuel.**   
  
6.  Dans **préfixe**, entrez un préfixe à utiliser pour identifier le modèle et les bureaux virtuels créés avec le modèle. Le préfixe par défaut est le nom d’ordinateur hôte.  
  
    Le préfixe est utilisé pour donner un nom au modèle et aux stations de bureaux virtuels. Le modèle sera <*préfixe*>-t. Les stations de bureaux virtuels portera <*préfixe*>-*n*, où *n* est l’identificateur de station.  
  
7.  Entrez un nom d’utilisateur et le mot de passe à utiliser pour le compte d’administrateur local pour le modèle. Dans un domaine, entrez les informations d’identification pour un compte de domaine qui sera ajouté au groupe Administrateurs local. Ce compte peut être utilisé pour ouvrir une session le modèle et toutes les stations de bureaux virtuels créés à partir du modèle.  
  
8. Cliquez sur **OK**et attendez la fin de la création de modèle.  
  
9. Le nouveau modèle apparaît sur le **bureaux virtuels** onglet. Le modèle est désactivé.  
  
L’étape suivante consiste à configurer le modèle avec le logiciel et le paramètre que vous voulez sur les bureaux virtuels. Vous devez effectuer cette opération avant de créer des bureaux virtuels à partir du modèle.  
  
##### <a name="to-customize-a-virtual-desktop-template"></a>Pour personnaliser un modèle de bureau virtuel  
  
1.  Connectez-vous au système d’exploitation hôte serveur MultiPoint avec un compte d’administrateur local ou, dans un domaine, avec un compte de domaine dans le groupe Administrateurs local.  
  
2.  À partir de la **Démarrer** écran, ouvrez le gestionnaire MultiPoint.  
  
3.  Cliquez sur le **bureaux virtuels** onglet.  
  
4.  Sélectionnez le modèle que vous souhaitez personnaliser, cliquez sur **personnaliser modèle**, puis cliquez sur **OK**.  
  
    > [!NOTE]  
    > Seuls les modèles qui n’ont pas été utilisés pour créer des stations de bureaux virtuels sont disponibles. Si vous souhaitez mettre à jour un modèle qui est déjà en cours d’utilisation, vous devez créer une copie du modèle à l’aide de la **importer le modèle** tâche, décrite plus loin, dans [copier un modèle de bureau virtuel existant](#BKMK_CopyExiistingVirtualDesktopTemplate).  
  
    Le modèle s’ouvre dans Hyper-V **machine virtuelle se connecter** fenêtre et ouverture de session automatique est effectuée à l’aide du compte administrateur intégré.  
  
5.  À ce stade vous pouvez installer des applications et mises à jour logicielles, modifier les paramètres et mettre à jour le profil d’administrateur. Toutes les modifications apportées au profil d’administrateur intégré du modèle seront copiées vers le profil utilisateur par défaut dans les stations de bureaux virtuels qui sont créés à partir du modèle.  
  
    Si vous vous connectez vos stations sur un domaine, nous vous recommandons de créer un compte d’utilisateur local et d’ajouter au groupe Administrateurs local au cours de personnalisation.  
  
    > [!NOTE]  
    > Si le redémarrage du système pendant un modèle est en cours personnalisé, ouverture de session automatique à l’aide du compte administrateur intégré peut échouer après le redémarrage du système. Pour contourner ce problème, manuellement une session à l’aide du compte administrateur local que vous avez créé, modifier le mot de passe du compte administrateur intégré, déconnectez-vous et reconnectez-vous en utilisant le compte administrateur intégré et le nouveau mot de passe. (Vous devez supprimer le profil a été créé lorsque vous connecté à l’aide du compte administrateur local.)  
  
6.  Une fois que vous avez terminé la configuration de votre système, double-cliquez sur le **CompleteCustomization** raccourci sur le bureau de l’administrateur pour exécuter Sysprep et arrêtez ensuite le modèle. Au cours de personnalisation, l’outil Sysprep supprime toutes les informations système uniques pour préparer l’installation de Windows pour l’image.  
  
### <a name="BKMK_CreateVirtualDesktopsfromTemplate"></a>Créer des postes de travail de machine virtuelle à partir du modèle  
Avec votre modèle de bureau virtuel configuré comme vous le souhaitez vos postes de travail soit, vous êtes prêt à commencer à créer des bureaux virtuels. Un bureau virtuel est créé pour chaque station est attachée à l’ordinateur MultiPoint Server. La prochaine fois qu’un utilisateur ouvre une session sur une station, ils verront le bureau virtuel au lieu du bureau basé sur session qui a été affiché avant.  
  
> [!NOTE]  
> Cette procédure fonctionne uniquement lorsque les serveur MultiPoint est en *mode station*. Si le système se trouve dans *mode console*, vous pouvez passer en mode station de gestionnaire MultiPoint. Si vous utilisez des paramètres MultiPoint par défaut, vous pouvez également démarrer en mode station en redémarrant l’ordinateur. Par défaut, l’ordinateur MultiPoint Server démarre toujours en mode station  
  
##### <a name="to-create-virtual-desktops-for-your-stations"></a>Pour créer des bureaux virtuels pour vos stations  
  
1.  Connectez-vous au serveur Windows MultiPoint à partir d’une station de travail distante (par exemple, à partir d’un ordinateur Windows à l’aide de connexion Bureau à distance) à l’aide d’un administrateur local de compte ou, dans un domaine, un compte de domaine dans le groupe Administrateurs local.  
  
    > [!NOTE]  
    > Ou bien, vous pouvez vous connecter au serveur à l’aide d’une console locale. Toutefois, lorsque vous créez un bureau virtuel station, vous devrez déconnecter de la station que vous avez utilisé pour créer le bureau virtuel afin de vous connecter l’autre station au nouveau bureau virtuel.  
  
2.  À partir de la **Démarrer** écran, ouvrez le gestionnaire MultiPoint.  
  
3.  Si l’ordinateur est en mode console, passer en mode station :  
  
    1.  Sur le **accueil** , cliquez sur **passer en mode station**.  
  
    2.  Lorsque l’ordinateur redémarre, ouvrez une session en tant qu’administrateur.  
  
4.  Cliquez sur le **bureaux virtuels** onglet.  
  
5.  Sélectionnez le modèle de bureau virtuel que vous souhaitez utiliser avec les stations, cliquez sur **créer des stations de bureaux virtuels**, puis cliquez sur **OK**.  
  
Lorsque la tâche se termine, chaque station locale se connecte à un bureau virtuel basé sur une machine virtuelle.  
  
> [!NOTE]  
> Si un compte d’utilisateur est connecté à une des stations locales, vous devez déconnecter la session pour obtenir la station pour se connecter à un des bureaux virtuels station nouvellement créé.  
  
### <a name="BKMK_CopyExiistingVirtualDesktopTemplate"></a>Copier un modèle de bureau virtuel existant  
Utilisez la procédure suivante pour créer une copie d’un modèle de bureau virtuel existant que vous pouvez personnaliser et utiliser. Il se peut que cela peut être utile dans les situations suivantes :  
  
-   Pour copier un modèle de référence à partir d’un partage réseau sur un ordinateur hôte de MultiPoint Server afin que les stations de bureaux virtuels peuvent être créées à partir du modèle principal.  
  
-   Pour créer une copie d’un modèle qui est actuellement en cours d’utilisation afin que vous pouvez créer des personnalisations supplémentaires.  
  
##### <a name="to-import-a-virtual-desktop-template"></a>Pour importer un modèle de bureau virtuel  
  
1.  Ouvrez une session en tant qu’administrateur le serveur MultiPoint.  
  
2.  À partir de la **Démarrer** écran, ouvrez le gestionnaire MultiPoint.  
  
3.  Cliquez sur le **bureaux virtuels** onglet.  
  
4.  Cliquez sur **modèle de bureau virtuel importation**et utiliser **Parcourir** pour sélectionner le fichier .vhd (modèle) que vous souhaitez importer. Lorsque vous importez un modèle, une copie est effectuée du .vhd d’origine. Par défaut, MultiPoint Services stocke les fichiers .vhd dans le lecteur C:\\utilisateurs\\Public\\Documents\\Hyper\-V\\disques durs virtuels\\ dossier.  
  
5.  Entrez un préfixe pour le nouveau modèle, puis cliquez sur **OK**.  
  
6.  Si vous effectuez des autres personnalisations à un modèle local, vous pouvez modifier le nom de préfixe en incrémentant un numéro de version à la fin du préfixe. Ou, si vous importez un modèle de référence, vous pouvez ajouter la version du modèle principal à la fin du nom de préfixe par défaut.  
  
7.  Lorsque la tâche est terminée, vous pouvez personnaliser le modèle ou l’utiliser tel qu’il doit créer des stations.  
