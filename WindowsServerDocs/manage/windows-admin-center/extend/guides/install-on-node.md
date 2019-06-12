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
ms.openlocfilehash: 1a068c0d33887e8e9287ff15c1aa14f3dc84915a
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445933"
---
# <a name="install-extension-payload-on-a-managed-node"></a>Installer la charge utile d’extension sur un nœud géré

>S'applique à : Windows Admin Center, version préliminaire de Windows Admin Center

## <a name="setup"></a>Installation
> [!NOTE]
> Pour suivre ce guide, vous devez générerez 1.2.1904.02001 ou une version ultérieure. Pour vérifier votre build nombre Windows Admin Center, cliquez sur le point d’interrogation dans le coin supérieur droit.

Si vous n’avez pas déjà, créez un [outil extension](../develop-tool.md) pour Windows Admin Center. Une fois que vous avez terminé cette marque note des valeurs utilisées lors de la création d’une extension :

| Value | Explication | Exemple |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | Nom de votre société (avec des espaces) | ```Contoso``` |
| ```{!Tool Name}``` | Votre nom de l’outil (avec des espaces) | ```InstallOnNode``` |

À l’intérieur de votre dossier d’extension outil créer un ```Node``` dossier (```{!Tool Name}\Node```). Quoi que ce soit placé dans ce dossier est copié au nœud géré lors de l’utilisation de cette API. Ajouter tous les fichiers nécessaires à votre cas d’utilisation. 

Créez également un ```{!Tool Name}\Node\installNode.ps1``` script. Ce script s’être exécuté sur le nœud géré une fois que tous les fichiers sont copiés à partir de la ```{!Tool Name}\Node``` dossier au nœud géré. Ajouter une logique supplémentaire pour votre cas d’usage. Un exemple ```{!Tool Name}\Node\installNode.ps1``` fichier :

``` ps1
# Add logic for installing payload on managed node
echo 'Success'
```

> [!NOTE]
> ```{!Tool Name}\Node\installNode.ps1``` a un nom spécifique recherchera l’API. Modification du nom de ce fichier entraîne une erreur.


## <a name="integration-with-ui"></a>Intégration avec l’interface utilisateur

Mise à jour ```\src\app\default.component.ts``` à ce qui suit :

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
Mise à jour des espaces réservés pour les valeurs qui ont été utilisés lors de la création de l’extension :
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

Mettre à jour également ```\src\app\default.component.html``` à :
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

## <a name="creating-and-installing-a-nuget-package"></a>Création et installation d’un NuGet Package

La dernière étape est la création d’un package NuGet avec les fichiers que nous avons ajouté et puis en installant ce package dans Windows Admin Center.

Suivez le [Extensions publication](../publish-extensions.md) guide si vous n’avez pas créé un package d’extension avant. 
> [!IMPORTANT]
> Dans votre fichier .nuspec pour cette extension, il est important que le ```<id>``` valeur correspond au nom de votre projet ```manifest.json``` et ```<version>``` correspond à ce qui a été ajouté au ```\src\app\default.component.ts```. Ajoutez également une entrée sous ```<files>```: 
> 
> ```<file src="Node\**\*.*" target="Node" />``` .

``` xml
<?xml version="1.0" encoding="utf-8"?>
<package xmlns="http://schemas.microsoft.com/packaging/2011/08/nuspec.xsd">
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

Une fois ce package est créé, ajoutez un chemin d’accès à ce flux. Dans Windows Admin Center, accédez à Paramètres > Extensions > flux et ajouter le chemin d’accès où ce package existe. Quand votre extension est effectuée en cours d’installation, vous pourrez cliquer sur le ```install``` bouton et l’API sont appelées.  