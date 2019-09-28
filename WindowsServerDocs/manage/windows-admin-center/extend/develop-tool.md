---
title: Développer une extension d'outil
description: Développer une extension d’outil Kit de développement logiciel (SDK) du centre d’administration Windows (projet Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: de2cbf3a47771555eef02cd7d18f93b2b33227b3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406902"
---
# <a name="develop-a-tool-extension"></a>Développer une extension d'outil

>S'applique à : Windows Admin Center, Windows Admin Center Preview

Une extension d’outil est la principale façon dont les utilisateurs interagissent avec le centre d’administration Windows pour gérer une connexion, telle qu’un serveur ou un cluster. Lorsque vous cliquez sur une connexion dans l’écran d’accueil du centre d’administration Windows et que vous vous connectez, une liste d’outils s’affiche dans le volet de navigation gauche. Lorsque vous cliquez sur un outil, l’extension de l’outil est chargée et affichée dans le volet droit.

Quand une extension d’outil est chargée, elle peut exécuter des appels WMI ou des scripts PowerShell sur un serveur ou un cluster cible et afficher des informations dans l’interface utilisateur ou exécuter des commandes en fonction de l’entrée d’utilisateur. Les extensions d’outils définissent les solutions pour lesquelles elles doivent être affichées, ce qui donne lieu à un ensemble d’outils différent pour chaque solution.

> [!NOTE]
> Vous n’êtes pas familiarisé avec les différents types d’extensions ? En savoir plus sur l' [architecture et les types d’extension d’extensibilité](understand-extensions.md).

## <a name="prepare-your-environment"></a>Préparer votre environnement

Si vous ne l’avez pas déjà fait, [Préparez votre environnement](prepare-development-environment.md) en installant des dépendances et des prérequis globaux requis pour tous les projets.

## <a name="create-a-new-tool-extension-with-the-windows-admin-center-cli"></a>Créer une extension d’outil avec l’interface de commande du centre d’administration Windows ##

Une fois que toutes les dépendances sont installées, vous êtes prêt à créer votre nouvelle extension d’outil.  Créez ou accédez à un dossier qui contient vos fichiers de projet, ouvrez une invite de commandes et définissez ce dossier comme répertoire de travail.  À l’aide de l’interface de commande du centre d’administration Windows qui a été précédemment installée, créez une nouvelle extension avec la syntaxe suivante :

``` cmd
wac create --company "{!Company Name}" --tool "{!Tool Name}"
```

| Value | Explication | Exemple |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | Nom de votre société (avec des espaces) | ```Contoso Inc``` |
| ```{!Tool Name}``` | Votre nom d’outil (avec des espaces) | ```Manage Foo Works``` |

Voici un exemple d’utilisation :

``` cmd
wac create --company "Contoso Inc" --tool "Manage Foo Works"
```

Cela crée un nouveau dossier dans le répertoire de travail actuel en utilisant le nom que vous avez spécifié pour votre outil, copie tous les fichiers de modèle nécessaires dans votre projet et configure les fichiers avec le nom de votre entreprise et de votre outil.  

Ensuite, accédez au dossier que vous venez de créer, puis installez les dépendances locales requises en exécutant la commande suivante :

``` cmd
npm install
```

Une fois cette opération terminée, vous avez configuré tout ce dont vous avez besoin pour charger votre nouvelle extension dans le centre d’administration Windows. 

## <a name="add-content-to-your-extension"></a>Ajouter du contenu à votre extension

Maintenant que vous avez créé une extension avec l’interface de commande du centre d’administration Windows, vous êtes prêt à personnaliser le contenu.  Pour obtenir des exemples de ce que vous pouvez faire, consultez les guides suivants :

- Ajouter un [module vide](guides/add-module.md)
- Ajouter un [IFRAME](guides/add-iframe.md)
 
Vous trouverez d’autres exemples sur notre [site SDK GitHub](https://aka.ms/wacsdk):
-  [Outils de développement](https://github.com/Microsoft/windows-admin-center-sdk/tree/master/windows-admin-center-developer-tools) est une extension entièrement fonctionnelle qui peut être chargée dans le centre d’administration Windows et contient une collection complète d’exemples de fonctionnalités et d’outils que vous pouvez parcourir et utiliser dans votre propre extension.

## <a name="customize-your-extensions-icon"></a>Personnaliser l’icône de l’extension

Vous pouvez personnaliser l’icône qui s’affiche pour votre extension dans la liste d’outils.  Pour ce faire, modifiez toutes les entrées ```icon``` dans ```manifest.json``` pour votre extension :

``` json
"icon": "{!icon-uri}",
```

| Value | Explication | Exemple d’URI |
| ----- | ----------- | ------- |
| ```{!icon-uri}``` | Emplacement de votre ressource icône | ```assets/foo-icon.svg``` |

REMARQUE : Actuellement, les icônes personnalisées ne sont pas visibles lorsque vous chargez votre extension en mode dev.  Pour contourner ce problème, supprimez le contenu de ```target``` comme suit :

``` json
"target": "",
```

Cette configuration n’est valide que pour le chargement latéral en mode dev. il est donc important de conserver la valeur contenue dans ```target```, puis de la restaurer avant de publier votre extension.

## <a name="build-and-side-load-your-extension"></a>Créez et chargez votre extension

Ensuite, créez et chargez votre extension dans le centre d’administration Windows.  Ouvrez une fenêtre de commande, accédez au répertoire source, puis vous êtes prêt à générer.

* Créez et proposez avec gulp :

    ``` cmd
    gulp build
    gulp serve -p 4201
    ```

Notez que vous devez choisir un port actuellement disponible. Veillez à ne pas essayer d’utiliser le port sur lequel Windows Admin Center est en cours d’exécution.

Votre projet peut être chargé de manière indépendante dans une instance locale de Windows Admin Center pour les tests en s’attachant au projet servi localement dans Windows Admin Center.

* Lancez Windows Admin Center dans un navigateur web
* Ouvrez le débogueur (F12)
* Ouvrez la Console, puis tapez la commande suivante :

    ``` cmd
    MsftSme.sideLoad("http://localhost:4201")
    ```

*   Actualisez le navigateur web

Votre projet sera désormais visible dans la liste d'outils avec (side loaded) en regard du nom.

## <a name="target-a-different-version-of-the-windows-admin-center-sdk"></a>Cibler une version différente du kit de développement logiciel (SDK) du centre d’administration Windows

Il est facile de tenir à jour votre extension avec les modifications du SDK et les modifications de la plateforme.  Découvrez comment [cibler une version différente](target-sdk-version.md) du kit de développement logiciel (SDK) du centre d’administration Windows.