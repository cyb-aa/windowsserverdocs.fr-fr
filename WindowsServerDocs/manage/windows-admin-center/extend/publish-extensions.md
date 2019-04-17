---
title: Publication des Extensions pour Windows Admin Center
description: Publication des Extensions pour Windows Admin Center (projet Honolulu)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 762bd4613fa8ad6cdfb5b44745a7ce78b331499d
ms.sourcegitcommit: 3883eebbba70bfea0221e510863ee1a724a5f926
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/29/2018
ms.locfileid: "5783751"
---
# Publication d’Extensions

>S’applique à: Windows Admin Center, Windows Admin Center Preview

Une fois que vous avez développé votre extension, vous devrez le publier et la rendre disponible aux autres tests ou d’utiliser. En fonction de votre public et le rôle de publication, il existe quelques options qui nous présenterons ci-dessous, ainsi que les étapes et la configuration requise pour la publication.

## Options de publication

Il existe trois options principales pour les sources de package configurable qui prend en charge de Windows Admin Center:
* Flux public Windows Admin Center NuGet de Microsoft
* Votre propre flux NuGet privé
* Local ou partage de fichiers réseau

### La publication dans la flux d’extension Windows Admin Center

Par défaut, Windows Admin Center est connecté à un NuGet flux gérée par l’équipe de produit Windows Admin Center chez Microsoft. Premières versions d’aperçu de nouvelles extensions développées par Microsoft peuvent être publiées à ce flux et disponible pour les utilisateurs de Windows Admin Center. Les développeurs externes planification pour créer et publier des extensions publiquement peuvent également [soumettre une demande](#publishing-your-extension-to-the-windows-admin-center-feed) de publier dans ce flux.

### Flux de publication dans un autre NuGet

Vous pouvez également créer vos propres NuGet pour publier vos extensions à une des nombreuses [options différentes pour la configuration d’une source privée ou à l’aide d’un NuGet qui héberge le service](https://docs.microsoft.com/nuget/hosting-packages/overview)à l’aide de flux. Le flux de NuGet doit prendre en charge l’API de v2 NuGet. Dans la mesure où Windows Admin Center ne prend pas en charge d’authentification, le flux doit être configuré pour autoriser l’accès en lecture à tout le monde.

### La publication dans un partage de fichiers

Pour limiter l’accès de votre extension dans votre organisation ou à un groupe limité de personnes, vous pouvez utiliser un partage de fichiers SMB comme une flux d’extension. Dans ce cas, les autorisations de partage et du dossier du fichier s’appliqueront pour autoriser l’accès au flux.

## Préparation de votre extension publiée

Veillez à lire et prendre en compte les rubriques de développement suivantes:

- [Contrôler la visibilité de votre outil](guides/dynamic-tool-display.md)
- [Chaînes et localisation](guides/strings-localization.md)

### Envisagez de publier comme une version préliminaire

Si vous publiez une version d’évaluation de votre extension à des fins d’évaluation, il est recommandé que vous:

- Ajouter «(aperçu)» à la fin du titre de votre extension dans le fichier .nuspec
- Expliquez les limites dans la description de votre extension dans le fichier .nuspec

## Création d’un package d’extension

Windows Admin Center utilise les packages NuGet et des flux de distribution et de téléchargement des extensions.  Dans l’ordre de votre package doivent être livrées, vous devez générer un package NuGet qui contient votre plug-ins et des extensions.  Un seul package peut contenir à la fois une extension de l’interface utilisateur, ainsi qu’un plug-in de passerelle, et la section suivante vous guide à travers le processus.

### 1. créer votre extension

Dès que vous êtes prêt à démarrer empaqueter votre extension, créez un nouveau répertoire sur votre système de fichiers, ouvrez une console et CD dedans.  Il s’agit du répertoire racine qui nous utiliserons pour contenir tous les répertoires de nuspec et le contenu qui composent notre package.  Nous référencera ce dossier en tant que «Package NuGet» pendant toute la durée de ce document.

#### Extensions de l’interface utilisateur

Pour commencer le processus de collecte de tout le contenu nécessaire pour une extension de l’interface utilisateur, exécutez «gulp build» sur votre outil et assurez-vous que la génération est réussie.  Packages de ce processus tous les composants dans un dossier appelé «offre groupée» qui se trouvent dans le répertoire racine de votre extension (au même niveau du répertoire src).  Copiez ce répertoire et tous il s’agit de contenu dans le dossier «NuGet Package».

#### Plug-ins de passerelle

À l’aide de votre infrastructure de Build (qui peut être aussi simple que vous ouvrez Visual Studio et en cliquant sur le bouton Générer), compiler et créer votre plug-in.  Ouvrez votre répertoire de sortie de génération et copier la DLL qui représentent votre plug-in et placez-les dans un nouveau dossier à l’intérieur de l’annuaire de «NuGet Package» appelé «package».  Vous n’avez pas besoin de copier la dll FeatureInterface, simplement la DLL qui représentent votre code.

### 2. créer le fichier .nuspec

Pour créer le package NuGet, vous devez d’abord créer un fichier .nuspec. Un fichier .nuspec est un manifeste XML qui contient des métadonnées de package NuGet. Ce manifeste est utilisé pour générer le package et à fournir des informations aux consommateurs.  Placez ce fichier à la racine du dossier «NuGet Package».

Voici un exemple de fichier .nuspec et la liste des propriétés requises ou recommandées. Pour le schéma complet, voir la [référence .nuspec](https://docs.microsoft.com/nuget/reference/nuspec). Enregistrez le fichier de .nuspec au dossier de racine de votre projet avec un nom de fichier de votre choix.

> [!IMPORTANT]
> Le ```<id>``` valeur dans le fichier .nuspec doit correspondre à la ```"name"``` valeur de votre projet ```manifest.json``` fichier, faute de votre extension publiée ne se charge pas correctement dans Windows Admin Center.

```xml
<?xml version="1.0" encoding="utf-8"?>
<package xmlns="http://schemas.microsoft.com/packaging/2011/08/nuspec.xsd">
  <metadata>
    <packageTypes>
      <packageType name="WindowsAdminCenterExtension" />
    </packageTypes>  
    <id>contoso.project.extension</id>
    <version>1.0.0</version>
    <title>Contoso Hello Extension</title>
    <authors>Contoso</authors>
    <owners>Contoso</owners>
    <requireLicenseAcceptance>false</requireLicenseAcceptance>
    <projectUrl>https://msft-sme.myget.org/feed/windows-admin-center-feed/package/nuget/contoso.sme.hello-extension</projectUrl>
    <licenseUrl>http://YourLicenseLink</licenseUrl>
    <iconUrl>http://YourLogoLink</iconUrl>
    <description>Hello World extension by Contoso</description>
    <copyright>(c) Contoso. All rights reserved.</copyright> 
    <tags></tags>
  </metadata>
  <files>
    <file src="bundle\**\*.*" target="ux" />
    <file src="package\**\*.*" target="gateway" />
  </files>
</package>
```

#### Requises ou recommandées propriétés

| Nom de propriété | Requis / recommandé | Description |
| ---- | ---- | ---- |
| packageType | Requis | Utilisez «WindowsAdminCenterExtension», qui est le type de package NuGet défini pour les extensions de Windows Admin Center. |
| id | Requis | Identificateur unique du Package dans le flux. Cette valeur doit correspondre à la valeur «name» dans le fichier manifest.json de votre projet.  Pour obtenir des instructions, consultez [choix d’un identificateur unique de package](https://docs.microsoft.com/nuget/create-packages/creating-a-package#choosing-a-unique-package-identifier-and-setting-the-version-number) . |
| title | Requis pour la publication sur le centre d’administration Windows de flux | Nom convivial pour le package qui s’affiche dans Windows Admin Center Extension Manager. |
| version | Requis | Version de l’extension. À l’aide du [Contrôle de version sémantique (convention SemVer)](http://semver.org/spec/v1.0.0.html) est recommandé mais non obligatoires. |
| auteurs | Requis | Si publiez pour le compte de votre société, utilisez le nom de votre société. |
| description | Requis | Fournir une description de la fonctionnalité de l’extension. |
| iconUrl | Recommandée lors de la publication dans le centre d’administration Windows de flux | URL d’icône à afficher dans le Gestionnaire d’extensions. |
| projectUrl | Requis pour la publication sur le centre d’administration Windows de flux | URL de site Web de votre extension. Si vous ne disposez pas d’un site Web distinct, utilisez l’URL de la page Web de package sur le flux de NuGet. |
| licenseUrl | Requis pour la publication sur le centre d’administration Windows de flux | URL de contrat de licence utilisateur final de votre extension. |
| fichiers | Requis | Ces deux paramètres de configuration de la structure de dossiers que Windows Admin Center s’attend des extensions de l’interface utilisateur et les plug-ins de passerelle. |

### 3. Générez le package NuGet d’extension

À l’aide du fichier .nuspec que vous avez créé ci-dessus, vous allez maintenant créer le fichier .nupkg du package NuGet que vous pouvez charger et publier dans le flux de NuGet.

1. Télécharger l’outil CLI nuget.exe à partir du [site Web des outils client NuGet](https://docs.microsoft.com/nuget/install-nuget-client-tools).
2. Exécutez «nuget.exe pack [nom du fichier .nuspec]» pour créer le fichier .nupkg.

### 4. signature de votre package NuGet d’extension

Tous les fichiers .dll inclus dans votre extension sont requises pour être signé avec un certificat à partir d’une autorité de certification approuvée (CA). Par défaut, les fichiers .dll non signées seront bloquées afin d’en cours d’exécution lorsque Windows Admin Center s’exécute en Mode de Production.

Nous vous recommandons également vivement que vous vous connectez le package NuGet d’extension pour garantir l’intégrité du package, mais il ne s’agit pas d’une étape obligatoire.

### 5. test de votre package NuGet d’extension

Votre package d’extension est prêt pour le test! Téléchargez le fichier .nupkg vers un flux NuGet ou copiez-la dans un partage de fichiers. Pour afficher et télécharger des packages à partir d’un flux différents ou d’un partage de fichiers, vous aurez besoin pour [modifier votre configuration de l’alimentation](../configure/using-extensions.md#installing-extensions-from-a-different-feed) pour pointer vers votre fichier ou de flux de NuGet partager. Lorsque vous testez, assurez-vous que les propriétés s’affichent correctement dans le Gestionnaire d’extensions et vous pouvez installer et désinstaller votre extension correctement.

## La publication de votre extension dans le centre d’administration Windows de flux

En les publiant sur le flux de Windows Admin Center, vous pouvez rendre votre extension disponible pour n’importe quel utilisateur Windows Admin Center. Dans la mesure où le SDK Windows Admin Center est toujours en version préliminaire, nous souhaitons travailler en étroite collaboration avec vous aider à résoudre les problèmes de développement et, assurez-vous que vous êtes en mesure de fournir un produit de qualité découvrir à vos utilisateurs.

Avant de libérer la version initiale de votre extension, nous recommandons que vous soumettez une demande de révision d’extension à Microsoft au moins 2 à 3 semaines avant le lancement de que nous disposer de suffisamment de temps pour passer en revue et vous pouvez apporter des modifications à votre extension si nécessaire. Une fois que votre extension est prête à être publiée, vous devrez nous les envoyer pour passer en revue et si approuvée, nous allons publier dans le flux pour vous.

Par la suite, si vous voulez effectuer une mise à jour à votre extension, vous devez soumettre une demande un autre avis. Alors que, selon l’étendue de modification, le délai pour les avis de mise à jour doit généralement être plus court.

### Soumettre une demande de révision d’extension à Microsoft

Pour soumettre une demande de révision d’extension, entrez les informations suivantes et envoyer par courrier électronique à [wacextensionrequest@microsoft.com](mailto:wacextensionrequest@microsoft.com?subject=Windows%20Admin%20Center%20Extension%20Review%20Request). Nous sera renvoyé à votre adresse e-mail au sein d’une semaine.

```
Windows Admin Center Extension Review Request
1. Name and email address of extension owner/developer (up to 3 users). If you will be releasing an extension on behalf of your company, provide your company email address.
2. Company name (Only required if you are releasing an extension on behalf of your company):
3. Extension name:
4. Release target date (estimate):
5. For new extension submission - Extension description (early design wire frames, screen mockups or product screenshots are highly recommended):
6. For extension update review – Description of changes (include product screenshots if UI has been significantly changed):
```

### Soumettre votre package d’extension pour passer en revue et sa publication

Assurez-vous que vous suivez les instructions ci-dessus pour la [Création d’un package d’extension](#creating-an-extension-package) et le fichier .nuspec est correctement défini et les fichiers sont signés. Nous vous recommandons également que vous disposez d’un site Web de projet, y compris les éléments suivants:

- Description détaillée de votre extension, y compris les captures d’écran ou une vidéo
- Fonctionnalité adresse ou le site Web par courrier électronique pour recevoir des commentaires ou questions

Lorsque vous êtes prêt à publier votre extension, envoyer un courrier électronique à [wacextensionrequest@microsoft.com](mailto:wacextensionrequest@microsoft.com?subject=Windows%20Admin%20Center%20Extension%20Package%20Review) et nous fournirons des instructions sur la façon de pour nous envoyer votre package d’extension. Une fois que nous recevons votre package, nous examinerons et si approuvée, publier dans le centre d’administration Windows de flux.