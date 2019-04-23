---
title: Utiliser un plug-in de passerelle personnalisé dans votre extension d'outil
description: 'Développer une extension de l’outil Kit de développement Windows Admin Center (projet Honolulu) : utilisez un plug-in passerelle personnalisée dans votre extension de l’outil'
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: c9b2e9201d58472286b42a9c89a36423f40d143d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834510"
---
# <a name="use-a-custom-gateway-plugin-in-your-tool-extension"></a>Utiliser un plug-in de passerelle personnalisé dans votre extension d'outil

>S'applique à : Windows Admin Center, version préliminaire de Windows Admin Center

Dans cet article, nous allons utiliser un plug-in de passerelle personnalisée dans une extension de l’outil vide que nous avons créé avec l’interface CLI de Windows Admin Center.

## <a name="prepare-your-environment"></a>Préparer votre environnement ##

Si vous n’avez pas déjà, suivez les instructions de [développer une extension de l’outil](..\develop-tool.md) pour préparer votre environnement et créer un nouveau, vide l’extension de l’outil.

## <a name="add-a-module-to-your-project"></a>Ajouter un module à votre projet ##

Si vous n’avez pas déjà, ajoutez une nouvelle [module vide](add-module.md) à votre projet, nous allons utiliser dans l’étape suivante.  

## <a name="add-integration-to-custom-gateway-plugin"></a>Ajouter l’intégration au plug-in de la passerelle personnalisée ##

Maintenant, nous allons utiliser un plug-in passerelle personnalisée dans le module vide que nous venons de créer.

### <a name="create-pluginservicets"></a>Créer plugin.service.ts

Accédez au répertoire du nouveau module outil créé ci-dessus (```\src\app\{!Module-Name}```) et créez un nouveau fichier ```plugin.service.ts```.

Ajoutez le code suivant au fichier venez de créer :
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

Modifier les références à ```Sample Uno``` et ```Sample%20Uno``` à votre nom de la fonctionnalité comme il convient.

### <a name="modify-modulets"></a>Modifier le module.TS

Ouvrez le ```module.ts``` fichier du nouveau module créé précédemment (par exemple, ```{!Module-Name}.module.ts```) :

Ajoutez les instructions import suivantes :

``` ts
import { HttpService } from '@microsoft/windows-admin-center-sdk/angular';
import { Http } from '@microsoft/windows-admin-center-sdk/core';
import { PluginService } from './plugin.service';
```

Ajoutez les fournisseurs suivants (après les déclarations) :

``` ts
  ,
  providers: [
    HttpService,
    PluginService,
    Http
  ]
```

### <a name="modify-componentts"></a>Modifier component.ts

Ouvrez le ```component.ts``` fichier du nouveau module créé précédemment (par exemple, ```{!Module-Name}.component.ts```) :

Ajoutez les instructions import suivantes :

``` ts
import { ActivatedRouteSnapshot } from '@angular/router';
import { AppContextService } from '@microsoft/windows-admin-center-sdk/angular';
import { Subscription } from 'rxjs';
import { Strings } from '../../generated/strings';
import { PluginService } from './plugin.service';
```

Ajoutez les variables suivantes :

``` ts
  private serviceSubscription: Subscription;
  private responseResult: string;
```

Modifiez le constructeur et modifiez/ajoutez des fonctions suivantes :

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

### <a name="modify-componenthtml"></a>Modifier component.html ###

Ouvrez le ```component.html``` fichier du nouveau module créé précédemment (par exemple, ```{!Module-Name}.component.html```) :

Ajoutez le contenu suivant au fichier html :
``` html
<button (click)="onClick()" >go</button>
{{ responseResult }}
```

## <a name="build-and-side-load-your-extension"></a>Génération et côté chargent votre extension

Vous êtes maintenant prêt à [générer et côté charge](..\develop-tool.md#build-and-side-load-your-extension) votre extension dans Windows Admin Center.
