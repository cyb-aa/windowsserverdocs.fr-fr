---
title: Utiliser un plug-in de passerelle personnalisé dans votre extension d'outil
description: Développement d’une extension d’outil SDK du centre d’administration Windows (Project Honolulu)-utilisation d’un plug-in de passerelle personnalisé dans votre extension d’outil
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 829cbf6df8cc2738bf4066b36210b860595774ed
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385228"
---
# <a name="use-a-custom-gateway-plugin-in-your-tool-extension"></a>Utiliser un plug-in de passerelle personnalisé dans votre extension d'outil

>S’applique à : Windows Admin Center, Windows Admin Center Preview

Dans cet article, nous allons utiliser un plug-in de passerelle personnalisé dans une nouvelle extension d’outil vide que nous avons créée à l’aide de l’interface de commande du centre d’administration Windows.

## <a name="prepare-your-environment"></a>Préparer votre environnement ##

Si vous ne l’avez pas déjà fait, suivez les instructions fournies dans [développer une extension d’outil](../develop-tool.md) pour préparer votre environnement et créer une extension d’outil vide.

## <a name="add-a-module-to-your-project"></a>Ajouter un module à votre projet ##

Si vous ne l’avez pas encore fait, ajoutez un nouveau [module vide](add-module.md) à votre projet, que nous utiliserons à l’étape suivante.  

## <a name="add-integration-to-custom-gateway-plugin"></a>Ajouter l’intégration au plug-in de passerelle personnalisé ##

À présent, nous allons utiliser un plug-in de passerelle personnalisé dans le nouveau module vide que nous venons de créer.

### <a name="create-pluginservicets"></a>Créer un plug-in. service. TS

Accédez au répertoire du nouveau module d’outil créé ci-dessus (```\src\app\{!Module-Name}```), puis créez un nouveau fichier ```plugin.service.ts```.

Ajoutez le code suivant au fichier que vous venez de créer :
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

Modifiez les références à ```Sample Uno``` et ```Sample%20Uno``` en fonction de votre nom de fonctionnalité.

[!WARNING]
> Il est recommandé d’utiliser le ```this.appContextService.node``` intégré pour appeler toute API définie dans le plug-in de votre passerelle personnalisée. Ainsi, si des informations d’identification sont requises dans le plug-in de votre passerelle, elles seront gérées correctement.

### <a name="modify-modulets"></a>Modifier le module. TS

Ouvrez le fichier ```module.ts``` du nouveau module créé précédemment (c’est-à-dire ```{!Module-Name}.module.ts```) :

Ajoutez les instructions d’importation suivantes :

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

### <a name="modify-componentts"></a>Modifier le composant. TS

Ouvrez le fichier ```component.ts``` du nouveau module créé précédemment (c’est-à-dire ```{!Module-Name}.component.ts```) :

Ajoutez les instructions d’importation suivantes :

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

Modifiez le constructeur et modifiez/ajoutez les fonctions suivantes :

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

### <a name="modify-componenthtml"></a>Modifier Component. html ###

Ouvrez le fichier ```component.html``` du nouveau module créé précédemment (c’est-à-dire ```{!Module-Name}.component.html```) :

Ajoutez le contenu suivant au fichier HTML :
``` html
<button (click)="onClick()" >go</button>
{{ responseResult }}
```

## <a name="build-and-side-load-your-extension"></a>Créez et chargez votre extension

Vous êtes maintenant prêt à [créer et à charger](../develop-tool.md#build-and-side-load-your-extension) votre extension dans le centre d’administration Windows.
