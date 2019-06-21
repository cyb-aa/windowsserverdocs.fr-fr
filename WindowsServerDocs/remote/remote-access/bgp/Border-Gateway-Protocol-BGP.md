---
title: Protocole BGP (Border Gateway Protocol)
description: Vous pouvez utiliser cette rubrique pour mieux comprendre de protocole BGP (Border Gateway) dans Windows Server 2016, y compris les topologies de déploiement BGP prises en charge et de fonctionnalités BGP et de fonctionnalités.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 78cc2ce3-a48e-45db-b402-e480b493fab1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 655a7b02468db4246b85b495289806a3f9735a95
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282002"
---
# <a name="border-gateway-protocol-bgp"></a>Protocole BGP (Border Gateway Protocol)

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour mieux comprendre le protocole BGP (Border Gateway Protocol), notamment les topologies de déploiement BGP prises en charge, ainsi que les fonctionnalités et capacités BGP.  
  
> [!NOTE]  
> Outre cette rubrique, la documentation de BGP suivante est disponible.  
>   
> -   [Référence à la commande Windows PowerShell BGP](../../remote-access/bgp/BGP-Windows-PowerShell-Command-Reference.md)  
  
Cette rubrique contient les sections suivantes.  
  
-   [BGP pris en charge les Topologies de déploiement](#bkmk_top)  
  
-   [Fonctionnalités BGP](#bkmk_features)  
  
Lorsque configurée sur un Service d’accès à distance de Windows Server 2016 \(RAS\) passerelle en mode partagé au protocole BGP (Border Gateway) vous offre la possibilité de gérer le routage du trafic réseau entre les réseaux de machines virtuelles de vos clients et leurs sites distants. Vous pouvez également utiliser le protocole BGP pour les déploiements de passerelle RAS client unique, et lorsque vous déployez l’accès à distance comme un réseau Local \(LAN\) routeur.  
  
Le protocole BGP réduit le besoin de configuration manuelle de routage sur les routeurs, car il s’agit d’un protocole de routage dynamique qui, de plus, découvre automatiquement les itinéraires entre les sites qui sont connectés via des connexions VPN de site à site.  
  
Pour utiliser le routage BGP, vous devez installer le **Remote Access Service \(RAS\)**  et/ou la **routage** service de rôle du rôle de serveur d’accès à distance sur un ordinateur ou d’une machine virtuelle \(Machine virtuelle\) -le type de système que vous utilisez dépend de si vous avez un déploiement partagé :  
  
-   Pour un déploiement partagé, il est recommandé d’installer la passerelle RAS sur une ou plusieurs machines virtuelles. Utilisation de plusieurs machines virtuelles offre une haute disponibilité. La passerelle RAS est capable de gérer plusieurs connexions à partir de plusieurs locataires et se compose d’un ordinateur hôte Hyper-V et une machine virtuelle qui est configurée en tant que la passerelle. Cette passerelle est configurée avec les connexions VPN de site à site comme un routeur BGP mutualisé au client exchange et le fournisseur de services Cloud \(CSP\) les itinéraires de sous-réseau.  
  
-   Pour un déploiement de passerelle de périphérie client unique ou un déploiement de routeur de réseau local, vous pouvez installer la passerelle RAS sur un ordinateur physique ou une machine virtuelle.  
  
> [!IMPORTANT]  
> Lorsque vous installez une passerelle RAS, vous devez spécifier si BGP est activé pour chaque client à l’aide de la **Enable-RemoteAccessRoutingDomain** commande Windows PowerShell avec le **Type** valeur du paramètre de  **Tous les**. Pour installer l’accès à distance comme un routeur LAN activée pour BGP sans fonctionnalités mutualisées, vous pouvez utiliser la commande **Install-RemoteAccess - VpnType RoutingOnly**.  
>   
> L’exemple de code suivant montre comment installer le service RAS en mode mutualisé avec toutes les fonctionnalités RAS (point-to-site VPN site à site VPN et BGP routage) activée pour les deux clients, Contoso et Fabrikam.  
  
```  
$Contoso_RoutingDomain = "ContosoTenant"  
$Fabrikam_RoutingDomain = "FabrikamTenant"  
  
Install-RemoteAccess -MultiTenancy  
  
Enable-RemoteAccessRoutingDomain -Name $Contoso_RoutingDomain -Type All -PassThru  
Enable-RemoteAccessRoutingDomain -Name $Fabrikam_RoutingDomain -Type All -PassThru  
```  
  
## <a name="bkmk_top"></a>BGP pris en charge les Topologies de déploiement  
Ci-dessous sont répertoriées les topologies de déploiement prises en charge lorsque les sites d’entreprise sont connectés au centre de données du fournisseur de services cloud (CSP).  
  
Dans tous les scénarios, la passerelle CSP est une passerelle RAS de Windows Server 2016 à la périphérie. La passerelle RAS, qui est capable de gérer plusieurs connexions à partir de plusieurs locataires, se compose d’un ordinateur hôte Hyper-V et une machine virtuelle qui est configurée en tant que la passerelle. Cette passerelle de périphérie est configurée avec les connexions VPN de site à site comme un routeur BGP mutualisé pour échanger les itinéraires du sous-réseau CSP et d’entreprise.  
  
Les clients se connectent à leurs ressources du centre de données CSP à l’aide d’une connexion VPN de site à site (S2S). En outre, le protocole de routage BGP est déployé pour l’échange d’informations de routage dynamiques entre les passerelles CSP et d’entreprise.  
  
Les topologies de déploiement suivantes sont prises en charge.  
  
-   [Passerelle de Site à Site VPN RAS avec protocole BGP en périphérie du site d’entreprise](#bkmk_top1)  
  
-   [Passerelle tierce avec protocole BGP en périphérie du site d’entreprise](#bkmk_top2)  
  
-   [Plusieurs sites d’entreprise avec des passerelles tierces](#bkmk_top3)  
  
-   [Points de terminaison distincts pour BGP et VPN](#bkmk_top4)  
  
Les sections suivantes contiennent des informations supplémentaires sur chaque topologie BGP prise en charge.  
  
### <a name="bkmk_top1"></a>Passerelle de Site à Site VPN RAS avec protocole BGP en périphérie du site d’entreprise  
Cette topologie décrit un site d’entreprise connecté à un fournisseur de services cloud (CSP). La topologie de routage Enterprise inclut un routeur interne, une passerelle RAS de Windows Server 2016 configurées pour les connexions de site à site VPN avec le CSP et un dispositif de pare-feu de périmètre. La passerelle RAS termine les connexions S2S VPN et BGP.  
  
![Passerelle de Site à Site VPN d’accès distant](../../media/Border-Gateway-Protocol-BGP/bgp_01.jpg)  
  
Les deux sites sont connectés à l’aide d’un protocole eBGP (External Border Gateway Protocol), qui peut transmettre des informations entre des routeurs compatibles avec le protocole BGP, au sein de systèmes autonomes distincts. Cela implique que l’entreprise et le fournisseur de services cloud ont des numéros de système autonome (ASN) distincts, ce qui constitue un paramètre faisant partie intégrante du protocole BGP.  
  
Dans ce scénario, le protocole BGP fonctionne de la façon suivante.  
  
-   L’appareil de périphérie du site d’entreprise apprend les itinéraires de sous-réseau virtualisé (10.2.1.0/24) hébergés dans le cloud à l’aide du protocole BGP. Cet appareil publie également les itinéraires de sous-réseau local (10.1.1.0/24) à la passerelle mutualisée du CSP RAS.  
  
-   Le routeur de périphérie client apprend les itinéraires internes locaux via l’un des mécanismes suivants :  
  
    -   L’appareil de périphérie exécute le protocole BGP avec un routeur interne et apprend les itinéraires internes (dans cet exemple, 10.1.1.0/24). Pendant ce temps, le routeur interne apprend les itinéraires externes (par exemple, 10.2.1.0/24) à partir de l’appareil de périphérie et doit ensuite les distribuer à d’autres routeurs locaux à l’aide d’un protocole IGP (Interior Gateway Protocol), comme OSPF (Open Shortest Path First) ou le protocole RIP (Routing Information Protocol).  
  
    -   L’appareil de périphérie peut être configuré avec des interface ou des itinéraires statiques pour sélectionner les itinéraires de publication à l’aide du protocole BGP. L’appareil de périphérie distribue également les itinéraires externes aux autres routeurs locaux à l’aide d’un protocole IGP.  
  
### <a name="bkmk_top2"></a>Passerelle tierce avec protocole BGP en périphérie du site d’entreprise  
Cette topologie décrit un site d’entreprise utilisant un routeur de périphérie tiers pour se connecter à un fournisseur de services cloud (CSP). Le routeur de périphérie sert également de passerelle VPN de site à site.  
  
![Passerelle tierce avec protocole BGP en périphérie du site d’entreprise](../../media/Border-Gateway-Protocol-BGP/bgp_02.jpg)  
  
Le routeur de périphérie d’entreprise apprend les itinéraires internes locaux via l’un des mécanismes suivants :  
  
-   L’appareil de périphérie exécute le protocole BGP avec un routeur interne et apprend les itinéraires internes (dans ce cas, 10.1.1.0/24).  
  
-   L’appareil de périphérie implémente un protocole IGP (Interior Gateway Protocol) et participe directement au routage interne.  
  
### <a name="bkmk_top3"></a>Plusieurs sites d’entreprise se connectant à CSP cloud de centre de données  
Cette topologie décrit plusieurs sites d’entreprise qui utilisent des passerelles tierces pour se connecter à un fournisseur de services cloud (CSP). Les appareils de périphérie tiers servent de passerelles de VPN de site à site et de routeurs BGP.  
  
![Plusieurs sites d’entreprise se connectant à CSP cloud de centre de données](../../media/Border-Gateway-Protocol-BGP/bgp_03.jpg)  
  
Les routeurs de périphérie client apprennent les itinéraires internes locaux via l’un des mécanismes suivants :  
  
-   L’appareil de périphérie exécute le protocole BGP avec un routeur interne et apprend les itinéraires internes (dans ce cas, 10.1.1.0/24).  
  
-   L’appareil de périphérie implémente un protocole IGP (Interior Gateway Protocol) et participe directement au routage interne.  
  
Chaque site d’entreprise apprend les itinéraires de l’autre site via la connectivité directe eBGP.  
  
Chaque site d’entreprise apprend directement les itinéraires du réseau hébergé à l’aide de l’autre site d’entreprise, mais il sélectionne le meilleur itinéraire en fonction de son coût.  
  
Si le routeur BGP au Site d’entreprise 1 ne peut pas se connecter avec le routeur BGP de Enterprise Site 2, car la connectivité a échoué, le routeur BGP de 1 Site commence dynamiquement à apprendre les itinéraires au réseau d’entreprise Site 2 à partir du routeur BGP de CSP et que le trafic est en toute transparence redirigé à partir du Site 1 vers le Site 2 via le routeur BGP Windows Server le CSP.  
  
### <a name="bkmk_top4"></a>Points de terminaison distincts pour BGP et VPN  
Cette topologie décrit une entreprise qui utilise deux routeurs différents comme points de terminaison BGP et VPN de site à site. VPN de site à site est arrêté sur la passerelle RAS de Windows Server 2016, tandis que le protocole BGP est interrompu sur un routeur interne. Sur le côté fournisseur de services cryptographiques des connexions, le fournisseur interrompt les connexions VPN et BGP avec la passerelle RAS. Avec cette configuration, le matériel de routeur tiers interne doit prendre en charge la redistribution des itinéraires IGP vers le protocole BGP, ainsi que la redistribution des itinéraires BGP vers le protocole IGP.  
  
![Points de terminaison distincts pour le protocole BGP et le VPN](../../media/Border-Gateway-Protocol-BGP/bgp_04.jpg)  
  
Le routeur interne apprend les itinéraires d’entreprise via l’un des mécanismes suivants :  
  
-   Protocole BGP  
  
-   Protocole IGP (Interior Gateway Protocol), comme OSPF ou RIP  
  
-   Configuration d’un itinéraire statique  
  
Lorsqu’un protocole IGP est utilisé sur le site d’entreprise, le routeur interne doit redistribuer les itinéraires IGP dans BGP et redistribuer les itinéraires BGP dans IGP pour maintenir la connectivité de sous-réseau entre les réseaux virtuels du CSP et les sous-réseaux locaux d’entreprise.  
  
Avec ce déploiement, la passerelle RAS d’entreprise a une connexion VPN de site à site avec la passerelle RAS CSP, qui fournit la passerelle RAS d’entreprise avec les itinéraires vers la passerelle CSP. Le routeur interne d’entreprise apprend ensuite cet itinéraire vers la passerelle CSP en utilisant iBGP avec la passerelle RAS d’entreprise. Pour cette raison, le routeur interne d’entreprise est alors en mesure d’établir une session d’homologation avec le routeur BGP de CSP RAS passerelle.  
  
À ce stade, le routeur interne d’entreprise et de la passerelle RAS du CSP échangent des informations de routage. Et le routeur BGP de RAS d’entreprise apprend les itinéraires du CSP et les itinéraires d’entreprise pour router physiquement des paquets entre les réseaux.  
  
## <a name="bkmk_features"></a>Fonctionnalités BGP  
Voici les fonctionnalités du routeur BGP passerelle RAS.  
  
**Routage BGP en tant que rôle service d’accès à distance**. Vous pouvez maintenant installer le **routage** service de rôle du rôle de serveur d’accès à distance sans installer le **Service d’accès à distance (RAS)** service de rôle lorsque vous souhaitez utiliser l’accès à distance comme un routeur BGP LAN.  Cela réduit l’encombrement de mémoire de routeur BGP et installe uniquement les composants requis pour le routage dynamique BGP. Le service de rôle routage est utile lorsque seuls un routeur BGP la machine virtuelle, et vous ne nécessitent pas l’utilisation de DirectAccess ou VPN. En outre, l’accès à distance en tant que routeur de réseau local avec le protocole BGP vous offre les avantages de routage dynamiques du protocole BGP sur votre réseau interne.  
  
**Statistiques BGP (compteurs de messages, compteurs d’itinéraires)** . Le routeur BGP prend en charge l’affichage des statistiques de message et d’itinéraire, si nécessaire, à l’aide de la commande Windows PowerShell **Get-BgpStatistics**.  
  
**Prise en charge du routage ECMP (Equal Cost Multi Path Routing)** . Le routeur BGP prend en charge le routage ECMP et peut avoir plusieurs itinéraires de même coût raccordés à la pile et la table de routage BGP. La sélection de l’itinéraire effectuée par le routeur BGP pour la transmission des paquets de données est aléatoire lorsque le routage ECMP est activé.  
  
**Configuration de la valeur HoldTime**. Le routeur BGP prend en charge la configuration de la valeur HoldTimer en fonction des besoins de votre réseau. Cette minuterie peut être modifiée dynamiquement pour s’adapter à l’interopérabilité avec les périphériques tiers ou pour respecter une durée maximale spécifique d’expiration de la session d’homologation du protocole BGP.  
  
**Prise en charge des protocoles BGP interne et externe**. Le routeur BGP prend en charge les homologations eBGP (protocole externe) et iBGP (protocole interne). Pour configurer l’une ou l’autre des homologations, vous devez vous assurer que les numéros de système autonome appropriés sont attribués aux routeurs BGP locaux et distants. Les quatre topologies de déploiement BGP utilisent l’homologation eBGP et la quatrième topologie utilise également l’homologation iBGP.  
  
**Interopérabilité avec les solutions tierces**. Le routeur BGP est basé sur les spécifications de la dernière version 4 du protocole BGP. Il a été testé pour son interopérabilité avec la plupart des périphériques de routage BGP tiers. Pour plus d’informations, consultez le document RFC (Request for Comments) 4271, [Protocole de passerelle frontière 4 (BGP-4)](https://tools.ietf.org/html/rfc4271).  
  
**Prise en charge de l’homologation du transport IPv4 et IPv6**. Le routeur BGP prend en charge les homologations IPv4 et IPv6. Toutefois, vous devez configurer l’identificateur BGP comme adresse IPv4 du routeur BGP. Pour toutes les topologies de déploiement du routeur BGP, vous pouvez utiliser l’un des deux types d’homologation (IPV4/IPv6).  
  
**Capacité d’apprentissage et de publication d’un itinéraire monodiffusion IPv4 et IPv6 (Multiprotocol Network Layer Reachability Information [NLRI])** . Peu importe le transport que vous utilisez, le routeur BGP peut échanger les itinéraires IPv4 et IPv6 si la capacité appropriée est annoncée par d’autres routeurs BGP lors de l’établissement de la session. Pour configurer le routage IPv6, le paramètre IPv6Routing doit être activé et une adresse IPv6 globale locale doit être configurée au niveau du routeur.  
  
**Homologation des modes mixte et passif**. Vous pouvez configurer des sessions d’homologation BGP en mode mixte - où le routeur BGP joue le rôle initiateur et répondeur, ou en mode passif, où le routeur BGP n’initie pas l’homologation, mais ne répond pas aux requêtes entrantes. Le mode mixte est activé par défaut. Il est recommandé pour l’homologation BGP, sauf si vous souhaitez utiliser le mode passif pour le débogage ou le diagnostic. Pour toutes les topologies de déploiement du routeur BGP, l’homologation en mode mixte est requise pour permettre le redémarrage automatique en cas d’événements d’échec.  
  
**Capacité de réécriture de l’attribut d’itinéraire**. Vous pouvez ajouter, modifier ou supprimer les attributs suivants des annonces d’itinéraires d’entrée et de sortie du routeur BGP à l’aide des stratégies de routage BGP : Tronçon suivant, MED, Préf. locale et Communauté.  
  
**Filtrage de l’itinéraire**. Le routeur BGP prend en charge les annonces de filtrage des itinéraires d’entrée et de sortie selon plusieurs attributs d’itinéraire comme Préfixe, Plage ASN, Communauté et Tronçon suivant.  
  
**Client de réflecteur d’itinéraire (RR) et RR**. Le routeur BGP peut agir comme un réflecteur d’itinéraire et un client de l’enregistrement de ressource. Cela est utile dans des topologies complexes où RR peut simplifier le réseau en formant des Clusters de l’enregistrement de ressource.  
  
**Prise en charge de l’actualisation de l’itinéraire**. Le routeur BGP prend en charge l’actualisation de l’itinéraire et publie cette fonctionnalité sur l’homologation, par défaut. Il est capable d’envoyer un ensemble de mises à jour de l’itinéraire lorsque demandé par un homologue via un message de l’actualisation de l’itinéraire, ainsi que l’envoi d’une actualisation de l’itinéraire pour mettre à jour sa table de routage dans les événements tels que les modifications de stratégie de routage pour un homologue. Ainsi, le scénario de modification ou de la mise à jour des stratégies de routage BGP dans Windows Server 2016 sans avoir à redémarrer l’homologation.  
  
**Prise en charge de la configuration d’un itinéraire statique**. Vous pouvez configurer des interfaces ou des itinéraires statiques sur le routeur BGP en utilisant la commande Windows PowerShell **Add-BgpCustomRoute** . Les itinéraires statiques que vous configurez peuvent être les préfixes ou le nom des interfaces à partir desquelles les itinéraires doivent être choisis. Toutefois, seuls les itinéraires avec des tronçons suivants pouvant être résolus sont raccordés aux tables de routage BGP et publiés sur les homologues.  
  
**Prise en charge du routage de transit**. Le routeur BGP prend en charge le routage de transit pour les connexions iBGP vers iBGP, les connexions iBGP vers eBGP et eBGP vers eBGP connexions.  
  
**Acheminer le rabat blocage**. Blocage de rabat itinéraire de routage BGP dans Windows Server 2016 prend en charge pour l’itinéraire rabat blocage. Par exemple, lorsqu’un itinéraire est publié en permanence et retiré, rendre la table de routage instable, vous pouvez configurer le routeur BGP pour attribuer un poids de blocage à l’itinéraire et analysez-la pour bagottements - et en conséquence de supprimer ou Annuler-supprimer en fonction des besoins. Cela permet au maintien d’une table de routage stable et moins de traitement par le routeur BGP.  
  
**Acheminer d’agrégation**. Agrégation d’itinéraire vers le routeur BGP vous offre la possibilité de pour configurer les itinéraires agrégés et remplacer les annonces d’itinéraire plus granulaires avec des itinéraires de résumé ou d’agrégats pour les homologues. Il en résulte un plus petit nombre de messages d’annonce de route transmises sur le réseau.  
  
> [!NOTE]  
> Dans System Center, la passerelle RAS est appelée passerelle Windows Server.  
  

  

