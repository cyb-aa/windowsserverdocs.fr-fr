---
title: Déployer l'accès à distance dans une entreprise
description: Cette rubrique fournit une introduction au scénario DirectAccess dans Windows Server 2016 pour l’entreprise.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: 4781df0a-158b-4562-b8f5-32b27615a4f8
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 6d0a9248865dca4afb3db9609b284048155f9eef
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857432"
---
# <a name="deploy-remote-access-in-an-enterprise"></a>Déployer l'accès à distance dans une entreprise

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique fournit une introduction au scénario DirectAccess pour l’entreprise.  
  
  
> [!IMPORTANT]  
> Pour déployer DirectAccess à l’aide de ce guide, vous devez utiliser un serveur DirectAccess qui exécute Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012.  
  
## <a name="before-you-begin-deploying-see-the-list-of-unsupported-configurations-known-issues-and-prerequisites"></a>Avant de commencer le déploiement, consultez la liste des configurations non prises en charge, des problèmes connus et des configurations requises  
  
-   [Configurations non prises en charge DirectAccess](https://technet.microsoft.com/windows-server-docs/networking/remote-access/directaccess/directaccess-unsupported-configurations)  
  
-   [Problèmes connus de DirectAccess](https://technet.microsoft.com/windows-server-docs/networking/remote-access/directaccess/directaccess-known-issues)  
  
-   [Conditions préalables pour le déploiement de DirectAccess) conditions préalables](https://technet.microsoft.com/windows-server-docs/networking/remote-access/directaccess/prerequisites-for-deploying-directaccess)  
  
## <a name="scenario-description"></a><a name="BKMK_OVER"></a>Description du scénario  
L’accès à distance inclut plusieurs fonctionnalités d’entreprise, dont notamment le déploiement de plusieurs serveurs d’accès à distance dans un cluster à charge équilibrée à l’aide de l’équilibrage de charge réseau Windows ou d’un équilibrage de charge externe, la configuration d’un déploiement multisite avec des serveurs d’accès à distance situés dans des emplacements géographiques disparates et le déploiement de DirectAccess avec une authentification de client à deux facteurs utilisant un mot de passe à usage unique.  
  
## <a name="in-this-scenario"></a>Dans ce scénario  
Chaque scénario d’entreprise est décrit dans un document qui inclut les instructions de planification et de déploiement. Pour plus d’informations, consultez :  
  
-   [Déployer l’accès à distance dans un cluster](cluster/Deploy-Remote-Access-In-Cluster.md)  
  
-   [Déployer plusieurs serveurs d’accès à distance dans un déploiement multisite](multisite/Deploy-Multiple-Remote-Access-Servers-in-a-Multisite-Deployment.md)  
  
-   [Déployer l’accès à distance avec l’authentification par mot de passe à usage unique](otp/Deploy-RA-OTP.md)  
  
-   [Déployer l’accès à distance dans un environnement à plusieurs forêts](multi-forest/Deploy-Remote-Access-in-a-Multi-Forest-Environment.md)  
  
## <a name="practical-applications"></a><a name="BKMK_APP"></a>Applications pratiques  
Les scénarios d’entreprise pour l’accès à distance fournissent les avantages suivants :  
  
-   **Disponibilité accrue**. Le déploiement de plusieurs serveurs d’accès à distance dans un cluster assure l’évolutivité et augmente la capacité de débit et de nombre d’utilisateurs. L’équilibrage de charge du cluster permet une haute disponibilité. Si un serveur du cluster échoue, les utilisateurs distants peuvent continuer à accéder au réseau d’entreprise interne via un autre serveur du cluster. Le basculement est transparent car les clients se connectent au cluster à l’aide d’une adresse IP virtuelle (VIP).  
  
-   **Facilité de gestion**. Un déploiement de cluster ou multisite peut être configuré et géré en tant qu’entité unique à l’aide de la console de gestion de l’accès à distance qui s’exécute sur l’un des serveurs de cluster. En outre, un déploiement multisite permet aux administrateurs d’aligner le déploiement de l’accès à distance vers des sites Active Directory, fournissant ainsi une architecture simplifiée. Les paramètres partagés peuvent aisément être définis entre les serveurs en cluster ou sur tous les serveurs de point d’entrée multisites. Les paramètres d’accès à distance peuvent être gérés à partir de l’un quelconque des serveurs dans le cluster ou le déploiement, ou à distance à l’aide des Outils d’administration de serveur distant (RSAT). De plus, le déploiement complet en cluster ou multisite peut être analysé à partir d’une console de gestion de l’accès à distance unique.  
  
-   **Rentabilité.** Un déploiement multisite d’accès à distance permet aux entreprises de déployer des serveurs d’accès à distance dans plusieurs sites correspondant aux emplacements des clients. Ceci fournit une expérience d’accès prévisible pour les clients distants, quel que soit leur emplacement, et réduit les coûts et la bande passante de l’intranet en effectuant le routage du trafic client via Internet jusqu’au serveur d’accès à distance le plus proche.  
  
-   **Sécurité**. Le déploiement d’une authentification de client forte avec un mot de passe à usage unique au lieu d’un mot de passe de Active Directory standard augmente la sécurité.  
  
## <a name="roles-and-features-included-in-this-scenario"></a><a name="BKMK_NEW"></a>Rôles et fonctionnalités inclus dans ce scénario  
Le tableau suivant répertorie les fonctionnalités et rôles utilisés dans le scénario d’entreprise.  
  
|Rôle/fonctionnalité|Prise en charge de ce scénario|  
|---------|-----------------|  
|Rôle de serveur d’accès à distance|Ce rôle est installé et désinstallé à l’aide de la console du Gestionnaire de serveur. Ce rôle englobe à la fois DirectAccess, qui était auparavant une fonctionnalité de Windows Server 2008 R2, et le service Routage et accès distant qui était auparavant un service de rôle sous le rôle de serveur Services de stratégie et d’accès réseau. Le rôle Accès à distance est constitué de deux composants :<p>1. DirectAccess et les services de routage et d’accès à distance (RRAS) VPN-DirectAccess et VPN sont gérés ensemble dans la console de gestion de l’accès à distance.<br />2. routage RRAS : les fonctionnalités de routage RRAS sont gérées dans la console de routage et d’accès distant héritée.<p>Le rôle de serveur d’accès à distance dépend des fonctionnalités de serveur suivantes :<p>-Internet Information Services (IIS) : cette fonctionnalité est requise pour configurer le serveur emplacement réseau et la sonde Web par défaut.<br />-La fonctionnalité de Console de gestion des stratégies de groupe fonctionnalité est requise par DirectAccess pour créer et gérer les objets de stratégie de groupe (GPO) dans Active Directory et doit être installée en tant que fonctionnalité requise pour le rôle de serveur.|  
|Fonctionnalité des outils de gestion de l’accès à distance|Cette fonctionnalité est installée comme suit :<p>-Elle est installée par défaut sur un serveur d’accès à distance lorsque le rôle accès à distance est installé et prend en charge l’interface utilisateur de la console de gestion à distance.<br />-Il peut éventuellement être installé sur un serveur qui n’exécute pas le rôle de serveur d’accès à distance. Dans ce cas, elle est utilisée pour la gestion à distance d’un ordinateur d’accès à distance qui exécute DirectAccess et le réseau privé virtuel (VPN).<p>La fonctionnalité des outils de gestion de l’accès à distance est constituée des éléments suivants :<p>1. interface utilisateur graphique d’accès à distance et outils en ligne de commande<br />2. module d’accès à distance pour Windows PowerShell<p>Les dépendances incluent :<p>1. Console de gestion des stratégies de groupe<br />2. kit d’administration du gestionnaire des connexions (CMAK) RAS<br />3. Windows PowerShell 3,0<br />4. outils et infrastructure de gestion graphique|  
|Équilibrage de charge réseau Windows|Cette fonctionnalité permet l’équilibrage de charge de plusieurs serveurs d’accès à distance.|  
  

  


