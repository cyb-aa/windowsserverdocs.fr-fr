---
title: Contrôler la visibilité de votre outil dans une solution
description: Contrôler la visibilité de votre outil dans une solution de SDK Windows Admin Center (projet Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: f3f34b4c86854bfc55cf4b1b57a0fd3c2baf2ffc
ms.sourcegitcommit: be0144eb59daf3269bebea93cb1c467d67e2d2f1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/20/2018
ms.locfileid: "4080966"
---
# Contrôler la visibilité de votre outil dans une solution #

>S’applique à: Windows Admin Center, Windows Admin Center Preview

Il peut arriver que lorsque vous souhaitez exclure (ou masquer) votre extension ou un outil à partir de la liste des outils disponibles. Par exemple, si votre outil cible uniquement Windows Server 2016 (pas les anciennes versions), vous souhaiterez pas un utilisateur qui se connecte à un serveur Windows Server 2012 R2 pour voir votre outil du tout. (Imaginez l’expérience utilisateur: ils clique dessus, attendez que l’outil pour charger, uniquement pour obtenir un message que ses fonctionnalités ne sont pas disponibles pour leur connexion.) Vous pouvez définir le moment afficher (ou masquer) d’une fonctionnalité de votre fichier de manifest.json de l’outil.

## Options pour déterminer à quel moment afficher un outil ##

Il existe trois options différentes, que vous pouvez utiliser pour déterminer si votre outil doit être affiché et disponible pour un serveur spécifique ou d’une connexion de cluster.

* hôte local
* inventaire (il s’agit d’un tableau de propriétés)
* script

### Hôte local ###

La propriété de l’hôte local de l’objet de Conditions contient une valeur booléenne qui peut être évaluée pour déduire si le nœud qui se connecte localHost (le même ordinateur Windows Admin Center est installé sur) ou non. En passant une valeur à la propriété, vous indiquez quand (condition) pour afficher l’outil. Par exemple, si vous ne souhaitez que l’outil à afficher si l’utilisateur se connecte en fait à l’hôte local, configurez-la comme suit:

``` json
"conditions": [
{
    "localhost": true
}]
```

Par ailleurs, si vous ne souhaitez que votre outil à afficher lorsque l’hôte local *n’est pas* de nœud qui se connecte:

``` json
"conditions": [
{
    "localhost": false
}]
```

Voici ce à quoi les paramètres de configuration ressembler à afficher uniquement un outil lorsque le nœud qui se connecte n’est pas localhost:

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

### Propriétés d’inventaire ###

Le SDK inclut un ensemble de propriétés d’inventaire que vous pouvez utiliser pour créer des conditions pour déterminer quand votre outil doit être disponible ou non au préalable. Il existe des propriétés différentes neuf dans le tableau «stock»:

| Nom de propriété | Type de valeur attendue |
| ------------- | ------------------- |
| computerManufacturer | chaîne |
| operatingSystemSKU | nombre |
| operatingSystemVersion | version_string (par ex.: «10.1. *») |
| productType | nombre |
| FQDNServeur | chaîne |
| isHyperVRoleInstalled | booléen |
| isHyperVPowershellInstalled | booléen |
| isManagementToolsAvailable | booléen |
| isWmfInstalled | booléen |

Chaque objet dans le tableau de stock doit être conforme à la structure de json suivantes:

``` json
"<property name>": {
    "type": "<expected type>",
    "operator": "<defined operator to use>",
    "value": "<expected value to evaluate using the operator>"
}
```

#### Valeurs de l’opérateur ####

| Opérateur | Description |
| -------- | ----------- |
| gt | supérieure à |
| GE | supérieur ou égal à |
| lt | inférieur à |
| le | inférieur ou égal à |
| EQ | égal à |
| ne | non égal à |
| est | vérifier si une valeur est true |
| pas | vérifier si une valeur est false |
| contient | élément existe dans une chaîne |
| notContains | élément n’existe pas dans une chaîne |

#### Types de données ####

Options disponibles pour la propriété «type»:

| Type | Description |
| ---- | ----------- |
| version | un numéro de version (par ex.: 10.1. *) |
| nombre | une valeur numérique |
| chaîne | une valeur de chaîne |
| booléen | True ou false |

#### Types de valeur ####

La propriété «value» accepte les types suivants:

* chaîne
* nombre
* booléen

Un ensemble de conditions de stock correctement formé se présente comme suit:

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

### Script ###

Enfin, vous pouvez exécuter un script PowerShell personnalisé pour identifier la disponibilité et l’état du nœud. Tous les scripts doivent retourner un objet avec la structure suivante:

``` ps
@{
    State = 'Available' | 'NotSupported' | 'NotConfigured';
    Message = '<Message to explain the reason of state such as not supported and not configured.>';
    Properties =
        @{ Name = 'Prop1'; Value = 'prop1 data'; Type = 'string' },
        @{Name='Prop2'; Value = 12345678; Type='number'; };
}
```
La propriété d’état est la valeur importante afin de contrôler la décision d’afficher ou masquer votre extension dans la liste d’outils.  Les valeurs autorisées sont:
| Valeur | Description |
| ---- | ----------- |
| Disponible | L’extension doit être affichée dans la liste d’outils. |
| NotSupported | L’extension ne doit pas être affichée dans la liste d’outils. |
| NotConfigured | Il s’agit d’une valeur d’espace réservé pour les travaux ultérieurs qui invite l’utilisateur pour une configuration supplémentaire avant de l’outil est mis à disposition.  Actuellement, cette valeur se traduit par l’outil affiche et est l’équivalent à «Disponible». |

Par exemple, si nous voulons un outil pour charger uniquement si le serveur distant a BitLocker installé, le script se présente comme suit:

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

Une configuration de point d’entrée à l’aide de l’option de script se présente comme suit:

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

## Prise en charge plusieurs jeux exigence ##

Vous pouvez utiliser plus d’un ensemble d’exigences pour déterminer le moment afficher votre outil en définissant plusieurs blocs «exigences».

Par exemple, pour afficher votre outil si «scénario A» ou «scénario B» a la valeur true, définir deux exigences blocs; Si une est vraie (autrement dit, toutes les conditions dans un bloc de conditions sont remplies), l’outil s’affiche.

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

## Prise en charge des plages de condition ##

Vous pouvez également définir un éventail de conditions en définissant plusieurs blocs «conditions» avec la même propriété, mais avec des opérateurs différents.

Lors de la même propriété est définie avec des opérateurs différents, l’outil s’affiche tant que la valeur est comprise entre les deux conditions.

Par exemple, cet outil s’affiche tant que le système d’exploitation est une version entre 6.3.0 et 10.0.0:

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
