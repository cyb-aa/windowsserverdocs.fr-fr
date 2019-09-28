---
title: Développer une extension de solution
description: Développer une extension de solution Kit de développement logiciel (SDK) du centre d’administration Windows (projet Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 6ac9c6296fdf9159c9f50a1304dd345932052ac9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357148"
---
# <a name="develop-a-solution-extension"></a>Développer une extension de solution

>S'applique à : Windows Admin Center, Windows Admin Center Preview

Les solutions définissent principalement un type unique d’objet que vous souhaitez gérer par le biais du centre d’administration Windows.  Ces solutions/types de connexion sont inclus par défaut dans le centre d’administration Windows :

* Connexions Windows Server
* Connexions de PC Windows
* Connexions de cluster de basculement
* Connexions de cluster hyper-convergé

Lorsque vous sélectionnez une connexion dans la page de connexion du centre d’administration Windows, l’extension de solution pour le type de cette connexion est chargée et le centre d’administration Windows tente de se connecter au nœud cible. Si la connexion réussit, l’interface utilisateur de l’extension de solution est chargée et le centre d’administration Windows affiche les outils pour cette solution dans le volet de navigation gauche.

Si vous souhaitez créer une interface utilisateur graphique de gestion pour les services qui ne sont pas définis par les types de connexion par défaut ci-dessus, un commutateur réseau ou tout autre matériel non détectable par le nom de l’ordinateur, vous pouvez créer votre propre extension de solution.

> [!NOTE]
> Vous n’êtes pas familiarisé avec les différents types d’extensions ? En savoir plus sur l' [architecture et les types d’extension d’extensibilité](understand-extensions.md).

## <a name="prepare-your-environment"></a>Préparer votre environnement

Si vous ne l’avez pas déjà fait, [Préparez votre environnement](prepare-development-environment.md) en installant des dépendances et des prérequis globaux requis pour tous les projets.

## <a name="create-a-new-solution-extension-with-the-windows-admin-center-cli"></a>Créer une extension de solution à l’aide de l’interface de commande du centre d’administration Windows ##

Une fois que toutes les dépendances sont installées, vous êtes prêt à créer votre extension de solution.  Créez ou accédez à un dossier qui contient vos fichiers de projet, ouvrez une invite de commandes et définissez ce dossier comme répertoire de travail.  À l’aide de l’interface de commande du centre d’administration Windows qui a été précédemment installée, créez une nouvelle extension avec la syntaxe suivante :

```
wac create --company "{!Company Name}" --solution "{!Solution Name}" --tool "{!Tool Name}"
```

| Value | Explication | Exemple |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | Nom de votre société (avec des espaces) | ```Contoso Inc``` |
| ```{!Solution Name}``` | Le nom de votre solution (avec des espaces) | ```Contoso Foo Works Suite``` |
| ```{!Tool Name}``` | Votre nom d’outil (avec des espaces) | ```Manage Foo Works``` |

Voici un exemple d’utilisation :

```
wac create --company "Contoso Inc" --solution "Contoso Foo Works Suite" --tool "Manage Foo Works"
```

Cela crée un nouveau dossier dans le répertoire de travail actuel en utilisant le nom que vous avez spécifié pour votre solution, copie tous les fichiers de modèle nécessaires dans votre projet et configure les fichiers avec le nom de votre société, de votre solution et de votre outil.  

Ensuite, accédez au dossier que vous venez de créer, puis installez les dépendances locales requises en exécutant la commande suivante :

```
npm install
```

Une fois cette opération terminée, vous avez configuré tout ce dont vous avez besoin pour charger votre nouvelle extension dans le centre d’administration Windows. 

## <a name="add-content-to-your-extension"></a>Ajouter du contenu à votre extension

Maintenant que vous avez créé une extension avec l’interface de commande du centre d’administration Windows, vous êtes prêt à personnaliser le contenu.  Pour obtenir des exemples de ce que vous pouvez faire, consultez les guides suivants :

- Ajouter un [module vide](guides/add-module.md)
- Ajouter un [IFRAME](guides/add-iframe.md)
- Créer un [fournisseur de connexion personnalisé](guides/create-connection-provider.md)
- Modifier le [comportement de navigation racine](guides/modify-root-navigation.md)
 
Vous trouverez d’autres exemples sur notre [site SDK GitHub](https://aka.ms/wacsdk):
-  [Outils de développement](https://github.com/Microsoft/windows-admin-center-sdk/tree/master/windows-admin-center-developer-tools) est une extension entièrement fonctionnelle qui peut être chargée dans le centre d’administration Windows et contient une collection complète d’exemples de fonctionnalités et d’outils que vous pouvez parcourir et utiliser dans votre propre extension.

## <a name="build-and-side-load-your-extension"></a>Créez et chargez votre extension

Ensuite, créez et chargez votre extension dans le centre d’administration Windows.  Ouvrez une fenêtre de commande, accédez au répertoire source, puis vous êtes prêt à générer.

* Créez et proposez avec gulp :

    ```
    gulp build
    gulp serve -p 4201
    ```

Notez que vous devez choisir un port actuellement disponible. Veillez à ne pas essayer d’utiliser le port sur lequel Windows Admin Center est en cours d’exécution.

Votre projet peut être chargé de manière indépendante dans une instance locale de Windows Admin Center pour les tests en s’attachant au projet servi localement dans Windows Admin Center.

* Lancez Windows Admin Center dans un navigateur web
* Ouvrez le débogueur (F12)
* Ouvrez la Console, puis tapez la commande suivante :

    ```
    MsftSme.sideLoad("http://localhost:4201")
    ```

*   Actualisez le navigateur web

Votre projet sera désormais visible dans la liste d'outils avec (side loaded) en regard du nom.

## <a name="target-a-different-version-of-the-windows-admin-center-sdk"></a>Cibler une version différente du kit de développement logiciel (SDK) du centre d’administration Windows

Il est facile de tenir à jour votre extension avec les modifications du SDK et les modifications de la plateforme.  Découvrez comment [cibler une version différente](target-sdk-version.md) du kit de développement logiciel (SDK) du centre d’administration Windows.