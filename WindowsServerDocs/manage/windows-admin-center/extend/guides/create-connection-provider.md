---
title: Créer un fournisseur de connexion pour une extension de solution
description: Développer une extension de solution SDK Windows Admin Center (projet Honolulu) - permet de créer un fournisseur de connexion
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 883fba96fcb71cb1c6e8162c1564d66924c4e24d
ms.sourcegitcommit: be0144eb59daf3269bebea93cb1c467d67e2d2f1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/20/2018
ms.locfileid: "4081136"
---
# Créer un fournisseur de connexion pour une extension de solution

>S’applique à: Windows Admin Center, Windows Admin Center Preview

Fournisseurs de connexion jouent un rôle important dans comment Windows Admin Center définit et communique avec les objets pouvant être connectés ou cibles. Principalement, un fournisseur de connexion effectue les actions pendant une connexion est établie, par exemple, s’assurer que la cible est en ligne et disponible et en garantissant que l’utilisateur connecté est autorisé à accéder à la cible.

Par défaut, Windows Admin Center est fourni avec les fournisseurs de connexion suivants:

* Serveur
* Client Windows
* Cluster de basculement
* Cluster HCI

Pour créer votre propre fournisseur de connexion personnalisé, procédez comme suit:

* Ajouter des détails de fournisseur de connexion à ```manifest.json```
* Définir le fournisseur de statut de connexion
* Implémenter le fournisseur de connexion de couche d’application

## Ajouter des détails de fournisseur de connexion à manifest.json

Maintenant que nous allons guidons tout ce que vous devez savoir pour définir un fournisseur de connexion dans de votre projet ```manifest.json``` fichier.

### Créer entrée dans manifest.json

Le ```manifest.json``` fichier se trouve dans le dossier \src et qu’il contient, entre autres choses, les définitions des points d’entrée dans votre projet. Types de points d’entrée incluent des outils, des Solutions et des fournisseurs de connexion. Nous allons définir un fournisseur de connexion.

Un exemple d’une entrée de fournisseur de connexion dans manifest.json est ci-dessous:

``` json
    {
      "entryPointType": "connectionProvider",
      "name": "addServer",
      "path": "/add",
      "displayName": "resources:strings:addServer_displayName",
      "icon": "sme-icon:icon-win-server",
      "description": "resources:strings:description",
      "connectionType": "msft.sme.connection-type.server",
      "connectionTypeName": "resources:strings:addServer_connectionTypeName",
      "connectionTypeUrlName": "server",
      "connectionTypeDefaultSolution": "msft.sme.server-manager!servers",
      "connectionTypeDefaultTool": "msft.sme.server-manager!overview",
      "connectionStatusProvider": {
        "powerShell": {
          "script": "## Get-My-Status ##\nfunction Get-Status()\n{\n# A function like this would be where logic would exist to identify if a node is connectable.\n$status = @{label = $null; type = 0; details = $null; }\n$caption = \"MyConstCaption\"\n$productType = \"MyProductType\"\n# A result object needs to conform to the following object structure to be interpreted properly by the Windows Admin Center shell.\n$result = @{ status = $status; caption = $caption; productType = $productType; version = $version }\n# DO FANCY LOGIC #\n# Once the logic is complete, the following fields need to be populated:\n$status.label = \"Display Thing\"\n$status.type = 0 # This value needs to conform to the LiveConnectionStatusType enum. >= 3 represents a failure.\n$status.details = \"success stuff\"\nreturn $result}\nGet-Status"
        },
        "displayValueMap": {
          "wmfMissing-label": "resources:strings:addServer_status_wmfMissing_label",
          "wmfMissing-details": "resources:strings:addServer_status_wmfMissing_details",
          "unsupported-label": "resources:strings:addServer_status_unsupported_label",
          "unsupported-details": "resources:strings:addServer_status_unsupported_details"
        }
      }
    },
```

Un point d’entrée de type «connnectionProvider» indique à l’interpréteur de commandes Windows Admin Center l’élément en cours de configuration en tant que fournisseur qui doit être utilisé par une Solution pour valider un état de connexion. Points d’entrée de fournisseur de connexion contient un certain nombre de propriétés importantes, défini ci-dessous:

| Propriété | Description |
| -------- | ----------- |
| entryPointType | Il s’agit d’une propriété requise. Il existe trois valeurs valides: «outil», «solution» et «connectionProvider». | 
| name | Identifie le fournisseur de connexion dans l’étendue d’une Solution. Cette valeur doit être unique à l’intérieur d’une instance de Windows Admin Center complète (et pas simplement une Solution). |
| path | Représente le chemin d’accès d’URL pour «ajouter» interface utilisateur de connexion, si elle sera configuré par la Solution. Cette valeur doit correspondre à un itinéraire est configuré dans le fichier de routing.module.ts de l’application. Lorsque le point d’entrée de Solution est configuré pour utiliser le connexions rootNavigationBehavior, cet itinéraire chargera le module qui est utilisé par l’interpréteur de commandes pour afficher l’interface utilisateur de connexion ajouter. Plus d’informations disponibles dans la section sur rootNavigationBehavior. |
| displayName | La valeur entrée ici s’affiche sur le côté droit de l’interpréteur de commandes, sous la barre de Windows Admin Center noire lorsqu’un utilisateur charge la page de connexions d’une Solution. |
| icon | Représente l’icône utilisée dans le menu déroulant Solutions pour représenter la Solution. |
| description | Entrez une brève description du point d’entrée. |
| connectionType | Représente le type de connexion que le fournisseur se charger. La valeur entrée ici sera également servir dans le point d’entrée de Solution pour spécifier que la Solution peut charger ces connexions. La valeur entrée ici sera également servir dans l’outil ou les points d’entrée pour indiquer que l’outil est compatible avec ce type. Cette valeur entrée ici est également utilisée dans l’objet de connexion qui est soumis à la RPC appeler sur la «fenêtre Ajouter», à l’étape de mise en œuvre de couche application. |
| connectionTypeName | Utilisé dans le tableau de connexions pour représenter une connexion qui utilise votre fournisseur de connexion. Cette valeur doit être le nom pluriel du type. |
| connectionTypeUrlName | Utilisé pour créer l’URL pour représenter la Solution chargée, une fois que Windows Admin Center s’est connecté à une instance. Cette entrée est utilisée après les connexions et avant la cible. Dans cet exemple, «connectionexample» est où cette valeur apparaît dans l’URL:http://localhost:6516/solutionexample/connections/connectionexample/con-fake1.corp.contoso.com |
| connectionTypeDefaultSolution | Représente le composant par défaut qui doit être chargé par le fournisseur de connexion. Cette valeur est une combinaison de: [a] le nom du package d’extension défini en haut du manifeste; [b] point d’Exclamation (!); [c] le nom de point de Solution.    Pour un projet avec le nom «msft.sme.mySample-extension» et un point d’entrée avec «exemple de nom de» Solution, cette valeur serait «msft.sme.solutionExample-extension! exemple». |
| connectionTypeDefaultTool | Représente la valeur par défaut outil qui doit être chargé sur une connexion réussie. Cette valeur de propriété est constituée de deux parties, similaires à la connectionTypeDefaultSolution. Cette valeur est une combinaison de: [a] le nom du package d’extension défini en haut du manifeste; [b] point d’Exclamation (!); [c] outil entrée point nom de l’outil qui doit être chargé initialement. Pour un projet avec le nom «msft.sme.solutionExample-extension» et un point d’entrée avec «exemple de nom de» Solution, cette valeur serait «msft.sme.solutionExample-extension! exemple». |
| connectionStatusProvider | Consultez la section «Définir le fournisseur du statut de connexion» |

## Définir le fournisseur de statut de connexion

Fournisseur de statut de connexion est le mécanisme par lequel une cible est validée pour être en ligne et disponibles, en garantissant que l’utilisateur connecté est autorisé à accéder à la cible. Il existe actuellement deux types de fournisseurs d’état de connexion: PowerShell et RelativeGatewayUrl.

*   Fournisseur de statut de connexion de PowerShell
    *   Détermine si une cible est en ligne et accessibles avec un script PowerShell. Le résultat doit être retourné dans un objet avec une seule propriété «état», défini ci-dessous.
*   Fournisseur de statut de connexion RelativeGatewayUrl
    *   Détermine si une cible est en ligne et accessibles avec un appel rest. Le résultat doit être retourné dans un objet avec une seule propriété «état», défini ci-dessous.

### Définir l’état

Les fournisseurs de statut de connexion sont nécessaires pour renvoyer un objet avec une seule propriété ```status``` qui doit être conforme au format suivant:

``` json
{
    status: {
        label: string;
        type: int;
        details: string;
    }
}
```

Propriétés de l’état:

* Label
    * Une étiquette décrivant le type de retour d’état. Notez que les valeurs d’étiquette peuvent être mappés dans le runtime. Consultez entrée ci-dessous pour les valeurs de mappage dans le runtime.

* Type
    * Le type de retour de l’état. Type présente des valeurs d’énumération suivantes. Pour n’importe quelle valeur 2 ou plus, la plateforme n’est pas accéder à l’objet connecté, et une erreur s’affichera dans l’interface utilisateur.

Types de:

| Valeur | Description |
| ----- | ----------- |
| 0 | Online |
| 1 | Warning |
| 2 | Non autorisé |
| 3 | Erreur |
| 4 | Irrécupérable |
| 5 | Inconnu |

* Détails
    * Type de retour qui décrit l’état des détails supplémentaires.

### Script de fournisseur de statut de connexion de PowerShell

Le script PowerShell de fournisseur d’état de la connexion détermine si une cible est en ligne et accessibles avec un script PowerShell. Le résultat doit être retourné dans un objet avec une seule propriété «état». Vous trouverez ci-dessous un exemple de script.

Exemple de script PowerShell:

``` ts
## Get-My-Status ##

function Get-Status()
{
    # A function like this would be where logic would exist to identify if a node is connectable.
    $status = @{label = $null; type = 0; details = $null; }
    $caption = "MyConstCaption"
    $productType = "MyProductType"

    # A result object needs to conform to the following object structure to be interperated properly by the Windows Admin Center shell.
    $result = @{ status = $status; caption = $caption; productType = $productType; version = $version }

    # DO FANCY LOGIC #

    # Once the logic is complete, the following fields need to be populated:
    $status.label = "Display Thing"
    $status.type = 0 # This value needs to conform to the LiveConnectionStatusType enum. >= 3 represents a failure.
    $status.details = "success stuff"

    return $result
}

Get-Status
```

### Définir la méthode de fournisseur de statut de connexion RelativeGatewayUrl

Le fournisseur d’état de connexion ```RelativeGatewayUrl``` méthode appelle un rest API pour déterminer si une cible est en ligne et accessibles. Le résultat doit être retourné dans un objet avec une seule propriété «état». Un exemple entrée du fournisseur de connexion dans manifest.json d’un RelativeGatewayUrl est illustré ci-dessous.

``` json
    {
      "entryPointType": "connectionProvider",
      "name": "addServer",
      "path": "/add/server",
      "displayName": "resources:strings:addServer_displayName",
      "icon": "sme-icon:icon-win-server",
      "description": "resources:strings:description",
      "connectionType": "msft.sme.connection-type.server",
      "connectionTypeName": "resources:strings:addServer_connectionTypeName",
      "connectionTypeUrlName": "server",
      "connectionTypeDefaultSolution": "msft.sme.server-manager!servers",
      "connectionTypeDefaultTool": "msft.sme.server-manager!overview",
      "connectionStatusProvider": {
        "relativeGatewayUrl": "<URL here post /api>",
        "displayValueMap": {
          "wmfMissing-label": "resources:strings:addServer_status_wmfMissing_label",
          "wmfMissing-details": "resources:strings:addServer_status_wmfMissing_details",
          "unsupported-label": "resources:strings:addServer_status_unsupported_label",
          "unsupported-details": "resources:strings:addServer_status_unsupported_details"
        }
      }
    },
```

Remarques sur l’utilisation de RelativeGatewayUrl:

* «relativeGatewayUrl» indique où obtenir l’état de connexion à partir d’une URL de passerelle. Cet URI est relatif à partir / API. Si $connectionName est introuvable dans l’URL, il est remplacé par le nom de la connexion.
* Toutes les propriétés de relativeGatewayUrl doivent être exécutées sur l’hôte passerelle, ce qui peut être obtenue en créant une extension de passerelle

### Mapper les valeurs de runtime

Les valeurs d’étiquette et les détails dans l’état du format de l’objet de retour peut être appliqué à régler le temps en incluant les clés et les valeurs dans la propriété «defaultValueMap» du fournisseur.

Par exemple, si vous ajoutez la valeur ci-dessous, chaque fois que «defaultConnection_test» s’affichaient comme une valeur pour l’étiquette ou plus d’informations, Windows Admin Center remplace automatiquement la clé avec la valeur de chaîne de ressource configurée.

``` json
    "defaultConnection_test": "resources:strings:addServer_status_defaultConnection_label"
```

## Implémenter le fournisseur de connexion de couche d’application

Nous allons maintenant pour implémenter le fournisseur de connexion de la couche d’application, en créant une classe TypeScript qui implémente OnInit. La classe possède les fonctions suivantes:

| Fonction | Description |
| -------- | ----------- |
| constructeur (appContextService privé: AppContextService, itinéraire privé: ActivatedRoute) |  |
| ngOnInit() publics |  |
| onSubmit() publics | Contient la logique pour mettre à jour d’interpréteur de commandes lorsqu’une tentative de connexion ajouter est effectuée. |
| onCancel() publique | Contient la logique pour mettre à jour d’interpréteur de commandes lorsqu’une tentative de connexion ajouter est annulée. |

### Définir onSubmit

```onSubmit``` problèmes un RPC rappeler le contexte de l’application pour signaler à l’interpréteur de commandes d’un «ajouter une connexion». L’appel de base utilise «updateData» comme suit:

``` ts
this.appContextService.rpc.updateData(
    EnvironmentModule.nameOfShell,
    '##',
    <RpcUpdateData>{
        results: {
            connections: connections,
            credentials: this.useCredentials ? this.creds : null
        }
    }
);
```

Le résultat est une propriété de connexion, qui est un tableau d’objets qui sont conformes à la structure suivante:

``` ts

/**
 * The connection attributes class.
 */
export interface ConnectionAttribute {

    /**
     * The id string of this attribute
     */
    id: string;

    /**
     * The value of the attribute. used for attributes that can have variable values such as Operating System
     */
    value?: string | number;
}

/**
 * The connection class.
 */
export interface Connection {

    /**
     * The id of the connection, this is unique per connection
     */
    id: string;

    /**
     * The type of connection
     */
    type: string;

    /**
     * The name of the connection, this is unique per connection type
     */
    name: string;

    /**
     * The property bag of the connection
     */
    properties?: ConnectionProperties;

    /**
     * The ids of attributes identified for this connection
     */
    attributes?: ConnectionAttribute[];

    /**
     * The tags the user(s) have assigned to this connection
     */
    tags?: string[];
}

/**
 * Defines connection type strings known by core
 * Be careful that these strings match what is defined by the manifest of @msft-sme/server-manager
 */
export const connectionTypeConstants = {
    server: 'msft.sme.connection-type.server',
    cluster: 'msft.sme.connection-type.cluster',
    hyperConvergedCluster: 'msft.sme.connection-type.hyper-converged-cluster',
    windowsClient: 'msft.sme.connection-type.windows-client',
    clusterNodesProperty: 'nodes'
};
```

### Définir onCancel

```onCancel``` Annule une tentative de «Ajouter une connexion» en transmettant un tableau de connexions vide:

``` ts
this.appContextService.rpc.updateData(EnvironmentModule.nameOfShell, '##', <RpcUpdateData>{ results: { connections: [] } });
```

## Exemple de fournisseur de connexion

La classe TypeScript complète pour l’implémentation d’un fournisseur de connexion est fourni ci-dessous. Notez que la chaîne «connectionType» correspond à la «connectionType tel que défini dans le fournisseur de connexion dans manifest.json.

``` ts
import { Component, OnInit } from '@angular/core';
import { ActivatedRoute } from '@angular/router';
import { AppContextService } from '@msft-sme/shell/angular';
import { Connection, ConnectionUtility } from '@msft-sme/shell/core';
import { EnvironmentModule } from '@msft-sme/shell/dist/core/manifest/environment-modules';
import { RpcUpdateData } from '@msft-sme/shell/dist/core/rpc/rpc-base';
import { Strings } from '../../generated/strings';

@Component({
  selector: 'add-example',
  templateUrl: './add-example.component.html',
  styleUrls: ['./add-example.component.css']
})
export class AddExampleComponent implements OnInit {
  public newConnectionName: string;
  public strings = MsftSme.resourcesStrings<Strings>().SolutionExample;
  private connectionType = 'msft.sme.connection-type.example'; // This needs to match the connectionTypes value used in the manifest.json.
  
  constructor(private appContextService: AppContextService, private route: ActivatedRoute) {
    // TODO:
  }

  public ngOnInit() {
    // TODO
  }

  public onSubmit() {
    let connections: Connection[] = [];

    let connection = <Connection> {
      id: ConnectionUtility.createConnectionId(this.connectionType, this.newConnectionName),
      type: this.connectionType,
      name: this.newConnectionName
    };

    connections.push(connection);

    this.appContextService.rpc.updateData(
      EnvironmentModule.nameOfShell,
      '##',
      <RpcUpdateData> {
        results: {
          connections: connections,
          credentials: null
        }
      }
    );
  }

  public onCancel() {
    this.appContextService.rpc.updateData(
      EnvironmentModule.nameOfShell, '##', <RpcUpdateData>{ results: { connections: [] } });
  }
}

```
