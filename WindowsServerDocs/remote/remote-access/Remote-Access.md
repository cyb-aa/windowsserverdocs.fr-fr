---
title: Accès à distance
description: Cette rubrique fournit une vue d’ensemble du rôle de serveur d’accès à distance dans Windows Server 2016.
manager: dougkim
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: eeca4cf7-90f0-485d-843c-76c5885c54b0
ms.author: lizross
author: eross-msft
ms.date: 05/18/2018
ms.openlocfilehash: dbcd0380dffca29e782be2179024270da73a2c11
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80309414"
---
# <a name="remote-access"></a>Accès à distance

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows 10

Le Guide d’accès à distance fournit une vue d’ensemble du rôle de serveur d’accès à distance dans Windows Server 2016 et aborde les sujets suivants :

- [Guide de déploiement de Always On VPN](vpn/always-on-vpn/deploy/always-on-vpn-deploy.md)
- [Border Gateway Protocol &#40;BGP&#41;](bgp/Border-Gateway-Protocol-BGP.md)
- [Passerelle du serveur d’accès à distance](ras-gateway/RAS-Gateway.md) 
- [Documentation du rôle serveur accès à distance](ras/Remote-Access-Server-Role-Documentation.md)
- [Passerelle du serveur d’accès à distance pour SDN](../../networking/sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)
- [Réseau privé virtuel (VPN)](vpn/vpn-top.md)
 
Pour plus d’informations sur les autres technologies de mise en réseau, voir [mise en réseau dans Windows Server 2016](https://docs.microsoft.com/windows-server/networking/networking).

Le rôle serveur d’accès à distance est un regroupement logique des technologies d’accès réseau associées : service d’accès [à distance (RAS)](#bkmk_da), [routage](#bkmk_rras)et [proxy d’application Web](#bkmk_proxy). Ces technologies sont les *services de rôle* du rôle serveur Accès à distance. Lorsque vous installez le rôle serveur d’accès à distance avec l' **Assistant Ajout de rôles et de fonctionnalités** ou Windows PowerShell, vous pouvez installer un ou plusieurs de ces trois services de rôle.

>[!IMPORTANT]
>N’essayez pas de déployer l’accès à distance sur une machine virtuelle \(\) de machines virtuelles dans Microsoft Azure. L’utilisation de l’accès à distance dans Microsoft Azure n’est pas prise en charge. Vous ne pouvez pas utiliser l’accès à distance dans une machine virtuelle Azure pour déployer un VPN, DirectAccess ou une autre fonctionnalité d’accès à distance dans Windows Server 2016 ou des versions antérieures de Windows Server. Pour plus d’informations, consultez [prise en charge des logiciels serveur Microsoft pour les machines virtuelles Microsoft Azure](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines).

## <a name="remote-access-service-ras---ras-gateway"></a><a name="bkmk_da"></a>Service d’accès à distance \(la passerelle RAS\)-RAS

Lorsque vous installez le service de rôle **DirectAccess et VPN (RAS)** , vous déployez la passerelle de service d’accès à distance \(la **passerelle RAS**\). Vous pouvez déployer la passerelle RAS sur un réseau privé virtuel de passerelle RAS à client unique \(VPN\) serveur, un serveur VPN de passerelle RAS mutualisée et un serveur DirectAccess.

- **Passerelle RAS-locataire unique**. À l’aide de la passerelle RAS, vous pouvez déployer des connexions VPN pour fournir aux utilisateurs finaux un accès à distance au réseau et aux ressources de votre organisation. Si vos clients exécutent Windows 10, vous pouvez déployer Always On VPN, qui maintient une connexion permanente entre les clients et le réseau de votre organisation chaque fois que des ordinateurs distants sont connectés à Internet. Avec la passerelle RAS, vous pouvez également créer une connexion VPN de site à site entre deux serveurs situés à différents emplacements, par exemple entre votre bureau principal et une filiale, et utiliser la traduction d’adresses réseau \(\) NAT afin que les utilisateurs du réseau puissent accéder à des ressources externes, telles qu’Internet. En outre, la passerelle RAS prend en charge Border Gateway Protocol (BGP), qui fournit des services de routage dynamique lorsque vos sites distants disposent également de passerelles de périphérie qui prennent en charge le protocole BGP.

- **Passerelle RAS-mutualisée**. Vous pouvez déployer la passerelle RAS comme un routeur et une passerelle de périphérie basée sur des logiciels si vous utilisez la virtualisation de réseau hyper\-V ou si vous avez déployé des réseaux de machines virtuelles avec des réseaux locaux virtuels \(des réseaux locaux virtuels\). Avec la passerelle RAS, les fournisseurs de services Cloud \(CSP\) et les entreprises peuvent activer le routage du trafic réseau entre les réseaux virtuels et physiques, y compris Internet. Avec la passerelle RAS, vos locataires peuvent utiliser des connexions VPN point à site pour accéder à leurs ressources réseau de machines virtuelles dans le centre de ressources depuis n’importe où. Vous pouvez également fournir aux locataires des connexions VPN de site à site entre leurs sites distants et votre centre de donnes CSP. En outre, vous pouvez configurer la passerelle RAS avec le protocole BGP pour le routage dynamique, et vous pouvez activer la traduction d’adresses réseau \(\) NAT pour fournir un accès Internet aux machines virtuelles sur les réseaux d’ordinateurs virtuels.

>[!IMPORTANT]
> La passerelle RAS avec des fonctionnalités multi-locataires est également disponible dans Windows Server 2012 R2.

- **Always on VPN**. Always On VPN permet aux utilisateurs distants d’accéder en toute sécurité aux ressources partagées, aux sites Web intranet et aux applications sur un réseau interne sans se connecter à un VPN. 

Pour plus d’informations, consultez [passerelle RAS](ras-gateway/RAS-Gateway.md) et [Border Gateway Protocol (BGP)](bgp/Border-Gateway-Protocol-BGP.md).

## <a name="routing"></a><a name="bkmk_rras"></a>Achemine

Vous pouvez utiliser l’accès à distance pour router le trafic réseau entre les sous-réseaux de votre réseau local. Le routage prend en charge les routeurs de traduction d’adresses réseau (NAT), les routeurs LAN exécutant BGP, le protocole RIP (Routing Information Protocol) et les routeurs compatibles avec la multidiffusion à l’aide du protocole IGMP (Internet Group Management Protocol). En tant que routeur complet, vous pouvez déployer le service d’accès à distance sur un ordinateur serveur ou en tant que machine virtuelle sur un ordinateur exécutant Hyper-V.

Pour installer l’accès à distance en tant que routeur de réseau local, utilisez l’Assistant Ajout de rôles et de fonctionnalités dans Gestionnaire de serveur et sélectionnez le rôle serveur d' **accès à distance** et le service de rôle **routage** . ou tapez la commande suivante à une invite Windows PowerShell, puis appuyez sur entrée.

```  
Install-RemoteAccess -VpnType RoutingOnly
```  

## <a name="web-application-proxy"></a><a name="bkmk_proxy"></a>Proxy d’application Web

Le proxy d’application Web est un service de rôle accès à distance dans Windows Server 2016. Le proxy d’application Web fournit la fonctionnalité de proxy inverse pour les applications Web au sein de votre réseau d’entreprise pour permettre aux utilisateurs d’y accéder à partir de n’importe quel appareil à l’extérieur du réseau d’entreprise. Le proxy d’application Web pré-authentifie l’accès aux applications Web à l’aide de Services ADFS (AD FS) et fonctionne également comme un proxy AD FS.

Pour installer l’accès à distance en tant que proxy d’application Web, utilisez l’Assistant Ajout de rôles et de fonctionnalités dans Gestionnaire de serveur et sélectionnez le rôle de serveur d' **accès à distance** et le service de rôle **proxy d’application Web** . ou tapez la commande suivante à une invite Windows PowerShell, puis appuyez sur entrée.  

```  
Install-RemoteAccess -VpnType SstpProxy  
```  

Pour plus d’informations, voir [proxy d’application Web](https://technet.microsoft.com/windows-server-docs/identity/web-application-proxy/web-application-proxy-windows-server).


---