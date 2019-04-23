---
title: Tunneling GRE dans Windows Server 2016
description: Vous pouvez utiliser cette rubrique pour mieux comprendre des mises à jour pour la fonctionnalité de tunnel GRE Generic Routing Encapsulation () pour la passerelle RAS dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: df2023bf-ba64-481e-b222-6f709edaa5c1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e0ec077ad5e97edd3db7d1dc4e662bb191f7885b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887860"
---
# <a name="gre-tunneling-in-windows-server-2016"></a>Tunneling GRE dans Windows Server 2016

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Windows Server 2016 fournit des mises à jour pour l’Encapsulation générique de routage \(GRE\) fonctionnalité de tunnel pour la passerelle RAS.  
  
GRE est un protocole de tunneling léger qui peut encapsuler une grande variété de protocoles de la couche réseau dans les liaisons point à point virtuelles sur un réseau d’interconnexion IP. L’implémentation Microsoft GRE peut encapsuler IPv4 et IPv6.  
  
Les tunnels GRE sont utiles dans de nombreux scénarios, car :  
  
-   Ils sont légers et 2890 RFC conforme, rendant interopérable avec différents périphériques de fournisseur  
  
-   Vous pouvez utiliser le protocole de passerelle frontière \(BGP\) pour le routage dynamique  
  
-   Vous pouvez configurer mutualisée GRE RAS passerelles pour une utilisation avec Sdn \(SDN\)
  
-   Vous pouvez utiliser System Center Virtual Machine Manager pour gérer GRE\-RAS passerelles
  
-   Vous pouvez obtenir jusqu'à 2.0 débit Gbits/s sur une machine virtuelle 6 cœurs qui est configuré comme passerelle RAS GRE
  
-   Une seule passerelle prend en charge plusieurs modes de connexion  
  
Les tunnels GRE activent la connectivité entre les réseaux virtuels clients et les réseaux externes. Étant donné que le protocole GRE est léger et prise en charge est disponible sur la plupart des périphériques réseau, il devient un choix idéal pour le tunneling, où le chiffrement de données n’est pas nécessaire. 

Prise en charge GRE dans les tunnels de Site à Site (S2S) résout le problème de transfert entre réseaux virtuels clients et les réseaux externes clients à l’aide d’une passerelle mutualisée, comme décrit plus loin dans cette rubrique.  
  
La fonctionnalité de tunnel GRE est conçue pour répondre aux exigences suivantes :  
  
-   Un fournisseur d’hébergement doit être en mesure de créer des réseaux virtuels pour le transfert sans modification de la configuration de commutateur physique.  
  
-   Un fournisseur d’hébergement doit être en mesure d’ajouter des sous-réseaux à leurs réseaux exposée en externe sans modifier la configuration des commutateurs physiques au sein de leur infrastructure.  
La fonctionnalité de tunnel GRE permet ou améliore les différents scénarios clés pour l’hébergement de fournisseurs de services à l’aide des technologies Microsoft pour implémenter Sdn dans leurs offres de service.  
  
Voici quelques exemples de scénarios :  
  
-   [Accès à partir de réseaux virtuels des locataires à des réseaux physiques de locataires](#BKMK_Access)  
  
-   [Connectivité haut débit](#BKMK_Speed)  
  
-   [Isolation basée sur l’intégration de réseau local virtuel](#BKMK_Integration)  
  
-   [Ressources d’accès partagé](#BKMK_Shared)  
  
-   [Services de périphériques tiers aux locataires](#BKMK_thirdparty)  
  
## <a name="key-scenarios"></a>Principaux scénarios

Scénarios clés que la GRE tunnel des adresses de fonctionnalité sont les suivantes :  
  
### <a name="BKMK_Access"></a>Accès à partir de réseaux virtuels des locataires à des réseaux physiques de locataires

Ce scénario permet à une méthode évolutive fournir l’accès à partir de réseaux virtuels des locataires à des réseaux physiques situés sur le fournisseur de services hébergement local du client. Un point de terminaison de tunnel GRE est établi sur la passerelle mutualisée, l’autre extrémité du tunnel GRE est établie sur un périphérique tiers sur le réseau physique. Le trafic de couche 3 est acheminé entre les ordinateurs virtuels dans le réseau virtuel et l’appareil par des tiers sur le réseau physique.  
  
![Tunnel GRE connexion réseau physique hébergeur et réseau virtuel locataire](../../media/gre-tunneling-in-windows-server/GRE_.png)  
  
### <a name="BKMK_Speed"></a>Connectivité haut débit

Ce scénario permet à une méthode évolutive fournir une connectivité haut débit du réseau de locataire en local à son réseau virtuel situé sur le réseau de fournisseur de service hébergement. Un client se connecte au réseau de fournisseur de service par le biais de MPLS (MPLS), où un tunnel GRE est établi entre le routeur de périphérie du fournisseur service d’hébergement et de la passerelle mutualisée à réseau virtuel du locataire.  
  
![Tunnel GRE connexion réseau MPLS entreprise du client et réseau virtuel locataire](../../media/gre-tunneling-in-windows-server/GRE-.png)  
  
### <a name="BKMK_Integration"></a>Isolation basée sur l’intégration de réseau local virtuel

Ce scénario vous permet d’intégrer isolation du VLAN en fonction avec la virtualisation de réseau Hyper-V. Un réseau physique sur le réseau de fournisseur hébergement contient un équilibreur de charge utilisant l’isolement basé sur le réseau local virtuel. Une passerelle mutualisée établit les tunnels GRE entre l’équilibreur de charge sur le réseau physique et de la passerelle mutualisée sur le réseau virtuel.  
  
Possible d’établir plusieurs tunnels entre la source et la destination, et la clé GRE est utilisée pour faire la distinction entre les tunnels.  
  
![GRE plusieurs tunnels de connexion de réseaux virtuels client](../../media/gre-tunneling-in-windows-server/GRE-VLANIsolation.png)  
  
### <a name="BKMK_Shared"></a>Ressources d’accès partagé

Ce scénario vous permet d’accéder aux ressources partagées sur un réseau physique sur le réseau de fournisseur hébergement.  
  
Vous pouvez avoir un service partagé situé sur un serveur sur un réseau physique situé dans le réseau de fournisseur hébergement que vous souhaitez partager avec plusieurs réseaux virtuels clients.  
  
Les réseaux de clients avec des sous-réseaux sans chevauchement accéder au réseau courantes via un tunnel GRE. Une passerelle unique locataire achemine entre les tunnels GRE, par conséquent, routage des paquets vers les réseaux client correspondante.  
  
Dans ce scénario, la passerelle client unique peut être remplacée par les appliances matérielles de fournisseurs tiers.  
  
![Une passerelle à client unique à l’aide de plusieurs tunnels pour se connecter plusieurs réseaux virtuels](../../media/gre-tunneling-in-windows-server/GRE-SharedResource.png)  
  
### <a name="BKMK_thirdparty"></a>Services de périphériques tiers aux locataires

Ce scénario peut être utilisé pour intégrer les périphériques tiers (par exemple, les équilibreurs de charge matérielle) dans le flux de trafic de réseau virtuel locataire. Par exemple, le trafic en provenance d’un site d’entreprise passe via un tunnel S2S à la passerelle mutualisée. Le trafic est acheminé vers l’équilibreur de charge via un tunnel GRE. L’équilibrage de charge achemine le trafic vers plusieurs machines virtuelles sur le réseau virtuel de l’entreprise. La même chose se produit pour un autre locataire avec potentiellement chevauchement des adresses IP dans les réseaux virtuels. Le trafic réseau est isolé sur l’équilibreur de charge à l’aide de réseaux locaux virtuels et s’applique à tous les appareils de couche 3 qui prennent en charge des réseaux locaux virtuels.  
  
![GRE plusieurs tunnels de connexion de réseaux virtuels pour les périphériques tiers](../../media/gre-tunneling-in-windows-server/GREThirdParty.png)  
  
## <a name="configuration-and-deployment"></a>Configuration et déploiement

Un tunnel GRE est exposé comme un protocole supplémentaire au sein d’une interface S2S. Il est implémenté de manière similaire en tant qu’un tunnel IPSec S2S décrit dans le Blog de mise en réseau suivantes : [Architecture mutualisée de Site à Site (S2S) VPN Gateway avec Windows Server 2012 R2](https://blogs.technet.com/b/networking/archive/2013/09/29/multi-tenant-site-to-site-s2s-vpn-gateway-with-windows-server-2012-r2.aspx)  
  
Consultez la rubrique suivante pour obtenir un exemple qui déploie des passerelles, y compris les passerelles de tunnel GRE :  
  
[Déployer une infrastructure de réseau à définition logicielle à l’aide de scripts](../../../networking/sdn/deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md)
  
## <a name="more-information"></a>Informations supplémentaires

Pour plus d’informations sur le déploiement de passerelles de S2S, consultez les rubriques suivantes :  
  
-   [Passerelle RAS](RAS-Gateway.md)  
  
-   [Protocole de passerelle frontière &#40;BGP&#41;](../bgp/Border-Gateway-Protocol-BGP.md)  
  
-   [Nouveau ! Guide de déploiement de Windows Server 2012 R2 RAS passerelle mutualisée](https://blogs.technet.com/b/wsnetdoc/archive/2014/03/26/new-windows-server-2012-r2-RAS-multitenant-gateway-deployment-guide.aspx)  
  
-   [Déployer le protocole de passerelle frontière (BGP) avec la passerelle mutualisée RAS](https://blogs.technet.com/b/wsnetdoc/archive/2014/04/03/deploy-border-gateway-protocol-bgp-with-the-RAS-multitenant-gateway.aspx)  
  


