---
title: Héberger des API du service de réseau de calcul (HCN) pour les machines virtuelles et conteneurs
description: Hôte API de service de réseau de calcul (HCN) est une API Win32 destinées au public qui fournit l’accès au niveau de la plateforme pour gérer les réseaux virtuels, les points de terminaison de réseau virtuel et les stratégies associées. Ensemble cela fournit une connectivité et sécurité pour les machines virtuelles (VM) et les conteneurs en cours d’exécution sur un ordinateur hôte Windows.
ms.author: jmesser
author: jmesser81
ms.date: 11/05/2018
ms.openlocfilehash: 50af0dab69633aa6e07ded68e9246aa0315377f0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844980"
---
# <a name="host-compute-network-hcn-service-api-for-vms-and-containers"></a>Héberger des API du service de réseau de calcul (HCN) pour les machines virtuelles et conteneurs

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Hôte API de service de réseau de calcul (HCN) est une API Win32 destinées au public qui fournit l’accès au niveau de la plateforme pour gérer les réseaux virtuels, les points de terminaison de réseau virtuel et les stratégies associées. Ensemble cela fournit une connectivité et sécurité pour les machines virtuelles (VM) et les conteneurs en cours d’exécution sur un ordinateur hôte Windows. 

Les développeurs utilisent l’API de service HCN pour gérer la mise en réseau pour les machines virtuelles et conteneurs dans leurs flux de travail d’application. L’API HCN a été conçu pour offrir la meilleure expérience pour les développeurs. Les utilisateurs finaux n’interagissent pas directement avec ces API.  

## <a name="features-of-the-hcn-service-api"></a>Fonctionnalités de l’API de Service HCN
-   Implémenté en tant qu’API C hébergé par le Service de réseau de l’hôte (HNS) sur la OnCore/machine virtuelle.

-   Fournit la possibilité de créer, modifier, supprimer et énumérer des objets HCN telles que les réseaux, les points de terminaison, les espaces de noms et les stratégies. Effectuent des opérations sur les handles aux objets (par exemple, un handle de réseau) et en interne ces poignées sont implémentées à l’aide de handles de contexte RPC.

-   Basée sur le schéma. La plupart des fonctions de l’API de définir l’entrée et les paramètres sous forme de chaînes contenant les arguments de l’appel de fonction en tant que documents JSON de sortie. Les documents JSON sont basées sur les schémas fortement typés et avec contrôle de version, ces schémas font partie de la documentation publique. 

-   Un abonnement/du rappel API est fourni pour permettre aux clients de s’inscrire aux notifications d’événements de service à l’échelle tels que les créations de réseau et les suppressions.

-   API de HCN fonctionne dans le pont du bureau (également appelé) Applications de Centennial) qui s’exécutent dans les services système. L’API vérifie la liste ACL en récupérant le jeton d’utilisateur à partir de l’appelant.

>[!TIP]
>L’API de service HCN est pris en charge dans les tâches en arrière-plan et non pas au premier plan windows. 

## <a name="terminology-host-vs-compute"></a>Terminologie&nbsp;: Hôte vs. Calcul :
Le service de calcul hôte permet aux appelants de créer et gérer des machines virtuelles et conteneurs sur un seul ordinateur physique. Il est nommé à suivre la terminologie du secteur. 

- **Hôte** est largement utilisé dans le secteur de la virtualisation pour faire référence au système d’exploitation qui fournit des ressources virtualisées.

- **Calcul** est utilisé pour faire référence aux méthodes de virtualisation des machines virtuelles juste plus étendu. Service de réseau hôte de calcul permet aux appelants de créer et gérer la mise en réseau pour les machines virtuelles et conteneurs sur un seul ordinateur physique.

## <a name="schema-based-configuration-documents"></a>Documents de configuration basée sur le schéma
Documents de configuration en fonction des schémas clairement définis est une norme du secteur établi dans l’espace de la virtualisation. La plupart des solutions de virtualisation, telles que Docker et Kubernetes, fournissent des Qu'api basées sur des documents de configuration. Plusieurs initiatives du secteur, avec la participation de Microsoft, un écosystème pour définir et valider ces schémas, tels que le lecteur [OpenAPI](https://www.openapis.org/).  Ces initiatives lecteur également la normalisation des définitions de schéma spécifique pour les schémas utilisés pour les conteneurs, tels que [Open Container Initiative (OCI)](https://www.opencontainers.org/).

Le langage utilisé pour la création de documents de configuration est [JSON](https://tools.ietf.org/html/rfc8259), que vous utilisez en combinaison avec :
-   Définitions de schéma qui définissent un modèle d’objet pour le document
-   Validation d’indique si un document JSON est conforme à un schéma
-   La conversion automatisée de documents JSON vers et à partir de représentations natives de ces schémas dans les langages de programmation utilisés par les appelants de l’API 

Définitions de schéma fréquemment utilisées sont [OpenAPI](https://www.openapis.org/) et [schéma JSON](http://json-schema.org/), ce qui vous permet de spécifier les définitions détaillées des propriétés dans un document, par exemple :
-   Le jeu de valeurs pour une propriété, tel que 0 et 100 pour une propriété représentant un pourcentage valide.
-   La définition d’énumérations qui sont représentées sous la forme d’un ensemble de chaînes valides pour une propriété.
-   Une expression régulière pour le format attendu d’une chaîne. 

Dans le cadre de la documentation des API HCN, nous avons l’intention de publier le schéma de nos documents JSON en tant qu’une spécification OpenAPI. En fonction de cette spécification, représentations spécifiques au langage du schéma peuvent autoriser pour une utilisation de type sécurisé les objets de schéma dans le langage de programmation utilisé par le client. 

### <a name="example"></a>Exemple 

Voici un exemple de ce flux de travail pour l’objet représentant un contrôleur SCSI dans le document de configuration d’une machine virtuelle. 

Dans le code source de Windows, nous définissons les schémas à l’aide de fichiers de .mars : onecore/vm/dv/net/hns/schema/mars/Schema/HCN.Schema.Network.mars

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
>Le [NewIn("2.0") annotations font partie de la prise en charge le contrôle de version pour les définitions de schéma.

À partir de cette définition interne, nous générons les spécifications d’OpenAPI pour le schéma :

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

Vous pouvez utiliser des outils, tels que [Swagger](https://swagger.io/), pour générer des représentations spécifiques au langage du schéma de langage de programmation utilisé par un client. Swagger prend en charge une variété de langages tels que C#, Go, Javascript et Python).

- [Exemple de généré C# code](example-c-sharp.md) au IPAM de niveau supérieur et le sous-réseau de l’objet.

- [Exemple de code Go généré](example-go.md) au IPAM de niveau supérieur et le sous-réseau de l’objet. Go est utilisé par Docker et Kubernetes qui sont deux des consommateurs de l’API du Service hôte de calcul réseau. Go est prise en charge intégrée pour le marshaling des types Go vers et à partir de documents JSON.

En plus de la génération de code et de validation, vous pouvez utiliser des outils pour simplifier le travail avec des documents JSON, autrement dit, [Visual Studio Code](https://code.visualstudio.com/Docs/languages/json).

### <a name="top-level-objects-defined-in-the-hcnschemasmars-file"></a>Objets de niveau supérieur définis dans le HCN. Fichier de schemas.mars
Comme mentionné ci-dessus, vous pouvez trouver le schéma de document pour les documents utilisés par les API HCN dans un ensemble de fichiers .mars sous : onecore/machine virtuelle/dv/net/hns / / mars/schéma

Les objets de niveau supérieur sont :
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

- En savoir plus sur la [courants HCN](hcn-scenarios.md).

- En savoir plus sur la [gère de contexte RPC pour HCN](hcn-declaration-handles.md).

- En savoir plus sur la [les schémas de document HCN JSON](hcn-json-document-schemas.md).