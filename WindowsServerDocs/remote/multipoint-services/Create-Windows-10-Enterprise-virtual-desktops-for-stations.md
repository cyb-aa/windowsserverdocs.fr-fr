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
ms.openlocfilehash: e68412808e037b788d5b25c1c2c7b14253e40ea6
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871729"
---
# <a name="create-windows-10-enterprise-virtual-desktops-for-stations"></a>Créer des bureaux virtuels Windows 10 Entreprise pour les stations
Cette configuration facultative dans MultiPoint services est principalement destinée aux situations où une application essentielle requiert sa propre instance d’un système d’exploitation client pour chaque utilisateur. Les exemples incluent des applications qui ne peuvent pas être installées sur Windows Server et les applications qui n’exécutent pas plusieurs instances sur le même ordinateur hôte.  
  
> [!NOTE]  
> Ces bureaux virtuels, également appelés « VDI », sont beaucoup plus gourmands en ressources que les sessions de bureau MultiPoint services par défaut. nous vous recommandons donc d’utiliser les sessions MultiPoint services par défaut dans la mesure du possible.  
  
## <a name="prerequisites"></a>Prérequis  
Pour préparer la création de bureaux virtuels de station, assurez-vous que votre système MultiPoint services remplit les conditions suivantes :      
  
|Matériel|Configuration requise|         |
|------------|----------------|----------------| 
|UC (multimédia)|1 cœur ou thread par ordinateur virtuel|  
|SSD (Solid State Drive)|Capacité > = 20 Go par station + 40 Go pour le système d’exploitation hôte MultiPoint services<br /><br />E/\/s par seconde en lecture/écriture aléatoire > = 3k par station|  
|RAM|2 Go par station + 2 Go pour le système d’exploitation hôte Windows MultiPoint Server|  
|Graphismes|DX11|  
|BIOS|Paramètre de l’UC du BIOS configuré pour activer la virtualisation-traduction d’adresses de second niveau (SLAT)|  
  
-   **Stations** : configurez les stations pour votre système multipoint services. Pour plus d’informations, consultez [attacher des stations supplémentaires à multipoint services](Attach-additional-stations-to-your-MultiPoint-services-computer.md).  
  
-   **Domaine** : dans un environnement de domaine, l’ordinateur Windows MultiPoint Server a été ajouté au domaine et un utilisateur de domaine a été ajouté au groupe Administrateurs local sur le système d’exploitation hôte multipoint services.  
  
## <a name="procedures"></a>Procédures  
Utilisez les procédures suivantes pour :  
  
-   [Créer un modèle pour les bureaux virtuels](#create-a-template-for-virtual-desktops)  
  
-   [Créer des bureaux virtuels à partir du modèle](#create-virtual-machine-desktops-from-the-template)  
  
-   [Copier un modèle de bureau virtuel existant](#copy-an-existing-virtual-desktop-template)  
  
### <a name="create-a-template-for-virtual-desktops"></a>Créer un modèle pour les bureaux virtuels  
Avant de pouvoir créer un modèle pour vos bureaux virtuels, vous devez activer la fonctionnalité de bureau virtuel dans MultiPoint Server.  
  
##### <a name="to-enable-the-virtual-desktop-feature"></a>Pour activer la fonctionnalité de bureau virtuel  
  
1.  Connectez-vous au système d’exploitation hôte du serveur MultiPoint à l’aide d’un compte d’administrateur local ou, dans un domaine, avec un compte de domaine membre du groupe Administrateurs local.  
  
2.  À partir de l’écran d' **Accueil** , ouvrez le gestionnaire multipoint.  
  
3.  Cliquez sur l’onglet **bureaux virtuels** , cliquez sur **activer les bureaux virtuels**, puis cliquez sur **OK**et attendez que le système redémarre.  
  
L’étape suivante consiste à créer un modèle de bureau virtuel. Vous créez littéralement un fichier de disque dur virtuel (VHD) que vous pouvez utiliser comme modèle pour créer des bureaux virtuels de station pour le gestionnaire MultiPoint. Vous pouvez utiliser le support d’installation physique pour Windows ou un. Fichier image ISO en tant que source pour le modèle. Vous pouvez également utiliser un. VHD de l’installation de Windows. Notez que pour utiliser un disque d’installation physique, vous devez insérer le disque avant de démarrer l’Assistant.  
  
##### <a name="to-create-a-virtual-desktop-template"></a>Pour créer un modèle de bureau virtuel  
  
1.  Connectez-vous au système d’exploitation hôte du serveur MultiPoint à l’aide d’un compte d’administrateur local ou, dans le domaine, d’un compte de domaine membre du groupe Administrateurs local.  
  
2.  À partir de l’écran d' **Accueil** , ouvrez le gestionnaire multipoint.  
  
3.  Cliquez sur l’onglet **bureaux virtuels** .  
  
4.   Copiez un fichier Windows 10 entreprise. ISO dans le disque SSD local.  
  
5.  Sous l’onglet bureaux virtuels, cliquez sur **créer un modèle de bureau virtuel.**   
  
6.  Dans **préfixe**, entrez un préfixe à utiliser pour identifier le modèle et les bureaux virtuels créés avec le modèle. Le préfixe par défaut est le nom de l’ordinateur hôte.  
  
    Le préfixe est utilisé pour donner un nom au modèle et aux stations de bureaux virtuels. Le modèle sera <*préfixe*>-t. Les stations de bureaux virtuels sont nommées < le*préfixe*>-*n*, où *n* est l’identificateur de station.  
  
7.  Entrez un nom d’utilisateur et un mot de passe à utiliser pour le compte d’administrateur local pour le modèle. Dans un domaine, entrez les informations d’identification d’un compte de domaine qui sera ajouté au groupe Administrateurs local. Ce compte peut être utilisé pour se connecter au modèle et à toutes les stations de bureau virtuelles créées à partir du modèle.  
  
8. Cliquez sur **OK**et attendez la fin de la création du modèle.  
  
9. Le nouveau modèle est affiché sous l’onglet **bureaux virtuels** . Le modèle sera désactivé.  
  
L’étape suivante consiste à configurer le modèle avec le logiciel et le paramètre que vous souhaitez sur les bureaux virtuels. Vous devez le faire avant de créer des bureaux virtuels à partir du modèle.  
  
##### <a name="to-customize-a-virtual-desktop-template"></a>Pour personnaliser un modèle de bureau virtuel  
  
1.  Connectez-vous au système d’exploitation hôte du serveur MultiPoint à l’aide d’un compte d’administrateur local ou, dans un domaine, avec un compte de domaine dans le groupe Administrateurs local.  
  
2.  À partir de l’écran d' **Accueil** , ouvrez le gestionnaire multipoint.  
  
3.  Cliquez sur l’onglet **bureaux virtuels** .  
  
4.  Sélectionnez le modèle que vous souhaitez personnaliser, cliquez sur **personnaliser le modèle**, puis sur **OK**.  
  
    > [!NOTE]  
    > Seuls les modèles qui n’ont pas été utilisés pour créer des stations de bureaux virtuels sont disponibles. Si vous souhaitez mettre à jour un modèle qui est déjà en cours d’utilisation, vous devez effectuer une copie du modèle à l’aide de la tâche d' **importation de modèle** , décrite plus loin, dans [copier un modèle de bureau virtuel existant](#copy-an-existing-virtual-desktop-template).  
  
    Le modèle s’ouvre dans une fenêtre connexion à une **machine virtuelle** Hyper-V, et l’ouverture de session automatique s’effectue à l’aide du compte administrateur intégré.  
  
5.  À ce stade, vous pouvez installer des applications et des mises à jour logicielles, modifier les paramètres et mettre à jour le profil administrateur. Toutes les modifications apportées au profil Administrateur intégré du modèle seront copiées dans le profil utilisateur par défaut dans les stations de bureau virtuelles créées à partir du modèle.  
  
    Si vous connectez vos stations à un domaine, nous vous recommandons de créer un compte d’utilisateur local et de l’ajouter au groupe Administrateurs local pendant la personnalisation.  
  
    > [!NOTE]  
    > Si le système redémarre alors qu’un modèle est en cours de personnalisation, la connexion automatique à l’aide du compte administrateur intégré peut échouer après le redémarrage du système. Pour contourner ce problème, connectez-vous manuellement à l’aide du compte d’administrateur local que vous avez créé, modifiez le mot de passe du compte administrateur intégré, déconnectez-vous, puis reconnectez-vous à l’aide du compte administrateur intégré et du nouveau mot de passe. (Vous devrez supprimer le profil qui a été créé lorsque vous vous êtes connecté à l’aide du compte administrateur local.)  
  
6.  Une fois que vous avez terminé de configurer votre système, double-cliquez sur le raccourci **CompleteCustomization** sur le Bureau de l’administrateur pour exécuter Sysprep, puis arrêtez le modèle. Pendant la personnalisation, l’outil Sysprep supprime toutes les informations système uniques pour préparer l’installation de Windows.  
  
### <a name="create-virtual-machine-desktops-from-the-template"></a>Créer des bureaux de machines virtuelles à partir du modèle  
Avec votre modèle de bureau virtuel configuré comme vous le souhaitez, vous êtes prêt à commencer à créer des bureaux virtuels. Un bureau virtuel sera créé pour chaque station attachée à l’ordinateur MultiPoint Server. La prochaine fois qu’un utilisateur se connecte à une station, il verra le bureau virtuel au lieu du Bureau basé sur une session qui a été affiché auparavant.  
  
> [!NOTE]  
> Cette procédure ne fonctionne que lorsque MultiPoint Server est en *mode station*. Si le système est en *mode console*, vous pouvez basculer en mode station à partir du gestionnaire multipoint. Si vous utilisez les paramètres MultiPoint par défaut, vous pouvez également démarrer le mode station en redémarrant l’ordinateur. Par défaut, l’ordinateur MultiPoint Server démarre toujours en mode station  
  
##### <a name="to-create-virtual-desktops-for-your-stations"></a>Pour créer des bureaux virtuels pour vos stations  
  
1.  Connectez-vous au serveur Windows MultiPoint à partir d’une station à distance (par exemple, à partir d’un ordinateur Windows à l’aide de Connexion Bureau à distance) à l’aide d’un compte d’administrateur local ou, dans un domaine, d’un compte de domaine dans le groupe Administrateurs local.  
  
    > [!NOTE]  
    > Vous pouvez également ouvrir une session sur le serveur à l’aide d’une station locale. Toutefois, lorsque vous créez un bureau virtuel de station, vous devez vous déconnecter de la station que vous avez utilisée pour créer le bureau virtuel afin de connecter l’autre station au nouveau bureau virtuel.  
  
2.  À partir de l’écran d' **Accueil** , ouvrez le gestionnaire multipoint.  
  
3.  Si l’ordinateur est en mode console, passez en mode station :  
  
    1.  Dans l’onglet dossier de **démarrage** , cliquez sur **basculer en mode station**.  
  
    2.  Au redémarrage de l’ordinateur, ouvrez une session en tant qu’administrateur.  
  
4.  Cliquez sur l’onglet **bureaux virtuels** .  
  
5.  Sélectionnez le modèle de bureau virtuel que vous souhaitez utiliser avec les stations, cliquez sur **créer des stations de bureau virtuelles**, puis cliquez sur **OK**.  
  
Lorsque la tâche est terminée, chaque station locale se connecte à un bureau virtuel basé sur une machine virtuelle.  
  
> [!NOTE]  
> Si un compte d’utilisateur est connecté à l’une des stations locales, vous devrez vous déconnecter de la session pour que la station se connecte à l’un des bureaux virtuels de station nouvellement créés.  
  
### <a name="copy-an-existing-virtual-desktop-template"></a>Copier un modèle de bureau virtuel existant  
Utilisez la procédure suivante pour créer une copie d’un modèle de bureau virtuel existant que vous pouvez personnaliser et utiliser. Cela peut être utile dans les situations suivantes :  
  
-   Pour copier un modèle principal à partir d’un partage réseau sur un ordinateur hôte MultiPoint Server afin que les stations de bureau virtuelles puissent être créées à partir du modèle principal.  
  
-   Pour créer une copie d’un modèle en cours d’utilisation afin de pouvoir effectuer des personnalisations supplémentaires.  
  
##### <a name="to-import-a-virtual-desktop-template"></a>Pour importer un modèle de bureau virtuel  
  
1.  Connectez-vous au serveur MultiPoint en tant qu’administrateur.  
  
2.  À partir de l’écran d' **Accueil** , ouvrez le gestionnaire multipoint.  
  
3.  Cliquez sur l’onglet **bureaux virtuels** .  
  
4.  Cliquez sur **Importer un modèle de bureau virtuel**et utilisez **Parcourir** pour sélectionner le fichier. vhd (modèle) que vous souhaitez importer. Lorsque vous importez un modèle, une copie du fichier. vhd d’origine est créée. Par défaut, multipoint Services stocke les fichiers. vhd dans le\\dossier\\C\\: Users\\public documents\\de\\ disques durs virtuels Hyper\-V.  
  
5.  Entrez un préfixe pour le nouveau modèle, puis cliquez sur **OK**.  
  
6.  Si vous apportez d’autres personnalisations à un modèle local, vous pouvez modifier le nom du préfixe en incrémentant un numéro de version à la fin du préfixe. Ou, si vous importez un modèle principal, vous souhaiterez peut-être ajouter la version du modèle principal à la fin du nom de préfixe par défaut.  
  
7.  Une fois la tâche terminée, vous pouvez personnaliser le modèle ou l’utiliser comme pour créer des stations.  
