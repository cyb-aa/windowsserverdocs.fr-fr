---
title: Préparer votre environnement de développement
description: Préparation de votre environnement de développement SDK Windows Admin Center (projet Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.date: 09/18/2018
ms.prod: windows-server-threshold
ms.openlocfilehash: 7b1a0672ee374f3e2d1339c43576db0e5cabdc36
ms.sourcegitcommit: be0144eb59daf3269bebea93cb1c467d67e2d2f1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/20/2018
ms.locfileid: "4080916"
---
# Préparer votre environnement de développement

>S’applique à: Windows Admin Center, Windows Admin Center Preview

Commençons à développer des extensions avec le SDK Windows Admin Center!  Dans ce document, nous allons décrire le processus en cours d’exécution pour créer et tester une extension pour Windows Admin Center et de votre environnement haut.

> [!NOTE]
> Vous débutez dans le SDK Windows Admin Center?  Découvrez-en davantage sur les [Extensions pour Windows Admin Center](extensibility-overview.md)

Pour préparer votre environnement de développement, procédez comme suit:

## Installer les prérequis

Pour commencer à développer avec le kit de développement logiciel (SDK), téléchargez et installez les composants requis suivants:

* [Windows Admin Center](https://aka.ms/WACDownloadPage) (Version GA ou de la version d’évaluation)
* Visual Studio ou [Visual Studio Code](http://code.visualstudio.com)
* [Node Package Manager](https://npmjs.com/get-npm) (8.12.0 ou version ultérieure)
* [NuGet](https://www.nuget.org/downloads) (pour la publication des extensions)

> [!NOTE]
> Vous devez installer et exécuter Windows Admin Center en mode de développement pour effectuer les étapes ci-dessous. Le mode de développement autorise Windows Admin Center à charger des packages d’extension non signés.
>
>  Pour activer le mode de développement, installez Windows Admin Center à partir de la ligne de commande avec le paramètre DEV_MODE=1. Dans l’exemple ci-dessous, remplacez ```<version>``` par la version que vous installez, c'est-à-dire ```WindowsAdminCenter1809.msi```.
>
> ```msiexec /i WindowsAdminCenter<version>.msi DEV_MODE=1```

## Installer des dépendances globales

Ensuite, installez ou mettez à jour les dépendances requises pour vos projets, avec Node Package Manager. Ces dépendances seront installées globalement et seront disponibles pour tous les projets.

```
npm install -g npm

npm install -g @angular/cli@1.6.5

npm install -g gulp
npm install -g typescript
npm install -g tslint
npm install -g windows-admin-center-cli
```

>[!NOTE]
>Vous pouvez installer une version ultérieure de @angular/cli, sachez que si vous installez une version supérieure à 1.6.5)., vous recevrez un avertissement lors de l’étape de génération gulp que la version locale cli ne correspond pas à la version installée.

## Étapes suivantes

Maintenant que votre environnement est prêt, vous êtes prêt à commencer la création de contenu.

- Créer une extension d'[outil](develop-tool.md)
- Créer une extension de [solution](develop-solution.md)
- Créer un [plug-in de passerelle](develop-gateway-plugin.md)
- En savoir plus sur nos [guides](guides.md)

## Kit d’outils de conception SDK

Regardez notre [Kit d’outils de conception SDK](https://github.com/Microsoft/windows-admin-center-sdk/blob/master/WindowsAdminCenterDesignToolkit.zip)de Windows Admin Center! Ce kit de ressources est conçu pour vous aider à créer rapidement une des extensions dans PowerPoint à l’aide de styles Windows Admin Center, des contrôles et modèles de page. Consultez ce à quoi votre extension peut ressembler dans Windows Admin Center avant de commencer à coder!

