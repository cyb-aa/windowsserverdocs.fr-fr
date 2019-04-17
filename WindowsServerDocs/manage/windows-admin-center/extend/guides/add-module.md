---
title: Ajouter un module à une extension d’outil
description: Développer une extension d’outil SDK Windows Admin Center (projet Honolulu) - ajouter un module à une extension d’outil
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: e6978ce20a7c6da8addb217de8d30f733b40d261
ms.sourcegitcommit: be0144eb59daf3269bebea93cb1c467d67e2d2f1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/20/2018
ms.locfileid: "4081206"
---
# Ajouter un module à une extension d’outil

>S’applique à: Windows Admin Center, Windows Admin Center Preview

Dans cet article, nous allons ajouter un module vide pour une extension d’outil que nous avons créé avec l’interface CLI de Windows Admin Center.

## Préparer votre environnement

Si vous n’avez pas déjà, suivez les instructions de développement une extension [d’outil](..\develop-tool.md) (ou [solution](..\develop-solution.md)) pour préparer votre environnement et de créer une extension d’outil vide.

## Utiliser la CLI angulaire pour créer un module (et le composant)

Si vous débutez dans Angular, il est vivement recommandé de lire la documentation sur le site Web Angular.Io pour en savoir plus sur Angular et NgModule. Pour plus d’informations sur NgModule, cliquez ici: https://angular.io/guide/ngmodule

* En savoir plus sur la création d’un module dans Angular CLI: https://github.com/angular/angular-cli/wiki/generate-module
* En savoir plus sur la création d’un composant dans Angular CLI: https://github.com/angular/angular-cli/wiki/generate-component


Ouvrez une invite de commandes, remplacez le répertoire par \src\app dans votre projet, puis exécutez les commandes suivantes, en remplaçant ```{!ModuleName}``` avec le nom de votre module (espaces supprimés):

```
cd \src\app
ng generate module {!ModuleName}
ng generate component {!ModuleName}
```

| Valeur | Explication | Exemple |
| ----- | ----------- | ------- |
| ```{!ModuleName}``` | Nom de votre module (espaces supprimés) | ```ManageFooWorksPortal``` |

Exemple d’utilisation:
```
cd \src\app
ng generate module ManageFooWorksPortal
ng generate component ManageFooWorksPortal
```


## Ajouter des informations de routage

Si vous débutez dans Angular, il est vivement recommandé d'étudier le routage et la navigation d'Angular. Les sections suivantes définissent les éléments de routage nécessaires qui permettent à Windows Admin Center d'accéder à votre extension et d'en parcourir les affichages en réponse à l’activité de l’utilisateur. Pour plus d’informations, cliquez ici: https://angular.io/guide/router

Utilisez le même nom de module que vous avez utilisé à l’étape précédente.

### Ajouter du contenu à un nouveau fichier de routage

* Accédez au dossier du module créé par  ``` ng generate ``` à l’étape précédente.

* Créer un fichier ```{!module-name}.routing.ts```, en respectant cette convention d’affectation de noms:

    | Valeur | Explication | Exemple de nom de fichier |
    | ----- | ----------- | ------- |
    | ```{!module-name}``` | Nom de votre module (minuscules, espaces remplacés par des tirets) | ```manage-foo-works-portal.routing.ts``` |

* Ajoutez ce contenu au fichier que vous venez de créer:

    ``` ts
    import { NgModule } from '@angular/core';
    import { RouterModule, Routes } from '@angular/router';
    import { {!ModuleName}Component } from './{!module-name}.component';

    const routes: Routes = [
        {
            path: '',
            component: {!ModuleName}Component,
            // if the component has child components that need to be routed to, include them in the children array.
            children: [
                {
                    path: '', 
                    redirectTo: 'base',
                    pathMatch: 'full'
                }
            ]
    }];

    @NgModule({
        imports: [
            RouterModule.forChild(routes)
        ],
        exports: [
            RouterModule
        ]
    })
    export class Routing { }
    ```

* Remplacez les valeurs dans le fichier que vous venez de créer par les valeurs de votre choix:

    | Valeur | Explication | Exemple |
    | ----- | ----------- | ------- |
    | ```{!ModuleName}``` | Nom de votre module (espaces supprimés) | ```ManageFooWorksPortal``` |
    | ```{!module-name}``` | Nom de votre module (minuscules, espaces remplacés par des tirets) | ```manage-foo-works-portal``` |

### Ajouter du contenu à un nouveau fichier de module

Ouvrez le fichier ```{!module-name}.module.ts```, qui respecte cette convention d’affectation de noms:

| Valeur | Explication | Exemple de nom de fichier |
| ----- | ----------- | ------- |
| ```{!module-name}``` | Nom de votre module (minuscules, espaces remplacés par des tirets) | ```manage-foo-works-portal.module.ts``` |

* Ajoutez du contenu au fichier:

    ``` ts
    import { Routing } from './{!module-name}.routing';
    ```

* Remplacez les valeurs dans le contenu que vous venez d'ajouter par les valeurs de votre choix:

    | Valeur | Explication | Exemple |
    | ----- | ----------- | ------- |
    | ```{!module-name}``` | Nom de votre module (minuscules, espaces remplacés par des tirets) | ```manage-foo-works-portal``` |

* Modifiez l’instruction imports par import Routing:

    | Valeur d’origine | Nouvelle valeur |
    | -------------- | --------- |
    | ```imports: [ CommonModule ]``` | ```imports: [ CommonModule, Routing ]``` |

* Assurez-vous que les instructions ```import``` sont classées par source.

### Ajouter du contenu à un nouveau fichier de typescript de composant

Ouvrez le fichier ```{!module-name}.component.ts```, qui respecte cette convention d’affectation de noms:

| Valeur | Explication | Exemple de nom de fichier |
| ----- | ----------- | ------- |
| ```{!module-name}``` | Nom de votre module (minuscules, espaces remplacés par des tirets) | ```manage-foo-works-portal.component.ts``` |
    
Modifier le contenu du fichier comme suit:

``` ts
constructor() {
    // TODO
}

public ngOnInit() {
    // TODO
}
```
### Mettre à jour d’application-routing.module.ts

Ouvrez le fichier ```app-routing.module.ts```et de modifier le chemin d’accès par défaut, donc il charge le nouveau module que vous venez de créer.  Recherchez l’entrée pour ```path: ''```et mettre à jour ```loadChildren``` pour charger votre module au lieu du module par défaut:

| Valeur | Explication | Exemple |
| ----- | ----------- | ------- |
| ```{!ModuleName}``` | Nom de votre module (espaces supprimés) | ```ManageFooWorksPortal``` |
| ```{!module-name}``` | Nom de votre module (minuscules, espaces remplacés par des tirets) | ```manage-foo-works-portal``` |

``` ts
    {
        path: '', 
        loadChildren: 'app/{!module-name}/{!module-name}.module#{!ModuleName}Module'
    },
```
Voici un exemple d’un chemin de mise à jour par défaut:
``` ts
    {
        path: '', 
        loadChildren: 'app/manage-foo-works-portal/manage-foo-works-portal.module#ManageFooWorksPortalModule'
    },
```


## Génération et côté chargent votre extension

Vous avez maintenant ajouté un module à votre extension.  Ensuite, vous pouvez [build et côté charge](..\develop-tool.md#build-and-side-load-your-extension) votre extension dans Windows Admin Center pour afficher les résultats.
