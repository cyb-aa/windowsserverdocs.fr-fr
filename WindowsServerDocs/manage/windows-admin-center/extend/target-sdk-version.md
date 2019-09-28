---
title: Cibler une version différente du kit de développement logiciel (SDK) du centre d’administration Windows
description: Cibler une version différente du kit de développement logiciel (SDK) du centre d’administration Windows (projet Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 0d3b7af5229f7b8487aa9f04eaf0d1756d8c02f4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356969"
---
# <a name="target-a-different-version-of-the-windows-admin-center-sdk"></a>Cibler une version différente du kit de développement logiciel (SDK) du centre d’administration Windows

>S'applique à : Windows Admin Center, Windows Admin Center Preview

Il est facile de tenir à jour votre extension avec les modifications du SDK et les modifications de la plateforme.  Nous utilisons des [balises NPM](https://www.npmjs.com/package/@microsoft/windows-admin-center-sdk) pour organiser la publication de nouvelles fonctionnalités dans les versions du kit de développement logiciel (SDK).

Il existe trois versions du kit de développement logiciel (SDK) que vous pouvez choisir :

* ```latest``` – ce package SDK s’aligne avec la version GA actuelle du centre d’administration Windows
* ```insider``` – ce package SDK s’aligne avec la version préliminaire actuelle du centre d’administration Windows (disponible dans la version préliminaire de Windows Server Insider)
* ```next``` : ce package SDK contient les fonctionnalités les plus récentes

> [!NOTE]
> En savoir plus sur les différentes [versions](https://aka.ms/WACDownloadPage) du centre d’administration Windows qui peuvent être téléchargées.

## <a name="targeting-sdk-version-on-a-new-project"></a>Ciblage de la version du kit de développement logiciel (SDK) sur un nouveau projet

Lorsque vous créez une extension, vous pouvez inclure le paramètre ```--version``` pour cibler une version différente du kit de développement logiciel (SDK) :

```
wac create --company "{!Company Name}" --tool "{!Tool Name}" --version {!version}
```

| Value | Explication | Exemple |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | Nom de votre société (avec des espaces) | ```Contoso Inc``` |
| ```{!Tool Name}``` | Votre nom d’outil (avec des espaces) | ```Manage Foo Works``` |
| ```{!version}``` | Version du SDK | ```latest``` |

Voici un exemple de création d’une nouvelle extension ciblant ```insider``` :

```
wac create --company "Contoso Inc" --tool "Manage Foo Works" --version insider
```

## <a name="targeting-sdk-version-on-an-existing-project"></a>Ciblage de la version du kit de développement logiciel (SDK) sur un projet existant

Pour modifier un projet existant afin de cibler une autre version du kit de développement logiciel (SDK), modifiez la ligne suivante dans ```package.json``` :

```
"@microsoft/windows-admin-center-sdk": "latest",
```
Dans cet exemple, remplacez ```latest``` par la version du kit de développement logiciel (SDK) de votre choix, c’est-à-dire ```insider``` :

```
"@microsoft/windows-admin-center-sdk": "insider",
```

Exécutez ensuite ```npm install``` pour mettre à jour les références dans votre projet.