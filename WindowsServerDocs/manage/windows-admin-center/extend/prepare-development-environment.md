---
title: Préparer votre environnement de développement
description: Préparation de votre environnement de développement SDK Windows Admin Center (projet Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.date: 09/18/2018
ms.prod: windows-server
ms.openlocfilehash: 136107210d2a8a4b336c9e4eb809e2ca096bfba2
ms.sourcegitcommit: 20d07170c7f3094c2fb4455f54b13ec4b102f2d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81269216"
---
# <a name="prepare-your-development-environment"></a>Préparer votre environnement de développement

>S’applique à : Windows Admin Center, Windows Admin Center Preview

Nous allons commencer à développer des extensions avec le kit de développement logiciel (SDK) du centre d’administration Windows.  Dans ce document, nous allons aborder le processus de mise en route de votre environnement pour créer et tester une extension pour le centre d’administration Windows.

> [!NOTE]
> Vous débutez dans le SDK Windows Admin Center ?  Découvrez-en davantage sur les [Extensions pour Windows Admin Center](extensibility-overview.md)

Pour préparer votre environnement de développement, procédez comme suit :

## <a name="install-prerequisites"></a>Installer les prérequis

Pour commencer à développer avec le kit de développement logiciel (SDK), téléchargez et installez les composants requis suivants :

* [Centre d’administration Windows](https://aka.ms/WACDownloadPage) (version GA ou Preview)
* Visual Studio ou [Visual Studio Code](https://code.visualstudio.com)
* [Node. js](https://nodejs.org/en/download/releases/) (version 10.3.0)
* [Gestionnaire de package de nœud](https://npmjs.com/get-npm) (8.12.0 ou version ultérieure)
* [NuGet](https://www.nuget.org/downloads) (pour la publication des extensions)

> [!NOTE]
> Vous devez installer et exécuter Windows Admin Center en mode de développement pour effectuer les étapes ci-dessous. Le mode de développement autorise Windows Admin Center à charger des packages d’extension non signés. Le centre d’administration Windows peut uniquement être installé en mode dev sur un ordinateur Windows 10. 
>
>  Pour activer le mode de développement, installez Windows Admin Center à partir de la ligne de commande avec le paramètre DEV_MODE=1. Dans l’exemple ci-dessous, remplacez ```<version>``` par la version que vous installez, c'est-à-dire ```WindowsAdminCenter1809.msi```.
>
> ```msiexec /i WindowsAdminCenter<version>.msi DEV_MODE=1```

## <a name="install-global-dependencies"></a>Installer les dépendances globales

Ensuite, installez ou mettez à jour les dépendances requises pour vos projets, avec node Package Manager. Ces dépendances seront installées globalement et seront disponibles pour tous les projets.

```
npm install -g npm

npm install -g @angular/cli@7.1.2

npm install -g gulp
npm install -g typescript
npm install -g tslint
npm install -g windows-admin-center-cli
```

>[!NOTE]
>Vous pouvez installer une version ultérieure de @angular/cli, mais sachez que si vous installez une version supérieure à 7.1.2, vous recevrez un avertissement lors de l’étape de génération Gulp que la version locale de l’interface de commande ne correspond pas à la version installée.

## <a name="next-steps"></a>Étapes suivantes :

Maintenant que votre environnement est préparé, vous êtes prêt à commencer à créer du contenu.

- Créer une extension d'[outil](develop-tool.md)
- Créer une extension de [solution](develop-solution.md)
- Créer un [plug-in de passerelle](develop-gateway-plugin.md)
- En savoir plus sur nos [guides](guides.md)

## <a name="sdk-design-toolkit"></a>Kit SDK Design

Consultez notre boîte à outils de [conception du kit de développement logiciel (SDK)](https://github.com/Microsoft/windows-admin-center-sdk/blob/master/WindowsAdminCenterDesignToolkit.zip)Windows Admin Center ! Ce kit d’outils est conçu pour vous aider à simuler rapidement des extensions dans PowerPoint à l’aide des styles, des contrôles et des modèles de pages du centre d’administration Windows. Avant de commencer à coder, consultez à quoi votre extension peut ressembler dans le centre d’administration Windows.

