---
title: Architecture des services Bureau à distance
description: Diagrammes d’architecture pour les services Bureau à distance
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 02/10/2017
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7f73bb0a-ce98-48a4-9d9f-cf7438936ca1
author: lizap
manager: dongill
ms.openlocfilehash: ba597318cdaf1d4659a72905eeb4e252c9020e49
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887870"
---
# <a name="remote-desktop-services-architecture"></a>Architecture des services Bureau à distance

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Voici différentes configurations pour le déploiement des Services Bureau à distance pour héberger les applications Windows et des postes de travail pour les utilisateurs finaux.

>[!NOTE]
> Les diagrammes d’architecture ci-dessous illustrent l’utilisation de RDS dans Azure. Toutefois, vous pouvez déployer des Services Bureau à distance en local et sur d’autres clouds. Ces diagrammes sont principalement destinés à illustrer la façon dont les rôles des services Bureau à distance sont colocalisés et utilisent d’autres services.

## <a name="standard-rds-deployment-architectures"></a>Architectures de déploiement RDS standards

Services Bureau à distance a deux architectures standards :
-   Déploiement de base contient le nombre minimal de serveurs pour créer un environnement de services Bureau à distance de manière pleinement efficace
-   Déploiement à haute disponibilité – contient tous les composants nécessaires pour que le temps d’activité garanti la plus élevée pour votre environnement de services Bureau à distance

### <a name="basic-deployment"></a>Déploiement de base

![Déploiement de base de RDS](./media/basic-rds.png)

### <a name="highly-available-deployment"></a>Déploiement à haute disponibilité

![Déploiement des services Bureau à distance à haute disponibilité](./media/ha-rds.png)

## <a name="rds-architectures-with-unique-azure-paas-roles"></a>Architectures de services Bureau à distance avec des rôles Azure PaaS uniques

Bien que les architectures de déploiement RDS standards s’adapte à la plupart des scénarios, Azure continue à investir dans les solutions PaaS internes, cette valeur pour les clients. Voici certaines architectures montrant comment ils incorporent avec RDS.

### <a name="rds-deployment-with-azure-ad-domain-services"></a>Déploiement des services Bureau à distance avec Azure AD Domain Services

Les diagrammes d’architecture standard deux ci-dessus sont basés sur un traditionnel Active Directory (AD) déployé sur une machine virtuelle Windows Server. Toutefois, si vous n’ont une publicité traditionnelle et avoir uniquement un locataire Azure AD, via les services comme Office 365, mais que vous souhaitez toujours tirer parti des services Bureau à distance, vous pouvez utiliser [Azure AD Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-overview) pour créer un domaine entièrement géré dans votre Azure IaaS environnement qui utilise les mêmes utilisateurs qui existent dans votre locataire Azure AD. Cette opération supprime la complexité de la synchronisation des utilisateurs et la gestion de plusieurs machines virtuelles manuellement. Azure AD Domain Services peuvent fonctionner dans un déploiement : base ou hautement disponible.

![Azure AD et le déploiement des services Bureau à distance](./media/aadds-rds.png)

### <a name="rds-deployment-with-azure-ad-application-proxy"></a>Déploiement des services Bureau à distance avec le Proxy d’Application Azure AD

Les diagrammes d’architecture standard deux ci-dessus utilisent les serveurs Web/de la passerelle Bureau à distance en tant que point d’entrée accessibles sur Internet dans le système des services Bureau à distance. Pour certains environnements, les administrateurs préféreront supprimer leurs propres serveurs de périmètre et d’utiliser à la place des technologies qui fournissent également une sécurité supplémentaire par le biais des technologies de proxy inverse. Le [Proxy d’Application Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started) rôle PaaS s’intègre parfaitement avec ce scénario.

Pour les configurations prises en charge et sur la création de ce programme d’installation, consultez Comment [publier le Bureau à distance avec le Proxy d’Application](/azure/active-directory/application-proxy-publish-remote-desktop).

![Services Bureau à distance avec le Proxy d’Application Azure AD](./media/aadappproxy-rds.png)
