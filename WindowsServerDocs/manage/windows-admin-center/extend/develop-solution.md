---
title: Développer une extension de solution
description: Développer une extension de solution SDK Windows Admin Center (projet Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: ed5ecddbaef91f127846825e408a9a6ec65ff741
ms.sourcegitcommit: be0144eb59daf3269bebea93cb1c467d67e2d2f1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/20/2018
ms.locfileid: "4081066"
---
# Développer une extension de solution

>S’applique à: Windows Admin Center, Windows Admin Center Preview

Les solutions définissent principalement un type d’objet que vous souhaitez gérer via Windows Admin Center unique.  Ces types de solutions/connexion sont inclus avec Windows Admin Center par défaut:

* Connexions de Windows Server
* Connexions de PC Windows
* Connexions de cluster de basculement
* Connexions de cluster hyperconvergé

Lorsque vous sélectionnez une connexion à partir de la page de connexion de Windows Admin Center, l’extension de solution pour le type de la connexion est chargée et Windows Admin Center tente de se connecter au nœud cible. Si la connexion réussit, la solution de l’interface utilisateur de l’extension se charger et Windows Admin Center affichera les outils pour cette solution dans le volet de navigation de gauche.

Si vous souhaitez créer une interface utilisateur graphique de gestion pour les services ne pas défini par les types de connexion par défaut ci-dessus, tel un commutateur réseau ou tout autre matériel non détectable par nom d’ordinateur, vous souhaiterez créer votre propre extension de la solution.

> [!NOTE]
> Pas familiarisé avec les types d’extension différente? En savoir plus sur les [types d’architecture et l’extension extensibilité](understand-extensions.md).

## Préparer votre environnement

Si vous n’avez pas déjà fait, [préparer votre environnement](prepare-development-environment.md) en installant les dépendances et les conditions préalables globales requis pour tous les projets.

## Créer une nouvelle extension solution avec l’interface CLI de Windows Admin Center ##

Une fois que vous avez toutes les dépendances installés, vous êtes prêt à créer votre nouvelle extension solution.  Créer ou rechercher un dossier qui contient vos fichiers de projet, ouvrez une invite de commandes et de définir ce dossier comme répertoire de travail.  À l’aide de Windows Admin Center CLI qui a été installé précédemment, créez une nouvelle extension avec la syntaxe suivante:

```
wac create --company "{!Company Name}" --solution "{!Solution Name}" --tool "{!Tool Name}"
```

| Valeur | Explication | Exemple |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | Nom de votre société (avec des espaces) | ```Contoso Inc``` |
| ```{!Solution Name}``` | Nom de votre solution (avec des espaces) | ```Contoso Foo Works Suite``` |
| ```{!Tool Name}``` | Nom de votre outil (avec des espaces) | ```Manage Foo Works``` |

Voici un exemple d’utilisation:

```
wac create --company "Contoso Inc" --solution "Contoso Foo Works Suite" --tool "Manage Foo Works"
```

Cela crée un nouveau dossier à l’intérieur du répertoire de travail actuel en utilisant le nom spécifié pour votre solution, copie tous les fichiers de modèle nécessaires dans votre projet et configure les fichiers avec votre société, la solution et le nom de l’outil.  

Ensuite, modifiez le répertoire dans le dossier venez de créer et installer des dépendances locales nécessaires en exécutant la commande suivante:

```
npm install
```

Une fois cette opération terminée, vous avez configuré tout ce dont vous avez besoin pour charger votre nouvelle extension dans Windows Admin Center. 

## Ajouter du contenu à votre extension

Maintenant que vous avez créé une extension avec l’interface CLI de Windows Admin Center, vous êtes prêt à personnaliser le contenu.  Consultez ces guides pour obtenir des exemples de ce que vous pouvez faire:

- Ajouter un [module vide](guides\add-module.md)
- Ajouter un [iFrame](guides\add-iframe.md)
- Créer un [fournisseur de connexion personnalisée](guides\create-connection-provider.md)
- Modifiez le [comportement de navigation racine](guides\modify-root-navigation.md)
 
Exemples encore plus se trouvent notre [site GitHub SDK](https://aka.ms/wacsdk):
-  [Outils de développement](https://github.com/Microsoft/windows-admin-center-sdk/tree/master/windows-admin-center-developer-tools) est une extension entièrement fonctionnelle qui peut être chargée dans Windows Admin Center et contient une riche collection des fonctionnalités de l’exemple et les exemples d’outil que vous pouvez parcourir et utiliser dans votre propre extension.

## Génération et côté chargent votre extension

Ensuite, build et côté chargent votre extension dans Windows Admin Center.  Ouvrez une fenêtre de commande, modifiez le répertoire à votre répertoire source, puis vous êtes prêt à générer.

* Créez et proposez avec gulp:

    ```
    gulp build
    gulp serve -p 4201
    ```

Notez que vous devez choisir un port actuellement disponible. Veillez à ne pas essayer d’utiliser le port sur lequel Windows Admin Center est en cours d’exécution.

Votre projet peut être chargé de manière indépendante dans une instance locale de Windows Admin Center pour les tests en s’attachant au projet servi localement dans Windows Admin Center.

* Lancez Windows Admin Center dans un navigateur web
* Ouvrez le débogueur (F12)
* Ouvrez la Console, puis tapez la commande suivante:

    ```
    MsftSme.sideLoad("http://localhost:4201")
    ```

*   Actualisez le navigateur web

Votre projet sera désormais visible dans la liste d'outils avec (side loaded) en regard du nom.

## Cibler une autre version du SDK Windows Admin Center

Il est facile de garder votre extension à jour avec les modifications du Kit de développement logiciel et les modifications de la plateforme.  En savoir plus sur la [cible une autre version](target-sdk-version.md) du SDK Windows Admin Center.