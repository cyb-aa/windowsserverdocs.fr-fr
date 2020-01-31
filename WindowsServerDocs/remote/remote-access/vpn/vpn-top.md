---
title: Réseau privé virtuel (VPN)
description: Vous pouvez utiliser cette rubrique pour en savoir plus sur les fonctionnalités et fonctionnalités VPN de Windows Server 2016 et Windows 10.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: cd4908f0-0d6f-4c02-8f98-4dc88c3dcb65
ms.date: 11/05/2018
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.openlocfilehash: fdd409dee5a7a957580daeeb05209336edfd86f6
ms.sourcegitcommit: 07c9d4ea72528401314e2789e3bc2e688fc96001
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76822612"
---
# <a name="virtual-private-networking-vpn"></a>Réseau privé virtuel (VPN)

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows 10

## <a name="ras-gateway-as-a-single-tenant-vpn-server"></a>Passerelle RAS en tant que serveur VPN à client unique

Dans Windows Server 2016, le rôle serveur d’accès à distance est un regroupement logique des technologies d’accès réseau associées suivantes.

- Service d’accès à distance (RAS)
- Routage
- Proxy d'application web

Ces technologies sont les services de rôle du rôle serveur accès à distance.

Lorsque vous installez le rôle serveur d’accès à distance avec l’Assistant Ajout de rôles et de fonctionnalités ou Windows PowerShell, vous pouvez installer un ou plusieurs de ces trois services de rôle.

Lorsque vous installez le service de rôle **DirectAccess et VPN (RAS)** , vous déployez la passerelle de service d’accès à distance (**passerelle RAS**). Vous pouvez déployer la passerelle RAS en tant que serveur de réseau privé virtuel (VPN) de passerelle RAS à client unique qui fournit de nombreuses fonctionnalités avancées et des fonctionnalités améliorées.

>[!NOTE]
>Vous pouvez également déployer la passerelle RAS en tant que serveur VPN mutualisée pour une utilisation avec SDN (Software Defined Networking) ou en tant que serveur DirectAccess. Pour plus d’informations, consultez [passerelle RAS](https://docs.microsoft.com/windows-server/remote/remote-access/ras-gateway/ras-gateway), [SDN (Software Defined Networking)](https://docs.microsoft.com/windows-server/networking/sdn/software-defined-networking)et [DirectAccess](https://docs.microsoft.com/windows-server/remote/remote-access/directaccess/directaccess).

## <a name="related-topics"></a>Rubriques connexes
- [Fonctionnalités et fonctionnalités de vpn Always on](vpn-map-da.md): dans cette rubrique, vous allez découvrir les fonctionnalités et fonctionnalités de Always on VPN. 

- [Configurer des tunnels de périphériques VPN dans Windows 10](vpn-device-tunnel-config.md): Always on VPN vous donne la possibilité de créer un profil VPN dédié pour l’appareil ou l’ordinateur. Always On les connexions VPN incluent deux types de tunnel : le tunnel de l' _appareil_ et le tunnel _utilisateur_. Le tunnel d’appareil est utilisé pour les scénarios de connectivité de pré-ouverture de session et de gestion des appareils. Le tunnel utilisateur permet aux utilisateurs d’accéder aux ressources de l’organisation via des serveurs VPN.

- [Always on déploiement VPN pour Windows Server 2016 et Windows 10](always-on-vpn/deploy/always-on-vpn-deploy.md): fournit des instructions sur le déploiement de l’accès à distance en tant que passerelle RAS VPN à client unique pour les connexions VPN de point à site qui permettent à vos employés distants de se connecter au réseau de votre organisation avec Always on connexions VPN. Nous vous recommandons de consulter les guides de conception et de déploiement pour chacune des technologies utilisées dans ce déploiement.

- [Guide technique du VPN Windows 10](https://docs.microsoft.com/windows/access-protection/vpn/vpn-guide): vous guide tout au long des décisions que vous allez prendre pour les clients Windows 10 dans votre solution VPN d’entreprise et la configuration de votre déploiement. Vous pouvez trouver des références au fournisseur de services de configuration VPNv2 et fournit des instructions de configuration de la gestion des appareils mobiles (MDM) à l’aide de Microsoft Intune et du modèle de profil VPN pour Windows 10.

- [Comment créer des profils VPN dans Configuration Manager](https://docs.microsoft.com/configmgr/protect/deploy-use/create-vpn-profiles): dans cette rubrique, vous allez apprendre à créer des profils vpn dans Configuration Manager.

- [Configurer le client Windows 10 Always on les connexions VPN](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/vpn-deploy-client-vpn-connections): cette rubrique décrit les options et le schéma de ProfileXML et explique comment créer le VPN ProfileXML. Après la configuration de l’infrastructure de serveur, vous devez configurer les ordinateurs clients Windows 10 pour qu’ils communiquent avec cette infrastructure avec une connexion VPN.

- [Options de profil VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-profile-options): cette rubrique décrit les paramètres de profil VPN dans Windows 10 et explique comment configurer des profils VPN à l’aide d’Intune ou de Configuration Manager. Vous pouvez configurer tous les paramètres VPN dans Windows 10 à l’aide du nœud ProfileXML dans le CSP VPNv2.
