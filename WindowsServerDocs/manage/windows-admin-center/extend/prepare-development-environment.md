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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834760"
---
# <a name="prepare-your-development-environment"></a>Préparer votre environnement de développement

>S'applique à : Windows Admin Center, version préliminaire de Windows Admin Center

Commençons à développer des extensions avec le SDK Windows Admin Center !  Dans ce document, nous aborderons le processus pour obtenir votre environnement et en cours d’exécution pour générer et tester une extension pour Windows Admin Center.

> [!NOTE]
> Vous débutez dans le SDK Windows Admin Center ?  Découvrez-en davantage sur les [Extensions pour Windows Admin Center](extensibility-overview.md)

Pour préparer votre environnement de développement, procédez comme suit :

## <a name="install-prerequisites"></a>Installer les prérequis

Pour commencer à développer avec le kit de développement logiciel (SDK), téléchargez et installez les composants requis suivants :

* [Windows Admin Center](https://aka.ms/WACDownloadPage) (version GA ou préversion)
* Visual Studio ou [Visual Studio Code](http://code.visualstudio.com)
* [Gestionnaire de Package de nœud](https://npmjs.com/get-npm) (8.12.0 ou version ultérieure)
* [NuGet](https://www.nuget.org/downloads) (pour la publication des extensions)

> [!NOTE]
> Vous devez installer et exécuter Windows Admin Center en mode de développement pour effectuer les étapes ci-dessous. Le mode de développement autorise Windows Admin Center à charger des packages d’extension non signés.
>
>  Pour activer le mode de développement, installez Windows Admin Center à partir de la ligne de commande avec le paramètre DEV_MODE=1. Dans l’exemple ci-dessous, remplacez ```<version>``` par la version que vous installez, c'est-à-dire ```WindowsAdminCenter1809.msi```.
>
> ```msiexec /i WindowsAdminCenter<version>.msi DEV_MODE=1```

## <a name="install-global-dependencies"></a>Installer les dépendances globales

Ensuite, installer ou mettre à jour les dépendances requises pour vos projets, avec le Gestionnaire de Package de nœud. Ces dépendances seront installées globalement et seront disponibles pour tous les projets.

```
npm install -g npm

npm install -g @angular/cli@1.6.5

npm install -g gulp
npm install -g typescript
npm install -g tslint
npm install -g windows-admin-center-cli
```

>[!NOTE]
>Vous pouvez installer une version ultérieure de @angular/cli, sachez que si vous installez une version supérieure à 1.6.5)., vous recevrez un avertissement lors de l’étape de génération gulp que la version de cli local ne correspond pas à la version installée.

## <a name="next-steps"></a>Étapes suivantes

Maintenant que votre environnement est prêt, vous êtes prêt à commencer la création de contenu.

- Créer une extension d'[outil](develop-tool.md)
- Créer une extension de [solution](develop-solution.md)
- Créer un [plug-in de passerelle](develop-gateway-plugin.md)
- En savoir plus sur nos [guides](guides.md)

## <a name="sdk-design-toolkit"></a>Kit de ressources de conception SDK

Découvrez notre de Windows Admin Center [boîte à outils de conception de kit de développement logiciel](https://github.com/Microsoft/windows-admin-center-sdk/blob/master/WindowsAdminCenterDesignToolkit.zip)! Cette boîte à outils est conçu pour vous aider à simuler rapidement extensions dans PowerPoint à l’aide de styles Windows Admin Center, les contrôles et les modèles de page. Consultez ce que votre extension peut ressembler dans Windows Admin Center avant de commencer à coder !

