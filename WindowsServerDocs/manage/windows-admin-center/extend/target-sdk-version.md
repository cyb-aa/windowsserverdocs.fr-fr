---
title: Cibler une autre version du SDK Windows Admin Center
description: Cible une version différente du SDK Windows Admin Center (projet Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 47ae669e517f963762ee6267594e18f3413a72ff
ms.sourcegitcommit: be0144eb59daf3269bebea93cb1c467d67e2d2f1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/20/2018
ms.locfileid: "4081036"
---
# Cibler une autre version du SDK Windows Admin Center

>S’applique à: Windows Admin Center, Windows Admin Center Preview

Il est facile de garder votre extension à jour avec les modifications du Kit de développement logiciel et les modifications de la plateforme.  Nous utilisons [les balises NPM](https://www.npmjs.com/package/@microsoft/windows-admin-center-sdk) pour organiser la publication de nouvelles fonctionnalités dans les versions SDK.

Il existe trois versions SDK que vous pouvez choisir parmi:

* ```latest``` – Ce package SDK s’aligne avec la version GA actuelle de Windows Admin Center
* ```insider``` – Ce package SDK s’aligne avec la version actuelle de la version d’évaluation de Windows Admin Center (disponible sur Windows Server Insider Preview)
* ```next``` – Ce package SDK contient les fonctionnalités plus récentes

> [!NOTE]
> Pour en savoir plus sur les différentes [versions](https://aka.ms/WACDownloadPage) de Windows Admin Center qui peuvent être téléchargés.

## Version SDK sur un nouveau projet de ciblage

Lorsque vous créez une nouvelle extension, vous pouvez inclure la ```--version``` paramètre pour cibler une autre version du SDK:

```
wac create --company "{!Company Name}" --tool "{!Tool Name}" --version {!version}
```

| Valeur | Explication | Exemple |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | Nom de votre société (avec des espaces) | ```Contoso Inc``` |
| ```{!Tool Name}``` | Nom de votre outil (avec des espaces) | ```Manage Foo Works``` |
| ```{!version}``` | Version SDK | ```latest``` |

Voici un exemple de création d’une nouvelle extension de ciblage ```insider```:

```
wac create --company "Contoso Inc" --tool "Manage Foo Works" --version insider
```

## Ciblant la version SDK sur un projet existant

Pour modifier un projet existant pour cibler une autre version SDK, modifiez la ligne suivante dans ```package.json```:

```
"@microsoft/windows-admin-center-sdk": "latest",
```
Dans cet exemple, remplacez ```latest``` avec votre version SDK souhaitée, autrement dit, ```insider```:

```
"@microsoft/windows-admin-center-sdk": "insider",
```

Ensuite, exécutez ```npm install``` pour mettre à jour les références tout au long de votre projet.