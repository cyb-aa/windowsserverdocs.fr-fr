---
title: Réseau privé virtuel (VPN)
description: Vous pouvez utiliser cette rubrique pour en savoir plus sur les fonctionnalités de Windows Server 2016 et VPN Windows 10.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: cd4908f0-0d6f-4c02-8f98-4dc88c3dcb65
ms.date: 11/05/2018
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.openlocfilehash: bf38995f0a2b396044d1f45b45eff8c3c2de329d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59891070"
---
# <a name="virtual-private-networking-vpn"></a>Un réseau privé virtuel \(VPN\)

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows 10

## <a name="ras-gateway-as-a-single-tenant-vpn-server"></a>Passerelle RAS comme un serveur VPN de client unique

Dans Windows Server 2016, le rôle de serveur d’accès à distance est un regroupement logique des technologies d’accès réseau associées suivantes.

- Service d’accès à distance (RAS)
- Routage
- Proxy d'application web

Ces technologies sont les services de rôle du rôle de serveur d’accès à distance.

Lorsque vous installez le rôle de serveur d’accès à distance avec l’ajout de rôles et de fonctionnalités Assistant ou de Windows PowerShell, vous pouvez installer un ou plusieurs de ces services de rôle de trois.

Lorsque vous installez le **DirectAccess et VPN (RAS)** service de rôle, vous déployez la passerelle de Service d’accès à distance \( **passerelle RAS**\). Vous pouvez déployer la passerelle RAS comme un réseau privé virtuel client unique passerelle RAS \(VPN\) serveur qui fournit de nombreuses fonctionnalités avancées et des fonctionnalités améliorées.

>[!NOTE]
>Vous pouvez également déployer la passerelle RAS comme un serveur VPN de mutualisée pour une utilisation avec Sdn \(SDN\), ou comme un serveur DirectAccess. Pour plus d’informations, consultez [passerelle RAS](https://docs.microsoft.com/windows-server/remote/remote-access/ras-gateway/ras-gateway), [mise en réseau SDN (Software Defined)](https://docs.microsoft.com/windows-server/networking/sdn/software-defined-networking), et [DirectAccess](https://docs.microsoft.com/windows-server/remote/remote-access/directaccess/directaccess).

## <a name="related-topics"></a>Rubriques connexes
- [Les fonctionnalités de VPN Always On](vpn-map-da.md): Dans cette rubrique, vous en savoir plus sur les fonctionnalités de VPN Always On. 

- [Configurer des Tunnels de périphérique VPN dans Windows 10](vpn-device-tunnel-config.md): VPN Always On vous donne la possibilité de créer un profil VPN dédié pour le périphérique ou ordinateur. Les connexions VPN Always On incluent deux types de tunnels : _tunnel de l’appareil_ et _tunnel de l’utilisateur_. Tunnel de l’appareil est utilisé pour les scénarios de connectivité d’avant ouverture de session et à des fins de gestion de périphérique. Tunnel de l’utilisateur permet aux utilisateurs d’accéder aux ressources de l’organisation via les serveurs VPN.

- [Toujours sur le déploiement de VPN pour Windows Server 2016 et Windows 10](always-on-vpn/deploy/always-on-vpn-deploy.md): Fournit des instructions sur le déploiement de l’accès à distance en tant qu’un seul locataire passerelle RAS de VPN de point de\-à\-permettent à vos employés à distance pour se connecter à votre réseau d’entreprise avec les connexions VPN Always On de connexions VPN de site. Il est recommandé de consulter les guides de conception et de déploiement pour chacune des technologies qui sont utilisés dans ce déploiement.

- [Guide technique de Windows 10 VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-guide): Vous guide dans les décisions à que prendre pour les clients Windows 10 dans votre solution VPN d’entreprise et comment configurer votre déploiement. Vous pouvez rechercher les références pour le fournisseur de services de Configuration (CSP) VPNv2 et fournit des instructions de configuration (MDM) à l’aide de Microsoft Intune et le modèle de profil VPN pour Windows 10 de gestion des appareils mobiles.

- [Comment créer des profils VPN dans System Center Configuration Manager](https://docs.microsoft.com/sccm/protect/deploy-use/create-vpn-profiles): Dans cette rubrique, vous allez apprendre à créer des profils VPN dans System Center Configuration Manager (SCCM).

- [Configurer les clients Windows 10 toujours sur les connexions VPN](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/vpn-deploy-client-vpn-connections): Cette rubrique décrit la ProfileXML options et de schéma et comment créer le VPN ProfileXML. Après avoir configuré l’infrastructure de serveur, vous devez configurer les ordinateurs clients Windows 10 pour communiquer avec cette infrastructure avec une connexion VPN. 

- [Options de profil VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-profile-options): Cette rubrique décrit les paramètres de profil VPN dans Windows 10 et découvrez comment configurer des profils VPN à l’aide d’Intune ou SCCM. Vous pouvez configurer tous les paramètres VPN dans Windows 10 à l’aide du nœud ProfileXML dans le VPNv2 CSP.

---