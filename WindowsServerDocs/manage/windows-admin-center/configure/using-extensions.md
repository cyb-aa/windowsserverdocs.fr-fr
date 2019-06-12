---
title: Installer et gérer des Extensions
description: Installer et gérer des Extensions dans Windows Admin Center (projet Honolulu)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 9038fd480ed105aed3949b0c48dffc7eab94f970
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445892"
---
# <a name="install-and-manage-extensions"></a>Installer et gérer des Extensions

>S'applique à : Windows Admin Center, version préliminaire de Windows Admin Center

Windows Admin Center est construit comme une plate-forme extensible où chaque outil et le type de connexion sont une extension que vous pouvez installer, désinstaller et mettre à jour individuellement. Vous pouvez rechercher les nouvelles extensions publiées par Microsoft et d’autres développeurs et installer et les mettre à jour individuellement sans avoir à mettre à jour de l’ensemble de l’installation de Windows Admin Center. Vous pouvez également configurer un flux NuGet distinct ou partage de fichiers et distribuer des extensions à utiliser en interne au sein de votre organisation.

## <a name="installing-an-extension"></a>Installation d’une extension

Windows Admin Center s’afficheront les extensions disponibles depuis le flux NuGet spécifié. Par défaut, Windows Admin Center pointe vers le flux NuGet officiels Microsoft qui héberge les extensions publiées par Microsoft et d’autres développeurs.

1. Cliquez sur le **paramètres** bouton dans l’angle supérieur droit > dans le volet gauche, cliquez sur **Extensions**. 
2. Le **Extensions disponibles** onglet répertorie les extensions sur le flux qui sont disponibles pour l’installation.
3. Cliquez sur une extension pour afficher la description de l’extension, version, serveur de publication et d’autres informations dans le **détails** volet.
4. Cliquez sur **installer** d’installer une extension. Si la passerelle doit s’exécuter avec élévation de privilèges pour effectuer cette modification, s’affiche avec une invite d’élévation UAC. Une fois l’installation terminée, votre navigateur sera automatiquement actualisé et Windows Admin Center sera rechargé avec la nouvelle extension installée. Si l’extension que vous voulez installer est une mise à jour une extension installée, vous pouvez cliquer sur le **mise à jour vers la dernière version** bouton pour installer la mise à jour. Vous pouvez également accéder à la **Extensions installées** si une mise à jour est disponible dans l’onglet à afficher installé les extensions et voir le **état** colonne.

## <a name="installing-extensions-from-a-different-feed"></a>Installation des extensions à partir d’un autre flux

Windows Admin Center prend en charge plusieurs flux et vous pouvez afficher et gérer les packages à partir de plusieurs flux à la fois. Tout NuGet flux pris en charge par les API V2 de NuGet ou un partage de fichiers peut être ajouté à Windows Admin Center pour l’installation des extensions à partir de.

1. Cliquez sur le **paramètres** bouton dans l’angle supérieur droit > dans le volet gauche, cliquez sur **Extensions**.
2. Dans le volet droit, cliquez sur le **flux** onglet.
3. Cliquez sur le **ajouter** pour ajouter un autre flux. Pour un flux NuGet, entrez la V2 NuGet URL du flux. Fournisseur de flux NuGet ou administrateur doit être en mesure de fournir les informations d’URL. Pour un partage de fichiers, entrez le chemin d’accès complet du fichier de partage dans lequel les fichiers de package d’extension (.nupkg) sont stockées.
4. Cliquez sur **Ajouter**. Si la passerelle doit s’exécuter avec élévation de privilèges pour effectuer cette modification, s’affiche avec une invite d’élévation UAC.

Le **Extensions disponibles** liste affichera les extensions à partir de tous les flux enregistrés. Vous pouvez vérifier le flux chaque extension est d’utiliser le **des flux de packages** colonne.

## <a name="uninstalling-an-extension"></a>Désinstallation d’une extension

Vous pouvez désinstaller les extensions que vous avez déjà installé, ou même désinstaller les outils qui ont été préinstallés dans le cadre de l’installation de Windows Admin Center.

1. Cliquez sur le **paramètres** bouton dans l’angle supérieur droit > dans le volet gauche, cliquez sur **Extensions**. 
2. Cliquez sur le **Extensions installées** onglet pour afficher toutes les extensions installées.
3. Choisissez une extension à désinstaller, puis cliquez sur **désinstallation**.

Une fois la désinstallation terminée, votre navigateur sera automatiquement actualisé et Windows Admin Center sera rechargé avec l’extension supprimée. Si vous avez désinstallé un outil qui a été préinstallé dans le cadre de Windows Admin Center, l’outil sera disponible pour la réinstallation dans le **Extensions disponibles** onglet.

## <a name="installing-extensions-on-a-computer-without-internet-connectivity"></a>Installation des extensions sur un ordinateur sans connexion internet

Si Windows Admin Center est installé sur un ordinateur qui n’est pas connecté à internet ou se trouve derrière un proxy, il n’est peut-être pas en mesure d’accéder et d’installer les extensions à partir de la Windows Admin Center de flux. Vous pouvez télécharger les packages d’extension manuellement ou avec un script PowerShell et configurer Windows Admin Center pour récupérer des packages à partir d’un partage de fichiers ou un lecteur local.

### <a name="manually-downloading-extension-packages"></a>Télécharger manuellement les packages d’extension

1. Sur un autre ordinateur qui dispose d’une connectivité internet, ouvrez un navigateur web et accédez à l’URL suivante : [https://msft-sme.myget.org/gallery/windows-admin-center-feed](https://msft-sme.myget.org/gallery/windows-admin-center-feed) 

   * Vous devrez peut-être créer un compte sur msft-sme.myget.org et de connexion pour afficher les packages d’extension.

2. Cliquez sur le nom du package que vous souhaitez installer pour afficher la page Détails du package.
3. Cliquez sur le **télécharger** lien dans le volet de droite de la page de détails du package et de télécharger le fichier .nupkg pour l’extension.
4. Répétez les étapes 2 et 3 pour tous les packages que vous souhaitez télécharger.
5. Copiez les fichiers de package vers un partage de fichiers qui sont accessibles à partir de l’ordinateur sur que Windows Admin Center est installé, ou sur le disque local de l’ordinateur.
6. [Suivez les instructions pour installer des extensions à partir d’un autre flux](#installing-extensions-from-a-different-feed).

### <a name="downloading-packages-with-a-powershell-script"></a>Téléchargement des packages avec un script PowerShell

De nombreux scripts sont disponibles sur Internet pour télécharger les packages NuGet à partir d’un flux NuGet. Nous allons utiliser le [script fourni par Jon Galloway](https://weblogs.asp.net/jongalloway/downloading-a-local-nuget-repository-with-powershell), responsable de programme Senior chez Microsoft.

1. Comme décrit dans la [billet de blog](https://weblogs.asp.net/jongalloway/downloading-a-local-nuget-repository-with-powershell), le script d’installation sous forme de package NuGet, ou copiez et collez le script dans PowerShell ISE.
2. Modifier que le flux de la première ligne du script à votre NuGet l’URL de v2 de le. Si vous téléchargez les packages à partir de la de Windows Admin Center officiel du flux, utilisez l’URL ci-dessous.

```powershell
$feedUrlBase = "https://aka.ms/sme-extension-feed"
```

3. Exécutez le script et il téléchargera tous les packages NuGet à partir du flux dans le dossier local suivant : %USERPROFILE%\Documents\NuGetLocal
4. [Suivez les instructions pour installer des extensions à partir d’un autre flux](#installing-extensions-from-a-different-feed).

## <a name="manage-extensions-with-powershell"></a>Gérer les extensions avec PowerShell

>S'applique à : Windows Admin Center, version préliminaire de Windows Admin Center

Version préliminaire de Windows Admin Center inclut un module PowerShell pour gérer vos extensions de passerelle.

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

### <a name="learn-more-about-building-an-extension-with-the-windows-admin-center-sdkextendextensibility-overviewmd"></a>[En savoir plus sur la création d’une extension avec le Kit de développement logiciel Windows Admin Center](../extend/extensibility-overview.md).