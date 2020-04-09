---
ms.assetid: ad0bf21d-2ace-4565-b1f5-ce57c8eb2689
title: Quand créer une batterie de serveurs proxy de fédération
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: d4b2b889159dee9f3b93a54a2b1924be286792f4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858492"
---
# <a name="when-to-create-a-federation-server-proxy-farm"></a>Quand créer une batterie de serveurs proxy de fédération

Envisagez d’installer des serveurs proxys de Fédération supplémentaires lorsque vous avez un grand Services ADFS \(AD FS\) déploiement et que vous souhaitez fournir la tolérance de pannes, la charge\-l’équilibrage et l’évolutivité de votre déploiement de proxy. La création d’au moins deux serveurs proxys de Fédération dans le même réseau de périmètre et la configuration de chacun d’eux pour protéger le même AD FS service FS (Federation Service) crée une batterie de serveurs proxy de Fédération.  
  
Vous pouvez créer une batterie de serveurs proxy de Fédération ou installer des serveurs proxy de Fédération supplémentaires sur une batterie existante à l’aide de l’Assistant Configuration du serveur proxy de fédération AD FS. Pour plus d'informations, voir [When to Create a Federation Server Proxy](When-to-Create-a-Federation-Server-Proxy.md).  
  
Pour que tous les serveurs proxys de Fédération puissent fonctionner ensemble comme une batterie de serveurs, vous devez d’abord les regrouper sous une seule adresse IP et dans un système de nom de domaine \(DNS\) nom de domaine complet \(nom de domaine complet\). Vous pouvez mettre en cluster les serveurs en déployant l’équilibrage de charge réseau Microsoft \(\) NLB dans le réseau de périmètre. Les tâches du tableau suivant requièrent que l’équilibrage de la charge réseau soit configuré correctement pour mettre en cluster les serveurs proxys de Fédération dans la batterie de serveurs.  
  
Pour plus d’informations sur la configuration d’un nom de domaine complet pour un cluster à l’aide de la technologie Microsoft NLB, consultez [spécification des paramètres de cluster](https://go.microsoft.com/fwlink/?linkid=74651).  
  
## <a name="configuring-federation-server-proxies-for-a-farm"></a>Configuration des serveurs proxy de fédération pour une batterie de serveurs  
Le tableau suivant décrit les tâches qui doivent être effectuées pour que chaque serveur proxy de Fédération puisse participer à une batterie de serveurs.  
  
|Tâche|Description|  
|--------|---------------|  
|Pointez tous les proxys de la batterie de serveurs vers le même AD FS nom de service FS (Federation Service)|Lorsque vous créez les serveurs proxys de Fédération, vous devez taper le même nom de service FS (Federation Service) dans l’Assistant Configuration du serveur proxy de fédération AD FS pour tous les serveurs proxys de Fédération qui feront partie de la batterie de serveurs. Le serveur proxy de Fédération utilise l’URL qui compose ce nom d’hôte DNS pour déterminer le AD FS service FS (Federation Service) instance qu’il contacte.<p>Pour plus d'informations, voir [Configure a Computer for the Federation Server Proxy Role](../../ad-fs/deployment/Configure-a-Computer-for-the-Federation-Server-Proxy-Role.md).|  
|Obtenir et partager des certificats|Vous pouvez obtenir un certificat d’authentification serveur auprès d’une autorité de certification publique \(autorité de certification\), par exemple, VeriSign, puis configurer le certificat afin que tous les serveurs proxys de fédération partagent la même partie de clé privée du même certificat sur le site Web par défaut pour chaque serveur proxy de Fédération. Pour partager le certificat, vous devez installer le même certificat d’authentification serveur sur le site Web par défaut pour chaque serveur proxy de Fédération. Pour plus d’informations, consultez [Importer un certificat d’authentification serveur sur le site Web par défaut](../../ad-fs/deployment/Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md).<p>Pour plus d'informations, voir [Certificate Requirements for Federation Server Proxies](Certificate-Requirements-for-Federation-Server-Proxies.md).|  
  
Pour plus d’informations sur l’ajout de nouveaux serveurs proxy de Fédération pour créer une batterie de serveurs proxy de Fédération, consultez [Checklist : Setting up a Federation Server Proxy](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server-Proxy.md).  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
