---
ms.assetid: 74ef34c8-e13f-499b-b2bb-952ad7036622
title: Exigences relatives à la résolution de noms pour les serveurs de fédération
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: b455596910e1b04569e087843da8b88f9f020137
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853032"
---
# <a name="name-resolution-requirements-for-federation-servers"></a>Exigences relatives à la résolution de noms pour les serveurs de fédération

Lorsque des ordinateurs clients sur le réseau d’entreprise tentent d’accéder à une application ou à un service Web protégé par Services ADFS \(AD FS\), ils doivent d’abord s’authentifier auprès d’un serveur de Fédération. L’une des façons d’authentifier est que les clients du réseau d’entreprise accèdent à un serveur de Fédération local via l’authentification intégrée de Windows.  
  
## <a name="configure-corporate-dns"></a>Configurer le système DNS d’entreprise  
Pour que la résolution de noms réussie via l’authentification intégrée de Windows sur les serveurs de Fédération locaux puisse se produire, le système DNS (Domain Name System) \(DNS\) dans le réseau d’entreprise du partenaire de compte doit être configuré pour un nouvel hôte \(un\) enregistrement de ressource qui résout le nom de domaine complet \(nom d’hôte du serveur de Fédération en adresse IP du cluster de serveurs de\)  
  
L’illustration suivante présente l’accomplissement de cette tâche pour un scénario donné. Dans ce scénario, l’équilibrage de charge réseau Microsoft \(NLB\) fournit un nom de domaine complet de cluster unique et une adresse IP de cluster unique pour une batterie de serveurs de fédération existante.  
  
![exigences relatives aux noms](media/adfs2_deploy_single_fs.gif)  
  
Pour plus d’informations sur la configuration d’une adresse IP de cluster ou d’un nom de domaine complet de cluster à l’aide de NLB, consultez [spécification des paramètres de cluster](https://go.microsoft.com/fwlink/?LinkId=75282).  
  
Pour plus d’informations sur la configuration du DNS d’entreprise pour un serveur de Fédération, consultez [Ajouter un enregistrement de &#40;ressource hôte a&#41; à un serveur DNS d’entreprise pour un serveur de Fédération](../../ad-fs/deployment/Add-a-Host--A--Resource-Record-to-Corporate-DNS-for-a-Federation-Server.md).  
  
Pour plus d’informations sur la configuration des serveurs proxys de Fédération dans le réseau de périmètre, consultez [Configuration requise pour la résolution de noms pour les serveurs proxys de Fédération](Name-Resolution-Requirements-for-Federation-Server-Proxies.md).  
  

## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
