---
title: Composants locaux de locataire
description: Décrit les composants en local dans votre déploiement des services Bureau à distance.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 08/01/2016
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b3eebb38-a835-4fa6-9e41-1966014bf2cb
author: lizap
manager: dongill
ms.openlocfilehash: a01dbd12d76b1efa84e38f2ded38cfd613fb2ac4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857400"
---
# <a name="tenant-on-premises-components"></a>Composants locaux de locataire

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Les informations suivantes décrivent les composants sur site qui composent le déploiement d’hébergement de bureau.  
  
##  <a name="clients"></a>Clients  
Pour accéder aux postes de travail hébergés et les applications, les utilisateurs doivent utiliser les clients Bureau à distance qui prennent en charge le protocole RDP (Remote Desktop) 7.1 ou version ultérieure. En particulier, le client doit prendre en charge passerelle des services Bureau à distance et service Broker pour les connexions Bureau à distance. Pour fournir des applications sur le bureau local, le client doit également en charge la fonctionnalité de RemoteApp. Pour obtenir un dimensionnement de passerelle la plus élevée, le client doit prendre en charge les connexions de transport HTTP pures pour la passerelle Bureau à distance.  
  
Informations complémentaires :  
[Appareils compatibles de RemoteFX](https://social.technet.microsoft.com/wiki/contents/articles/14534.remotefx-enabled-devices.aspx)  
[Quelles sont les nouveautés dans Windows Server 2012 R2 à distance passerelle des services Bureau](https://blogs.technet.microsoft.com/enterprisemobility/2013/03/14/whats-new-in-windows-server-2012-remote-desktop-gateway/#transport)  
[Clients de bureau à distance Microsoft](https://technet.microsoft.com/library/dn473009.aspx)  
[Application de bureau à distance pour Windows dans Microsoft Store](https://apps.microsoft.com/windows/app/remote-desktop/051f560e-5e9b-4dad-8b2e-fa5e0b05a480)  
[Bureau à distance Microsoft - applications Android sur Google Play](https://play.google.com/store/apps/details?id=com.microsoft.rdc.android)  
[Mac App Store - Bureau à distance Microsoft](https://itunes.apple.com/us/app/microsoft-remote-desktop/id715768417?mt=12)  
[Bureau à distance Microsoft dans l’App Store](https://itunes.apple.com/us/app/microsoft-remote-desktop/id714464092?mt=8)  
  
##  <a name="active-directory-domain-services"></a>Services de domaine Active Directory  
Certains clients plus volumineux et plus sophistiqués peuvent choisir d’héberger un serveur de Services de domaine Active Directory (AD DS) dans leurs locaux. Dans ce cas, le serveur AD DS dans un environnement du locataire sera généralement un réplica du serveur AD DS qui se trouve sur les locaux du client. Cela est pris en charge par la création d’un réseau virtuel dans un environnement du locataire et à l’aide de la connexion VPN Azure pour créer une connexion de site à site du locataire sur un réseau local à réseau virtuel du locataire dans le centre de données Azure.  
  
Informations complémentaires :  
[Vue d’ensemble du réseau virtuel Microsoft Azure](https://azure.microsoft.com/documentation/articles/virtual-networks-overview/)  
[Créer une ressource Gestionnaire de réseau virtuel avec une connexion VPN de Site à l’aide du portail Azure](https://azure.microsoft.com/documentation/articles/vpn-gateway-howto-site-to-site-resource-manager-portal/)  


