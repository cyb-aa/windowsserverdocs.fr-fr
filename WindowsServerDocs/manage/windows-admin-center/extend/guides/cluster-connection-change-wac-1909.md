---
title: Modifications du type de connexion au cluster dans le centre d’administration Windows v1909
description: Modifications du type de connexion au cluster dans le centre d’administration Windows v1909
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 10/01/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: a07b30517f0d45b7e6f4f41f0ef9a6549e6e2117
ms.sourcegitcommit: de71970be7d81b95610a0977c12d456c3917c331
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/04/2019
ms.locfileid: "71952770"
---
# <a name="cluster-connection-type-changes-in-windows-admin-center-v1909"></a>Modifications du type de connexion au cluster dans le centre d’administration Windows v1909

>S'applique à : Windows Admin Center, Windows Admin Center Preview

> [!IMPORTANT]
> Ce document décrit les modifications requises par les développeurs d’extensions du centre d’administration Windows qui développent des outils du centre d’administration Windows pour le cluster de basculement et des solutions HCI (Hyper-Converged cluster). Il s’agit d’une modification obligatoire requise pour que votre extension soit compatible avec le centre d’administration Windows v1909 version préliminaire et les futures versions GA.

Dans le centre d’administration Windows v1909, nous avons unifié les deux types de connexion de cluster différents (cluster de basculement et connexions de cluster hyper-convergé) en un type de connexion de cluster unique. Les utilisateurs n’ont plus besoin d’identifier la configuration d’un cluster à l’avance pour décider du type de connexion à ajouter au cluster, ni d’ajouter deux fois le cluster en tant que types de connexion différents pour accéder aux différents ensembles d’outils. Les clusters peuvent désormais être ajoutés en tant que « cluster Windows Server » et les outils appropriés sont chargés, principalement selon que espaces de stockage direct est activé ou non.

Dans la mesure où cela nécessitait une modification de la définition du type de connexion et de la manière dont les outils liés au cluster décident de la date du chargement, les extensions qui fournissent des outils pour les clusters (les clusters HCI ou non-HCI, ou les deux) requièrent les modifications de l’implémentation comme décrit dessus.

## <a name="manifestjson---solutionsids-and-connectiontypes"></a>Manifest. JSON-solutionsIds et connectionTypes

Auparavant, pour que votre outil soit affiché pour un cluster de basculement ou un type de connexion de cluster HCI, vous auriez utilisé l’une des définitions suivantes dans le fichier ```manifest.json```.

Pour les clusters de basculement :
``` json
    {
        "entryPointType": "tool",
        "name": "MyToolName",
        "urlName": "MyToolUrl",
        "displayName": "MyToolDisplayName",
        "description": "MyToolDescription",
        "icon": "MyToolIcon",
        "path": "MyToolPath",
        "iframeId": "MyToolIframeId",
        "requirements": [
        {
            "solutionIds": [
                "msft.sme.failover-cluster!failover-cluster"
            ],
            "connectionTypes": [
                "msft.sme.connection-type.cluster"
            ]
        }
        ]
    }
```

Pour les clusters HCI, la section en surbrillance ci-dessus aurait été remplacée par ce qui suit :
``` json
    "requirements": [
    {
        "solutionIds": [
            "msft.sme.software-defined-data-center!SDDC"
        ],
        "connectionTypes": [
            "msft.sme.connection-type.hyper-converged-cluster"
        ]
    }
    ]
```

Dans le centre d’administration Windows 1909 et versions ultérieures, les deux solutionIds et connectionTypes ont été remplacés par les éléments suivants :
``` json
    "requirements": [
    {
        "solutionIds": [
            "msft.sme.software-defined-data-center!cluster"
        ],
        "connectionTypes": [
            "msft.sme.connection-type.cluster"
        ]
    }
    ]
```

Il s’agit du seul type solutionIds et connectionTypes de cluster pris en charge à partir de maintenant. Si votre outil est défini uniquement avec ce type solutionIds et connectionTypes, il sera chargé pour toute connexion de cluster de basculement, qu’il s’agisse ou non d’un cluster HCI. Si vous souhaitez limiter votre outil pour qu’il ne soit disponible que pour les clusters HCI ou non-HCI, vous devez en outre utiliser les nouvelles propriétés d’inventaire décrites dans la section suivante.

## <a name="manifestjson--inventory-properties"></a>Manifest. JSON – propriétés d’inventaire
Lors de la connexion à un serveur ou à un cluster, le centre d’administration Windows interroge un ensemble de propriétés d’inventaire que vous pouvez utiliser pour déterminer si votre outil doit être disponible ou non (voir la section « Propriétés de l’inventaire » dans le [contrôle de l’outil ](dynamic-tool-display.md)document de visibilité pour plus d’informations). Dans le centre d’administration Windows v1909, nous avons ajouté deux nouvelles propriétés à cette liste qui peuvent être utilisées pour déterminer si un cluster est un cluster hyper-convergé. 

### <a name="iss2denabled"></a>isS2dEnabled
Techniquement, un cluster hyper-convergé est défini en tant que cluster de basculement avec espaces de stockage direct (S2D) activé. Si vous souhaitez que votre outil soit disponible uniquement pour les clusters hyper-convergents, par exemple, lorsque S2D est activé, ajoutez la condition d’inventaire suivante :
``` json
    "requirements": [
    {
        "solutionIds": [
            "msft.sme.software-defined-data-center!cluster"
        ],
        "connectionTypes": [
            "msft.sme.connection-type.cluster"
        ],
        "conditions": [
        {
            "inventory": {
                "isS2dEnabled": {
                    "type": "boolean",
                    "operator": "is"
                }
            }
        }
        ]
    }
    ]
```
> [!TIP] 
> Si vous souhaitez que votre outil soit disponible uniquement lorsque S2D n’est pas activé, affectez la valeur « Not » à « opérateur ».

### <a name="isbritannicaenabled"></a>isBritannicaEnabled
En outre, si vous êtes dépendant de la ressource de cluster de gestion SDDC et que vous utilisez le modèle objet SDDC, vous pouvez vérifier la condition suivante :
``` json
    "conditions": [
        {
            "inventory": {
                "isS2dEnabled": {
                    "type": "boolean",
                    "operator": "is"
                },
                "isBritannicaEnabled": {
                    "type": "boolean",
                    "operator": "is"
                }
            }
        }
    ]
```

## <a name="backward-compatibility-to-support-previous-versions-of-windows-admin-center"></a>Compatibilité descendante pour prendre en charge les versions précédentes du centre d’administration Windows
Pour vous assurer que votre extension continue de fonctionner avec des versions antérieures du centre d’administration Windows, telles que la version v1904 GA, vous pouvez utiliser la définition solutionIds et connectionTypes précédente avec la nouvelle définition. Consultez l’exemple ci-dessous pour afficher votre outil uniquement pour les clusters HCI dans toutes les versions du centre d’administration Windows.
``` json
    "requirements": [
    {
        "solutionIds": [
            "msft.sme.software-defined-data-center!SDDC"
        ],
        "connectionTypes": [
            "msft.sme.connection-type.hyper-converged-cluster"
        ]
    },
    {
        "solutionIds": [
            "msft.sme.software-defined-data-center!SDDC",
            "msft.sme.software-defined-data-center!cluster"
        ],
        "connectionTypes": [
            "msft.sme.connection-type.hyper-converged-cluster",
            "msft.sme.connection-type.cluster"
        ],
        "conditions": [
            {
                "inventory": {
                    "isS2dEnabled": {
                        "type": "boolean",
                        "operator": "is"
                    }
                }
            }
        ]
    }
    ]
```

## <a name="known-issue-appcontextserviceactiveconnectionishyperconvergedclusterisfailovercluster-is-not-set-properly-in-windows-admin-center-v1909"></a>Problème connu : AppContextService. activeConnection. isHyperConvergedCluster/isFailoverCluster n’est pas défini correctement dans le centre d’administration Windows v1909
Une régression des modifications récentes est que les propriétés ```AppContextService.activeConnection.isHyperConvergedCluster/isFailoverCluster``` ne sont pas définies correctement dans le centre d’administration Windows v1909 et qu’elles ont toujours la valeur false. Ce problème sera résolu dans la prochaine version, V1910, mais sera également déconseillé et n’est plus disponible dans la version GA suivante dans 2020. À l’avenir, vous pouvez remplacer ceci par le code ci-dessous et utiliser ```this.connectHCI```.
```
    import { ClusterInventoryCache } from '@msft-sme/core';

    constructor(
        ...
        private appContextService: AppContextService,
        ...
    ) {
        ...
        this.clusterInventoryCache = new ClusterInventoryCache(
            this.appContextService,
            <SharedCacheOptions>{ expiration: Constants.sharedCacheExpiration }
        );

         this.isBritannicEnabled().subscribe(result => this.connectHCI = result);
    }

    private isBritannicEnabled(): Observable<boolean> {
        return this.clusterInventoryCache.query(this.getClusterInventoryQueryParameters())
        .pipe(
            map(inventory => {
                return inventory.instance.isBritannicaEnabled;
        }));
    }

    private getClusterInventoryQueryParameters(): ClusterInventoryParams {
        const nodeName = this.appContextService.activeConnection.validNodeName;
        const endpoint = this.appContextService.authorizationManager.getJeaEndpoint(nodeName);
        const options = { powerShellEndpoint: endpoint };
        return {
            name: nodeName,
            requestOptions: options
        };
    }
```