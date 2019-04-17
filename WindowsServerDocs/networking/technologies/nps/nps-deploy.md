---
title: Déployer le serveur de stratégie réseau
description: Cette rubrique fournit des liens vers le contenu de déploiement de serveur de stratégie réseau pour Windows Server2016 et inclut des liens vers des informations supplémentaires sur le serveur NPS.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 6cfb50e0-7088-4295-97c5-14ff8776cbf8
ms.author: pashort
author: shortpatti
ms.openlocfilehash: daf62e2a593014e16bcb5fd16542b2e9c75b9c1d
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="deploy-network-policy-server"></a>Déployer le serveur de stratégie réseau

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette rubrique pour plus d’informations sur le déploiement de serveur de stratégie réseau.

>[!NOTE]
>Pour la documentation supplémentaire sur le serveur de stratégie réseau, vous pouvez utiliser les sections suivantes de la bibliothèque.  
>- [Prise en main de serveur de stratégie réseau](nps-getstart-top.md)
>- [Planifier le serveur de stratégie réseau](nps-plan-top.md)
>- [Gérer le serveur de stratégie réseau](nps-manage-top.md)

Le Guide du réseau Windows Server2016 Core comprend une section sur la planification et l’installation du serveur de stratégie réseau \(NPS\), et les technologies présentées dans le guide servent des conditions préalables au déploiement du serveur NPS dans un domaine ActiveDirectory. Pour plus d’informations, consultez la section «Déployer NPS1» dans Windows Server2016 [Guide réseau de base](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide#BKMK_deployNPS1).

## <a name="deploy-nps-server-certificates-for-vpn-and-8021x-access"></a>Déployer des certificats de serveur NPS pour VPN et d’accès de 802. 1 X

Si vous souhaitez déployer des méthodes d’authentification qui nécessitent l’utilisation de certificats de serveur sur votre serveur NPS comme Extensible Authentication Protocol \(EAP\) et EAP protégé, vous pouvez déployer des certificats de serveur NPS avec le guide [déployer des certificats de serveur pour 802.1 X câblé et sans fil déploiements](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments).

## <a name="deploy-nps-for-8021x-wireless-access"></a>Déployer un serveur NPS pour 802. 1 X accès sans fil

Pour déployer NPS pour l’accès sans fil, vous pouvez utiliser le guide [déployer le mot de passe accès 802. 1 X authentifié sans fil](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/wireless/a-deploy-8021x-wireless-access).

## <a name="deploy-nps-for-windows-10-vpn-access"></a>Déployer un serveur NPS pour l’accès VPN de Windows10

Vous pouvez utiliser le serveur NPS pour traiter les demandes de connexion pour les connexions \(VPN\) toujours de réseau privé virtuel pour les employés distants qui utilisent des ordinateurs et les appareils exécutant Windows10.

Pour plus d’informations, voir la [à distance accès toujours sur VPN déploiement Guide pour Windows Server2016 et Windows10](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/always-on-vpn-deploy).

