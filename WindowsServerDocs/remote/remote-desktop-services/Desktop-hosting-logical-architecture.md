---
title: Architecture de Services Bureau à distance
description: Schémas de l’architecture de Services Bureau à distance
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 02/10/2017
ms.topic: article
ms.assetid: 7f73bb0a-ce98-48a4-9d9f-cf7438936ca1
author: lizap
manager: dongill
ms.openlocfilehash: 441b0b24fd4b4dc18d3afd65283bbf7ff2417048
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80818434"
---
# <a name="remote-desktop-services-architecture"></a>Architecture de Services Bureau à distance

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2019, Windows Server 2016

Vous trouverez ci-dessous différentes configurations de déploiement de Services Bureau à distance pour héberger des applications Windows et bureaux pour les utilisateurs finals.

>[!NOTE]
> Les schémas d’architecture ci-dessous illustrent l’utilisation de Services Bureau à distance dans Azure. Toutefois, vous pouvez déployer Services Bureau à distance en local et sur d’autres clouds. Ces schémas sont principalement destinés à illustrer la façon dont les rôles Services Bureau à distance sont colocalisés et utilisent d’autres services.

## <a name="standard-rds-deployment-architectures"></a>Architectures de déploiement de Services de Bureau à distance standard

La fonctionnalité Services Bureau à distance a deux architectures standard :
-    Déploiement de base : contient le nombre minimal de serveurs pour créer un environnement Services Bureau à distance pleinement efficace
-    Déploiement à haute disponibilité : contient tous les composants nécessaires pour que votre environnement Services Bureau à distance puisse bénéficier du temps de disponibilité maximal garanti

### <a name="basic-deployment"></a>Déploiement de base

![Déploiement de base de Services Bureau à distance](./media/basic-rds.png)

### <a name="highly-available-deployment"></a>Déploiement à haute disponibilité

![Déploiement à haute disponibilité de Services Bureau à distance](./media/ha-rds.png)

## <a name="rds-architectures-with-unique-azure-paas-roles"></a>Architectures de Services Bureau à distance avec des rôles Azure PaaS uniques

Bien que les architectures de déploiement Services Bureau à distance standard s’adaptent à la plupart des scénarios, Azure continue à investir dans des solutions PaaS internes qui génèrent de la valeur pour le client. Vous trouverez ci-dessous certaines architectures présentant leur intégration aux Services Bureau à distance.

### <a name="rds-deployment-with-azure-ad-domain-services"></a>Déploiement de Services Bureau à distance avec Azure AD Domain Services

Les deux schémas d’architecture standard ci-dessus reposent sur un répertoire Active Directory (AD) classique, déployé sur une machine virtuelle Windows Server. Toutefois, si vous n’avez pas de répertoire AD classique, mais que vous avez juste un locataire Azure AD (via les services comme Office365) et que vous souhaitez tout de même utiliser Services Bureau à distance, vous pouvez faire appel à [Azure AD Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-overview) pour créer un domaine totalement géré dans votre environnement Azure IaaS qui utilise les mêmes utilisateurs que ceux présents dans votre locataire Azure AD. Cela supprime la complexité de la synchronisation des utilisateurs, ainsi que de la gestion de davantage de machines virtuelles. Azure AD Domain Services peut fonctionner dans un déploiement de base ou hautement disponible.

![Azure AD et déploiement Services Bureau à distance](./media/aadds-rds.png)

### <a name="rds-deployment-with-azure-ad-application-proxy"></a>Déploiement Services Bureau à distance avec proxy d’application Azure AD

Les deux schémas d’architecture standard ci-dessus utilisent les serveurs de passerelle/web Bureau à distance en tant que points d’entrée côté Internet dans le système Services de Bureau à distance. Pour certains environnements, les administrateurs préféreront supprimer leurs propres serveurs du périmètre et utiliser à la place des technologies qui fournissent également une sécurité supplémentaire via des technologies de proxy inverse. Le rôle PaaS de [proxy d’application Azure Ad](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started) est parfaitement adapté à ce scénario.

Pour connaître les configurations prises en charge et apprendre à créer cette configuration, consultez l’article expliquant comment [publier le Bureau à distance avec le proxy d’application Azure AD](/azure/active-directory/application-proxy-publish-remote-desktop).

![Services Bureau à distance avec proxy d’application Azure AD](./media/aadappproxy-rds.png)
