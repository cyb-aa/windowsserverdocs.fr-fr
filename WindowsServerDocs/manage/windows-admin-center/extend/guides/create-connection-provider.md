---
title: Créer un fournisseur de connexion pour une extension de solution
description: Développer une extension de solution Kit de développement logiciel (SDK) du centre d’administration Windows (Project Honolulu)-créer un fournisseur de connexion
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 06/06/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 9c04db3196d1e806e50af9164b3c8bcdfb19b079
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406881"
---
# <a name="create-a-connection-provider-for-a-solution-extension"></a>Créer un fournisseur de connexion pour une extension de solution

>S'applique à : Windows Admin Center, Windows Admin Center Preview

Les fournisseurs de connexions jouent un rôle important dans la façon dont le centre d’administration Windows définit et communique avec les objets connectables, ou cibles. En premier lieu, un fournisseur de connexion effectue des actions lors de l’établissement d’une connexion, par exemple en vérifiant que la cible est en ligne et disponible, tout en veillant à ce que l’utilisateur qui se connecte ait l’autorisation d’accéder à la cible.

Par défaut, le centre d’administration Windows est fourni avec les fournisseurs de connexions suivants :

* Server
* Client Windows
* Cluster de basculement
* Cluster HCI

Pour créer votre propre fournisseur de connexion personnalisé, procédez comme suit :

* Ajouter les détails du fournisseur de connexion à```manifest.json```
* Définir le fournisseur d’état de connexion
* Implémenter le fournisseur de connexion dans la couche application

## <a name="add-connection-provider-details-to-manifestjson"></a>Ajouter les détails du fournisseur de connexion à manifest. JSON

Nous allons maintenant examiner ce que vous devez savoir pour définir un fournisseur de connexion dans le fichier de ```manifest.json``` votre projet.

### <a name="create-entry-in-manifestjson"></a>Créer une entrée dans Manifest. JSON

Le ```manifest.json``` fichier se trouve dans le dossier \SRC et contient, entre autres choses, des définitions de points d’entrée dans votre projet. Les types de points d’entrée incluent les outils, les solutions et les fournisseurs de connexion. Nous allons définir un fournisseur de connexion.

Voici un exemple d’entrée de fournisseur de connexion dans Manifest. JSON :

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

Un point d’entrée de type « connnectionProvider » indique à l’interpréteur de commandes du centre d’administration Windows que l’élément en cours de configuration est un fournisseur qui sera utilisé par une solution pour valider un état de connexion. Les points d’entrée du fournisseur de connexion contiennent un certain nombre de propriétés importantes, définies ci-dessous :

| Propriété | Description |
| -------- | ----------- |
| entryPointType | Cette propriété est obligatoire. Il existe trois valeurs valides : « Tool », « solution » et « connectionProvider ». | 
| name | Identifie le fournisseur de connexion dans l’étendue d’une solution. Cette valeur doit être unique au sein d’une instance complète du centre d’administration Windows (pas seulement une solution). |
| path | Représente le chemin d’accès de l’URL de l’interface utilisateur « Ajouter une connexion », s’il est configuré par la solution. Cette valeur doit être mappée à un itinéraire configuré dans le fichier app-Routing. module. TS. Lorsque le point d’entrée de solution est configuré pour utiliser les connexions rootNavigationBehavior, cet itinéraire charge le module utilisé par le Shell pour afficher l’interface utilisateur Add Connection. Pour plus d’informations, retrouvez la section sur rootNavigationBehavior. |
| displayName | La valeur entrée ici s’affiche à droite de l’interpréteur de commandes, sous la barre Centre d’administration Windows noire quand un utilisateur charge la page connexions d’une solution. |
| icon | Représente l’icône utilisée dans le menu déroulant des solutions pour représenter la solution. |
| description | Entrez une brève description du point d’entrée. |
| connectionType | Représente le type de connexion que le fournisseur chargera. La valeur entrée ici sera également utilisée dans le point d’entrée de la solution pour spécifier que la solution peut charger ces connexions. La valeur entrée ici sera également utilisée dans le ou les points d’entrée de l’outil pour indiquer que l’outil est compatible avec ce type. Cette valeur entrée ici sera également utilisée dans l’objet de connexion qui est envoyé à l’appel RPC sur « Ajouter une fenêtre », dans l’étape implémentation de la couche application. |
| connectionTypeName | Utilisé dans la table connexions pour représenter une connexion qui utilise votre fournisseur de connexion. Il doit s’agir du nom au pluriel du type. |
| connectionTypeUrlName | Utilisé lors de la création de l’URL pour représenter la solution chargée, une fois que le centre d’administration Windows s’est connecté à une instance de. Cette entrée est utilisée après les connexions et avant la cible. Dans cet exemple, « connectionexample » correspond à l’emplacement où cette valeur apparaît dans l’URL :`http://localhost:6516/solutionexample/connections/connectionexample/con-fake1.corp.contoso.com` |
| connectionTypeDefaultSolution | Représente le composant par défaut qui doit être chargé par le fournisseur de connexion. Cette valeur est une combinaison des éléments suivants : <br>[a] nom du package d’extension défini en haut du manifeste ; <br>[b] point d’exclamation ( !); <br>[c] nom du point d’entrée de la solution.    <br>Pour un projet portant le nom « msft. SME. mySample-extension » et un point d’entrée de solution nommé « example », cette valeur est « msft. SME. solutionExample-extension ! example ». |
| connectionTypeDefaultTool | Représente l’outil par défaut qui doit être chargé sur une connexion réussie. Cette valeur de propriété est composée de deux parties, similaires à connectionTypeDefaultSolution. Cette valeur est une combinaison des éléments suivants : <br>[a] nom du package d’extension défini en haut du manifeste ; <br>[b] point d’exclamation ( !); <br>[c] nom du point d’entrée de l’outil qui doit être chargé initialement. <br>Pour un projet portant le nom « msft. SME. solutionExample-extension » et un point d’entrée de solution nommé « example », cette valeur est « msft. SME. solutionExample-extension ! example ». |
| connectionStatusProvider | Veuillez consulter la section « définir le fournisseur de l’état de la connexion » |

## <a name="define-connection-status-provider"></a>Définir le fournisseur d’état de connexion

Le fournisseur d’état de connexion est le mécanisme par lequel une cible est validée pour être en ligne et disponible, garantissant également que l’utilisateur qui se connecte a l’autorisation d’accéder à la cible. Il existe actuellement deux types de fournisseurs d’état de connexion :  PowerShell et RelativeGatewayUrl.

*   <strong>Fournisseur d’état de connexion PowerShell</strong> : détermine si une cible est en ligne et accessible à l’aide d’un script PowerShell. Le résultat doit être retourné dans un objet avec une propriété unique « Status », définie ci-dessous.
*   <strong>Fournisseur d’état de connexion RelativeGatewayUrl</strong> : détermine si une cible est en ligne et accessible à l’aide d’un appel Rest. Le résultat doit être retourné dans un objet avec une propriété unique « Status », définie ci-dessous.

### <a name="define-status"></a>Définir l’État

Les fournisseurs d’état de connexion sont requis pour retourner un objet avec ```status``` une propriété unique conforme au format suivant :

``` json
{
    status: {
        label: string;
        type: int;
        details: string;
    }
}
```

Propriétés de l’État :

* <strong>Label</strong> : étiquette décrivant le type de retour de l’État. Notez que les valeurs de l’étiquette peuvent être mappées dans le Runtime. Consultez l’entrée ci-dessous pour mapper les valeurs dans le Runtime.

* <strong>Type</strong> : le type de retour de l’État. Le type a les valeurs d’énumération suivantes. Pour toute valeur 2 ou supérieure, la plateforme n’accède pas à l’objet connecté et une erreur s’affiche dans l’interface utilisateur.

   Modes

  | Value | Description |
  | ----- | ----------- |
  | 0 | La licence |
  | 1 | Warning |
  | 2 | Non autorisé |
  | 3 | Error |
  | 4 | Fatal |
  | 5 | Inconnu |

* <strong>Détails</strong> : détails supplémentaires qui décrivent le type de retour de l’État.

### <a name="powershell-connection-status-provider-script"></a>Script du fournisseur d’état de connexion PowerShell

Le script PowerShell du fournisseur d’état de connexion détermine si une cible est en ligne et accessible à l’aide d’un script PowerShell. Le résultat doit être retourné dans un objet avec une propriété unique « Status ». Vous trouverez ci-dessous un exemple de script.

Exemple de script PowerShell :

```PowerShell
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

### <a name="define-relativegatewayurl-connection-status-provider-method"></a>Définir la méthode du fournisseur d’état de connexion RelativeGatewayUrl

La méthode du fournisseur ```RelativeGatewayUrl``` d’état de connexion appelle une API REST pour déterminer si une cible est en ligne et accessible. Le résultat doit être retourné dans un objet avec une propriété unique « Status ». Un exemple d’entrée de fournisseur de connexion dans le fichier manifest. JSON d’un RelativeGatewayUrl est illustré ci-dessous.

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

* « relativeGatewayUrl » indique où obtenir l’état de la connexion à partir d’une URL de passerelle. Cet URI est relatif à partir de/API. Si $connectionName se trouve dans l’URL, il est remplacé par le nom de la connexion.
* Toutes les propriétés relativeGatewayUrl doivent être exécutées sur la passerelle hôte, ce qui peut être accompli en créant une extension de passerelle.

### <a name="map-values-in-runtime"></a>Mapper des valeurs dans le Runtime

Vous pouvez mettre en forme les valeurs d’étiquette et de détails dans l’objet de retour d’État au moment de l’optimisation en incluant des clés et des valeurs dans la propriété « defaultValueMap » du fournisseur.

Par exemple, si vous ajoutez la valeur ci-dessous, chaque fois que « defaultConnection_test » a été affiché en tant que valeur pour l’étiquette ou les détails, le centre d’administration Windows remplace automatiquement la clé par la valeur de chaîne de ressource configurée.

``` json
    "defaultConnection_test": "resources:strings:addServer_status_defaultConnection_label"
```

## <a name="implement-connection-provider-in-application-layer"></a>Implémenter le fournisseur de connexion dans la couche application

À présent, nous allons implémenter le fournisseur de connexion dans la couche d’application, en créant une classe de type de machine à écrire qui implémente OnInit. La classe possède les fonctions suivantes :

| Fonction | Description |
| -------- | ----------- |
| constructeur (appContextService privé : AppContextService, Itinéraire privé : ActivatedRoute) |  |
| public ngOnInit () |  |
| onSubmit public () | Contient une logique pour mettre à jour le shell quand une tentative d’ajout de connexion est effectuée |
| onCancel public () | Contient une logique pour mettre à jour le shell quand une tentative d’ajout de connexion est annulée |

### <a name="define-onsubmit"></a>Définir onSubmit

```onSubmit```lance un rappel RPC vers le contexte d’application pour notifier l’interpréteur de commandes d’une « ajout de connexion ». L’appel de base utilise « updateData » comme suit :

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

Le résultat est une propriété de connexion, qui est un tableau d’objets conformes à la structure suivante :

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

```onCancel```annule une tentative d’ajout de connexion en passant un tableau de connexions vide :

``` ts
this.appContextService.rpc.updateData(EnvironmentModule.nameOfShell, '##', <RpcUpdateData>{ results: { connections: [] } });
```

## <a name="connection-provider-example"></a>Exemple de fournisseur de connexion

La classe dactylographié complète pour l’implémentation d’un fournisseur de connexion est indiquée ci-dessous. Notez que la chaîne « connectionType » correspond à « connectionType », comme défini dans le fournisseur de connexion dans Manifest. JSON.

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
