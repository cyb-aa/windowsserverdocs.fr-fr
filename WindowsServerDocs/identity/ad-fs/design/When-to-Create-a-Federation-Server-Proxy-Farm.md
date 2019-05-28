---
ms.assetid: ad0bf21d-2ace-4565-b1f5-ce57c8eb2689
title: Quand créer une batterie de serveurs proxy de fédération
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: c33475d7420383448439e2b769562e55127c7b0e
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190629"
---
# <a name="when-to-create-a-federation-server-proxy-farm"></a>Quand créer une batterie de serveurs proxy de fédération

Envisagez d’installer des serveurs proxy de fédération supplémentaires lorsque vous avez un grand Active Directory Federation Services \(AD FS\) déploiement et que vous souhaitez fournir une tolérance de panne, la charge\-équilibrage et évolutivité pour votre déploiement de proxy. Le fait de la création des serveurs proxy de fédération de deux ou plusieurs dans le même réseau de périmètre et la configuration de chacun d’eux pour protéger le même Service de fédération AD FS crée une batterie de serveurs du proxy du serveur de fédération.  
  
Vous pouvez créer une batterie de proxy de serveur de fédération ou installer des serveurs proxy de fédération supplémentaires à une batterie existante à l’aide de l’Assistant Configuration Proxy des serveur de fédération AD FS. Pour plus d'informations, voir [When to Create a Federation Server Proxy](When-to-Create-a-Federation-Server-Proxy.md).  
  
Avant que tous les serveurs proxy de fédération puissent fonctionner ensemble comme une batterie de serveurs, vous devez tout d’abord les mettre en cluster sous une seule adresse IP et un système de nom de domaine \(DNS\) nom de domaine complet \(nom de domaine complet\). Vous pouvez mettre en cluster les serveurs en déployant l’équilibrage de charge réseau Microsoft \(NLB\) au sein du réseau de périmètre. Les tâches dans le tableau suivant nécessitent NLB pour être correctement configuré pour les serveurs proxy de fédération dans la batterie de serveurs du cluster.  
  
Pour plus d’informations sur la configuration d’un nom de domaine complet pour un cluster à l’aide de la technologie Microsoft NLB, consultez [en spécifiant les paramètres de Cluster](https://go.microsoft.com/fwlink/?linkid=74651).  
  
## <a name="configuring-federation-server-proxies-for-a-farm"></a>Configuration des serveurs proxy de fédération pour une batterie de serveurs  
Le tableau suivant décrit les tâches qui doivent être effectuées afin que chaque serveur proxy de fédération peut participer à une batterie de serveurs.  
  
|Tâche|Description|  
|--------|---------------|  
|Pointez tous les proxys de la batterie de serveurs sur le même nom de Service de fédération AD FS|Lorsque vous créez des serveurs proxy de la fédération, vous devez taper le même nom de Service de fédération dans l’Assistant Configuration Proxy des serveur de fédération AD FS pour tous les serveurs proxy de fédération qui participeront à la batterie de serveurs. Le serveur proxy de fédération utilise l’URL qui constitue ce nom d’hôte DNS pour déterminer quelle instance de Service de fédération AD FS de contacts.<br /><br />Pour plus d'informations, voir [Configure a Computer for the Federation Server Proxy Role](../../ad-fs/deployment/Configure-a-Computer-for-the-Federation-Server-Proxy-Role.md).|  
|Obtenir et partager des certificats|Vous pouvez obtenir un serveur de certificat d’authentification auprès d’une autorité de certification publique \(autorité de certification\)— par exemple, VeriSign, puis configurez le certificat afin que tous les serveurs proxy de fédération partagent la même partie de clé privée de la même certificat sur le site Web par défaut pour chaque serveur proxy de fédération. Pour partager le certificat, vous devez installer le même certificat d’authentification serveur sur le site Web par défaut pour chaque serveur proxy de fédération. Pour plus d’informations, consultez [importer un certificat d’authentification serveur pour le Site Web par défaut](../../ad-fs/deployment/Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md).<br /><br />Pour plus d'informations, voir [Certificate Requirements for Federation Server Proxies](Certificate-Requirements-for-Federation-Server-Proxies.md).|  
  
Pour plus d’informations sur l’ajout de nouveaux serveurs proxy de fédération pour créer une batterie de proxy de serveur de fédération, consultez [liste de vérification : Configuration d’un serveur Proxy de fédération](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server-Proxy.md).  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
