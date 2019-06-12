---
title: Développer une extension d'outil
description: Développer une extension de l’outil Kit de développement Windows Admin Center (projet Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 31d8dbd3df4c44b6e0a3780b022dfbd9fffdffec
ms.sourcegitcommit: 48bb3e5c179dc520fa879b16c9afe09e07c87629
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66452582"
---
# <a name="develop-a-tool-extension"></a>Développer une extension d'outil

>S'applique à : Windows Admin Center, version préliminaire de Windows Admin Center

Une extension de l’outil est le principal moyen des utilisateurs interagissent avec Windows Admin Center pour gérer une connexion, comme un serveur ou cluster. Lorsque vous cliquez sur une connexion dans l’écran d’accueil Windows Admin Center et vous connecter, se puis affiche une liste d’outils dans le volet de navigation gauche. Lorsque vous cliquez sur un outil, l’extension de l’outil est chargée et affichée dans le volet droit.

Quand une extension de l’outil est chargée, il peut exécuter des appels WMI ou des scripts PowerShell sur un serveur cible ou un cluster et afficher des informations dans l’interface utilisateur ou exécuter des commandes en fonction de l’entrée d’utilisateur. Extensions de l’outil définissent quelles solutions doit être affiché, ce qui entraîne un autre ensemble d’outils pour chaque solution.

> [!NOTE]
> Ne connaissent pas les types d’extension différents ? En savoir plus sur la [types d’architecture et l’extension d’extensibilité](understand-extensions.md).

## <a name="prepare-your-environment"></a>Préparer votre environnement

Si vous n’avez pas déjà fait, [préparer votre environnement](prepare-development-environment.md) en installant des dépendances et globales conditions préalables requises pour tous les projets.

## <a name="create-a-new-tool-extension-with-the-windows-admin-center-cli"></a>Créer une nouvelle extension de l’outil avec l’interface CLI de Windows Admin Center ##

Une fois que vous avez toutes les dépendances installés, vous êtes prêt à créer votre nouvelle extension de l’outil.  Créer ou rechercher un dossier qui contient vos fichiers projet, ouvrez une invite de commandes et de définir ce dossier comme répertoire de travail.  À l’aide de l’interface Windows Admin Center CLI qui était déjà installé, créez une nouvelle extension avec la syntaxe suivante :

``` cmd
wac create --company "{!Company Name}" --tool "{!Tool Name}"
```

| Value | Explication | Exemple |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | Nom de votre société (avec des espaces) | ```Contoso Inc``` |
| ```{!Tool Name}``` | Votre nom de l’outil (avec des espaces) | ```Manage Foo Works``` |

Voici un exemple d’utilisation :

``` cmd
wac create --company "Contoso Inc" --tool "Manage Foo Works"
```

Cette opération crée un nouveau dossier dans le répertoire de travail actuel dans le nom spécifié pour votre outil de copie tous les fichiers de modèle nécessaire dans votre projet et configure les fichiers avec le nom de votre entreprise et l’outil.  

Ensuite, changez de répertoire dans le dossier venez de créer, puis installer les dépendances requises locales en exécutant la commande suivante :

``` cmd
npm install
```

Une fois cette opération terminée, vous avez configuré tous les éléments que nécessaires pour charger votre nouvelle extension dans Windows Admin Center. 

## <a name="add-content-to-your-extension"></a>Ajouter du contenu à votre extension

Maintenant que vous avez créé une extension avec l’interface CLI de Windows Admin Center, vous êtes prêt à personnaliser le contenu.  Consultez ces guides pour obtenir des exemples de ce que vous pouvez faire :

- Ajouter un [module vide](guides/add-module.md)
- Ajouter un [iFrame](guides/add-iframe.md)
 
Vous pouvez trouver davantage d’exemples notre [site GitHub SDK](https://aka.ms/wacsdk):
-  [Outils de développement](https://github.com/Microsoft/windows-admin-center-sdk/tree/master/windows-admin-center-developer-tools) est une extension totalement fonctionnelle qui peut être chargées dans Windows Admin Center et contient une collection riche de fonctionnalités et l’outil exemples que vous pouvez parcourir et utiliser dans votre propre extension.

## <a name="customize-your-extensions-icon"></a>Personnaliser l’icône de votre extension

Vous pouvez personnaliser l’icône illustrant pour votre extension dans la liste d’outils.  Pour ce faire, modifiez toutes les ```icon``` entrées dans ```manifest.json``` pour votre extension :

``` json
"icon": "{!icon-uri}",
```

| Value | Explication | Uri de l’exemple |
| ----- | ----------- | ------- |
| ```{!icon-uri}``` | L’emplacement de votre ressource d’icône | ```assets/foo-icon.svg``` |

REMARQUE : Actuellement, les icônes personnalisées ne sont pas visibles lorsque côté chargeant votre extension en mode de développement.  Pour résoudre ce problème, supprimez le contenu de ```target``` comme suit :

``` json
"target": "",
```

Cette configuration est valide uniquement pour le chargement de côté en mode de développement, il est donc important de conserver la valeur contenue dans ```target``` puis restaurez-la avant de publier votre extension.

## <a name="build-and-side-load-your-extension"></a>Génération et côté chargent votre extension

Ensuite, build et côté chargent votre extension dans Windows Admin Center.  Ouvrez une fenêtre de commande, accédez à votre répertoire source, puis vous êtes prêt à créer.

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

## <a name="target-a-different-version-of-the-windows-admin-center-sdk"></a>Cibler une version différente du Kit de développement Windows Admin Center

Il est facile de garder votre extension à jour avec les modifications apportées au SDK et les modifications de la plateforme.  En savoir plus sur comment [cibler une version différente](target-sdk-version.md) du Kit de développement Windows Admin Center.