---
title: Créer un fournisseur de connexion pour une extension de la solution
description: Développer une extension de la solution Windows Admin Center SDK (projet Honolulu) - Création d’un fournisseur de connexion
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 883fba96fcb71cb1c6e8162c1564d66924c4e24d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885650"
---
# <a name="create-a-connection-provider-for-a-solution-extension"></a>Créer un fournisseur de connexion pour une extension de la solution

>S'applique à : Windows Admin Center, version préliminaire de Windows Admin Center

Fournisseurs de connexion jouent un rôle important dans la façon dont Windows Admin Center définit et communique avec les objets connectables, ou les cibles. Essentiellement, un fournisseur de connexion exécute des actions pendant une connexion est établie, telles que de s’assurer que la cible est en ligne et disponible et en faisant en sorte que l’utilisateur connecté est autorisé à accéder à la cible.

Par défaut, Windows Admin Center est fourni avec les fournisseurs de connexion suivants :

* Server
* Client Windows
* Cluster de basculement
* Cluster de HCL

Pour créer votre propre fournisseur de connexion personnalisé, procédez comme suit :

* Ajouter les détails du fournisseur de connexion à ```manifest.json```
* Définir le fournisseur d’état de connexion
* Implémenter le fournisseur de connexions dans la couche d’application

## <a name="add-connection-provider-details-to-manifestjson"></a>Ajouter les détails du fournisseur de connexion à manifest.json

Maintenant nous allons voir ce que vous devez savoir pour définir un fournisseur de connexion dans votre projet ```manifest.json``` fichier.

### <a name="create-entry-in-manifestjson"></a>Créer l’entrée dans manifest.json

Le ```manifest.json``` fichier se trouve dans le dossier \src et qu’il contient, entre autres choses, les définitions des points d’entrée dans votre projet. Types de points d’entrée incluent des outils, des Solutions et des fournisseurs de connexion. Nous allons définir un fournisseur de connexion.

Un exemple d’une entrée de fournisseur de connexion dans manifest.json est ci-dessous :

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

Un point d’entrée de type « connnectionProvider » indique à l’interpréteur de commandes Windows Admin Center que l’élément en cours de configuration est un fournisseur qui servira par une Solution pour valider un état de la connexion. Points d’entrée de fournisseur de connexion contient un nombre de propriétés importantes, définis ci-dessous :

| Propriété | Description |
| -------- | ----------- |
| entryPointType | Cette propriété est obligatoire. Il existe trois valeurs valides : « tool », « solution » et « fournisseur de connexion ». | 
| name | Identifie le fournisseur de connexion dans l’étendue d’une Solution. Cette valeur doit être unique à l’intérieur d’une instance complète de Windows Admin Center (et pas seulement une Solution). |
| path | Représente le chemin d’URL pour « ajouter » interface utilisateur de connexion, s’il est configuré par la Solution. Cette valeur doit correspondre à un itinéraire qui est configuré dans le fichier d’application-routing.module.ts. Lorsque le point d’entrée de Solution est configuré pour utiliser le rootNavigationBehavior de connexions, cet itinéraire chargera le module qui est utilisé par l’interpréteur de commandes pour afficher l’interface utilisateur de connexion ajouter. Informations supplémentaires disponibles dans la section sur rootNavigationBehavior. |
| displayName | La valeur entrée ici s’affiche sur le côté droit de l’interpréteur de commandes, sous la barre noire de Windows Admin Center lorsqu’un utilisateur charge la page connexions de la Solution. |
| icon | Représente l’icône utilisée dans le menu déroulant des Solutions pour représenter la Solution. |
| description | Entrez une brève description du point d’entrée. |
| connectionType | Représente le type de connexion qui charge le fournisseur. La valeur entrée ici sera également être utilisée dans le point d’entrée de Solution pour spécifier que la Solution peut charger ces connexions. La valeur entrée ici sera également servir dans les points d’entrée outil pour indiquer que l’outil est compatible avec ce type. Cette valeur entrée ici sera également utilisée dans l’objet de connexion qui est soumis à l’appel RPC appeler sur la « fenêtre Ajouter », à l’étape de mise en œuvre de couche application. |
| connectionTypeName | Utilisé dans la table de connexions pour représenter une connexion qui utilise le fournisseur de connexions. Il est censé être le nom au pluriel du type. |
| connectionTypeUrlName | Utilisé lors de la création de l’URL pour représenter la Solution chargée, une fois que Windows Admin Center s’est connecté à une instance. Cette entrée est utilisée après les connexions et avant la cible. Dans cet exemple, « connectionexample » est dans lequel cette valeur apparaît dans l’URL : http://localhost:6516/solutionexample/connections/connectionexample/con-fake1.corp.contoso.com |
| connectionTypeDefaultSolution | Représente le composant par défaut qui doit être chargé par le fournisseur de connexion. Cette valeur est une combinaison de : [a] le nom du package d’extension défini en haut du manifeste ; [b] point d’Exclamation ( !) ; [c] le nom de point de Solution.    Pour un projet avec le nom « msft.sme.mySample-extension » et un point d’entrée de Solution avec l’exemple « nom », cette valeur doit être « msft.sme.solutionExample-extension ! exemple ». |
| connectionTypeDefaultTool | Représente la valeur par défaut outil qui doit être chargé sur une connexion réussie. Cette valeur de propriété se compose de deux parties, similaires à la connectionTypeDefaultSolution. Cette valeur est une combinaison de : [a] le nom du package d’extension défini en haut du manifeste ; [b] point d’Exclamation ( !) ; [c] le nom outil de point d’entrée pour l’outil qui doit être chargé initialement. Pour un projet avec le nom « msft.sme.solutionExample-extension » et un point d’entrée de Solution avec l’exemple « nom », cette valeur doit être « msft.sme.solutionExample-extension ! exemple ». |
| connectionStatusProvider | Consultez la section « Définir fournisseur d’état de connexion » |

## <a name="define-connection-status-provider"></a>Définir le fournisseur d’état de connexion

Fournisseur d’état de connexion est le mécanisme par lequel une cible est validée pour être en ligne et disponibles, en faisant en sorte que l’utilisateur connecté est autorisé à accéder à la cible. Il existe actuellement deux types de fournisseurs d’état de connexion :  PowerShell et RelativeGatewayUrl.

*   Fournisseur d’état de connexion de PowerShell
    *   Détermine si une cible est en ligne et accessible avec un script PowerShell. Le résultat doit être retourné dans un objet avec une seule propriété « état », défini ci-dessous.
*   Fournisseur d’état de connexion de RelativeGatewayUrl
    *   Détermine si une cible est en ligne et accessible par un appel rest. Le résultat doit être retourné dans un objet avec une seule propriété « état », défini ci-dessous.

### <a name="define-status"></a>Définir l’état

Fournisseurs d’état de connexion sont requises pour retourner un objet avec une seule propriété ```status``` qui est conforme au format suivant :

``` json
{
    status: {
        label: string;
        type: int;
        details: string;
    }
}
```

Propriétés de l’état :

* Etiquette
    * Une étiquette qui décrit le type de retour d’état. Notez les valeurs d’étiquette peuvent être mappés dans le runtime. Consultez l’entrée ci-dessous pour mapper des valeurs dans le runtime.

* Type
    * Le type de retour de l’état. Type a des valeurs d’énumération suivantes. Pour n’importe quelle valeur 2 ou ultérieur, la plateforme n’est pas naviguer vers l’objet connecté, et une erreur s’affichera dans l’interface utilisateur.

Types :

| Value | Description |
| ----- | ----------- |
| 0 | La licence |
| 1 | Warning |
| 2 | Non autorisé |
| 3 | Erreur |
| 4 | Fatal |
| 5 | Inconnu |

* Détails
    * Type de retour décrivant l’état des détails supplémentaires.

### <a name="powershell-connection-status-provider-script"></a>Script du fournisseur d’état de connexion de PowerShell

Le script PowerShell de fournisseur de statut de connexion détermine si une cible est en ligne et accessible avec un script PowerShell. Le résultat doit être retourné dans un objet avec une seule propriété « état ». Vous trouverez ci-dessous un exemple de script.

Exemple de script PowerShell :

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

### <a name="define-relativegatewayurl-connection-status-provider-method"></a>Définir la méthode du fournisseur d’état RelativeGatewayUrl connexion

Le fournisseur d’état de connexion ```RelativeGatewayUrl``` méthode appelle une API pour déterminer si une cible est en ligne et accessible rest. Le résultat doit être retourné dans un objet avec une seule propriété « état ». Un exemple entrée de fournisseur de connexion dans manifest.json d’un RelativeGatewayUrl est indiqué ci-dessous.

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

Remarques sur l’utilisation de RelativeGatewayUrl :

* « relativeGatewayUrl » indique où obtenir de l’état de connexion à partir d’une URL de passerelle. Cet URI est relatif à partir / API. Si $connectionName se trouve dans l’URL, il sera remplacé par le nom de la connexion.
* Toutes les propriétés relativeGatewayUrl doivent être exécutées sur la passerelle d’hôte, ce qui peut être effectuée en créant une extension de la passerelle

### <a name="map-values-in-runtime"></a>Mapper les valeurs de runtime

Les valeurs d’étiquette et les détails dans l’état d’objet de retour peut être mis en forme à régler le temps en incluant des clés et valeurs dans la propriété « defaultValueMap » du fournisseur.

Par exemple, si vous ajoutez la valeur ci-dessous, chaque fois que « defaultConnection_test » s’affichaient en tant que valeur pour l’étiquette ou plus d’informations, Windows Admin Center remplace automatiquement la clé avec la valeur de chaîne de ressource configurée.

``` json
    "defaultConnection_test": "resources:strings:addServer_status_defaultConnection_label"
```

## <a name="implement-connection-provider-in-application-layer"></a>Implémenter le fournisseur de connexions dans la couche d’application

Maintenant, nous allons implémenter le fournisseur de connexions dans la couche application, en créant une classe TypeScript qui implémente OnInit. La classe a les fonctions suivantes :

| Fonction | Description |
| -------- | ----------- |
| constructor(Private appContextService: AppContextService, itinéraire privé : ActivatedRoute) |  |
| public ngOnInit() |  |
| onSubmit() public | Contient la logique pour mettre à jour d’interpréteur de commandes lorsqu’une tentative de connexion add est effectuée |
| onCancel() publique | Contient la logique pour mettre à jour d’interpréteur de commandes lorsqu’une tentative de connexion d’ajouter est annulée |

### <a name="define-onsubmit"></a>Définir onSubmit

```onSubmit``` problèmes de RPC rappeler le contexte de l’application pour informer le shell d’une « ajouter une connexion ». L’appel de base utilise « updateData » comme suit :

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

Le résultat est une propriété de connexion, qui est un tableau d’objets qui sont conformes à la structure suivante :

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

### <a name="define-oncancel"></a>Définir onCancel

```onCancel``` Annule une tentative de « Ajouter une connexion » en passant un tableau vide de connexions :

``` ts
this.appContextService.rpc.updateData(EnvironmentModule.nameOfShell, '##', <RpcUpdateData>{ results: { connections: [] } });
```

## <a name="connection-provider-example"></a>Exemple de fournisseur de connexion

La classe TypeScript complète pour l’implémentation d’un fournisseur de connexion est ci-dessous. Notez que la chaîne « connectionType » correspond à la « connectionType tel que défini dans le fournisseur de connexion dans manifest.json.

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
