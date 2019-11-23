---
title: Déployer un serveur NPS (Network Policy Server)
description: Cette rubrique fournit des liens vers le contenu de déploiement du serveur de stratégie réseau pour Windows Server 2016 et inclut des liens vers des conseils supplémentaires sur NPS.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 6cfb50e0-7088-4295-97c5-14ff8776cbf8
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 33cada472c314088bc1485bab6d9631226b0ffaf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405418"
---
# <a name="deploy-network-policy-server"></a>Déployer un serveur NPS (Network Policy Server)

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour obtenir des informations sur le déploiement du serveur NPS (Network Policy Server).

>[!NOTE]
>Pour obtenir une documentation supplémentaire sur le serveur de stratégie réseau, vous pouvez utiliser les sections suivantes de la bibliothèque.  
>- [Prise en main avec le serveur NPS (Network Policy Server)](nps-getstart-top.md)
>- [Planifier le serveur NPS (Network Policy Server)](nps-plan-top.md)
>- [Gérer le serveur NPS (Network Policy Server)](nps-manage-top.md)

Le Guide du réseau de base de Windows Server 2016 comprend une section sur la planification et l’installation du serveur NPS \(\), et les technologies présentées dans le guide servent de prérequis pour le déploiement de NPS dans un domaine Active Directory. Pour plus d’informations, consultez la section « deploy le serveur NPS1 » dans le Guide du [réseau de base](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide#BKMK_deployNPS1)de Windows Server 2016.

## <a name="deploy-nps-certificates-for-vpn-and-8021x-access"></a>Déployer des certificats NPS pour l’accès VPN et 802.1 X

Si vous souhaitez déployer des méthodes d’authentification comme le protocole EAP (Extensible Authentication Protocol \(EAP\) et le protocole EAP protégé qui nécessitent l’utilisation de certificats de serveur sur votre serveur NPS, vous pouvez déployer des certificats NPS avec le guide [déployer des certificats de serveur pour les déploiements sans fil et câblés 802.1 x](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments).

## <a name="deploy-nps-for-8021x-wireless-access"></a>Déployer un serveur NPS pour l’accès sans fil 802.1 X

Pour déployer NPS pour l’accès sans fil, vous pouvez utiliser le guide [déployer un accès sans fil authentifié 802.1 x basé sur un mot de passe](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/wireless/a-deploy-8021x-wireless-access).

## <a name="deploy-nps-for-windows-10-vpn-access"></a>Déployer NPS pour l’accès VPN Windows 10

Vous pouvez utiliser NPS pour traiter les demandes de connexion pour Always On réseau privé virtuel \(connexions VPN\) pour les employés distants qui utilisent des ordinateurs et des appareils exécutant Windows 10.

Pour plus d’informations, consultez le [Guide de déploiement de l’accès à distance Always on VPN pour Windows Server 2016 et Windows 10](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/always-on-vpn-deploy).

