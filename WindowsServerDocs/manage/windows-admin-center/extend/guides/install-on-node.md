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
ms.openlocfilehash: 3a93a1105862ffbf4fcbd1d23b15d9bcaa6010dc
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75950499"
---
# <a name="install-extension-payload-on-a-managed-node"></a>Installer la charge de l’extension sur un nœud géré

>S’applique à : Windows Admin Center, Windows Admin Center Preview

## <a name="setup"></a>Installation
> [!NOTE]
> Pour suivre ce guide, vous avez besoin de Build 1.2.1904.02001 ou version ultérieure. Pour vérifier votre numéro de build, ouvrez le centre d’administration Windows, puis cliquez sur le point d’interrogation dans le coin supérieur droit.

Si vous ne l’avez pas encore fait, créez une [extension d’outil](../develop-tool.md) pour le centre d’administration Windows. Une fois que vous avez terminé, notez les valeurs utilisées lors de la création d’une extension :

| Value | Explication | Exemple |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | Nom de votre société (avec des espaces) | ```Contoso``` |
| ```{!Tool Name}``` | Votre nom d’outil (avec des espaces) | ```InstallOnNode``` |

Dans le dossier de votre extension Tool, créez un dossier de ```Node``` (```{!Tool Name}\Node```). Tout ce qui est placé dans ce dossier est copié vers le nœud géré lors de l’utilisation de cette API. Ajoutez les fichiers nécessaires à votre cas d’usage. 

Créez également un script de ```{!Tool Name}\Node\installNode.ps1```. Ce script est exécuté sur le nœud géré une fois que tous les fichiers sont copiés du dossier ```{!Tool Name}\Node``` vers le nœud géré. Ajoutez toute logique supplémentaire pour votre cas d’utilisation. Exemple de fichier ```{!Tool Name}\Node\installNode.ps1``` :

``` ps1
# Add logic for installing payload on managed node
echo 'Success'
```

> [!NOTE]
> ```{!Tool Name}\Node\installNode.ps1``` a un nom spécifique que l’API recherchera. La modification du nom de ce fichier génère une erreur.


## <a name="integration-with-ui"></a>Intégration avec l’interface utilisateur

Mettez à jour ```\src\app\default.component.ts``` comme suit :

``` ts
import { Component } from '@angular/core';
import { AppContextService } from '@microsoft/windows-admin-center-sdk/angular';
import { Observable } from 'rxjs';

@Component({
  selector: 'default-component',
  templateUrl: './default.component.html',
  styleUrls: ['./default.component.css']
})

export class DefaultComponent {
  constructor(private appContextService: AppContextService) { }

  public response: any;
  public loading = false;

  public installOnNode() {
    this.loading = true;
    this.post('{!Company Name}.{!Tool Name}', '1.0.0',
      this.appContextService.activeConnection.nodeName).subscribe(
        (response: any) => {
          console.log(response);
          this.response = response;
          this.loading = false;
        },
        (error) => {
          console.log(error);
          this.response = error;
          this.loading = false;
        }
      );
  }

  public post(id: string, version: string, targetNode: string): Observable<any> {
    return this.appContextService.node.post(targetNode,
      `features/extensions/${id}/versions/${version}/install`);
  }

}
```
Mettez à jour les espaces réservés avec les valeurs utilisées lors de la création de l’extension :
``` ts
this.post('contoso.install-on-node', '1.0.0',
      this.appContextService.activeConnection.nodeName).subscribe(
        (response: any) => {
          console.log(response);
          this.response = response;
          this.loading = false;
        },
        (error) => {
          console.log(error);
          this.response = error;
          this.loading = false;
        }
      );
```

Mettez également à jour ```\src\app\default.component.html``` vers :
``` html
<button (click)="installOnNode()">Click to install</button>
<sme-loading-wheel *ngIf="loading" size="large"></sme-loading-wheel>
<p *ngIf="response">{{response}}</p>
```
Et enfin ```\src\app\default.module.ts```:
``` ts
import { CommonModule } from '@angular/common';
import { NgModule } from '@angular/core';

import { LoadingWheelModule } from '@microsoft/windows-admin-center-sdk/angular';
import { DefaultComponent } from './default.component';
import { Routing } from './default.routing';

@NgModule({
  imports: [
    CommonModule,
    LoadingWheelModule,
    Routing
  ],
  declarations: [DefaultComponent]
})
export class DefaultModule { }

```

## <a name="creating-and-installing-a-nuget-package"></a>Création et installation d’un package NuGet

La dernière étape consiste à créer un package NuGet avec les fichiers que nous avons ajoutés, puis à installer ce package dans le centre d’administration Windows.

Si vous n’avez pas créé de package d’extension avant, suivez le guide des [extensions de publication](../publish-extensions.md) . 
> [!IMPORTANT]
> Dans votre fichier. NuSpec pour cette extension, il est important que la valeur ```<id>``` corresponde au nom du ```manifest.json``` de votre projet et que le ```<version>``` corresponde à ce qui a été ajouté à ```\src\app\default.component.ts```. Ajoutez également une entrée sous ```<files>```: 
> 
> ```<file src="Node\**\*.*" target="Node" />```.

``` xml
<?xml version="1.0" encoding="utf-8"?>
<package xmlns="https://schemas.microsoft.com/packaging/2011/08/nuspec.xsd">
  <metadata>
    <id>contoso.install-on-node</id>
    <version>1.0.0</version>
    <authors>Contoso</authors>
    <owners>Contoso</owners>
    <requireLicenseAcceptance>false</requireLicenseAcceptance>
    <projectUrl>https://msft-sme.myget.org/feed/windows-admin-center-feed/package/nuget/contoso.sme.install-on-node-extension</projectUrl>
    <licenseUrl>http://YourLicenseLink</licenseUrl>
    <iconUrl>http://YourLogoLink</iconUrl>
    <description>Install on node extension by Contoso</description>
    <copyright>(c) Contoso. All rights reserved.</copyright> 
  </metadata>
    <files>
    <file src="bundle\**\*.*" target="ux" />
    <file src="package\**\*.*" target="gateway" />
    <file src="Node\**\*.*" target="Node" />
  </files>
</package>
```

Une fois ce package créé, ajoutez un chemin d’accès à ce flux. Dans le centre d’administration Windows, accédez à paramètres > Extensions > flux, puis ajoutez le chemin d’accès à l’emplacement où se trouve ce package. Une fois l’installation de votre extension terminée, vous devez être en mesure de cliquer sur le bouton ```install``` et l’API est appelée.  