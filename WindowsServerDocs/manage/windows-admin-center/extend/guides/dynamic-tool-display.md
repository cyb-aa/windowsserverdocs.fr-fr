---
title: Contrôler la visibilité de votre outil dans une solution
description: Contrôler la visibilité de votre outil dans une solution Windows Admin Center SDK (projet Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 3cce07ba5b3d2cc89f1363bbb2af5acd048c0466
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445939"
---
# <a name="control-your-tools-visibility-in-a-solution"></a>Contrôler la visibilité de votre outil dans une solution #

>S'applique à : Windows Admin Center, version préliminaire de Windows Admin Center

Il peut arriver lorsque vous souhaitez exclure (ou masquer) votre extension ou l’outil à partir de la liste des outils disponibles. Par exemple, si votre outil cible uniquement Windows Server 2016 (pas les anciennes versions), vous pouvez exclure un utilisateur qui se connecte à un serveur Windows Server 2012 R2 pour voir votre outil tout. (Imaginez l’expérience utilisateur - ils cliquer dessus, attendez que l’outil de chargement, uniquement pour recevoir un message que ses fonctionnalités ne sont pas disponibles pour leur connexion.) Vous pouvez définir quand afficher (ou masquer) votre fonctionnalité dans le fichier de l’outil manifest.json.

## <a name="options-for-deciding-when-to-show-a-tool"></a>Options pour déterminer à quel moment afficher un outil ##

Il existe trois options différentes, que vous pouvez utiliser pour déterminer si votre outil doit être affichées et disponibles pour un serveur spécifique ou d’une connexion au cluster.

* localhost
* inventaire (il s’agit d’un tableau de propriétés)
* script

### <a name="localhost"></a>LocalHost ###

La propriété localHost de l’objet de Conditions contient une valeur booléenne qui peut être évaluée pour déduire si le nœud qui se connecte est localHost (le même ordinateur Windows Admin Center est installé sur) ou non. En transmettant une valeur à la propriété, vous indiquez quand (condition) pour afficher l’outil. Par exemple, si vous ne voulez que l’outil à afficher si l’utilisateur se connecte en fait à l’hôte local, configurez-le comme suit :

``` json
"conditions": [
{
    "localhost": true
}]
```

Vous pouvez également, si vous ne voulez que votre outil à afficher lorsque le nœud connexion *n’est pas* localhost :

``` json
"conditions": [
{
    "localhost": false
}]
```

Voici ce que les paramètres de configuration ressemblent à afficher uniquement un outil lorsque le nœud de connexion n’est pas localhost :

``` json
"entryPoints": [
{
    "entryPointType": "tool",
    "name": "main",
    "urlName": "processes",
    "displayName": "resources:strings:displayName",
    "description": "resources:strings:description",
    "icon": "sme-icon:icon-win-serverProcesses",
    "path": "",
    "requirements": [
    {
        "solutionIds": [
        "msft.sme.server-manager!windowsClients"
        ],
        "connectionTypes": [
        "msft.sme.connection-type.windows-client"
        ],
        "conditions": [
        {
            "localhost": true
        }
        ]
    }
    ]
}
```

### <a name="inventory-properties"></a>Propriétés de l’inventaire ###

Le SDK inclut un ensemble organisé préalable de propriétés stock que vous pouvez utiliser pour générer des conditions pour déterminer quand votre outil doit être disponible ou non. Il existe neuf propriétés différentes dans le tableau « stock » :

| Nom de la propriété | Type de valeur attendu |
| ------------- | ------------------- |
| computerManufacturer | chaîne |
| operatingSystemSKU | nombre |
| operatingSystemVersion | version_string (par exemple : "10.1.*") |
| productType | nombre |
| clusterFqdn | chaîne |
| isHyperVRoleInstalled | booléenne |
| isHyperVPowershellInstalled | booléenne |
| isManagementToolsAvailable | booléenne |
| isWmfInstalled | booléenne |

Chaque objet dans le tableau d’inventaire doit être conforme à la structure json suivante :

``` json
"<property name>": {
    "type": "<expected type>",
    "operator": "<defined operator to use>",
    "value": "<expected value to evaluate using the operator>"
}
```

#### <a name="operator-values"></a>Valeurs d’opérateur ####

| Opérateur | Description |
| -------- | ----------- |
| gt | Supérieur à |
| GE | Supérieur ou égal à |
| lt | Inférieur à |
| le | Inférieur ou égal à |
| eq | Égal à |
| Nou | Non égal à |
| est | vérifier si une valeur est true |
| not | vérifier si une valeur est false |
| contient | élément existe dans une chaîne |
| notContains | élément n’existe pas dans une chaîne |

#### <a name="data-types"></a>Types de données ####

Options disponibles pour la propriété 'type' :

| type | Description |
| ---- | ----------- |
| version | un numéro de version (par exemple : 10.1.*) |
| nombre | une valeur numérique |
| chaîne | valeur de chaîne |
| booléenne | la valeur True ou false |

#### <a name="value-types"></a>Types valeur ####

La propriété 'value' accepte ces types :

* chaîne
* nombre
* booléenne

Un ensemble de conditions de stock correctement formé ressemble à ceci :

``` json
"entryPoints": [
{
    "entryPointType": "tool",
    "name": "main",
    "urlName": "processes",
    "displayName": "resources:strings:displayName",
    "description": "resources:strings:description",
    "icon": "sme-icon:icon-win-serverProcesses",
    "path": "",
    "requirements": [
    {
        "solutionIds": [
        "msft.sme.server-manager!servers"
        ],
        "connectionTypes": [
        "msft.sme.connection-type.server"
        ],
        "conditions": [
        {
            "inventory": {
            "operatingSystemVersion": {
                "type": "version",
                "operator": "gt",
                "value": "6.3"
            },
            "operatingSystemSKU": {
                "type": "number",
                "operator": "eq",
                "value": "8"
            }
            }
        }
        ]
    }
    ]
}
```

### <a name="script"></a>Script ###

Enfin, vous pouvez exécuter un script PowerShell personnalisé afin d’identifier la disponibilité et l’état du nœud. Tous les scripts doivent retourner un objet avec la structure suivante :

``` ps
@{
    State = 'Available' | 'NotSupported' | 'NotConfigured';
    Message = '<Message to explain the reason of state such as not supported and not configured.>';
    Properties =
        @{ Name = 'Prop1'; Value = 'prop1 data'; Type = 'string' },
        @{Name='Prop2'; Value = 12345678; Type='number'; };
}
```
La propriété State est la valeur importante qui contrôle la décision pour afficher ou masquer votre extension dans la liste des outils.  Les valeurs autorisées sont :

| Value | Description |
| ---- | ----------- |
| Disponible | L’extension doit être affichée dans la liste des outils. |
| NotSupported | L’extension ne doit pas être affichée dans la liste des outils. |
| NotConfigured | Il s’agit d’une valeur d’espace réservé pour les travaux ultérieurs qui invitent l’utilisateur pour une configuration supplémentaire avant que l’outil est mis à disposition.  Actuellement, cette valeur entraîne l’outil d’affichage et est l’équivalent fonctionnel sur « Disponible ». |

Par exemple, si nous voulons un outil pour charger uniquement si le serveur distant a BitLocker installé, le script ressemble à ceci :

``` ps
$response = @{
    State = 'NotSupported';
    Message = 'Not executed';
    Properties = @{ Name = 'Prop1'; Value = 'prop1 data'; Type = 'string' },
        @{Name='Prop2'; Value = 12345678; Type='number'; };
}

if (Get-Module -ListAvailable -Name servermanager) {
    Import-module servermanager; 
    $isInstalled = (Get-WindowsFeature -name bitlocker).Installed;
    $isGood = $isInstalled;
}

if($isGood) {
    $response.State = 'Available';
    $response.Message = 'Everything should work.';
}

$response
```

Une configuration de point d’entrée à l’aide de l’option de script ressemble à ceci :

``` json
"entryPoints": [
{
    "entryPointType": "tool",
    "name": "main",
    "urlName": "processes",
    "displayName": "resources:strings:displayName",
    "description": "resources:strings:description",
    "icon": "sme-icon:icon-win-serverProcesses",
    "path": "",
    "requirements": [
    {
        "solutionIds": [
        "msft.sme.server-manager!windowsClients"
        ],
        "connectionTypes": [
        "msft.sme.connection-type.windows-client"
        ],
        "conditions": [
        {
            "localhost": true,
            "inventory": {
            "operatingSystemVersion": {
                "type": "version",
                "operator": "eq",
                "value": "10.0.*"
            },
            "operatingSystemSKU": {
                "type": "number",
                "operator": "eq",
                "value": "4"
            }
            },
            "script": "$response = @{ State = 'NotSupported'; Message = 'Not executed'; Properties = @{ Name = 'Prop1'; Value = 'prop1 data'; Type = 'string' }, @{Name='Prop2'; Value = 12345678; Type='number'; }; }; if (Get-Module -ListAvailable -Name servermanager) { Import-module servermanager; $isInstalled = (Get-WindowsFeature -name bitlocker).Installed; $isGood = $isInstalled; }; if($isGood) { $response.State = 'Available'; $response.Message = 'Everything should work.'; }; $response"
        }
        ]
    }
    ]
}
```

## <a name="supporting-multiple-requirement-sets"></a>Prise en charge de plusieurs jeux d’exigence ##

Vous pouvez utiliser plus d’un ensemble d’exigences pour déterminer le moment afficher votre outil en définissant plusieurs blocs de « configuration requise ».

Par exemple, pour afficher votre outil si « scénario A » ou « scénario B » est vraie, définir deux blocs d’exigences ; Si une est vraie (autrement dit, toutes les conditions dans un bloc d’exigences sont remplies), l’outil s’affiche.

``` json
"entryPoints": [
{
    "requirements": [
    {
        "solutionIds": [
            …"scenario A"…
        ],
        "connectionTypes": [
            …"scenario A"…
        ],
        "conditions": [
            …"scenario A"…
        ]
    },
    {
        "solutionIds": [
            …"scenario B"…
        ],
        "connectionTypes": [
            …"scenario B"…
        ],
        "conditions": [
            …"scenario B"…
        ]
    }
    ]
}

```

## <a name="supporting-condition-ranges"></a>Prise en charge des plages de condition ##

Vous pouvez également définir un éventail de conditions en définissant plusieurs blocs de « conditions » avec la même propriété, mais avec différents opérateurs.

Lors de la même propriété est définie avec différents opérateurs, l’outil s’affiche tant que la valeur est comprise entre les deux conditions.

Par exemple, cet outil s’affiche tant que le système d’exploitation est une version entre 6.3.0 et 10.0.0 :

``` json
"entryPoints": [
{
    "entryPointType": "tool",
    "name": "main",
    "urlName": "processes",
    "displayName": "resources:strings:displayName",
    "description": "resources:strings:description",
    "icon": "sme-icon:icon-win-serverProcesses",
    "path": "",
    "requirements": [
    {
        "solutionIds": [
             "msft.sme.server-manager!servers"
        ],
        "connectionTypes": [
             "msft.sme.connection-type.server"
        ],
        "conditions": [
        {
            "inventory": {
                "operatingSystemVersion": {
                    "type": "version",
                    "operator": "gt",
                    "value": "6.3.0"
                },
            }
        },
        {
            "inventory": {
                "operatingSystemVersion": {
                    "type": "version",
                    "operator": "lt",
                    "value": "10.0.0"
                }
            }
        }
        ]
    }
    ]
}

```
