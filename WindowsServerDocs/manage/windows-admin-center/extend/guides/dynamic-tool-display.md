---
title: Contrôler la visibilité de votre outil dans une solution
description: Contrôler la visibilité de votre outil dans une solution Kit de développement logiciel (SDK) du centre d’administration Windows (projet Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 440ba3d11da671beedc2c2fb90caa3e176f83877
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385319"
---
# <a name="control-your-tools-visibility-in-a-solution"></a>Contrôler la visibilité de votre outil dans une solution #

>S'applique à : Windows Admin Center, Windows Admin Center Preview

Il peut arriver que vous souhaitiez exclure (ou masquer) votre extension ou outil dans la liste des outils disponibles. Par exemple, si votre outil cible uniquement Windows Server 2016 (pas les versions antérieures), il est possible que vous ne souhaitiez pas qu’un utilisateur se connecte à un serveur Windows Server 2012 R2 pour voir votre outil. (Imaginez l’expérience de l’utilisateur : cliquez dessus, attendez que l’outil se charge, uniquement pour obtenir un message indiquant que ses fonctionnalités ne sont pas disponibles pour la connexion.) Vous pouvez définir quand afficher (ou masquer) votre fonctionnalité dans le fichier manifest. JSON de l’outil.

## <a name="options-for-deciding-when-to-show-a-tool"></a>Options pour décider quand afficher un outil ##

Vous pouvez utiliser trois options différentes pour déterminer si votre outil doit être affiché et disponible pour une connexion de serveur ou de cluster spécifique.

* localhost
* Inventory (un tableau de propriétés)
* script

### <a name="localhost"></a>LocalHost ###

La propriété localHost de l’objet conditions contient une valeur booléenne qui peut être évaluée pour déduire si le nœud de connexion est localHost (l’ordinateur sur lequel le centre d’administration Windows est installé) ou non. En passant une valeur à la propriété, vous indiquez quand (la condition) afficher l’outil. Par exemple, si vous souhaitez que l’outil s’affiche uniquement si l’utilisateur se connecte en fait à l’hôte local, configurez-le comme suit :

``` json
"conditions": [
{
    "localhost": true
}]
```

Sinon, si vous souhaitez que votre outil s’affiche uniquement lorsque le nœud de connexion *n’est pas* localhost :

``` json
"conditions": [
{
    "localhost": false
}]
```

Voici à quoi ressemblent les paramètres de configuration pour afficher uniquement un outil lorsque le nœud de connexion n’est pas localhost :

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

Le kit de développement logiciel (SDK) comprend un ensemble préconfiguré de propriétés d’inventaire que vous pouvez utiliser pour créer des conditions afin de déterminer si votre outil doit être disponible ou non. Il existe neuf propriétés différentes dans le tableau « Inventory » :

| Nom de la propriété | Type de valeur ATTENDU |
| ------------- | ------------------- |
| computerManufacturer | chaîne |
| operatingSystemSKU | nombre |
| operatingSystemVersion renvoyée | version_string (par exemple : « 10,1. * ») |
| productType | nombre |
| clusterFqdn | chaîne |
| isHyperVRoleInstalled | booléenne |
| isHyperVPowershellInstalled | booléenne |
| isManagementToolsAvailable | booléenne |
| isWmfInstalled | booléenne |

Chaque objet dans le tableau d’inventaire doit se conformer à la structure JSON suivante :

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
| TB | Supérieur à |
| & | Supérieur ou égal à |
| terme | Inférieur à |
| WinINSTALL | Inférieur ou égal à |
| EQ | Égal à |
| ne | Différent de |
| est | vérification de la présence d’une valeur true |
| not | vérification de la présence d’une valeur false |
| comprend | l’élément existe dans une chaîne |
| notContains | l’élément n’existe pas dans une chaîne |

#### <a name="data-types"></a>Types de données ####

Options disponibles pour la propriété’type' :

| Type | Description |
| ---- | ----------- |
| version | un numéro de version (par exemple : 10,1. *) |
| nombre | Valeur numérique |
| chaîne | valeur de chaîne |
| booléenne | true ou false |

#### <a name="value-types"></a>Types valeur ####

La propriété « Value » accepte ces types :

* chaîne
* nombre
* booléenne

Un jeu de conditions d’inventaire correctement formé se présente comme suit :

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

Enfin, vous pouvez exécuter un script PowerShell personnalisé pour identifier la disponibilité et l’état du nœud. Tous les scripts doivent retourner un objet avec la structure suivante :

``` ps
@{
    State = 'Available' | 'NotSupported' | 'NotConfigured';
    Message = '<Message to explain the reason of state such as not supported and not configured.>';
    Properties =
        @{ Name = 'Prop1'; Value = 'prop1 data'; Type = 'string' },
        @{Name='Prop2'; Value = 12345678; Type='number'; };
}
```
La propriété State est la valeur importante qui contrôlera la décision d’afficher ou de masquer votre extension dans la liste des outils.  Les valeurs autorisées sont les suivantes:

| Value | Description |
| ---- | ----------- |
| Disponible | L’extension doit être affichée dans la liste des outils. |
| NotSupported | L’extension ne doit pas être affichée dans la liste des outils. |
| NotConfigured | Il s’agit d’une valeur d’espace réservé pour un travail futur qui invite l’utilisateur à effectuer une configuration supplémentaire avant que l’outil ne soit mis à disposition.  Actuellement, cette valeur entraîne l’affichage de l’outil et est l’équivalent fonctionnel de « available ». |

Par exemple, si vous souhaitez qu’un outil se charge uniquement si BitLocker est installé sur le serveur distant, le script ressemble à ceci :

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

## <a name="supporting-multiple-requirement-sets"></a>Prise en charge de plusieurs ensembles de spécifications ##

Vous pouvez utiliser plusieurs ensembles d’exigences pour déterminer quand afficher votre outil en définissant plusieurs blocs « Requirements ».

Par exemple, pour afficher votre outil si « scénario A » ou « scénario B » a la valeur true, définissez deux blocs de spécifications. Si l’une ou l’autre est vraie (autrement dit, toutes les conditions d’un bloc d’exigences sont remplies), l’outil est affiché.

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

## <a name="supporting-condition-ranges"></a>Plages de conditions de prise en charge ##

Vous pouvez également définir une plage de conditions en définissant plusieurs blocs « conditions » avec la même propriété, mais avec des opérateurs différents.

Quand la même propriété est définie avec différents opérateurs, l’outil s’affiche tant que la valeur est comprise entre les deux conditions.

Par exemple, cet outil s’affiche tant que le système d’exploitation est une version comprise entre 6.3.0 et 10.0.0 :

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
