---
ms.assetid: 74ef34c8-e13f-499b-b2bb-952ad7036622
title: "Exigences de résolution de nom pour les serveurs de fédération"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 74701cbaa403611b081942f016b21db1c0b3ff70
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="name-resolution-requirements-for-federation-servers"></a>Exigences de résolution de nom pour les serveurs de fédération

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Si les ordinateurs clients sur le réseau d’entreprise tentent d’accéder à une application ou un service Web qui est protégé par Active Directory Federation Services \(AD FS\), ils doivent d’abord s’authentifier un serveur de fédération. Une méthode d’authentification est pour que les clients du réseau d’entreprise à accéder à un serveur de fédération local via l’authentification Windows intégrée.  
  
## <a name="configure-corporate-dns"></a>Configurer le système DNS d’entreprise  
Pour que la résolution de noms correctement via l’authentification intégrée Windows sur les serveurs de fédération local peut se produire, \(DNS\) système de nom de domaine dans le réseau d’entreprise du partenaire de compte doit être configuré pour un nouvel enregistrement de ressource \(A\) de l’hôte qui se résoudra le nom d’hôte \(FQDN\) de nom de domaine complet du serveur de fédération à l’adresse IP du cluster de serveurs de fédération.  
  
Dans l’illustration suivante, vous pouvez voir comment cette tâche est effectuée pour un scénario donné. Dans ce scénario, \(NLB\) l’équilibrage de charge réseau Microsoft fournit un nom de domaine complet de cluster unique et une adresse IP de cluster unique pour une batterie de serveurs de fédération existante.  
  
![exigences relatives au nom](media/adfs2_deploy_single_fs.gif)  
  
Pour plus d’informations sur la façon de configurer une adresse IP de cluster ou le nom de domaine complet à l’aide d’équilibrage de charge réseau de cluster, voir [spécifiant les paramètres de Cluster](https://go.microsoft.com/fwlink/?LinkId=75282).  
  
Pour plus d’informations sur la configuration DNS d’entreprise pour un serveur de fédération, consultez [ajouter un ordinateur hôte & #40; A & #41; Enregistrement de ressource au DNS d’entreprise pour un serveur de fédération](../../ad-fs/deployment/Add-a-Host--A--Resource-Record-to-Corporate-DNS-for-a-Federation-Server.md).  
  
Pour plus d’informations sur la configuration des serveurs proxy de fédération dans le réseau de périmètre, consultez [Name Resolution Requirements for Federation Server Proxies](Name-Resolution-Requirements-for-Federation-Server-Proxies.md).  
  

## <a name="see-also"></a>Voir aussi
[Guide de conception ADFS dans Windows Server2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
