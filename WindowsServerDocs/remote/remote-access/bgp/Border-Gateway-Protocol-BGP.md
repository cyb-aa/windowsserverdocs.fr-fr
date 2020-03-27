---
title: Protocole BGP (Border Gateway Protocol)
description: Vous pouvez utiliser cette rubrique pour mieux comprendre les Border Gateway Protocol (BGP) dans Windows Server 2016, y compris les topologies de déploiement BGP prises en charge et les fonctionnalités et fonctionnalités BGP.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 78cc2ce3-a48e-45db-b402-e480b493fab1
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 3e04c732cbacb182731717215a4cf99cf3cc1f76
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80309291"
---
# <a name="border-gateway-protocol-bgp"></a>Protocole BGP (Border Gateway Protocol)

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour mieux comprendre le protocole BGP (Border Gateway Protocol), notamment les topologies de déploiement BGP prises en charge, ainsi que les fonctionnalités et capacités BGP.  
  
> [!NOTE]  
> En plus de cette rubrique, la documentation BGP suivante est disponible.  
>   
> -   [Référence à la commande Windows PowerShell BGP](../../remote-access/bgp/BGP-Windows-PowerShell-Command-Reference.md)  
  
Cette rubrique contient les sections suivantes.  
  
-   [Topologies de déploiement BGP prises en charge](#bkmk_top)  
  
-   [Fonctionnalités BGP](#bkmk_features)  
  
Lorsqu’il est configuré sur un service d’accès à distance Windows Server 2016 \(passerelle RAS\) en mode multi-locataire, Border Gateway Protocol (BGP) vous permet de gérer le routage du trafic réseau entre les réseaux d’ordinateurs virtuels de vos clients et leurs sites distants. Vous pouvez également utiliser le protocole BGP pour les déploiements de passerelle RAS à client unique et lorsque vous déployez l’accès à distance en tant que réseau local \(routeur\) LAN.  
  
Le protocole BGP réduit le besoin de configuration manuelle de routage sur les routeurs, car il s’agit d’un protocole de routage dynamique qui, de plus, découvre automatiquement les itinéraires entre les sites qui sont connectés via des connexions VPN de site à site.  
  
Pour utiliser le routage BGP, vous devez installer le **service d’accès à distance \(les\)RAS** et/ou le service de rôle **routage** du rôle serveur d’accès à distance sur un ordinateur ou un ordinateur virtuel \(machine virtuelle\)-le type de système que vous utilisez dépend de la présence ou non d’un déploiement multi-locataire :  
  
-   Pour un déploiement mutualisée, il est recommandé d’installer la passerelle RAS sur une ou plusieurs machines virtuelles. L’utilisation de plusieurs machines virtuelles offre une haute disponibilité. La passerelle RAS est capable de gérer plusieurs connexions à partir de plusieurs locataires, et se compose d’un hôte Hyper-V et d’une machine virtuelle qui est réellement configurée comme passerelle. Cette passerelle est configurée avec des connexions VPN de site à site en tant que routeur BGP multilocataires pour échanger le client et le fournisseur de services Cloud \(CSP\) des itinéraires de sous-réseau.  
  
-   Pour un déploiement de passerelle Edge à client unique ou un déploiement de routeur LAN, vous pouvez installer la passerelle RAS sur un ordinateur physique ou une machine virtuelle.  
  
> [!IMPORTANT]  
> Lorsque vous installez une passerelle RAS, vous devez spécifier si le protocole BGP est activé pour chaque locataire à l’aide de la commande Windows PowerShell **Enable-remoteaccessroutingdomain et définissez** avec la valeur de paramètre de **type** **All**. Pour installer l’accès à distance en tant que routeur LAN compatible BGP sans fonctionnalités multi-locataires, vous pouvez utiliser la commande **install-RemoteAccess-VpnType RoutingOnly**.  
>   
> L’exemple de code suivant illustre l’installation du service RAS en mode d’architecture mutualisée avec toutes les fonctionnalités RAS (VPN de point à site, VPN de site à site et routage BGP) activées pour deux locataires, contoso et fabrikam.  
  
```  
$Contoso_RoutingDomain = "ContosoTenant"  
$Fabrikam_RoutingDomain = "FabrikamTenant"  
  
Install-RemoteAccess -MultiTenancy  
  
Enable-RemoteAccessRoutingDomain -Name $Contoso_RoutingDomain -Type All -PassThru  
Enable-RemoteAccessRoutingDomain -Name $Fabrikam_RoutingDomain -Type All -PassThru  
```  
  
## <a name="bgp-supported-deployment-topologies"></a><a name="bkmk_top"></a>Topologies de déploiement BGP prises en charge  
Ci-dessous sont répertoriées les topologies de déploiement prises en charge lorsque les sites d’entreprise sont connectés au centre de données du fournisseur de services cloud (CSP).  
  
Dans tous les scénarios, la passerelle CSP est une passerelle RAS Windows Server 2016 en périphérie. La passerelle RAS, qui est capable de gérer plusieurs connexions à partir de plusieurs locataires, se compose d’un hôte Hyper-V et d’une machine virtuelle qui est réellement configurée comme passerelle. Cette passerelle de périphérie est configurée avec les connexions VPN de site à site comme un routeur BGP mutualisé pour échanger les itinéraires du sous-réseau CSP et d’entreprise.  
  
Les clients se connectent à leurs ressources du centre de données CSP à l’aide d’une connexion VPN de site à site (S2S). En outre, le protocole de routage BGP est déployé pour l’échange d’informations de routage dynamiques entre les passerelles CSP et d’entreprise.  
  
Les topologies de déploiement suivantes sont prises en charge.  
  
-   [Passerelle de site à site VPN RAS avec BGP sur le périmètre du site d’entreprise](#bkmk_top1)  
  
-   [Passerelle tierce avec BGP sur le périmètre du site d’entreprise](#bkmk_top2)  
  
-   [Plusieurs sites d’entreprise avec des passerelles tierces](#bkmk_top3)  
  
-   [Points de terminaison distincts pour le protocole BGP et le VPN](#bkmk_top4)  
  
Les sections suivantes contiennent des informations supplémentaires sur chaque topologie BGP prise en charge.  
  
### <a name="ras-vpn-site-to-site-gateway-with-bgp-at-enterprise-site-edge"></a><a name="bkmk_top1"></a>Passerelle de site à site VPN RAS avec BGP sur le périmètre du site d’entreprise  
Cette topologie décrit un site d’entreprise connecté à un fournisseur de services cloud (CSP). La topologie de routage d’entreprise comprend un routeur interne, une passerelle RAS Windows Server 2016 configurée pour les connexions VPN de site à site avec le fournisseur de services de chiffrement et un périphérique de pare-feu de périmètre. La passerelle RAS met fin aux connexions VPN S2S et BGP.  
  
![Passerelle VPN de site à site RAS](../../media/Border-Gateway-Protocol-BGP/bgp_01.jpg)  
  
Les deux sites sont connectés à l’aide d’un protocole eBGP (External Border Gateway Protocol), qui peut transmettre des informations entre des routeurs compatibles avec le protocole BGP, au sein de systèmes autonomes distincts. Cela implique que l’entreprise et le fournisseur de services cloud ont des numéros de système autonome (ASN) distincts, ce qui constitue un paramètre faisant partie intégrante du protocole BGP.  
  
Dans ce scénario, le protocole BGP fonctionne de la façon suivante.  
  
-   L’appareil de périphérie du site d’entreprise apprend les itinéraires de sous-réseau virtualisé (10.2.1.0/24) hébergés dans le cloud à l’aide du protocole BGP. Cet appareil publie également les itinéraires de sous-réseau local (10.1.1.0/24) vers la passerelle mutualisée RAS CSP.  
  
-   Le routeur de périphérie client apprend les itinéraires internes locaux via l’un des mécanismes suivants :  
  
    -   L’appareil de périphérie exécute le protocole BGP avec un routeur interne et apprend les itinéraires internes (dans cet exemple, 10.1.1.0/24). Pendant ce temps, le routeur interne apprend les itinéraires externes (par exemple, 10.2.1.0/24) à partir de l’appareil de périphérie et doit ensuite les distribuer à d’autres routeurs locaux à l’aide d’un protocole IGP (Interior Gateway Protocol), comme OSPF (Open Shortest Path First) ou le protocole RIP (Routing Information Protocol).  
  
    -   L’appareil de périphérie peut être configuré avec des interface ou des itinéraires statiques pour sélectionner les itinéraires de publication à l’aide du protocole BGP. L’appareil de périphérie distribue également les itinéraires externes aux autres routeurs locaux à l’aide d’un protocole IGP.  
  
### <a name="third-party-gateway-with-bgp-at-enterprise-site-edge"></a><a name="bkmk_top2"></a>Passerelle tierce avec BGP sur le périmètre du site d’entreprise  
Cette topologie décrit un site d’entreprise utilisant un routeur de périphérie tiers pour se connecter à un fournisseur de services cloud (CSP). Le routeur de périphérie sert également de passerelle VPN de site à site.  
  
![Passerelle tierce avec protocole BGP en périphérie du site d’entreprise](../../media/Border-Gateway-Protocol-BGP/bgp_02.jpg)  
  
Le routeur de périphérie d’entreprise apprend les itinéraires internes locaux via l’un des mécanismes suivants :  
  
-   L’appareil de périphérie exécute le protocole BGP avec un routeur interne et apprend les itinéraires internes (dans ce cas, 10.1.1.0/24).  
  
-   L’appareil de périphérie implémente un protocole IGP (Interior Gateway Protocol) et participe directement au routage interne.  
  
### <a name="multiple-enterprise-sites-connecting-to-csp-cloud-datacenter"></a><a name="bkmk_top3"></a>Plusieurs sites d’entreprise se connectant au centre de solutions de CHIFFREment Cloud  
Cette topologie décrit plusieurs sites d’entreprise qui utilisent des passerelles tierces pour se connecter à un fournisseur de services cloud (CSP). Les appareils de périphérie tiers servent de passerelles de VPN de site à site et de routeurs BGP.  
  
![Plusieurs sites d’entreprise se connectant au centre de solutions de CHIFFREment Cloud](../../media/Border-Gateway-Protocol-BGP/bgp_03.jpg)  
  
Les routeurs de périphérie client apprennent les itinéraires internes locaux via l’un des mécanismes suivants :  
  
-   L’appareil de périphérie exécute le protocole BGP avec un routeur interne et apprend les itinéraires internes (dans ce cas, 10.1.1.0/24).  
  
-   L’appareil de périphérie implémente un protocole IGP (Interior Gateway Protocol) et participe directement au routage interne.  
  
Chaque site d’entreprise apprend les itinéraires de l’autre site via la connectivité directe eBGP.  
  
Chaque site d’entreprise apprend directement les itinéraires du réseau hébergé à l’aide de l’autre site d’entreprise, mais il sélectionne le meilleur itinéraire en fonction de son coût.  
  
Si le routeur BGP du site d’entreprise 1 ne peut pas se connecter au routeur BGP du site d’entreprise 2 en raison de l’échec de la connectivité, le routeur BGP du site 1 commence de manière dynamique à apprendre les itinéraires vers le réseau Enterprise site 2 à partir du routeur BGP du CSP, et le trafic est en toute transparence. reroutage du site 1 vers le site 2 par le biais du routeur BGP Windows Server sur le fournisseur de services de chiffrement.  
  
### <a name="separate-termination-points-for-bgp-and-vpn"></a><a name="bkmk_top4"></a>Points de terminaison distincts pour le protocole BGP et le VPN  
Cette topologie décrit une entreprise qui utilise deux routeurs différents comme points de terminaison BGP et VPN de site à site. Le réseau VPN de site à site est arrêté sur la passerelle RAS Windows Server 2016, alors que BGP est terminé sur un routeur interne. Au niveau du CSP des connexions, le fournisseur de services de chiffrement met fin aux connexions VPN et BGP avec la passerelle RAS. Avec cette configuration, le matériel de routeur tiers interne doit prendre en charge la redistribution des itinéraires IGP vers le protocole BGP, ainsi que la redistribution des itinéraires BGP vers le protocole IGP.  
  
![Points de terminaison distincts pour le protocole BGP et le VPN](../../media/Border-Gateway-Protocol-BGP/bgp_04.jpg)  
  
Le routeur interne apprend les itinéraires d’entreprise via l’un des mécanismes suivants :  
  
-   Protocole BGP  
  
-   Protocole IGP (Interior Gateway Protocol), comme OSPF ou RIP  
  
-   Configuration d’un itinéraire statique  
  
Lorsqu’un protocole IGP est utilisé sur le site d’entreprise, le routeur interne doit redistribuer les itinéraires IGP dans BGP et redistribuer les itinéraires BGP dans IGP pour maintenir la connectivité de sous-réseau entre les réseaux virtuels du CSP et les sous-réseaux locaux d’entreprise.  
  
Avec ce déploiement, la passerelle RAS d’entreprise possède une connexion VPN de site à site avec la passerelle RAS CSP, qui fournit à la passerelle RAS de l’entreprise les itinéraires vers la passerelle du fournisseur de services de chiffrement. Le routeur interne d’entreprise apprend ensuite cet itinéraire vers la passerelle CSP en utilisant iBGP avec la passerelle RAS d’entreprise. Pour cette raison, le routeur interne d’entreprise est en mesure d’établir une session d’homologation avec le routeur BGP de la passerelle RAS CSP.  
  
À partir de ce stade, le routeur interne d’entreprise et la passerelle RAS CSP échangent des informations de routage. Et le routeur BGP RAS de l’entreprise Découvre les itinéraires du CSP et les itinéraires d’entreprise pour router physiquement les paquets entre les réseaux.  
  
## <a name="bgp-features"></a><a name="bkmk_features"></a>Fonctionnalités BGP  
Voici les fonctionnalités du routeur BGP de la passerelle RAS.  
  
**Routage BGP en tant que service de rôle de l’accès à distance**. Vous pouvez maintenant installer le service de rôle **routage** du rôle serveur d’accès à distance sans installer le service de rôle **service d’accès à distance (RAS)** lorsque vous souhaitez utiliser l’accès à distance en tant que routeur de réseau local BGP.  Cela réduit l’encombrement de mémoire du routeur BGP et installe uniquement les composants requis pour le routage BGP dynamique. Le service de rôle routage est utile lorsque seule une machine virtuelle de routeur BGP est requise et que vous n’avez pas besoin d’utiliser DirectAccess ou VPN. En outre, l’utilisation de l’accès à distance en tant que routeur LAN avec BGP vous offre les avantages de routage dynamique de BGP sur votre réseau interne.  
  
**Statistiques BGP (compteurs de messages, compteurs d’itinéraires)** . Le routeur BGP prend en charge l’affichage des statistiques de message et d’itinéraire, si nécessaire, à l’aide de la commande Windows PowerShell **Get-BgpStatistics**.  
  
**Prise en charge du routage ECMP (Equal Cost Multi Path Routing)** . Le routeur BGP prend en charge le routage ECMP et peut avoir plusieurs itinéraires de même coût raccordés à la pile et la table de routage BGP. La sélection de l’itinéraire effectuée par le routeur BGP pour la transmission des paquets de données est aléatoire lorsque le routage ECMP est activé.  
  
**Configuration de la valeur HoldTime**. Le routeur BGP prend en charge la configuration de la valeur HoldTimer en fonction des besoins de votre réseau. Cette minuterie peut être modifiée dynamiquement pour s’adapter à l’interopérabilité avec les périphériques tiers ou pour respecter une durée maximale spécifique d’expiration de la session de peering du protocole BGP.  
  
**Prise en charge des protocoles BGP interne et externe**. Le routeur BGP prend en charge les peerings eBGP (protocole externe) et iBGP (protocole interne). Pour configurer l’une ou l’autre des homologations, vous devez vous assurer que les numéros de système autonome appropriés sont attribués aux routeurs BGP locaux et distants. Les quatre topologies de déploiement BGP utilisent le peering eBGP et la quatrième topologie utilise également le peering iBGP.  
  
**Interopérabilité avec les solutions tierces**. Le routeur BGP est basé sur les spécifications de la dernière version 4 du protocole BGP. Il a été testé pour son interopérabilité avec la plupart des périphériques de routage BGP tiers. Pour plus d’informations, consultez le document RFC (Request for Comments) 4271, [Protocole de passerelle frontière 4 (BGP-4)](https://tools.ietf.org/html/rfc4271).  
  
**Prise en charge du peering du transport IPv4 et IPv6**. Le routeur BGP prend en charge les peerings IPv4 et IPv6. Toutefois, vous devez configurer l’identificateur BGP comme adresse IPv4 du routeur BGP. Pour toutes les topologies de déploiement du routeur BGP, vous pouvez utiliser l’un des deux types de peering (IPV4/IPv6).  
  
**Capacité d’apprentissage et de publication d’un itinéraire monodiffusion IPv4 et IPv6 (Multiprotocol Network Layer Reachability Information [NLRI])** . Peu importe le transport que vous utilisez, le routeur BGP peut échanger les itinéraires IPv4 et IPv6 si la capacité appropriée est annoncée par d’autres routeurs BGP lors de l’établissement de la session. Pour configurer le routage IPv6, le paramètre IPv6Routing doit être activé et une adresse IPv6 globale locale doit être configurée au niveau du routeur.  
  
**Peering des modes mixte et passif**. Vous pouvez configurer des sessions d’homologation BGP en mode mixte, où le routeur BGP joue le rôle d’initiateur et de répondeur ou de mode passif, où le routeur BGP ne lance pas l’homologation, mais répond aux demandes entrantes. Le mode mixte est activé par défaut. Il est recommandé pour le peering BGP, sauf si vous souhaitez utiliser le mode passif pour le débogage ou le diagnostic. Pour toutes les topologies de déploiement du routeur BGP, le peering en mode mixte est requis pour permettre le redémarrage automatique en cas d’événements d’échec.  
  
**Capacité de réécriture de l’attribut d’itinéraire**. Vous pouvez ajouter, modifier ou supprimer les attributs suivants des annonces d’itinéraires d’entrée et de sortie du routeur BGP à l’aide des stratégies de routage BGP : Tronçon suivant, MED, Préf. locale et Communauté.  
  
**Filtrage de l’itinéraire**. Le routeur BGP prend en charge les annonces de filtrage des itinéraires d’entrée et de sortie selon plusieurs attributs d’itinéraire comme Préfixe, Plage ASN, Communauté et Tronçon suivant.  
  
**Route-réflecteur (RR) et client RR**. Le routeur BGP peut agir comme un réflecteur d’itinéraire et un client de RR. Cela est utile dans les topologies complexes où RR peut simplifier le réseau en formant des clusters de RR.  
  
**Prise en charge de l’actualisation de l’itinéraire**. Le routeur BGP prend en charge l’actualisation de l’itinéraire et publie cette fonctionnalité sur le peering, par défaut. Il est en mesure d’envoyer un nouvel ensemble de mises à jour d’itinéraires lorsqu’il est demandé par un homologue via un message d’actualisation de routage, ainsi que d’envoyer un itinéraire d’actualisation pour mettre à jour sa table de routage dans les événements tels que les modifications de stratégie de routage pour un homologue. Cela permet de modifier ou de mettre à jour les stratégies de routage BGP dans Windows Server 2016 sans avoir à redémarrer l’homologation.  
  
**Prise en charge de la configuration d’un itinéraire statique**. Vous pouvez configurer des interfaces ou des itinéraires statiques sur le routeur BGP en utilisant la commande Windows PowerShell **Add-BgpCustomRoute** . Les itinéraires statiques que vous configurez peuvent être les préfixes ou le nom des interfaces à partir desquelles les itinéraires doivent être choisis. Toutefois, seuls les itinéraires avec des tronçons suivants pouvant être résolus sont raccordés aux tables de routage BGP et publiés sur les homologues.  
  
**Prise en charge du routage de transit**. Le routeur BGP prend en charge le routage de transit pour les connexions iBGP à iBGP, iBGP aux connexions eBGP et eBGP aux connexions eBGP.  
  
**Blocage du rabat de route**. Le bagottement des itinéraires vers le routage BGP dans Windows Server 2016 assure la prise en charge du blocage du rabat. Par exemple, lorsqu’un itinéraire est en cours de publication et de retrait, rendant la table de routage instable, vous pouvez configurer le routeur BGP pour affecter un poids de mouillage à l’itinéraire et le surveiller pour les rabats, et en conséquence supprimer ou annuler la suppression de celui-ci selon les besoins. Cela permet de maintenir une table de routage stable et de réduire le traitement par le routeur BGP.  
  
**Agrégation de routage**. L’agrégation d’itinéraires au routeur BGP vous permet de configurer des itinéraires d’agrégation et de remplacer les annonces d’itinéraires plus granulaires par des itinéraires de synthèse ou d’agrégation à des homologues. Cela entraîne un nombre moins important de messages d’annonce de routage transmis sur le réseau.  
  
> [!NOTE]  
> Dans System Center, la passerelle RAS est nommée passerelle Windows Server.  
  

  

