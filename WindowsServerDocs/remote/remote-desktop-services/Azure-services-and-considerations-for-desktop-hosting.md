---
title: Services et considérations Azure pour l’hébergement de postes de travail
description: Découvrez les considérations spécifiques à Azure avec une solution d’hébergement de postes de travail à distance.
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
ms.openlocfilehash: c07a86c8168d323cf6e2af373ad51dc6a6b640b5
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/17/2019
ms.locfileid: "63749244"
---
# <a name="azure-services-and-considerations-for-desktop-hosting"></a>Services et considérations Azure pour l’hébergement de postes de travail

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2019, Windows Server 2016

Les sections suivantes décrivent les services d’infrastructure Azure.
  
## <a name="azure-portal"></a>Portail Azure

Une fois que le fournisseur crée un abonnement Azure, le portail Azure peut servir à créer manuellement l’environnement de chaque client. Ce processus peut également être automatisé avec des scripts PowerShell.  

Pour plus d’informations, visitez le site Web de [Microsoft Azure](https://www.azure.microsoft.com).
  
## <a name="azure-load-balancer"></a>Équilibreur de charge Azure

Les composants du locataire s’exécutent sur des machines virtuelles communiquant entre elles sur un réseau isolé. Pendant le processus de déploiement, vous pouvez accéder à ces machines virtuelles de façon externe via l’équilibreur de charge Azure à l’aide de points de terminaison du protocole RDP ou d’un point de terminaison PowerShell à distance. Une fois le déploiement terminé, ces points de terminaison seront généralement supprimés pour réduire la surface d’attaque. Les seuls points de terminaison seront les points de terminaison HTTPS et UDP créés pour la machine virtuelle exécutant les composants RD Web et RD Gateway. Cela permet aux clients sur internet de se connecter à des sessions exécutées depuis le service d’hébergement de postes de travail du locataire. Si un utilisateur ouvre une application qui se connecte à internet, comme un navigateur web, les connexions sont transmises via l’équilibreur de charge Azure.  
  
Pour plus d’informations, voir [Qu’est-ce que l’équilibreur de charge Azure ?](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-load-balance/)
  
## <a name="security-considerations"></a>Éléments à prendre en compte en matière de sécurité

Ce guide d’architecture de référence pour l’hébergement de postes de travail Azure est conçu pour fournir un environnement hautement sécurisé et isolé pour chaque locataire. La sécurité du système dépend également de dispositifs de protection établis par le fournisseur au cours du déploiement et du fonctionnement du service hébergé. La liste suivante décrit certaines considérations que le fournisseur doit avoir à l’esprit pour conserver leur solution d’hébergement de postes de travail basée sur cette architecture de référence sécurisée.

- Tous les mots de passe d’administration doivent être forts, idéalement générés de façon aléatoire, fréquemment modifiés et enregistrés dans un emplacement central sécurisé et accessible uniquement à quelques administrateurs du fournisseur.  
- Lors de la réplication de l’environnement du locataire pour de nouveaux locataires, évitez d’utiliser des mots de passe d’administration identiques ou faibles.
- L’URL du site RD Web Access, le nom et les certificats doivent être uniques et reconnaissables pour chaque client afin d’empêcher les attaques d’usurpation d’identité.  
- Pendant le fonctionnement normal du service d’hébergement de postes de travail, toutes les adresses IP publiques doivent être supprimées pour toutes les machines virtuelles à l’exception de la machine virtuelle RD Web et RD Gateway qui permet aux utilisateurs de se connecter en toute sécurité à un service cloud d’hébergement de postes de travail du locataire. Des adresses IP publiques peuvent être temporairement ajoutées si nécessaire pour les tâches de gestion, mais elles doivent toujours être supprimées ensuite.  
  
Pour plus d’informations, consultez les articles suivants :

- [Sécurité et protection](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831778(v=ws.11))  
- [Meilleures pratiques de sécurité pour IIS 8](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj635855(v=ws.11))  
- [Sécuriser Windows Server 2012 R2](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831360(v=ws.11))  
  
## <a name="design-considerations"></a>Considérations relatives à la conception

Il est important de prendre en compte les contraintes des services d’infrastructure Microsoft Azure lors de la conception d’un service d’hébergement de postes de travail mutualisé. La liste suivante décrit les considérations que le fournisseur doit avoir à l’esprit pour obtenir une solution d’hébergement de postes de travail fonctionnelle et économique, basée sur cette architecture de référence.  
  
- Un abonnement Azure dispose d’un nombre maximal de réseaux virtuels, de cœurs de machines virtuelles et de services cloud utilisables. Si un fournisseur a besoin de davantage de ressources, plusieurs abonnements seront nécessaires.
- Un service cloud Azure dispose d’un nombre maximal de machines virtuelles utilisables. Le fournisseur devra peut-être créer plusieurs services cloud pour les locataires plus volumineux et dépassant la limite.  
- Les coûts de déploiement Azure sont partiellement basés sur le nombre de machines virtuelles et sur leur taille. Le fournisseur doit optimiser le nombre et la taille des machines virtuelles pour chaque locataire afin d’offrir un environnement d’hébergement de postes de travail fonctionnel et hautement sécurisé, à moindre coût.  
- Les ressources de l’ordinateur physique dans le centre de données Azure sont virtualisées à l’aide de Hyper-V. Les hôtes Hyper-V ne sont pas configurés dans les clusters hôtes, par conséquent, la disponibilité des machines virtuelles dépend de celle des serveurs individuels utilisés dans l’infrastructure Azure. Pour offrir une meilleure disponibilité, plusieurs instances de machine virtuelle de chaque service de rôle peuvent être créées dans un groupe à haute disponibilité, puis le clustering invité peut être implémenté au sein des machines virtuelles.  
- Dans une configuration de stockage classique, un fournisseur de services aura un seul compte de stockage avec plusieurs conteneurs (par exemple, un pour chaque locataire) et plusieurs disques dans chaque conteneur. Toutefois, il existe une limite pour le stockage total et les performances atteignables pour un seul compte de stockage. Pour les fournisseurs de services qui prennent en charge de nombreux locataires ou des locataires avec un stockage haute capacité et des exigences de performances, le fournisseur de services pourrait avoir besoin de créer plusieurs comptes de stockage.  
  
Pour plus d’informations, consultez les articles suivants :

- [Tailles pour les services cloud](https://docs.microsoft.com/azure/cloud-services/cloud-services-sizes-specs)  
- [Détails de tarification des machines virtuelles Microsoft Azure](https://azure.microsoft.com/pricing/details/virtual-machines/)  
- [Vue d’ensemble d’Hyper-V](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831531(v=ws.11))  
- [Objectifs de performance et d’extensibilité du stockage Azure](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets)  

## <a name="azure-active-directory-application-proxy"></a>Proxy d’application Azure Active Directory

Le proxy d’application Azure Active Directory (AD) est un service fourni dans des références SKU payantes d’Azure AD qui permettent aux utilisateurs de se connecter à des applications internes via le service de proxy inverse d’Azure. Ainsi, les points de terminaison RD Web et RD Gateway peuvent être masqués à l’intérieur du réseau virtuel, en éliminant la nécessité d’exposition à internet par une adresse IP publique. Les hébergeurs peuvent utiliser le proxy d’application Azure AD pour condenser le nombre de machines virtuelles dans l’environnement d’un locataire tout en conservant un déploiement complet. Le proxy d’application Azure AD vous offre également de nombreux avantages d’Azure AD, comme l’accès conditionnel et l’authentification multifacteur.

Pour plus d’informations, consultez [Prise en main du proxy d’application et installation du connecteur](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-enable).