---
title: Installer et gérer des Extensions
description: Installer et gérer des Extensions dans Windows Admin Center (projet Honolulu)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: c775dd5a3011115bbb031c0b9e4e24a8911d378e
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296701"
---
# Installer et gérer des Extensions

>S’applique à: Windows Admin Center, Windows Admin Center Preview

Windows Admin Center est conçu comme une plateforme extensible où chaque type de connexion et l’outil sont une extension que vous pouvez installer, désinstaller et mettre à jour individuellement. Vous pouvez rechercher des nouvelles extensions publiées par Microsoft et d’autres développeurs et installer et les mettre à jour individuellement sans avoir à mettre à jour de l’installation complète de Windows Admin Center. Vous pouvez également configurer un flux NuGet distinct ou partage de fichiers et distribuer les extensions à utiliser en interne au sein de votre organisation.

## Installation d’une extension

Windows Admin Center s’affichent des extensions disponibles à partir de la NuGet spécifié de flux. Par défaut, Windows Admin Center pointe vers le NuGet officiel Microsoft de flux qui héberge les extensions publiées par Microsoft et d’autres développeurs.

1. Cliquez sur le bouton **paramètres** de la > en haut à droite dans le volet gauche, cliquez sur **les Extensions**. 
2. L’onglet **Extensions disponibles** répertorie les extensions qui sont disponibles pour l’installation sur le flux.
3. Cliquez sur une extension pour afficher la description de l’extension, version, publisher et d’autres informations dans le volet **d’informations** .
4. Cliquez sur **installer** pour installer une extension. Si la passerelle doit s’exécuter en mode élevé pour effectuer cette modification, vous s’affiche avec une invite d’élévation UAC. Une fois l’installation terminée, votre navigateur est actualisée automatiquement et avec la nouvelle extension installée Windows Admin Center sera rechargé. Si l’extension que vous essayez d’installer est une mise à jour une extension précédemment installée, vous pouvez cliquer sur le bouton **mettre à jour vers la dernière version** pour installer la mise à jour. Vous pouvez également accéder à l’onglet **Extensions installées** pour afficher les extensions installées et voir si une mise à jour est disponible dans la colonne **état** .

## L’installation des extensions d’un flux différents

Windows Admin Center prend en charge plusieurs flux et vous pouvez afficher et gérer les packages à partir de plusieurs flux simultanément. N’importe quel NuGet flux qui prend en charge les API de V2 NuGet ou un partage de fichiers peut être ajouté à Windows Admin Center pour l’installation des extensions de.

1. Cliquez sur le bouton **paramètres** de la > en haut à droite dans le volet gauche, cliquez sur **les Extensions**.
2. Dans le volet droit, cliquez sur l’onglet **flux** .
3. Cliquez sur le bouton **Ajouter** pour ajouter un autre flux. Pour un flux de NuGet, entrez le V2 NuGet URL du flux. Fournisseur de flux de le NuGet ou administrateur doit être en mesure de fournir les informations de l’URL. Pour un partage de fichiers, entrez le chemin d’accès complet du fichier partager dans lequel les fichiers de package d’extension (.nupkg) sont stockés.
4. Cliquez sur **Ajouter**. Si la passerelle doit s’exécuter en mode élevé pour effectuer cette modification, vous s’affiche avec une invite d’élévation UAC.

La liste des **Extensions disponibles** affiche les extensions de tous les flux enregistrés. Vous pouvez vérifier le flux de chaque extension est d’utiliser la colonne de **Flux de Package** .

## Désinstallation d’une extension

Vous pouvez désinstaller les extensions que vous avez déjà installé, ou même désinstaller les outils qui ont été installés au préalable dans le cadre de l’installation de Windows Admin Center.

1. Cliquez sur le bouton **paramètres** de la > en haut à droite dans le volet gauche, cliquez sur **les Extensions**. 
2. Cliquez sur l’onglet **Extensions installées** pour afficher tous les extensions.
3. Choisir une extension pour désinstaller, puis cliquez sur **désinstaller**.

Une fois la désinstallation est terminée, votre navigateur est actualisée automatiquement et Windows Admin Center sera rechargé avec l’extension supprimée. Si vous avez désinstallé un outil qui a été installé au préalable dans le cadre de Windows Admin Center, l’outil sera disponible pour la réinstallation dans l’onglet **Extensions disponibles** .

## L’installation des extensions sur un ordinateur sans connectivité internet

Si Windows Admin Center est installé sur un ordinateur qui n’est pas connecté à internet ou se trouve derrière un proxy, il ne peut pas être en mesure d’accéder et d’installer les extensions à partir du centre d’administration Windows de flux. Vous pouvez télécharger des packages d’extension manuellement ou avec un script PowerShell et la configuration de Windows Admin Center pour extraire les packages à partir d’un partage de fichiers ou un lecteur local.

### Téléchargement manuel de packages d’extension

1. Sur un autre ordinateur qui dispose d’une connectivité internet, ouvrez un navigateur web et accédez à l’URL suivante:[https://msft-sme.myget.org/gallery/windows-admin-center-feed](https://msft-sme.myget.org/gallery/windows-admin-center-feed) 

  * Vous devrez peut-être créer un compte sur msft-sme.myget.org et vous connecter pour afficher les packages d’extension.

2. Cliquez sur le nom du package à installer pour afficher la page de détails du package.
3. Cliquez sur le lien de **téléchargement** dans le volet de droite de la page de détails du package et télécharger le fichier .nupkg pour l’extension.
4. Répétez les étapes 2 et 3 pour tous les packages à télécharger.
5. Copiez les fichiers de package dans un partage de fichiers qui sont accessibles à partir de l’ordinateur sur que Windows Admin Center est installé, ou sur le disque local de l’ordinateur.
6. [Suivez les instructions d’installation des extensions d’un flux différents](#installing-extensions-from-a-different-feed).

### Téléchargement de packages avec un script PowerShell

Il existe de nombreux scripts disponibles sur Internet pour le téléchargement des packages NuGet à partir d’un flux de NuGet. Nous allons utiliser le [script fourni par Jon commis](https://weblogs.asp.net/jongalloway/downloading-a-local-nuget-repository-with-powershell), responsable de programme Senior chez Microsoft.

1. Comme décrit dans le [billet de blog](https://weblogs.asp.net/jongalloway/downloading-a-local-nuget-repository-with-powershell), installez le script en tant que package NuGet, ou copier et coller le script PowerShell ISE.
2. La première ligne du script à votre NuGet de flux de modifier l’URL v2 de le. Si vous téléchargez des packages à partir du centre d’administration Windows officiel de flux, utilisez l’URL ci-dessous.

```powershell
$feedUrlBase = "https://aka.ms/sme-extension-feed"
```

3. Exécutez le script, il télécharge tous les packages NuGet à partir du flux dans le dossier local suivant: %USERPROFILE%\Documents\NuGetLocal
4. [Suivez les instructions d’installation des extensions d’un flux différents](#installing-extensions-from-a-different-feed).

## Gérer les extensions avec PowerShell

>S’applique à: Windows Admin Center, Windows Admin Center Preview

Windows Admin Center Preview inclut un module PowerShell pour gérer vos extensions de passerelle.

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

### [En savoir plus sur la création d’une extension avec le SDK Windows Admin Center](../extend/extensibility-overview.md).