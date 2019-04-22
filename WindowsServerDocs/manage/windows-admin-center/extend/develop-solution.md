---
title: Développer une extension de solution
description: Développer une extension de la solution Windows Admin Center SDK (projet Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: ed5ecddbaef91f127846825e408a9a6ec65ff741
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825470"
---
# <a name="develop-a-solution-extension"></a>Développer une extension de solution

>S'applique à : Windows Admin Center, version préliminaire de Windows Admin Center

Solutions définissent principalement un type unique de l’objet que vous souhaitez gérer par le biais de Windows Admin Center.  Ces types de solutions et de connexion sont inclus avec Windows Admin Center par défaut :

* Connexions de Windows Server
* Connexions de PC de Windows
* Connexions de cluster de basculement
* Connexions au cluster hyperconvergé

Lorsque vous sélectionnez une connexion à partir de la page de connexion Windows Admin Center, l’extension de la solution pour ce type de connexion est chargée, et Windows Admin Center tentera de se connecter au nœud cible. Si la connexion est établie, la solution de l’interface utilisateur de l’extension chargera et Windows Admin Center affichera les outils pour cette solution dans le volet de navigation gauche.

Si vous souhaitez créer une interface graphique utilisateur de gestion pour les services non définis par les types de connexion par défaut ci-dessus, tel un commutateur réseau ou autre matériel non détectable par nom de l’ordinateur, vous souhaiterez créer votre propre extension de la solution.

> [!NOTE]
> Ne connaissent pas les types d’extension différents ? En savoir plus sur la [types d’architecture et l’extension d’extensibilité](understand-extensions.md).

## <a name="prepare-your-environment"></a>Préparer votre environnement

Si vous n’avez pas déjà fait, [préparer votre environnement](prepare-development-environment.md) en installant des dépendances et globales conditions préalables requises pour tous les projets.

## <a name="create-a-new-solution-extension-with-the-windows-admin-center-cli"></a>Créer une nouvelle extension de solution avec l’interface CLI de Windows Admin Center ##

Une fois que vous avez toutes les dépendances installés, vous êtes prêt à créer votre nouvelle extension de la solution.  Créer ou rechercher un dossier qui contient vos fichiers projet, ouvrez une invite de commandes et de définir ce dossier comme répertoire de travail.  À l’aide de l’interface Windows Admin Center CLI qui était déjà installé, créez une nouvelle extension avec la syntaxe suivante :

```
wac create --company "{!Company Name}" --solution "{!Solution Name}" --tool "{!Tool Name}"
```

| Value | Explication | Exemple |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | Nom de votre société (avec des espaces) | ```Contoso Inc``` |
| ```{!Solution Name}``` | Nom de votre solution (avec des espaces) | ```Contoso Foo Works Suite``` |
| ```{!Tool Name}``` | Votre nom de l’outil (avec des espaces) | ```Manage Foo Works``` |

Voici un exemple d’utilisation :

```
wac create --company "Contoso Inc" --solution "Contoso Foo Works Suite" --tool "Manage Foo Works"
```

Cette opération crée un nouveau dossier à l’intérieur du répertoire de travail actuel en utilisant le nom que vous spécifié pour votre solution, copie tous les fichiers de modèle nécessaire dans votre projet et configure les fichiers avec votre entreprise, la solution et le nom de l’outil.  

Ensuite, changez de répertoire dans le dossier venez de créer, puis installer les dépendances requises locales en exécutant la commande suivante :

```
npm install
```

Une fois cette opération terminée, vous avez configuré tous les éléments que nécessaires pour charger votre nouvelle extension dans Windows Admin Center. 

## <a name="add-content-to-your-extension"></a>Ajouter du contenu à votre extension

Maintenant que vous avez créé une extension avec l’interface CLI de Windows Admin Center, vous êtes prêt à personnaliser le contenu.  Consultez ces guides pour obtenir des exemples de ce que vous pouvez faire :

- Ajouter un [module vide](guides\add-module.md)
- Ajouter un [iFrame](guides\add-iframe.md)
- Créer un [fournisseur de connexion personnalisée](guides\create-connection-provider.md)
- Modifier [racine le comportement de navigation](guides\modify-root-navigation.md)
 
Vous pouvez trouver davantage d’exemples notre [site GitHub SDK](https://aka.ms/wacsdk):
-  [Outils de développement](https://github.com/Microsoft/windows-admin-center-sdk/tree/master/windows-admin-center-developer-tools) est une extension totalement fonctionnelle qui peut être chargées dans Windows Admin Center et contient une collection riche de fonctionnalités et l’outil exemples que vous pouvez parcourir et utiliser dans votre propre extension.

## <a name="build-and-side-load-your-extension"></a>Génération et côté chargent votre extension

Ensuite, build et côté chargent votre extension dans Windows Admin Center.  Ouvrez une fenêtre de commande, accédez à votre répertoire source, puis vous êtes prêt à créer.

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

## <a name="target-a-different-version-of-the-windows-admin-center-sdk"></a>Cibler une version différente du Kit de développement Windows Admin Center

Il est facile de garder votre extension à jour avec les modifications apportées au SDK et les modifications de la plateforme.  En savoir plus sur comment [cibler une version différente](target-sdk-version.md) du Kit de développement Windows Admin Center.