---
title: Composants locaux de locataire
description: Décrit les composants locaux dans votre déploiement RDS.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 08/01/2016
ms.topic: article
ms.assetid: b3eebb38-a835-4fa6-9e41-1966014bf2cb
author: lizap
manager: dongill
ms.openlocfilehash: 849b0e3eb751c4e45a7c23da4230c7c4eb6bfcb1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854702"
---
# <a name="tenant-on-premises-components"></a>Composants locaux de locataire

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2019, Windows Server 2016

Les informations suivantes décrivent les composants locaux faisant partie d’un déploiement d’hébergement de postes de travail.  
  
##  <a name="clients"></a>Clients  
Pour accéder aux postes de travail et aux applications hébergés, les utilisateurs doivent utiliser les clients Bureau à distance prenant en charge le protocole RDP (Remote Desktop) 7.1 ou une version ultérieure. En particulier, le client doit prendre en charge la passerelle des services Bureau à distance et le service Broker pour les connexions Bureau à distance. Pour fournir des applications sur le poste de travail local, le client doit également en charge la fonctionnalité RemoteApp. Pour obtenir une mise à l’échelle de passerelle la plus élevée, le client doit prendre en charge les connexions de transport HTTP pures pour la passerelle des services Bureau à distance.  
  
Informations complémentaires :  
[Nouveautés de la passerelle Bureau à distance Windows Server 2012 R2](https://blogs.technet.microsoft.com/enterprisemobility/2013/03/14/whats-new-in-windows-server-2012-remote-desktop-gateway/#transport)  
[Clients Bureau à distance Microsoft](https://technet.microsoft.com/library/dn473009.aspx)  
[Application Bureau à distance pour Windows dans Microsoft Store](https://apps.microsoft.com/windows/app/remote-desktop/051f560e-5e9b-4dad-8b2e-fa5e0b05a480)  
[Bureau à distance Microsoft - Applications Android sur Google Play](https://play.google.com/store/apps/details?id=com.microsoft.rdc.android)  
[Mac App Store - Bureau à distance Microsoft](https://itunes.apple.com/app/microsoft-remote-desktop/id715768417?mt=12)  
[Bureau à distance Microsoft dans l’App Store](https://itunes.apple.com/app/microsoft-remote-desktop/id714464092?mt=8)  
  
##  <a name="active-directory-domain-services"></a>Services de domaine Active Directory  
Certains locataires plus volumineux et plus sophistiqués peuvent choisir d’héberger un serveur Services de domaine Active Directory (AD DS) dans leurs locaux. Dans ce cas, le serveur AD DS dans l’environnement du locataire sera généralement un réplica du serveur AD DS qui se trouve dans les locaux du client. Cela est pris en charge par la création d’un réseau virtuel au sein de l’environnement du locataire et l’utilisation de Azure VPN pour créer une connexion de site à site du locataire, depuis son réseau local vers son réseau virtuel dans le centre de données Azure.  
  
Informations complémentaires :  
[Vue d’ensemble du réseau virtuel Microsoft Azure](https://azure.microsoft.com/documentation/articles/virtual-networks-overview/)  
[Créer un réseau virtuel de gestionnaire de ressources avec une connexion VPN de site à site à l’aide du portail Azure](https://azure.microsoft.com/documentation/articles/vpn-gateway-howto-site-to-site-resource-manager-portal/)  


