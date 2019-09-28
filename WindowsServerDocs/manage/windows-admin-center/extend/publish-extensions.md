---
title: Publication des extensions pour le centre d’administration Windows
description: Publication des extensions pour le centre d’administration Windows (projet Honolulu)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 24beb287aa35757e1f8057920e8fd95828baf83b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385204"
---
# <a name="publishing-extensions"></a>Extensions de publication

>S'applique à : Windows Admin Center, Windows Admin Center Preview

Une fois que vous avez développé votre extension, vous pouvez la publier et la mettre à la disposition des autres utilisateurs pour la tester ou l’utiliser. Selon votre audience et l’objectif de la publication, il existe quelques options que nous allons introduire ci-dessous, ainsi que les étapes et la configuration requise pour la publication.

## <a name="publishing-options"></a>Options de publication

Il existe trois options principales pour les sources de package configurables que le centre d’administration Windows prend en charge :
* Flux NuGet du centre d’administration Windows public de Microsoft
* Votre propre flux NuGet privé
* Partage de fichiers local ou réseau

### <a name="publishing-to-the-windows-admin-center-extension-feed"></a>Publication dans le flux d’extension du centre d’administration Windows

Par défaut, le centre d’administration Windows est connecté à un flux NuGet géré par l’équipe du centre d’administration Windows de Microsoft. Les premières versions préliminaires des nouvelles extensions développées par Microsoft peuvent être publiées sur ce flux et mises à la disposition des utilisateurs du centre d’administration Windows. Les développeurs externes qui envisagent de créer et de publier des extensions publiquement peuvent également [soumettre une demande](#publishing-your-extension-to-the-windows-admin-center-feed) de publication sur ce flux.

### <a name="publishing-to-a-different-nuget-feed"></a>Publication sur un flux NuGet différent

Vous pouvez également créer votre propre flux NuGet pour publier vos extensions sur à l’aide de l’une des nombreuses [options de configuration d’une source privée ou de l’utilisation d’un service d’hébergement NuGet](https://docs.microsoft.com/nuget/hosting-packages/overview). Le flux NuGet doit prendre en charge l’API NuGet v2. Étant donné que le centre d’administration Windows ne prend pas en charge l’authentification par flux, le flux doit être configuré pour autoriser l’accès en lecture à n’importe qui.

### <a name="publishing-to-a-file-share"></a>Publication sur un partage de fichiers

Pour restreindre l’accès de votre extension à votre organisation ou à un groupe limité de personnes, vous pouvez utiliser un partage de fichiers SMB en tant que flux d’extension. Dans ce cas, le partage de fichiers et les autorisations de dossier seront appliqués pour autoriser l’accès au flux.

## <a name="preparing-your-extension-for-release"></a>Préparation de votre extension pour la mise en production

Veillez à lire et à prendre en compte les rubriques de développement suivantes :

- [Contrôler la visibilité de votre outil](guides/dynamic-tool-display.md)
- [Chaînes et localisation](guides/strings-localization.md)

### <a name="consider-releasing-as-a-preview-release"></a>Envisagez de publier en version préliminaire

Si vous publiez une version préliminaire de votre extension à des fins d’évaluation, nous vous recommandons d’effectuer les opérations suivantes :

- Ajoutez « (Preview) » à la fin du titre de votre extension dans le fichier. NuSpec
- Expliquer les limitations de la description de votre extension dans le fichier. NuSpec

## <a name="creating-an-extension-package"></a>Création d’un package d’extension

Le centre d’administration Windows utilise des packages et des flux NuGet pour distribuer et télécharger des extensions.  Pour que votre package soit expédié, vous devez générer un package NuGet contenant vos plug-ins et extensions.  Un package unique peut contenir à la fois une extension d’interface utilisateur et un plug-in de passerelle, et la section suivante vous guide tout au long du processus.

### <a name="1-build-your-extension"></a>1. Générer votre extension

Dès que vous êtes prêt à commencer à empaqueter votre extension, créez un nouveau répertoire sur votre système de fichiers, ouvrez une console et CD-ROM.  Il s’agit du répertoire racine que nous allons utiliser pour contenir tous les répertoires NuSpec et de contenu qui composent notre package.  Nous allons faire référence à ce dossier en tant que « package NuGet » pour la durée de ce document.

#### <a name="ui-extensions"></a>Extensions d’interface utilisateur

Pour commencer le processus de collecte de tout le contenu nécessaire pour une extension d’interface utilisateur, exécutez « Gulp Build » sur votre outil et assurez-vous que la génération a réussi.  Ce processus regroupe tous les composants dans un dossier appelé « Bundle » situé dans le répertoire racine de votre extension (au même niveau du répertoire SRC).  Copiez ce répertoire et tout son contenu dans le dossier « NuGet package ».

#### <a name="gateway-plugins"></a>Plug-ins de passerelle

À l’aide de votre infrastructure de génération (cela peut être aussi simple que l’ouverture de Visual Studio et le clic sur le bouton Générer), compilez et générez votre plug-in.  Ouvrez votre répertoire de sortie de génération et copiez la ou les dll qui représentent votre plug-in, puis placez-les dans un nouveau dossier à l’intérieur du répertoire « package NuGet » appelé « Package ».  Vous n’avez pas besoin de copier la dll FeatureInterface, mais simplement la ou les dll qui représentent votre code.

### <a name="2-create-the-nuspec-file"></a>2. Créer le fichier. NuSpec

Pour créer le package NuGet, vous devez d’abord créer un fichier. nuspec. Un fichier. NuSpec est un manifeste XML qui contient des métadonnées de package NuGet. Ce manifeste est utilisé à la fois pour générer le package et pour fournir des informations aux consommateurs.  Placez ce fichier à la racine du dossier « NuGet package ».

Voici un exemple de fichier. NuSpec et la liste des propriétés requises ou recommandées. Pour obtenir le schéma complet, consultez la [référence. NuSpec](https://docs.microsoft.com/nuget/reference/nuspec). Enregistrez le fichier. NuSpec dans le dossier racine de votre projet avec le nom de fichier de votre choix.

> [!IMPORTANT]
> La ```<id>``` valeur dans le fichier. NuSpec doit correspondre à la ```"name"``` valeur dans le fichier de ```manifest.json``` votre projet, sinon votre extension publiée ne sera pas chargée correctement dans le centre d’administration Windows.

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

#### <a name="required-or-recommended-properties"></a>Propriétés requises ou recommandées

| Nom de la propriété | Obligatoire/recommandé | Description |
| ---- | ---- | ---- |
| PackageType | Obligatoire | Utilisez « WindowsAdminCenterExtension », qui est le type de package NuGet défini pour les extensions du centre d’administration Windows. |
| id | Obligatoire | Identificateur de package unique dans le flux. Cette valeur doit correspondre à la valeur « Name » dans le fichier manifest. JSON de votre projet.  Pour obtenir de l’aide, consultez [choix d’un identificateur de package unique](https://docs.microsoft.com/nuget/create-packages/creating-a-package#choosing-a-unique-package-identifier-and-setting-the-version-number) . |
| title | Requis pour la publication dans le flux du centre d’administration Windows | Nom convivial du package affiché dans le gestionnaire d’extensions du centre d’administration Windows. |
| version | Obligatoire | Version de l’extension. L’utilisation [de la gestion sémantique des versions (SemVer Convention)](http://semver.org/spec/v1.0.0.html) est recommandée, mais n’est pas obligatoire. |
| extrait | Obligatoire | Si vous publiez au nom de votre société, utilisez le nom de votre société. |
| description | Obligatoire | Fournissez une description de la fonctionnalité de l’extension. |
| iconUrl | Recommandé lors de la publication dans le flux du centre d’administration Windows | URL de l’icône à afficher dans le gestionnaire d’extensions. |
| projectUrl | Requis pour la publication dans le flux du centre d’administration Windows | URL du site Web de votre extension. Si vous n’avez pas de site Web distinct, utilisez l’URL de la page Web du package sur le flux NuGet. |
| licenseUrl | Requis pour la publication dans le flux du centre d’administration Windows | URL du contrat de licence utilisateur final de votre extension. |
| fichiers | Obligatoire | Ces deux paramètres configurent la structure de dossiers attendue par le centre d’administration Windows pour les extensions d’interface utilisateur et les plug-ins de passerelle. |

### <a name="3-build-the-extension-nuget-package"></a>3. Générer le package NuGet d’extension

À l’aide du fichier. NuSpec que vous avez créé ci-dessus, vous allez maintenant créer le fichier. nupkg du package NuGet que vous pouvez télécharger et publier sur le flux NuGet.

1. Téléchargez l’outil CLI NuGet. exe à partir du [site Web des outils clients NuGet](https://docs.microsoft.com/nuget/install-nuget-client-tools).
2. Exécutez « NuGet. exe Pack [. NuSpec nom de fichier] » pour créer le fichier. nupkg.

### <a name="4-signing-your-extension-nuget-package"></a>4. Signature de votre package NuGet d’extension

Tous les fichiers. dll inclus dans votre extension doivent être signés avec un certificat d’une autorité de certification approuvée. Par défaut, l’exécution des fichiers. dll non signés est bloquée lorsque le centre d’administration Windows s’exécute en mode production.

Nous vous recommandons également de signer le package NuGet d’extension pour garantir l’intégrité du package, mais cette étape n’est pas obligatoire.

### <a name="5-test-your-extension-nuget-package"></a>5. Tester votre package NuGet d’extension

Votre package d’extension est maintenant prêt pour le test. Chargez le fichier. nupkg dans un flux NuGet ou copiez-le dans un partage de fichiers. Pour afficher et télécharger des packages à partir d’un autre flux ou partage de fichiers, vous devez [modifier votre configuration de flux](../configure/using-extensions.md#installing-extensions-from-a-different-feed) pour pointer vers votre flux ou partage de fichiers NuGet. Lors du test, assurez-vous que les propriétés s’affichent correctement dans le gestionnaire d’extensions et que vous pouvez installer et désinstaller correctement votre extension.

## <a name="publishing-your-extension-to-the-windows-admin-center-feed"></a>Publication de votre extension dans le flux du centre d’administration Windows

En publiant sur le flux du centre d’administration Windows, vous pouvez rendre votre extension disponible pour n’importe quel utilisateur du centre d’administration Windows. Étant donné que le kit de développement logiciel (SDK) du centre d’administration Windows est toujours en version préliminaire, nous aimerions vous aider à résoudre les problèmes de développement et à vous assurer que vous êtes en mesure de fournir un produit et une expérience de qualité à vos utilisateurs.

Avant de publier la version initiale de votre extension, nous vous recommandons de soumettre une demande de révision d’extension à Microsoft au moins 2-3 semaines avant le lancement pour vous assurer que nous disposons d’un délai suffisant pour la révision et que vous souhaitez apporter des modifications à votre extension si nécessaire. Une fois que votre extension est prête à être publiée, vous devez nous l’envoyer pour examen et, si elle est approuvée, nous la publierons pour vous.

Par la suite, si vous souhaitez publier une mise à jour de votre extension, vous devrez envoyer une autre demande de révision. En fonction de l’étendue des modifications, le temps de bouclage pour les révisions de mise à jour doit généralement être plus court.

### <a name="submit-an-extension-review-request-to-microsoft"></a>Soumettre une demande de révision d’extension à Microsoft

Pour soumettre une demande de révision d’extension, entrez les informations suivantes et envoyez-les [wacextensionrequest@microsoft.com](mailto:wacextensionrequest@microsoft.com?subject=Windows%20Admin%20Center%20Extension%20Review%20Request)en tant qu’e-mail à. Nous répondrons à votre courrier électronique dans une semaine.

```
Windows Admin Center Extension Review Request
1. Name and email address of extension owner/developer (up to 3 users). If you will be releasing an extension on behalf of your company, provide your company email address.
2. Company name (Only required if you are releasing an extension on behalf of your company):
3. Extension name:
4. Release target date (estimate):
5. For new extension submission - Extension description (early design wire frames, screen mockups or product screenshots are highly recommended):
6. For extension update review – Description of changes (include product screenshots if UI has been significantly changed):
```

### <a name="submit-your-extension-package-for-review-and-publishing"></a>Envoi de votre package d’extension pour la révision et la publication

Veillez à suivre les instructions ci-dessus pour [créer un package d’extension](#creating-an-extension-package) et le fichier. NuSpec est correctement défini, et les fichiers sont signés. Nous vous recommandons également de disposer d’un site Web de projet incluant les éléments suivants :

- Description détaillée de votre extension, y compris les captures d’écran ou la vidéo
- Adresse de messagerie ou fonctionnalité de site Web pour recevoir des commentaires ou des questions

Lorsque vous êtes prêt à publier votre extension, envoyez un e-mail à [wacextensionrequest@microsoft.com](mailto:wacextensionrequest@microsoft.com?subject=Windows%20Admin%20Center%20Extension%20Package%20Review) et nous vous fournissons des instructions sur la façon de nous envoyer votre package d’extension. Une fois que nous avons reçu votre package, nous passerons en revue et, s’ils sont approuvés, à publier dans le flux du centre d’administration Windows.