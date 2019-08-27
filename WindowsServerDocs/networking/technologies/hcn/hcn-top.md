---
title: API du service HCN (Host Compute Network) pour les machines virtuelles et les conteneurs
description: L’API de service HCN (Host Compute Network) est une API Win32 publique qui fournit un accès au niveau de la plateforme pour gérer les réseaux virtuels, les points de terminaison de réseau virtuel et les stratégies associées. Cela permet de connecter et de sécuriser les machines virtuelles et les conteneurs s’exécutant sur un hôte Windows.
ms.author: jmesser
author: jmesser81
ms.date: 11/05/2018
ms.openlocfilehash: e30a778d661fa7c6d2e248234218eb25fba007a1
ms.sourcegitcommit: 213989f29cc0c30a39a78573bd4396128a59e729
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/26/2019
ms.locfileid: "70031554"
---
# <a name="host-compute-network-hcn-service-api-for-vms-and-containers"></a>API du service HCN (Host Compute Network) pour les machines virtuelles et les conteneurs

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2019

L’API de service HCN (Host Compute Network) est une API Win32 publique qui fournit un accès au niveau de la plateforme pour gérer les réseaux virtuels, les points de terminaison de réseau virtuel et les stratégies associées. Cela permet de connecter et de sécuriser les machines virtuelles et les conteneurs s’exécutant sur un hôte Windows. 

Les développeurs utilisent l’API de service HCN pour gérer la mise en réseau des machines virtuelles et des conteneurs dans leurs workflows d’application. L’API HCN a été conçue pour offrir la meilleure expérience aux développeurs. Les utilisateurs finaux n’interagissent pas directement avec ces API.  

## <a name="features-of-the-hcn-service-api"></a>Fonctionnalités de l’API du service HCN
-   Implémenté en tant qu’API C hébergée par le service de réseau hôte (HNS) sur la machine virtuelle OnCore/.

-   Offre la possibilité de créer, modifier, supprimer et énumérer des objets HCN tels que des réseaux, des points de terminaison, des espaces de noms et des stratégies. Les opérations s’exécutent sur des handles des objets (par exemple, un handle de réseau), et en interne, ces handles sont implémentés à l’aide de handles de contexte RPC.

-   Basé sur un schéma. La plupart des fonctions de l’API définissent les paramètres d’entrée et de sortie sous forme de chaînes contenant les arguments de l’appel de fonction en tant que documents JSON. Les documents JSON sont basés sur des schémas fortement typés et avec version, qui font partie de la documentation publique. 

-   Une API d’abonnement/de rappel est fournie pour permettre aux clients de s’inscrire pour les notifications d’événements à l’ensemble du service, tels que les créations et les suppressions de réseau.

-   L’API HCN fonctionne dans le pont Desktop (également appelé Centennial) qui s’exécutent dans les services système. L’API vérifie la liste de contrôle d’accès en extrayant le jeton d’utilisateur de l’appelant.

>[!TIP]
>L’API de service HCN est prise en charge dans les tâches en arrière-plan et les fenêtres non au premier plan. 

## <a name="terminology-host-vs-compute"></a>Terminologie&nbsp;: Hôte et Calcul
Le service de calcul hôte permet aux appelants de créer et de gérer des ordinateurs virtuels et des conteneurs sur un seul ordinateur physique. Elle est nommée pour suivre la terminologie du secteur. 

- L' **hôte** est largement utilisé dans le secteur de la virtualisation pour faire référence au système d’exploitation qui fournit des ressources virtualisées.

- **Compute** est utilisé pour faire référence aux méthodes de virtualisation qui sont plus larges que les machines virtuelles. Le service réseau de calcul hôte permet aux appelants de créer et de gérer la mise en réseau pour les ordinateurs virtuels et les conteneurs sur un seul ordinateur physique.

## <a name="schema-based-configuration-documents"></a>Documents de configuration basés sur un schéma
Les documents de configuration basés sur des schémas bien définis sont une norme industrielle établie dans l’espace de virtualisation. La plupart des solutions de virtualisation, telles que docker et Kubernetes, fournissent des API basées sur les documents de configuration. Plusieurs initiatives du secteur, avec la participation de Microsoft, pilotent un écosystème pour la définition et la validation de ces schémas, tels que [openapi](https://www.openapis.org/).  Ces initiatives pilotent également la normalisation de définitions de schéma spécifiques pour les schémas utilisés pour les conteneurs, tels que [Open Container Initiative (OCI)](https://www.opencontainers.org/).

La langue utilisée pour la création de documents de configuration est [JSON](https://tools.ietf.org/html/rfc8259), que vous utilisez conjointement à:
-   Définitions de schéma qui définissent un modèle objet pour le document
-   Validation du fait qu’un document JSON est conforme à un schéma
-   Conversion automatisée de documents JSON vers et à partir de représentations natives de ces schémas dans les langages de programmation utilisés par les appelants des API 

Les définitions de schéma fréquemment utilisées sont [openapi](https://www.openapis.org/) et le [schéma JSON](http://json-schema.org/), ce qui vous permet de spécifier les définitions détaillées des propriétés dans un document, par exemple:
-   Jeu de valeurs valide pour une propriété, tel que 0-100 pour une propriété représentant un pourcentage.
-   Définition des énumérations, qui sont représentées sous la forme d’un ensemble de chaînes valides pour une propriété.
-   Expression régulière pour le format attendu d’une chaîne. 

Dans le cadre de la documentation des API HCN, nous envisageons de publier le schéma de nos documents JSON en tant que spécification OpenAPI. En fonction de cette spécification, les représentations spécifiques au langage du schéma peuvent permettre l’utilisation de type sécurisé des objets de schéma dans le langage de programmation utilisé par le client. 

### <a name="example"></a>Exemple 

Voici un exemple de ce flux de travail pour l’objet représentant un contrôleur SCSI dans le document de configuration d’une machine virtuelle. 

Dans le code source Windows, nous définissons des schémas à l’aide de fichiers. mars: onecore/VM/DV/net/HNS/Schema/mars/Schema/HCN. Schema. Network. mars

```
enum IpamType
{
    [NewIn("2.0")] Static,
    [NewIn("2.0")] Dhcp,
};

class Ipam
{
    // Type : dhcp
    [NewIn("2.0"),OmitEmpty] IpamType   Type;
    [NewIn("2.0"),OmitEmpty] Subnet     Subnets[];
};

class Subnet : HCN.Schema.Common.Base
{
    [NewIn("2.0"),OmitEmpty] string         IpAddressPrefix;
    [NewIn("2.0"),OmitEmpty] SubnetPolicy   Policies[];
    [NewIn("2.0"),OmitEmpty] Route          Routes[];
};


enum SubnetPolicyType
{
    [NewIn("2.0")] VLAN
};

class SubnetPolicy
{
    [NewIn("2.0"),OmitEmpty] SubnetPolicyType                 Type;
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Common.PolicySettings Data;
};

class PolicySettings
{
    [NewIn("2.0"),OmitEmpty]  string      Name;
};

class VlanPolicy : HCN.Schema.Common.PolicySettings 
{
    [NewIn("2.0")] uint32 IsolationId;
};

class Route 
{
    [NewIn("2.0"),OmitEmpty] string NextHop;
    [NewIn("2.0"),OmitEmpty] string DestinationPrefix;
    [NewIn("2.0"),OmitEmpty] uint16 Metric;
};

```

>[!TIP]
>Les annotations [NewIn ("2.0") font partie de la prise en charge du contrôle de version pour les définitions de schéma.

À partir de cette définition interne, nous générons les spécifications OpenAPI pour le schéma:

```
{ 
    "swagger" : "2.0", 
    "info" : { 
       "version" : "2.1", 
       "title" : "HCN API" 
    },
    "definitions": {        
        "Ipam": {
            "type": "object",
            "properties": {
                "Type": {
                    "type": "string",
                    "enum": [
                        "Static",
                        "Dhcp"
                    ],
                    "description": " Type : dhcp"
                },
                "Subnets": {
                    "type": "array",
                    "items": {
                        "$ref": "#/definitions/Subnet"
                    }
                }
            }
        },
        "Subnet": {
            "type": "object",
            "properties": {
                "ID": {
                    "type": "string",
                    "pattern": "^[0-9A-Fa-f]{8}-([0-9A-Fa-f]{4}-){3}[0-9A-Fa-f]{12}$"
                },                
                "IpAddressPrefix": {
                    "type": "string"
                },
                "Policies": {
                    "type": "array",
                    "items": {
                        "$ref": "#/definitions/SubnetPolicy"
                    }
                },
                "Routes": {
                    "type": "array",
                    "items": {
                        "$ref": "#/definitions/Route"
                    }
                }
            }
        },
        "SubnetPolicy": {
            "type": "object",
            "properties": {
                "Type": {
                    "type": "string",
                    "enum": [
                        "VLAN",
                        "VSID"
                    ]
                },
                "Data": {
                    "$ref": "#/definitions/PolicySettings"
                }
            }
        },
        "PolicySettings": {
            "type": "object",
            "properties": {
                "Name": {
                    "type": "string"
                }
            }
        },                      
        "VlanPolicy": {
            "type": "object",
            "properties": {
                "Name": {
                    "type": "string"
                },
                "IsolationId": {
                    "type": "integer",
                    "format": "uint32"
                }
            }
        },
        "Route": {
            "type": "object",
            "properties": {
                "NextHop": {
                    "type": "string"
                },
                "DestinationPrefix": {
                    "type": "string"
                },
                "Metric": {
                    "type": "integer",
                    "format": "uint16"
                }
            }
        }        
    }
} 
```

Vous pouvez utiliser des outils, tels que [Swagger](https://swagger.io/), pour générer des représentations spécifiques au langage du langage de programmation de schéma utilisé par un client. Swagger prend en charge un large éventail de C#langages tels que, Go, JavaScript et Python).

- [Exemple de code C# généré](example-c-sharp.md) pour l’objet sous-réseau & IPAM de niveau supérieur.

- [Exemple de code Go généré](example-go.md) pour l’objet sous-réseau & IPAM de niveau supérieur. Go est utilisé par docker et Kubernetes, qui sont deux des consommateurs des API de service réseau de calcul de l’hôte. Go offre une prise en charge intégrée pour le marshaling des types Go vers et à partir de documents JSON.

En plus de la génération et de la validation de code, vous pouvez utiliser des outils pour simplifier le travail avec des documents JSON, autrement dit, [Visual Studio code](https://code.visualstudio.com/Docs/languages/json).

### <a name="top-level-objects-defined-in-the-hcnschemasmars-file"></a>Objets de niveau supérieur définis dans HCN. Fichier schemas. mars
Comme indiqué ci-dessus, vous trouverez le schéma de document pour les documents utilisés par les API HCN dans un ensemble de fichiers. mars sous: onecore/VM/DV/net/HNS/Schema/mars/Schema

Les objets de niveau supérieur sont les suivants:
- [HostComputeNetwork](hcn-scenarios.md#scenario-hcn)
- [HostComputeEndpoint](hcn-scenarios.md#scenario-hcn-endpoint)
- [HostComputeNamespace](hcn-scenarios.md#scenario-hcn-namespace)
- [HostComputeLoadBalancer](hcn-scenarios.md#scenario-hcn-load-balancer)

```
class HostComputeNetwork : HCN.Schema.Common.Base
{
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Network.NetworkMode          Type;
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Network.NetworkPolicy        Policies[];
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Network.MacPool              MacPool;
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Network.DNS                  Dns;
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Network.Ipam                 Ipams[];
};

class HostComputeEndpoint : HCN.Schema.Common.Base
{
    [NewIn("2.0"),OmitEmpty] string                                     HostComputeNetwork;
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Network.Endpoint.EndpointPolicy Policies[];
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Network.Endpoint.IpConfig       IpConfigurations[];
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Network.DNS                     Dns;
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Network.Route                   Routes[];
    [NewIn("2.0"),OmitEmpty] string                                     MacAddress;
};

class HostComputeNamespace : HCN.Schema.Common.Base
{
    [NewIn("2.0"),OmitEmpty] uint32                                    NamespaceId;
    [NewIn("2.0"),OmitEmpty] Guid                                      NamespaceGuid;
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Namespace.NamespaceType        Type;
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Namespace.NamespaceResource    Resources[];
};

class HostComputeLoadBalancer : HCN.Schema.Common.Base
{
    [NewIn("2.0"), OmitEmpty] string                                               HostComputeEndpoints[]; 
    [NewIn("2.0"), OmitEmpty] string                                               VirtualIPs[]; 
    [NewIn("2.0"), OmitEmpty] HCN.Schema.Network.Endpoint.Policy.PortMappingPolicy PortMappings[]; 
    [NewIn("2.0"), OmitEmpty] HCN.Schema.LoadBalancer.LoadBalancerPolicy           Policies[];
};
```

## <a name="next-steps"></a>Étapes suivantes

- En savoir plus sur les [scénarios de HCN courants](hcn-scenarios.md).

- En savoir plus sur les descripteurs [de contexte RPC pour HCN](hcn-declaration-handles.md).

- En savoir plus sur les [schémas de document JSON HCN](hcn-json-document-schemas.md).