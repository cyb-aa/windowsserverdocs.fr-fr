---
title: Publication d’Extensions pour Windows Admin Center
description: Publication d’Extensions pour Windows Admin Center (projet Honolulu)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 762bd4613fa8ad6cdfb5b44745a7ce78b331499d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820420"
---
# <a name="publishing-extensions"></a>Extensions de publication

>S'applique à : Windows Admin Center, version préliminaire de Windows Admin Center

Une fois que vous avez développé votre extension, vous devez le publier et le rendre disponible à d’autres personnes pour tester ou utiliser. En fonction de votre audience et l’objectif de la publication, il existe quelques options qui nous présenterons ci-dessous, ainsi que les étapes et les exigences pour la publication.

## <a name="publishing-options"></a>Options de publication

Il existe trois options principales pour les sources de package configurable qui prend en charge de Windows Admin Center :
* Public Windows Admin Center flux NuGet de Microsoft
* Vos propres flux NuGet privé
* Local ou partage de fichiers réseau

### <a name="publishing-to-the-windows-admin-center-extension-feed"></a>Publication à l’extension Windows Admin Center de flux

Par défaut, Windows Admin Center est connecté à un package NuGet flux géré par l’équipe de produit Windows Admin Center chez Microsoft. Premières versions préliminaires de nouvelles extensions développées par Microsoft peuvent être publiée sur ce flux et disponibles pour les utilisateurs Windows Admin Center. Planification créer et distribuer publiquement les extensions des développeurs externes peuvent également [soumettre une demande](#publishing-your-extension-to-the-windows-admin-center-feed) à publier sur ce flux.

### <a name="publishing-to-a-different-nuget-feed"></a>Flux de publication sur un autre package de NuGet

Vous pouvez également créer votre propre flux pour publier vos extensions à l’aide d’une des nombreuses NuGet [différentes options pour la configuration d’une source privée ou à l’aide d’un service d’hébergement de NuGet](https://docs.microsoft.com/nuget/hosting-packages/overview). Le flux NuGet doit prendre en charge l’API v2 de NuGet. Dans la mesure où Windows Admin Center ne prend pas en charge les flux d’authentification, le flux doit être configuré pour autoriser l’accès en lecture à tout le monde.

### <a name="publishing-to-a-file-share"></a>Publication sur un partage de fichiers

Pour restreindre l’accès de votre extension à votre organisation ou à un groupe limité d’utilisateurs, vous pouvez utiliser un partage de fichiers SMB en tant qu’extension du flux. Dans ce cas, les autorisations de partage et le dossier de fichiers seront appliqueront pour autoriser l’accès au flux.

## <a name="preparing-your-extension-for-release"></a>Préparation de votre extension pour la mise en production

Vérifiez que vous lire et prendre en compte les rubriques de développement suivantes :

- [Contrôler la visibilité de votre outil](guides/dynamic-tool-display.md)
- [Chaînes et localisation](guides/strings-localization.md)

### <a name="consider-releasing-as-a-preview-release"></a>Envisager de sortir en version préliminaire

Si vous lancez une version préliminaire de votre extension à des fins d’évaluation, nous recommandons que vous :

- Ajouter « (aperçu) » à la fin du titre de votre extension dans le fichier .nuspec
- Expliquez les limites dans la description de votre extension dans le fichier .nuspec

## <a name="creating-an-extension-package"></a>Création d’un package d’extension

Windows Admin Center utilise des flux pour la distribution et le téléchargement des extensions et des packages NuGet.  Dans l’ordre pour votre paquet à expédier, vous devez générer un package NuGet contenant votre plug-ins et les extensions.  Un package unique peut contenir une extension de l’interface utilisateur ainsi que sur un plug-in de la passerelle, et la section suivante vous guidera dans le processus.

### <a name="1-build-your-extension"></a>1. Générer votre extension

Dès que vous êtes prêt à empaqueter votre extension, créez un répertoire sur votre système de fichiers, ouvrez une console et CD dedans.  Il s’agit du répertoire racine qui nous utiliserons pour contenir tous les répertoires de nuspec et de contenu qui composent notre package.  Nous ferons référence à ce dossier en tant que « Package NuGet » pendant la durée de ce document.

#### <a name="ui-extensions"></a>Extensions de l’interface utilisateur

Pour commencer le processus sur la collecte de tout le contenu nécessaire pour une extension de l’interface utilisateur, exécutez « gulp build » sur votre outil et vérifiez que la génération a réussi.  Packages de ce processus tous les composants ensemble dans un dossier nommé « regrouper » situés dans le répertoire racine de votre extension (au même niveau du répertoire src).  Copiez ce répertoire et tous ses contenu dans le dossier « NuGet Package ».

#### <a name="gateway-plugins"></a>Plug-ins de la passerelle

À l’aide de votre infrastructure de Build (il peut s’agir aussi simple que d’ouvrir Visual Studio et en cliquant sur le bouton de génération), compiler et générer votre plug-in.  Ouvrez votre répertoire de sortie et copier la DLL qui représentent votre plug-in et les placer dans un nouveau dossier dans le répertoire « NuGet Package » appelé « package ».  Il est inutile de copier la dll FeatureInterface, simplement l’ou les DLL qui représentent votre code.

### <a name="2-create-the-nuspec-file"></a>2. Créer le fichier .nuspec

Pour créer le package NuGet, vous devez d’abord créer un fichier .nuspec. Un fichier .nuspec est un manifeste XML qui contient des métadonnées de package NuGet. Ce manifeste est utilisé pour générer le package et pour fournir des informations aux consommateurs.  Placez ce fichier à la racine du dossier « NuGet Package ».

Voici un exemple de fichier .nuspec et la liste des propriétés requises ou recommandées. Pour le schéma complet, consultez la [de référence sur .nuspec](https://docs.microsoft.com/nuget/reference/nuspec). Enregistrez le fichier .nuspec au dossier racine de votre projet avec un nom de fichier de votre choix.

> [!IMPORTANT]
> Le ```<id>``` valeur dans le fichier .nuspec doit correspondre à la ```"name"``` valeur dans votre projet ```manifest.json``` fichier, sans quoi votre extension publiée ne se charge pas correctement dans Windows Admin Center.

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

#### <a name="required-or-recommended-properties"></a>Requis ou recommandés propriétés

| Nom de la propriété | Requis / recommandé | Description |
| ---- | ---- | ---- |
| packageType | Obligatoire | Utilisez « WindowsAdminCenterExtension », qui est le type de package NuGet défini pour les extensions Windows Admin Center. |
| id | Obligatoire | Identificateur unique du Package dans le flux. Cette valeur doit correspondre à la valeur « name » dans le fichier manifest.json de votre projet.  Consultez [choix d’un identificateur de package unique](https://docs.microsoft.com/nuget/create-packages/creating-a-package#choosing-a-unique-package-identifier-and-setting-the-version-number) pour obtenir des conseils. |
| title | Requis pour la publication sur le Windows Admin Center de flux | Nom convivial pour le package s’affiche dans le Gestionnaire d’extensions Windows Admin Center. |
| version | Obligatoire | Version de l’extension. À l’aide de [Semver (convention SemVer)](http://semver.org/spec/v1.0.0.html) est recommandé mais pas obligatoire. |
| Auteurs | Obligatoire | Si la publication pour le compte de votre entreprise, utilisez le nom de votre entreprise. |
| description | Obligatoire | Fournissez une description des fonctionnalités de l’extension. |
| iconUrl | Recommandé lors de la publication pour le Windows Admin Center de flux | URL d’icône à afficher dans le Gestionnaire d’extensions. |
| projectUrl | Requis pour la publication sur le Windows Admin Center de flux | URL vers le site Web de votre extension. Si vous n’avez pas d’un site Web distinct, utilisez l’URL de la page Web de package sur le flux NuGet. |
| licenseUrl | Requis pour la publication sur le Windows Admin Center de flux | URL vers le contrat de licence utilisateur final de votre extension. |
| fichiers | Obligatoire | Ces deux paramètres, définir la structure de dossier Windows Admin Center attend des extensions de l’interface utilisateur et des plug-ins de la passerelle. |

### <a name="3-build-the-extension-nuget-package"></a>3. Générer le package NuGet d’extension

Utilisez le fichier .nuspec que vous avez créé ci-dessus, vous allez maintenant créer le fichier .nupkg de package NuGet que vous pouvez télécharger et publier sur le flux NuGet.

1. Télécharger l’outil CLI de nuget.exe à partir de la [site Web des outils clients NuGet](https://docs.microsoft.com/nuget/install-nuget-client-tools).
2. Exécutez « pack de nuget.exe [nom de fichier .nuspec] » pour créer le fichier .nupkg.

### <a name="4-signing-your-extension-nuget-package"></a>4. Signature de votre package NuGet d’extension

Tous les fichiers .dll inclus dans votre extension sont requis pour être signé avec un certificat à partir d’une autorité de certificat approuvé (CA). Par défaut, les fichiers .dll non signé ne pourra s’exécuter lorsque Windows Admin Center s’exécute en Mode de Production.

Nous recommandons également vivement que vous vous connectez le package NuGet d’extension pour garantir l’intégrité du package, mais ce n’est pas une étape obligatoire.

### <a name="5-test-your-extension-nuget-package"></a>5. Tester votre package NuGet d’extension

Votre package d’extension est désormais prêt à être testé ! Télécharger le fichier .nupkg vers un flux NuGet ou copiez-la dans un partage de fichiers. Pour afficher et télécharger des packages à partir d’un autre flux ou un partage de fichiers, vous devrez [modifier votre configuration de l’alimentation](../configure/using-extensions.md#installing-extensions-from-a-different-feed) pour pointer vers votre flux NuGet ou partage de fichiers. Lorsque vous testez, assurez-vous que les propriétés sont affichent correctement dans le Gestionnaire d’extensions et pouvoir installer et désinstaller votre extension.

## <a name="publishing-your-extension-to-the-windows-admin-center-feed"></a>Flux de publication de votre extension pour le Windows Admin Center

En le publiant sur le Windows Admin Center flux, vous pouvez mettre votre extension de disposition à tout utilisateur Windows Admin Center. Étant donné que le Kit de développement logiciel Windows Admin Center est toujours en version préliminaire, nous souhaitons travailler en étroite collaboration avec vous aider à résoudre les problèmes de développement et, assurez-vous que vous êtes en mesure de livrer un produit de qualité découvrir à vos utilisateurs.

Avant de libérer de la version initiale de votre extension, nous recommandons que vous envoyez une demande de révision d’extension à Microsoft au moins 2 à 3 semaines avant la mise en production afin de que nous avoir suffisamment de temps pour passer en revue et vous pouvez apporter des modifications à votre extension si nécessaire. Une fois que votre extension est prête à être publié, vous devez nous l’envoyer pour révision et approbation, nous allons le publier au flux pour vous.

Par la suite, si vous souhaitez publier une mise à jour vers votre extension, vous devez envoyer une autre demande de révision. Bien que, selon l’étendue de modification, le délai d’exécution pour les évaluations de mise à jour doit généralement être plus court.

### <a name="submit-an-extension-review-request-to-microsoft"></a>Soumettre une demande de révision d’extension à Microsoft

Pour soumettre une demande de révision d’extension, entrez les informations suivantes et envoyer sous forme d’un e-mail à [ wacextensionrequest@microsoft.com ](mailto:wacextensionrequest@microsoft.com?subject=Windows%20Admin%20Center%20Extension%20Review%20Request). Nous vous répondrons à votre courrier électronique dans une semaine.

```
Windows Admin Center Extension Review Request
1. Name and email address of extension owner/developer (up to 3 users). If you will be releasing an extension on behalf of your company, provide your company email address.
2. Company name (Only required if you are releasing an extension on behalf of your company):
3. Extension name:
4. Release target date (estimate):
5. For new extension submission - Extension description (early design wire frames, screen mockups or product screenshots are highly recommended):
6. For extension update review – Description of changes (include product screenshots if UI has been significantly changed):
```

### <a name="submit-your-extension-package-for-review-and-publishing"></a>Soumettre votre package d’extension pour révision et la publication

Assurez-vous de suivre les instructions ci-dessus pour [création d’un package d’extension](#creating-an-extension-package) et le fichier .nuspec est défini correctement et que les fichiers sont signés. Nous vous recommandons également d’avoir un site Web du projet, y compris les éléments suivants :

- Description détaillée de votre extension, y compris des captures d’écran ou vidéo
- Fonctionnalité d’adresse ou le site Web par courrier électronique pour recevoir des commentaires ou questions

Lorsque vous êtes prêt à publier votre extension, envoyez un e-mail à [ wacextensionrequest@microsoft.com ](mailto:wacextensionrequest@microsoft.com?subject=Windows%20Admin%20Center%20Extension%20Package%20Review) et nous fournirons des instructions sur la façon de nous envoyer votre package d’extension. Une fois que nous recevons votre package, nous vérifierons et cas d’approbation, publier sur le Windows Admin Center de flux.