---
title: Accès à distance
description: Cette rubrique fournit une vue d’ensemble du rôle de serveur d’accès à distance dans Windows Server 2016.
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: eeca4cf7-90f0-485d-843c-76c5885c54b0
ms.author: pashort
author: shortpatti
ms.date: 05/18/2018
ms.openlocfilehash: faf12ad22678fa58ea933613759e3e8414966bca
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818360"
---
# <a name="remote-access"></a>Accès à distance

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows 10

Le guide de l’accès à distance vous offre une vue d’ensemble du rôle de serveur d’accès à distance dans Windows Server 2016 et couvre les sujets suivants :

- [Guide de déploiement VPN Always On](vpn/always-on-vpn/deploy/always-on-vpn-deploy.md)
- [Protocole de passerelle frontière &#40;BGP&#41;](bgp/Border-Gateway-Protocol-BGP.md)
- [Passerelle RAS](ras-gateway/RAS-Gateway.md) 
- [Documentation du rôle serveur accès à distance](ras/Remote-Access-Server-Role-Documentation.md)
- [Passerelle RAS pour SDN](../../networking/sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)
- [Réseau privé virtuel (VPN)](vpn/vpn-top.md)
 
Pour plus d’informations sur les autres technologies de mise en réseau, consultez [mise en réseau dans Windows Server 2016](https://docs.microsoft.com/windows-server/networking/networking).

Le rôle de serveur d’accès à distance est un regroupement logique de ces technologies d’accès réseau associés : [Service d’accès à distance (RAS)](#bkmk_da), [routage](#bkmk_rras), et [Proxy d’Application Web](#bkmk_proxy). Ces technologies sont les *services de rôle* du rôle serveur Accès à distance. Lorsque vous installez le rôle de serveur d’accès à distance avec le **Assistant Ajouter des rôles et fonctionnalités** ou Windows PowerShell, vous pouvez installer un ou plusieurs de ces services de rôle de trois.

>[!IMPORTANT]
>N’essayez pas de déployer l’accès à distance sur une machine virtuelle \(machine virtuelle\) dans Microsoft Azure. L’accès à distance dans Microsoft Azure n’est pas pris en charge. Vous ne pouvez pas utiliser l’accès à distance dans une machine virtuelle Azure pour déployer le VPN, DirectAccess ou toute autre fonctionnalité d’accès à distance dans Windows Server 2016 ou versions antérieures de Windows Server. Pour plus d’informations, consultez [prise en charge du logiciel Microsoft server pour machines virtuelles Microsoft Azure](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines).

## <a name="bkmk_da"></a>Service accès à distance \(RAS\) -passerelle RAS

Lorsque vous installez le **DirectAccess et VPN (RAS)** service de rôle, vous déployez la passerelle de Service d’accès à distance \( **passerelle RAS**\). Vous pouvez déployer la passerelle RAS un réseau privé virtuel client unique passerelle RAS \(VPN\) serveur, un serveur VPN de passerelle RAS mutualisé et comme un serveur DirectAccess.

- **Passerelle RAS - client unique**. À l’aide de passerelle RAS, vous pouvez déployer des connexions VPN pour fournir aux utilisateurs finaux un accès à distance au réseau et les ressources de votre organisation. Si vos clients exécutent Windows 10, vous pouvez déployer le VPN Always On, qui conserve une connexion persistante entre les clients et le réseau de votre organisation, chaque fois que les ordinateurs distants sont connectés à Internet. Avec cette passerelle, vous pouvez également créer une connexion VPN de site à site entre deux serveurs situés à différents emplacements, comme entre votre bureau principal et une filiale et utiliser la traduction d’adresses réseau \(NAT\) afin que les utilisateurs à l’intérieur de la réseau permettre accéder à des ressources externes, tels qu’Internet. En outre, passerelle RAS prend en charge le protocole BGP (Border Gateway), qui fournit des services de routage dynamiques lorsque vos sites distants ont également des passerelles de périmètre qui prennent en charge le protocole BGP.

- **Passerelle RAS - mutualisée**. Vous pouvez déployer la passerelle RAS comme un routeur mutualisée, passerelle edge basé sur logiciel et lorsque vous utilisez Hyper\-V la virtualisation de réseau ou que vous avez des réseaux de machines virtuelles déployées avec les réseaux locaux virtuels \(VLAN\). Avec la passerelle RAS, fournisseurs de services Cloud \(CSP\) et les entreprises peuvent activer le centre de données et cloud routage du trafic réseau entre les réseaux virtuels et physiques, y compris Internet. Avec la passerelle RAS, vos clients peuvent utiliser des connexions VPN de point site pour accéder à leurs ressources de réseau de machine virtuelle dans le centre de données à partir de n’importe quel endroit. Vous pouvez également fournir des clients avec les connexions VPN de site à site entre votre centre de données CSP et de leurs sites distants. En outre, vous pouvez configurer la passerelle RAS avec le protocole BGP pour le routage dynamique, et vous pouvez activer la traduction d’adresses réseau \(NAT\) pour fournir un accès Internet pour les machines virtuelles sur des réseaux de machines virtuelles.

>[!IMPORTANT]
> La passerelle RAS avec fonctionnalités mutualisées est également disponible dans Windows Server 2012 R2.

- **VPN Always On**. VPN Always On permet aux utilisateurs distants d’accéder en toute sécurité des ressources partagées, des sites Web intranet et des applications sur un réseau interne sans vous connecter à un réseau privé virtuel. 

Pour plus d’informations, consultez [passerelle RAS](ras-gateway/RAS-Gateway.md) et [protocole BGP (Border Gateway)](bgp/Border-Gateway-Protocol-BGP.md).

## <a name="bkmk_rras"></a>Routage

Vous pouvez utiliser l’accès à distance pour acheminer le trafic de réseau entre les sous-réseaux sur votre réseau local. Routage prend en charge pour les routeurs de traduction d’adresses réseau (NAT), les routeurs de réseau local en cours d’exécution BGP, protocole RIP (Routing Information) et routeurs multidiffusion à l’aide de la gestion de protocole IGMP (Internet Group). Comme un routeur complet, vous pouvez déployer RAS sur un ordinateur serveur ou comme une machine virtuelle (VM) sur un ordinateur qui exécute Hyper-V.

Pour installer l’accès à distance en tant que routeur de réseau local, soit utiliser l’ajout de rôles et fonctionnalités Assistant dans le Gestionnaire de serveur et sélectionnez le **accès à distance** rôle de serveur et le **routage** service de rôle ; ou tapez la ligne suivante commande à l’invite de Windows PowerShell, puis appuyez sur ENTRÉE.

```  
Install-RemoteAccess -VpnType RoutingOnly
```  

## <a name="bkmk_proxy"></a>Proxy d’Application Web

Proxy d’Application Web est un service de rôle accès à distance dans Windows Server 2016. Le proxy d’application Web fournit la fonctionnalité de proxy inverse pour les applications Web au sein de votre réseau d’entreprise pour permettre aux utilisateurs d’y accéder à partir de n’importe quel appareil à l’extérieur du réseau d’entreprise. Proxy d’Application Web authentifie au préalable les accès aux applications web à l’aide d’Active Directory Federation Services (ADFS) et fonctionne également comme un proxy AD FS.

Pour installer l’accès à distance comme un Proxy d’Application Web, soit utiliser l’ajout de rôles et fonctionnalités Assistant dans le Gestionnaire de serveur et sélectionnez le **accès à distance** rôle de serveur et le **Proxy d’Application Web** service de rôle ; ou Tapez la commande suivante à l’invite de Windows PowerShell, puis appuyez sur ENTRÉE.  

```  
Install-RemoteAccess -VpnType SstpProxy  
```  

Pour plus d’informations, consultez [Proxy d’Application Web](https://technet.microsoft.com/windows-server-docs/identity/web-application-proxy/web-application-proxy-windows-server).


---