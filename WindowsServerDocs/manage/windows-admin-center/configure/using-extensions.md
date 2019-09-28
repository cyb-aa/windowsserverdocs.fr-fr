---
title: Installer et gérer des extensions
description: Installer et gérer des extensions dans le centre d’administration Windows (projet Honolulu)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: d49e25591c705afa217b2332ee48eb42c5c2f7ab
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357240"
---
# <a name="install-and-manage-extensions"></a>Installer et gérer des extensions

>S'applique à : Windows Admin Center, Windows Admin Center Preview

Le centre d’administration Windows est construit comme une plateforme extensible où chaque type de connexion et outil est une extension que vous pouvez installer, désinstaller et mettre à jour individuellement. Vous pouvez rechercher de nouvelles extensions publiées par Microsoft et d’autres développeurs, et les installer et les mettre à jour individuellement sans avoir à mettre à jour l’ensemble de l’installation du centre d’administration Windows. Vous pouvez également configurer un partage de fichiers ou un flux NuGet distinct et distribuer des extensions à utiliser en interne au sein de votre organisation.

## <a name="installing-an-extension"></a>Installation d’une extension

Le centre d’administration Windows indique les extensions disponibles à partir du flux NuGet spécifié. Par défaut, le centre d’administration Windows pointe vers le flux NuGet officiel de Microsoft qui héberge les extensions publiées par Microsoft et d’autres développeurs.

1. Cliquez sur le bouton **paramètres** en haut à droite > dans le volet gauche, cliquez sur **Extensions**. 
2. L’onglet **extensions disponibles** répertorie les extensions sur le flux qui sont disponibles pour l’installation.
3. Cliquez sur une extension pour afficher la description, la version, l’éditeur et d’autres informations de l’extension dans le volet d' **informations** .
4. Cliquez sur **installer** pour installer une extension. Si la passerelle doit s’exécuter en mode élevé pour effectuer cette modification, une invite d’élévation du contrôle de compte d’utilisateur s’affiche. Une fois l’installation terminée, votre navigateur sera automatiquement actualisé et le centre d’administration Windows sera rechargé avec la nouvelle extension installée. Si l’extension que vous essayez d’installer est une mise à jour d’une extension précédemment installée, vous pouvez cliquer sur le bouton **mettre à jour vers la dernière version** pour installer la mise à jour. Vous pouvez également accéder à l’onglet **extensions installées** pour afficher les extensions installées et voir si une mise à jour est disponible dans la colonne **État** .

## <a name="installing-extensions-from-a-different-feed"></a>Installation des extensions à partir d’un autre flux

Le centre d’administration Windows prend en charge plusieurs flux et vous pouvez afficher et gérer des packages à partir de plusieurs flux à la fois. Tout flux NuGet prenant en charge les API NuGet v2 ou un partage de fichiers peut être ajouté au centre d’administration Windows pour l’installation des extensions à partir de.

1. Cliquez sur le bouton **paramètres** en haut à droite > dans le volet gauche, cliquez sur **Extensions**.
2. Dans le volet droit, cliquez sur l’onglet **flux** .
3. Cliquez sur le bouton **Ajouter** pour ajouter un autre flux. Pour un flux NuGet, entrez l’URL du flux NuGet v2. Le fournisseur de flux NuGet ou l’administrateur doit être en mesure de fournir les informations de l’URL. Pour un partage de fichiers, entrez le chemin d’accès complet du partage de fichiers dans lequel sont stockés les fichiers de package d’extension (. nupkg).
4. Cliquez sur **Ajouter**. Si la passerelle doit s’exécuter en mode élevé pour effectuer cette modification, une invite d’élévation du contrôle de compte d’utilisateur s’affiche.

La liste **extensions disponibles** indique les extensions de tous les flux enregistrés. Vous pouvez vérifier le flux de chaque extension à l’aide de la colonne de **flux de package** .

## <a name="uninstalling-an-extension"></a>Désinstallation d’une extension

Vous pouvez désinstaller toutes les extensions que vous avez installées précédemment, ou même désinstaller les outils préinstallés dans le cadre de l’installation du centre d’administration Windows.

1. Cliquez sur le bouton **paramètres** en haut à droite > dans le volet gauche, cliquez sur **Extensions**. 
2. Cliquez sur l’onglet **extensions installées** pour afficher toutes les extensions installées.
3. Choisissez une extension à désinstaller, puis cliquez sur **désinstaller**.

Une fois la désinstallation terminée, votre navigateur sera automatiquement actualisé et le centre d’administration Windows sera rechargé avec l’extension supprimée. Si vous avez désinstallé un outil qui a été préinstallé dans le cadre du centre d’administration Windows, l’outil sera disponible pour la réinstallation sous l’onglet **extensions disponibles** .

## <a name="installing-extensions-on-a-computer-without-internet-connectivity"></a>Installation d’extensions sur un ordinateur sans connectivité Internet

Si le centre d’administration Windows est installé sur un ordinateur qui n’est pas connecté à Internet ou qui se trouve derrière un proxy, il est possible qu’il ne puisse pas accéder aux extensions et les installer à partir du flux du centre d’administration Windows. Vous pouvez télécharger des packages d’extension manuellement ou à l’aide d’un script PowerShell, et configurer le centre d’administration Windows pour récupérer des packages à partir d’un partage de fichiers ou d’un lecteur local.

### <a name="manually-downloading-extension-packages"></a>Téléchargement manuel des packages d’extension

1. Sur un autre ordinateur disposant d’une connexion Internet, ouvrez un navigateur Web et accédez à l’URL suivante : [https://msft-sme.myget.org/gallery/windows-admin-center-feed](https://msft-sme.myget.org/gallery/windows-admin-center-feed) 

   * Vous devrez peut-être créer un compte sur msft-sme.myget.org et vous connecter pour afficher les packages d’extension.

2. Cliquez sur le nom du package que vous souhaitez installer pour afficher la page Détails du package.
3. Cliquez sur le lien de **Téléchargement** dans le volet droit de la page Détails du package et téléchargez le fichier. nupkg pour l’extension.
4. Répétez les étapes 2 et 3 pour tous les packages que vous souhaitez télécharger.
5. Copiez les fichiers du package dans un partage de fichiers accessible à partir de l’ordinateur sur lequel le centre d’administration Windows est installé, ou sur le disque local de l’ordinateur.
6. [Suivez les instructions pour installer des extensions à partir d’un autre flux](#installing-extensions-from-a-different-feed).

### <a name="downloading-packages-with-a-powershell-script"></a>Téléchargement de packages à l’aide d’un script PowerShell

De nombreux scripts sont disponibles sur Internet pour le téléchargement des packages NuGet à partir d’un flux NuGet. Nous allons utiliser le [script fourni par Jon Galloway](https://weblogs.asp.net/jongalloway/downloading-a-local-nuget-repository-with-powershell), responsable de programme senior chez Microsoft.

1. Comme décrit dans le billet de [blog](https://weblogs.asp.net/jongalloway/downloading-a-local-nuget-repository-with-powershell), installez le script sous la forme d’un package NuGet, ou copiez et collez le script dans PowerShell ISE.
2. Modifiez la première ligne du script avec l’URL v2 de votre flux NuGet. Si vous téléchargez des packages à partir du flux officiel du centre d’administration Windows, utilisez l’URL ci-dessous.

```powershell
$feedUrlBase = "https://aka.ms/sme-extension-feed"
```

3. Exécutez le script pour télécharger tous les packages NuGet du flux dans le dossier local suivant :%USERPROFILE%\Documents\NuGetLocal
4. [Suivez les instructions pour installer des extensions à partir d’un autre flux](#installing-extensions-from-a-different-feed).

## <a name="manage-extensions-with-powershell"></a>Gérer les extensions avec PowerShell

>S'applique à : Windows Admin Center, Windows Admin Center Preview

La version préliminaire du centre d’administration Windows comprend un module PowerShell pour gérer vos extensions de passerelle.

```powershell
# Add the module to the current session
Import-Module "$env:ProgramFiles\windows admin center\PowerShell\Modules\ExtensionTools"
# Available cmdlets: Get-Feed, Add-Feed, Remove-Feed, Get-Extension, Install-Extension, Uninstall-Extension, Update-Extension

# List feeds
Get-Feed "https://wac.contoso.com"

# Add a new extension feed
Add-Feed -GatewayEndpoint "https://wac.contoso.com" -Feed "\\WAC\our-private-extensions"

# Remove an extension feed
Remove-Feed -GatewayEndpoint "https://wac.contoso.com" -Feed "\\WAC\our-private-extensions"

# List all extensions
Get-Extension "https://wac.contoso.com"

# Install an extension (locate the latest version from all feeds and install it)
Install-Extension -GatewayEndpoint "https://wac.contoso.com" "msft.sme.containers"

# Install an extension (latest version from a specific feed, if the feed is not present, it will be added)
Install-Extension -GatewayEndpoint "https://wac.contoso.com" "msft.sme.containers" -Feed "https://aka.ms/sme-extension-feed"

# Install an extension (install a specific version)
Install-Extension "https://wac.contoso.com" "msft.sme.certificate-manager" "0.133.0"

# Uninstall-Extension
Uninstall-Extension "https://wac.contoso.com" "msft.sme.containers"

# Update-Extension
Update-Extension "https://wac.contoso.com" "msft.sme.containers"
```

### <a name="learn-more-about-building-an-extension-with-the-windows-admin-center-sdkextendextensibility-overviewmd"></a>[En savoir plus sur la création d’une extension avec le kit de développement logiciel (SDK) du centre d’administration Windows](../extend/extensibility-overview.md).