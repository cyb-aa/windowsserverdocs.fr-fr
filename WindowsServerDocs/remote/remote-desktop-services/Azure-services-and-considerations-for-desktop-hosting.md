---
title: Services et considérations Azure pour l’hébergement de postes de travail
description: Découvrez les considérations spécifiques à Azure avec une solution d’hébergement de bureau à distance.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 07/06/2018
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0f402ae3-5391-4c7d-afea-2c5c9044de46
author: heidilohr
manager: dougkim
ms.openlocfilehash: 37210a5d75399309c53364f5b8ee9e06d26d6f32
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849800"
---
# <a name="azure-services-and-considerations-for-desktop-hosting"></a>Services et considérations Azure pour l’hébergement de postes de travail

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Les sections suivantes décrivent les Services d’Infrastructure Azure.
  
## <a name="azure-portal"></a>Portail Azure

Une fois que le fournisseur crée un abonnement Azure, le portail Azure peut servir à créer manuellement les environnement de chaque client. Ce processus peut également être automatisé à l’aide de scripts PowerShell.  

Pour plus d’informations, visitez le [Microsoft Azure](https://www.azure.microsoft.com) site Web.
  
## <a name="azure-load-balancer"></a>Équilibrage de charge Azure

Composants du locataire s’exécutent sur des machines virtuelles qui communiquent entre eux sur un réseau isolé. Pendant le processus de déploiement, vous pouvez en externe accéder ces machines virtuelles via l’équilibreur de charge Azure à l’aide de points de terminaison de protocole Bureau à distance ou un point de terminaison PowerShell à distance. Une fois qu’un déploiement est terminé, ces points de terminaison seront généralement supprimés pour réduire la surface d’attaque. Les seuls points de terminaison seront les points de terminaison HTTPS et UDP créés pour la machine virtuelle exécutant le Web de bureau à distance et les composants de passerelle Bureau à distance. Ainsi, les clients sur internet pour se connecter à des sessions en cours d’exécution dans le service d’hébergement bureau du locataire. Si un utilisateur ouvre une application qui se connecte à internet, comme un navigateur web, les connexions sont transmises via l’équilibreur de charge Azure.  
  
Pour plus d’informations, consultez [What ' s Azure Load Balancer ?](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-load-balance/)
  
## <a name="security-considerations"></a>Considérations relatives à la sécurité

Ce Guide d’Architecture référence hébergement Azure Desktop est conçu pour fournir un environnement hautement sécurisé et isolé pour chaque client. Sécurité du système dépend également de dispositifs de protection effectuées par le fournisseur au cours de déploiement et le fonctionnement du service hébergé. La liste suivante décrit certaines considérations que le fournisseur doit prendre à conserver leur solution d’hébergement bureau basée sur cette architecture de référence sécurisée.

- Tous les mots de passe d’administration doivent être forts, dans l’idéal, de façon aléatoire généré, fréquemment modifiés et enregistrés dans un emplacement centralisé sécurisé accessible uniquement à un peu d’administrateurs fournisseur sélectionnez.  
- Lors de la réplication de l’environnement client pour les nouveaux locataires, évitez d’utiliser les mots de passe d’administration mêmes ou faibles.
- L’URL du site accès Web de bureau à distance, les nom et les certificats doivent être unique et reconnaissables à chaque client pour empêcher les attaques d’usurpation d’identité.  
- Pendant le fonctionnement normal du service d’hébergement bureau, toutes les adresses IP publiques doivent être supprimés pour toutes les machines virtuelles à l’exception de la machine virtuelle Web Bureau à distance et passerelle Bureau à distance qui permet aux utilisateurs de se connecter en toute sécurité à un service de cloud d’hébergement bureau du locataire. Les adresses IP publiques peuvent être temporairement ajoutés lorsque cela est nécessaire pour les tâches de gestion, mais ils doivent toujours être supprimés par la suite.  
  
Pour plus d’informations, consultez les articles suivants :

- [Sécurité et protection](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831778(v=ws.11))  
- [Meilleures pratiques de sécurité pour IIS 8](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj635855(v=ws.11))  
- [Sécuriser Windows Server 2012 R2](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831360(v=ws.11))  
  
## <a name="design-considerations"></a>Considérations relatives à la conception

Il est important de prendre en compte les contraintes des Services d’Infrastructure Microsoft Azure lorsque vous concevez un service d’hébergement de bureau mutualisé. La liste suivante décrit les considérations sur que le fournisseur doit suivre pour effectuer une solution d’hébergement bureau fonctionnelle et économique en fonction de cette architecture de référence.  
  
- Un abonnement Azure a un nombre maximal de réseaux virtuels, les cœurs de machines virtuelles et les Services de Cloud qui peut être utilisé. Si un fournisseur a besoin de davantage de ressources que cela, il faudra créer plusieurs abonnements.
- Un Service Cloud Azure a un nombre maximal de machines virtuelles qui peut être utilisé. Le fournisseur devra peut-être créer plusieurs Services de Cloud pour les clients plus volumineux qui dépassent la valeur maximale.  
- Les coûts de déploiement Azure sont partiellement basées sur le nombre et la taille des machines virtuelles. Le fournisseur doit optimiser le nombre et la taille des machines virtuelles pour chaque client pour fournir un fonctionnel et hautement sécurisé environnement d’hébergement de bureau à moindre coût.  
- Les ressources d’ordinateur physique du centre de données Azure sont virtualisés à l’aide de Hyper-V. Hôtes Hyper-V ne sont pas configurés dans les clusters hôtes, par conséquent, la disponibilité des machines virtuelles est dépendante de la disponibilité des serveurs individuels utilisés dans l’infrastructure Azure. Pour fournir une disponibilité plus élevée, plusieurs instances de machine virtuelle du service de chaque rôle peuvent être créés dans un groupe à haute disponibilité, puis le clustering invité peut être implémenté dans les machines virtuelles.  
- Dans une configuration de stockage classique, un fournisseur de services aura un seul compte de stockage avec plusieurs conteneurs (par exemple, un pour chaque client) et plusieurs disques dans chaque conteneur. Toutefois, il existe une limite pour le stockage total et les performances qui peut être atteint pour un seul compte de stockage. Fournisseurs de services qui prennent en charge de nombreux clients ou locataires aux exigences de stockage haute capacité et de performances, le fournisseur de services devrez peut-être créer plusieurs comptes de stockage.  
  
Pour plus d’informations, consultez les articles suivants :

- [Tailles de Services Cloud](https://docs.microsoft.com/azure/cloud-services/cloud-services-sizes-specs)  
- [Machine virtuelle de Microsoft Azure détails de la tarification](https://azure.microsoft.com/pricing/details/virtual-machines/)  
- [Vue d’ensemble d’Hyper-V](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831531(v=ws.11))  
- [Objectifs de performance et d’extensibilité de stockage Azure](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets)  

## <a name="azure-active-directory-application-proxy"></a>Proxy d’Application Azure Active Directory

Proxy d’Application Azure Active Directory (AD) est un service fourni dans payant références (SKU) d’Azure AD qui permettent aux utilisateurs pour se connecter à des applications internes via le service de proxy inverse d’Azure. Ainsi, les points de terminaison Web de bureau à distance et passerelle Bureau à distance être masquées à l’intérieur du réseau virtuel, en éliminant la nécessité d’être exposée à internet par une adresse IP publique. Hébergeurs permettent le Proxy d’Application pour condenser le nombre de machines virtuelles dans un environnement du locataire tout en conservant un déploiement complet. Proxy d’Application Azure AD vous permet également de nombreux avantages par Azure AD, telles que l’accès conditionnel et l’authentification multifacteur.

Pour plus d’informations, consultez [prise en main le Proxy d’Application et installer le connecteur](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-enable).