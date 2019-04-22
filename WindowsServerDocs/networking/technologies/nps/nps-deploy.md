---
title: Déployer un serveur NPS (Network Policy Server)
description: Cette rubrique fournit des liens vers du contenu de déploiement de serveur NPS pour Windows Server 2016 et inclut des liens vers des conseils supplémentaires sur le serveur NPS.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 6cfb50e0-7088-4295-97c5-14ff8776cbf8
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8da8951a9c6ed5022c892bbf01b33614d38abc5f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814840"
---
# <a name="deploy-network-policy-server"></a>Déployer un serveur NPS (Network Policy Server)

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour plus d’informations sur le déploiement de serveur NPS.

>[!NOTE]
>Pour plus de documentation de serveur NPS, vous pouvez utiliser les sections suivantes de la bibliothèque.  
>- [Mise en route avec le serveur NPS](nps-getstart-top.md)
>- [Planifier le serveur NPS](nps-plan-top.md)
>- [Gérer le serveur de stratégie réseau](nps-manage-top.md)

Le Guide du réseau Windows Server 2016 Core inclut une section sur la planification et l’installation du serveur de stratégie réseau \(NPS\), et les technologies présentées dans le guide servent de conditions préalables au déploiement de serveur NPS dans un domaine Active Directory. Pour plus d’informations, consultez la section « Déployer serveur NPS1 » dans Windows Server 2016 [Guide du réseau](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide#BKMK_deployNPS1).

## <a name="deploy-nps-certificates-for-vpn-and-8021x-access"></a>Déployer des certificats de serveur NPS VPN et accès de 802. 1 X

Si vous souhaitez déployer des méthodes d’authentification comme protocole d’authentification Extensible \(EAP\) et EAP protégé qui nécessitent l’utilisation du serveur de certificats sur le serveur NPS, vous pouvez déployer des certificats de serveur NPS avec le guide [ Déployer des certificats de serveur pour 802. 1 X câblés et sans fil déploiements](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments).

## <a name="deploy-nps-for-8021x-wireless-access"></a>Déployer NPS pour 802. 1 X accès sans fil

Pour déployer NPS pour l’accès sans fil, vous pouvez utiliser le guide [par mot de passe déployer accès 802. 1 X authentifié sans fil](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/wireless/a-deploy-8021x-wireless-access).

## <a name="deploy-nps-for-windows-10-vpn-access"></a>Déployer NPS pour l’accès VPN Windows 10

Vous pouvez utiliser NPS pour traiter les demandes de connexion pour toujours sur réseau privé virtuel \(VPN\) connexions pour les employés distants qui utilisent des ordinateurs et périphériques exécutant Windows 10.

Pour plus d’informations, consultez le [distant accès toujours sur VPN déploiement Guide pour Windows Server 2016 et Windows 10](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/always-on-vpn-deploy).

