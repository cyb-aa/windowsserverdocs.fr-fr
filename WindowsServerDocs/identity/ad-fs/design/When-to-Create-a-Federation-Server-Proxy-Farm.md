---
ms.assetid: ad0bf21d-2ace-4565-b1f5-ce57c8eb2689
title: "Création d’une batterie de serveurs de fédération Proxy"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 8935760cad272d5b82edb675cda85caf0456565f
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="when-to-create-a-federation-server-proxy-farm"></a>Création d’une batterie de serveurs de fédération Proxy

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Envisagez d’installer des serveurs proxy de fédération supplémentaires lorsque vous disposez d’un déploiement important plus \(AD FS\) Active Directory Federation Services et que vous souhaitez fournir une tolérance de panne, l’équilibrage de la load\ et l’évolutivité pour votre déploiement de proxy. Le fait de la création des serveurs proxy de fédération deux ou plusieurs dans le même réseau de périmètre et la configuration de chacun d’eux pour protéger le même Service de fédération AD FS crée une batterie de serveurs de fédération proxy.  
  
Vous pouvez créer une batterie de proxy de serveur de fédération ou installer des serveurs proxy de fédération supplémentaires à une batterie existante à l’aide de l’Assistant Configuration Proxy des serveurs de fédération AD FS. Pour plus d’informations, voir [quand créer un serveur Proxy de fédération](When-to-Create-a-Federation-Server-Proxy.md).  
  
Avant de tous les serveurs proxy de fédération puissent fonctionner ensemble comme une batterie de serveurs, vous devez tout d’abord les mettre en cluster sous une seule adresse IP et un domaine complet de système de nom de domaine \(DNS\) \(FQDN\) de nom. Vous pouvez mettre en cluster les serveurs en déployant l’équilibrage de charge réseau Microsoft \(NLB\) à l’intérieur du réseau de périmètre. Les tâches dans le tableau suivant nécessitent NLB être correctement configurées pour les serveurs proxy de fédération dans la batterie de serveurs du cluster.  
  
Pour plus d’informations sur la configuration d’un nom de domaine complet pour un cluster à l’aide de la technologie Microsoft NLB, consultez [spécifiant les paramètres de Cluster](https://go.microsoft.com/fwlink/?linkid=74651).  
  
## <a name="configuring-federation-server-proxies-for-a-farm"></a>Configuration des serveurs proxy de fédération pour une batterie de serveurs  
Le tableau suivant décrit les tâches qui doivent être effectuées pour que chaque serveur proxy de fédération peut participer à une batterie de serveurs.  
  
|Tâche|Description|  
|--------|---------------|  
|Pointez tous les proxys de la batterie de serveurs sur le même nom de Service de fédération AD FS|Lorsque vous créez des serveurs proxy de fédération, vous devez taper le même nom de Service de fédération dans l’Assistant de Configuration AD FS Federation Server Proxy pour tous les serveurs proxy de fédération qui fera partie de la batterie de serveurs. Le serveur proxy de fédération utilise l’URL qui constitue ce nom d’hôte DNS pour déterminer quelle instance de Service de fédération AD FS de contacts.<br /><br />Pour plus d’informations, voir [configurer un ordinateur pour le rôle de Proxy de serveur de fédération](../../ad-fs/deployment/Configure-a-Computer-for-the-Federation-Server-Proxy-Role.md).|  
|Obtenir et partager des certificats|Vous pouvez obtenir un serveur de certificat d’authentification auprès d’une autorité de certification publique \ (CA\), par exemple, VeriSign, puis configurez le certificat afin que tous les serveurs proxy de fédération partage la même partie de clé privée du même certificat sur le site Web par défaut pour chaque serveur proxy de fédération. Pour partager le certificat, vous devez installer le même certificat d’authentification serveur sur le site Web par défaut pour chaque serveur proxy de fédération. Pour plus d’informations, voir [importer un certificat d’authentification serveur sur le site Web par défaut](../../ad-fs/deployment/Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md).<br /><br />Pour plus d’informations, voir [certificats requis pour les serveurs proxy de fédération](Certificate-Requirements-for-Federation-Server-Proxies.md).|  
  
Pour plus d’informations sur l’ajout de nouveaux serveurs proxy de fédération pour créer une batterie de proxy de serveur de fédération, consultez [liste de vérification: configuration d’un serveur Proxy de fédération](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server-Proxy.md).  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception ADFS dans Windows Server2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
