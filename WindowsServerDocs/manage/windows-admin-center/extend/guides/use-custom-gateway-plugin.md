---
title: Utiliser un plug-in de passerelle personnalisé dans votre extension d'outil
description: Développer une extension d’outil SDK Windows Admin Center (projet Honolulu) - utilisez un plug-in de passerelle personnalisées dans votre extension d’outil
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 4652616478b7b05bde97db48bf84648984b5a325
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296761"
---
# Utiliser un plug-in de passerelle personnalisé dans votre extension d'outil

>S’applique à: Windows Admin Center, Windows Admin Center Preview

Dans cet article, nous allons utiliser un plug-in de passerelle personnalisées dans une extension d’outil vide que nous avons créé avec l’interface CLI de Windows Admin Center.

## Préparer votre environnement ##

Si vous n’avez pas déjà, suivez les instructions de [développer une extension d’outil](..\develop-tool.md) pour préparer votre environnement et créer une extension d’outil vide.

## Ajouter un module à votre projet ##

Si vous n’avez pas déjà, ajoutez un nouveau [module vide](add-module.md) à votre projet, nous allons utiliser à l’étape suivante.  

## Ajouter l’intégration de plug-in de passerelle personnalisées ##

Maintenant, nous allons utiliser un plug-in de passerelle personnalisées dans le module vide que nous venons de créer.

### Créer des plugin.service.ts

Accédez au répertoire du nouveau module outil créé ci-dessus (```\src\app\{!Module-Name}```) et créer un nouveau fichier ```plugin.service.ts```.

Ajoutez le code suivant au fichier venez de créer:
``` ts
import { Injectable } from '@angular/core';
import { AppContextService, HttpService } from '@microsoft/windows-admin-center-sdk/angular';
import { Cim, Http, PowerShell, PowerShellSession } from '@microsoft/windows-admin-center-sdk/core';
import { AjaxResponse, Observable } from 'rxjs';

@Injectable()
export class PluginService {
    constructor(private appContextService: AppContextService, private http: Http) {
    }
    
    public getGatewayRestResponse(): Observable<any> {
        let callUrl = this.appContextService.activeConnection.nodeName;

        return this.appContextService.node.get(callUrl, 'features/Sample%20Uno').map(
            (response: any) => {
                return response;
            }
        )
    }
}
```

Remplacez les références à ```Sample Uno``` et ```Sample%20Uno``` au nom de votre fonction selon le cas.

[!WARNING]
> Il est recommandé qu’intégré dans ```this.appContextService.node``` est utilisée pour appeler toute API qui est défini dans votre plug-in de passerelle personnalisée. Cela permet de garantir que si des informations d’identification sont nécessaires à l’intérieur de votre plug-in de passerelle, il pourra être correctement traités.

### Modifier module.ts

Ouvrez le ```module.ts``` fichier du nouveau module créé précédemment (autrement dit, ```{!Module-Name}.module.ts```):

Ajoutez les instructions d’importation suivantes:

``` ts
import { HttpService } from '@microsoft/windows-admin-center-sdk/angular';
import { Http } from '@microsoft/windows-admin-center-sdk/core';
import { PluginService } from './plugin.service';
```

Ajoutez les fournisseurs suivants (après les déclarations):

``` ts
  ,
  providers: [
    HttpService,
    PluginService,
    Http
  ]
```

### Modifier component.ts

Ouvrez le ```component.ts``` fichier du nouveau module créé précédemment (autrement dit, ```{!Module-Name}.component.ts```):

Ajoutez les instructions d’importation suivantes:

``` ts
import { ActivatedRouteSnapshot } from '@angular/router';
import { AppContextService } from '@microsoft/windows-admin-center-sdk/angular';
import { Subscription } from 'rxjs';
import { Strings } from '../../generated/strings';
import { PluginService } from './plugin.service';
```

Ajoutez les variables suivantes:

``` ts
  private serviceSubscription: Subscription;
  private responseResult: string;
```

Modifiez le constructeur et modifier/ajouter des fonctions suivantes:

``` ts
  constructor(private appContextService: AppContextService, private plugin: PluginService) {
    //
  }

  public ngOnInit() {
    this.responseResult = 'click go to do something';
  }

  public onClick() {
    this.serviceSubscription = this.plugin.getGatewayRestResponse().subscribe(
      (response: any) => {
        this.responseResult = 'response: ' + response.message;
      },
      (error) => {
        console.log(error);
      }
    );
  }
```

### Modifier component.html ###

Ouvrez le ```component.html``` fichier du nouveau module créé précédemment (autrement dit, ```{!Module-Name}.component.html```):

Dans le fichier html, ajoutez le contenu suivant:
``` html
<button (click)="onClick()" >go</button>
{{ responseResult }}
```

## Génération et côté chargent votre extension

Désormais vous êtes prêt pour le [chargement de version et côté](..\develop-tool.md#build-and-side-load-your-extension) votre extension dans Windows Admin Center.
