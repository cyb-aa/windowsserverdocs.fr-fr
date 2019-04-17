---
title: Réseau privé virtuel (VPN)
description: Vous pouvez utiliser cette rubrique pour en savoir plus sur les fonctions et fonctionnalités de Windows Server 2016 et VPN Windows 10.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: cd4908f0-0d6f-4c02-8f98-4dc88c3dcb65
ms.date: 11/05/2018
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.openlocfilehash: bf38995f0a2b396044d1f45b45eff8c3c2de329d
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067120"
---
# \(VPN\) de réseau privé virtuel

>S’applique à: WindowsServer (canal semi-annuel), WindowsServer2016, Windows10

## Passerelle RAS comme serveur VPN seul locataire

Dans Windows Server 2016, le rôle de serveur d’accès à distance est un regroupement logique des technologies d’accès réseau associés suivants.

- Service d’accès à distance (RAS)
- Routage
- Proxy d'application web

Ces technologies sont les services de rôle du rôle de serveur accès à distance.

Lorsque vous installez le rôle de serveur d’accès à distance avec l’ajout de rôles et de fonctionnalités Assistant ou de Windows PowerShell, vous pouvez installer une ou plusieurs de ces services de trois rôle.

Lorsque vous installez le service de rôle **DirectAccess et VPN (RAS)** , vous déployez la passerelle de Service d’accès à distance \ (**Passerelle RAS**\). Vous pouvez déployer la passerelle RAS comme un seul locataire RAS réseau privé virtuel \(VPN\) serveur de passerelle fournit de nombreuses fonctionnalités avancées et des fonctionnalités améliorées.

>[!NOTE]
>Vous pouvez également déployer la passerelle RAS comme un serveur VPN multi-locataire pour une utilisation avec Software Defined Networking \(SDN\) ou comme un serveur DirectAccess. Pour plus d’informations, voir [RAS passerelle](https://docs.microsoft.com/windows-server/remote/remote-access/ras-gateway/ras-gateway), [Logiciel défini mise en réseau SDN ()](https://docs.microsoft.com/windows-server/networking/sdn/software-defined-networking)et [DirectAccess](https://docs.microsoft.com/windows-server/remote/remote-access/directaccess/directaccess).

## Rubriquesconnexes
- [Des fonctionnalités et toujours actif (AlwaysOn)](vpn-map-da.md): dans cette rubrique, vous en savoir plus sur les fonctionnalités de toujours actif (AlwaysOn). 

- [Configurer des Tunnels d’appareil VPN dans Windows 10](vpn-device-tunnel-config.md): toujours actif (AlwaysOn) vous donne la possibilité de créer un profil VPN dédié pour appareil ou un ordinateur. Connexions toujours actif (AlwaysOn) incluent deux types de tunnels: _tunnel de périphérique_ et de _tunnel de l’utilisateur_. Tunnel de périphérique est utilisé pour les scénarios de connexion avant ouverture de session et à des fins de gestion de périphérique. Tunnel utilisateur permet aux utilisateurs d’accéder aux ressources de l’organisation par le biais de serveurs VPN.

- [Toujours sur déploiement VPN pour Windows Server 2016 et Windows 10](always-on-vpn/deploy/always-on-vpn-deploy.md): fournit des instructions sur le déploiement d’accès à distance en tant qu’un seul client VPN RAS passerelle pour les connexions VPN point\-à\-site qui permettent à vos employés à distance pour se connecter à votre organisation réseau avec des connexions toujours actif (AlwaysOn). Il est recommandé de consulter les guides de conception et de déploiement pour chacune des technologies qui sont utilisés dans ce déploiement.

- [Guide technique du VPN Windows 10](https://docs.microsoft.com/windows/access-protection/vpn/vpn-guide): vous guide à travers les décisions que vous créerez pour les clients Windows 10 dans votre solution VPN d’entreprise et la configuration de votre déploiement. Vous trouverez des références pour le fournisseur de services de Configuration (CSP) VPNv2 et fournit des instructions de configuration (MDM) à l’aide de Microsoft Intune et le modèle de profil VPN pour Windows 10 de gestion des périphériques mobiles.

- [Comment créer un VPN profils dans System Center Configuration Manager](https://docs.microsoft.com/sccm/protect/deploy-use/create-vpn-profiles): dans cette rubrique, vous apprendre à créer des profils VPN dans System Center Configuration Manager (SCCM).

- [Configurer Windows 10 toujours sur les connexions VPN Client](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/vpn-deploy-client-vpn-connections): cette rubrique décrit la ProfileXML options et du schéma et comment créer le VPN ProfileXML. Après avoir configuré l’infrastructure de serveur, vous devez configurer les ordinateurs clients de Windows 10 pour communiquer avec cette infrastructure avec une connexion VPN. 

- [Options de profil VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-profile-options): cette rubrique décrit les paramètres de profil VPN dans Windows 10 et découvrez comment configurer les profils VPN à l’aide de Intune ou SCCM. Vous pouvez configurer tous les paramètres VPN dans Windows 10 à l’aide du nœud ProfileXML dans le CSP VPNv2.

---