---
title: Cibler une version différente du Kit de développement Windows Admin Center
description: Cibler une version différente du SDK Windows Admin Center (projet Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 47ae669e517f963762ee6267594e18f3413a72ff
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833620"
---
# <a name="target-a-different-version-of-the-windows-admin-center-sdk"></a>Cibler une version différente du Kit de développement Windows Admin Center

>S'applique à : Windows Admin Center, version préliminaire de Windows Admin Center

Il est facile de garder votre extension à jour avec les modifications apportées au SDK et les modifications de la plateforme.  Nous utilisons [NPM balises](https://www.npmjs.com/package/@microsoft/windows-admin-center-sdk) pour organiser la publication de nouvelles fonctionnalités dans les versions SDK.

Il existe trois versions SDK que vous pouvez choisir à partir de :

* ```latest``` – Ce package SDK s’aligne avec la version actuelle de la disponibilité générale de Windows Admin Center
* ```insider``` – Ce package SDK s’aligne avec la version préliminaire actuelle de Windows Admin Center (disponible sur Windows Server Insider Preview)
* ```next``` – Ce package du Kit de développement logiciel contient la fonctionnalité la plus récente

> [!NOTE]
> En savoir plus sur les différentes [versions](https://aka.ms/WACDownloadPage) de Windows Admin Center qui sont disponibles en téléchargement.

## <a name="targeting-sdk-version-on-a-new-project"></a>Ciblage de version SDK sur un nouveau projet

Lorsque vous créez une nouvelle extension, vous pouvez inclure le ```--version``` paramètre pour cibler une version différente du SDK :

```
wac create --company "{!Company Name}" --tool "{!Tool Name}" --version {!version}
```

| Value | Explication | Exemple |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | Nom de votre société (avec des espaces) | ```Contoso Inc``` |
| ```{!Tool Name}``` | Votre nom de l’outil (avec des espaces) | ```Manage Foo Works``` |
| ```{!version}``` | Version SDK | ```latest``` |

Voici un exemple de création d’une nouvelle extension de ciblage ```insider```:

```
wac create --company "Contoso Inc" --tool "Manage Foo Works" --version insider
```

## <a name="targeting-sdk-version-on-an-existing-project"></a>Ciblage de version SDK sur un projet existant

Pour modifier un projet existant pour cibler une autre version du Kit de développement logiciel, modifiez la ligne suivante dans ```package.json```:

```
"@microsoft/windows-admin-center-sdk": "latest",
```
Dans cet exemple, remplacez ```latest``` avec votre version de kit de développement logiciel souhaitée, par exemple, ```insider```:

```
"@microsoft/windows-admin-center-sdk": "insider",
```

Puis exécutez ```npm install``` pour mettre à jour les références dans votre projet.