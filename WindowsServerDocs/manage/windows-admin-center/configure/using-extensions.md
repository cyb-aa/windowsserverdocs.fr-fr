---
title: Installer et gérer des extensions
description: Installer et gérer des extensions dans Windows Admin Center (projet Honolulu)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 2b8a9f5ebab22891b8b97c9c56bba3837cbb9371
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2020
ms.locfileid: "81269296"
---
# <a name="install-and-manage-extensions"></a>Installer et gérer des extensions

>S'applique à : Windows Admin Center, Windows Admin Center Preview

Windows Admin Center est construit comme une plateforme extensible où chaque type de connexion et chaque outil est une extension que vous pouvez installer, désinstaller et mettre à jour individuellement. Vous pouvez rechercher de nouvelles extensions publiées par Microsoft et d’autres développeurs, les installer et les mettre à jour individuellement sans avoir à mettre à jour l’ensemble de l’installation Windows Admin Center. Vous pouvez également configurer un partage de fichiers ou un flux NuGet distinct, et distribuer les extensions à utiliser en interne au sein de votre organisation.

## <a name="installing-an-extension"></a>Installation d’une extension

Windows Admin Center indique les extensions disponibles à partir du flux NuGet spécifié. Par défaut, Windows Admin Center pointe vers le flux NuGet officiel de Microsoft qui héberge les extensions publiées par Microsoft et d’autres développeurs.

1. Cliquez sur le bouton **Paramètres** en haut à droite. Dans le volet gauche, cliquez sur **Extensions**. 
2. L’onglet **Extensions disponibles** liste les extensions sur le flux qui sont disponibles pour l’installation.
3. Cliquez sur une extension pour afficher sa description, sa version, son éditeur et d’autres informations dans le volet **Détails**.
4. Cliquez sur **Installer** pour installer une extension. Si la passerelle doit s’exécuter en mode élevé pour effectuer cette modification, une invite d’élévation du contrôle de compte d’utilisateur s’affiche. Une fois l’installation terminée, votre navigateur est automatiquement actualisé et Windows Admin Center est rechargé avec la nouvelle extension installée. Si l’extension que vous essayez d’installer est une mise à jour d’une extension précédemment installée, vous pouvez cliquer sur le bouton **Update to latest** (Mettre à jour vers la dernière) pour installer cette mise à jour. Vous pouvez également accéder à l’onglet **Installed Extensions** (Extensions installées) pour afficher les extensions installées et voir si une mise à jour est disponible dans la colonne **État**.

## <a name="installing-extensions-from-a-different-feed"></a>Installation des extensions à partir d’un autre flux

Windows Admin Center prend en charge plusieurs flux et vous pouvez afficher et gérer des packages à partir de plusieurs flux à la fois. Tout flux NuGet prenant en charge les API NuGet V2 ou un partage de fichiers peut être ajouté dans Windows Admin Center comme source d’installation d’extensions.

1. Cliquez sur le bouton **Paramètres** en haut à droite. Dans le volet gauche, cliquez sur **Extensions**.
2. Dans le volet droit, cliquez sur l’onglet **Feeds** (Flux).
3. Cliquez sur le bouton **Add** (Ajouter) pour ajouter un autre flux. Pour un flux NuGet, entrez l’URL du flux NuGet V2. L’administrateur ou le fournisseur de flux NuGet doit être en mesure de fournir les informations d’URL. Pour un partage de fichiers, entrez le chemin complet du partage de fichiers dans lequel les fichiers de package d’extension (.nupkg) sont stockés.
4. Cliquez sur **Ajouter**. Si la passerelle doit s’exécuter en mode élevé pour effectuer cette modification, une invite d’élévation du contrôle de compte d’utilisateur s’affiche. Cette invite vous est uniquement présentée si vous exécutez Windows Admin Center en mode bureau.

La liste **Extensions disponibles** affiche les extensions de tous les flux enregistrés. Vous pouvez vérifier le flux d’où provient chaque extension à l’aide de la colonne **flux du package**.

## <a name="uninstalling-an-extension"></a>Désinstallation d’une extension

Vous pouvez désinstaller toutes les extensions que vous avez installées précédemment, ou même désinstaller les outils préinstallés dans le cadre de l’installation Windows Admin Center.

1. Cliquez sur le bouton **Paramètres** en haut à droite. Dans le volet gauche, cliquez sur **Extensions**. 
2. Cliquez sur l’onglet **Extensions installées** pour afficher toutes les extensions installées.
3. Choisissez une extension à désinstaller, puis cliquez sur **Désinstaller**.

Une fois la désinstallation terminée, votre navigateur est automatiquement actualisé et Windows Admin Center est rechargé avec l’extension supprimée. Si vous avez désinstallé un outil qui était préinstallé dans le cadre de Windows Admin Center, l’outil est disponible pour réinstallation sous l’onglet **Extensions disponibles**.

## <a name="installing-extensions-on-a-computer-without-internet-connectivity"></a>Installation d’extensions sur un ordinateur sans connexion Internet

Si Windows Admin Center est installé sur un ordinateur qui n’est pas connecté à Internet ou qui se trouve derrière un proxy, il est possible qu’il ne puisse pas accéder aux extensions ni les installer à partir du flux Windows Admin Center. Vous pouvez télécharger des packages d’extension manuellement ou à l’aide d’un script PowerShell, et configurer Windows Admin Center pour récupérer les packages à partir d’un partage de fichiers ou d’un lecteur local.

### <a name="manually-downloading-extension-packages"></a>Téléchargement manuel des packages d’extension

1. Sur un autre ordinateur disposant d’une connexion Internet, ouvrez un navigateur web et accédez à l’URL suivante : [https://msft-sme.myget.org/gallery/windows-admin-center-feed](https://msft-sme.myget.org/gallery/windows-admin-center-feed) 

   * Vous devrez peut-être créer un compte sur msft-sme.myget.org et vous connecter pour afficher les packages d’extension.

2. Cliquez sur le nom du package que vous souhaitez installer pour afficher la page de détails du package.
3. Cliquez sur le lien **Télécharger** dans le volet de droite de la page de détails du package et téléchargez le fichier .nupkg pour l’extension.
4. Répétez les étapes 2 et 3 pour tous les packages que vous souhaitez télécharger.
5. Copiez les fichiers de package dans un partage de fichiers accessible à partir de l’ordinateur sur lequel Windows Admin Center est installé, ou sur le disque local de l’ordinateur.
6. [Suivez ces instructions pour installer des extensions à partir d’un flux différent](#installing-extensions-from-a-different-feed).

### <a name="downloading-packages-with-a-powershell-script"></a>Téléchargement de packages à l’aide d’un script PowerShell

De nombreux scripts sont disponibles sur Internet pour le téléchargement des packages NuGet à partir d’un flux NuGet. Nous allons utiliser le [script fourni par Jon Galloway](https://weblogs.asp.net/jongalloway/downloading-a-local-nuget-repository-with-powershell), responsable de programme senior chez Microsoft.

1. Comme le décrit son [billet de blog](https://weblogs.asp.net/jongalloway/downloading-a-local-nuget-repository-with-powershell), installez le script sous la forme d’un package NuGet ou effectuez un copier-coller du script dans PowerShell ISE.
2. Modifiez la première ligne du script avec l’URL v2 de votre flux NuGet. Si vous téléchargez des packages à partir du flux officiel Windows Admin Center, utilisez l’URL ci-dessous.

```powershell
$feedUrlBase = "https://aka.ms/sme-extension-feed"
```

3. Exécutez le script pour télécharger tous les packages NuGet du flux dans le dossier local suivant :%USERPROFILE%\Documents\NuGetLocal
4. [Suivez ces instructions pour installer des extensions à partir d’un flux différent](#installing-extensions-from-a-different-feed).

## <a name="manage-extensions-with-powershell"></a>Gérer les extensions avec PowerShell

La préversion de Windows Admin Center comprend un module PowerShell pour gérer vos extensions de passerelle.

[!INCLUDE [ps-extensions](../includes/ps-extensions.md)]

### <a name="learn-more-about-building-an-extension-with-the-windows-admin-center-sdk"></a>[En savoir plus sur la création d’une extension avec le kit SDK de Windows Admin Center](../extend/extensibility-overview.md).