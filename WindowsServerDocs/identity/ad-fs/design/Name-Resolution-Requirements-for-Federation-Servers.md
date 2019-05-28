---
ms.assetid: 74ef34c8-e13f-499b-b2bb-952ad7036622
title: Exigences relatives à la résolution de noms pour les serveurs de fédération
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e2776cc29b8c9ede884a6b304cd541f700f516ca
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191259"
---
# <a name="name-resolution-requirements-for-federation-servers"></a>Exigences relatives à la résolution de noms pour les serveurs de fédération

Lorsque les ordinateurs clients sur le réseau d’entreprise essaient d’accéder à une application ou un service Web qui est protégé par Active Directory Federation Services \(AD FS\), ils doivent s’authentifier tout d’abord à un serveur de fédération. Une méthode d’authentification consiste à avoir les clients du réseau d’entreprise à accéder à un serveur de fédération local via l’authentification intégrée Windows.  
  
## <a name="configure-corporate-dns"></a>Configurer le système DNS d’entreprise  
Pour que la résolution de noms réussie via l’authentification intégrée de Windows sur les serveurs de fédération local peut se produire, Domain Name System \(DNS\) du réseau d’entreprise du compte partenaire doit être configuré pour un nouvel hôte \(A\) enregistrement de ressource qui résout le nom de domaine pleinement qualifié \(FQDN\) nom d’hôte du serveur de fédération à l’adresse IP du cluster de serveurs de fédération.  
  
L’illustration suivante présente l’accomplissement de cette tâche pour un scénario donné. Dans ce scénario, l’équilibrage de charge réseau Microsoft \(NLB\) fournit un nom de domaine complet de cluster unique et une adresse IP de cluster unique pour une batterie de serveurs de fédération existante.  
  
![exigences relatives aux noms](media/adfs2_deploy_single_fs.gif)  
  
Pour plus d’informations sur la façon de configurer une adresse IP de cluster ou cluster de nom de domaine complet à l’aide de NLB, consultez [en spécifiant les paramètres de Cluster](https://go.microsoft.com/fwlink/?LinkId=75282).  
  
Pour plus d’informations sur la configuration DNS d’entreprise pour un serveur de fédération, consultez [ajouter un hôte &#40;A&#41; enregistrement de ressource au DNS d’entreprise pour un serveur de fédération](../../ad-fs/deployment/Add-a-Host--A--Resource-Record-to-Corporate-DNS-for-a-Federation-Server.md).  
  
Pour plus d’informations sur la configuration des serveurs proxy de fédération dans le réseau de périmètre, consultez [Name Resolution Requirements for Federation Server Proxies](Name-Resolution-Requirements-for-Federation-Server-Proxies.md).  
  

## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
