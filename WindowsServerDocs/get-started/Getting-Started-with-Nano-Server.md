---
title: Installer Nano Server
description: Nouvelle installation, mise à niveau, migration et évaluation de Nano Server
ms.prod: windows-server
ms.service: na
manager: dougkim
ms.technology: server-nano
ms.date: 09/06/2017
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 2c2fa45b-6f3b-4663-b421-2da6ecc463bf
author: jaimeo
ms.author: jaimeo
ms.localizationpriority: medium
ms.openlocfilehash: d395c72a1e21cd8eda043eebf3b72bbd5c9a13e8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71391801"
---
# <a name="install-nano-server"></a>Installer Nano Server

>S'applique à : Windows Server 2016

> [!IMPORTANT]
> À compter de Windows Server, version 1709, Nano Server sera uniquement disponible sous forme d’[image de système d’exploitation de base du conteneur](/virtualization/windowscontainers/quick-start/using-insider-container-images#install-base-container-image). Pour en savoir plus, consultez [Changements apportés à Nano Server](nano-in-semi-annual-channel.md). 

Windows Server 2016 propose une nouvelle option d’installation : Nano Server. Nano Server est un système d’exploitation de serveur administré à distance et optimisé pour les centres de données et clouds privés. Il est similaire à Windows Server en mode Server Core (Installation minimale), mais il est beaucoup plus petit, n’a aucune fonction d’ouverture de session locale et ne prend en charge que les agents, outils et applications 64 bits. Il occupe beaucoup moins d’espace disque, installe beaucoup plus rapidement et nécessite beaucoup moins de mises à jour et de redémarrages que Windows Server. Lorsqu’il redémarre, il le fait beaucoup plus rapidement. L’option d’installation Nano Server est disponible dans les éditions Standard et Datacenter de Windows Server 2016.  

Nano Server est idéal dans différentes situations :  
  
-   Comme un hôte de « calcul » pour les machines virtuelles Hyper-V, dans des clusters ou non  
  
-   Comme un hôte de stockage pour un serveur de fichiers avec montée en puissance parallèle  
  
-   Comme un serveur DNS  
  
-   Comme un serveur web exécutant Internet Information Services (IIS)  
  
-   Comme un hôte pour les applications développées à l’aide de modèles d’application cloud et exécutées dans le système d’exploitation invité d’un conteneur ou d’une machine virtuelle  
  
## <a name="important-differences-in-nano-server"></a>Différences importantes dans Nano Server

Étant donné que Nano Server est optimisé comme un système d’exploitation léger pour exécuter des applications « cloud natives » basées sur des conteneurs et micro-services ou comme un hôte de centre de données agile et rentable avec un encombrement considérablement réduit, il existe des différences importantes dans Nano Server entre les options d’installation minimale et Serveur avec Expérience utilisateur :

- Nano Server est « sans périphérique de contrôle » ; il n’existe aucune fonctionnalité d’ouverture de session locale ni d’interface utilisateur graphique.
- Seuls les agents, outils et applications 64 bits sont pris en charge.
- Nano Server ne peut pas servir de contrôleur de domaine Active Directory.
- Aucune stratégie de groupe n’est prise en charge. Toutefois, vous pouvez utiliser la [configuration d’état souhaité](https://msdn.microsoft.com/powershell/dsc/nanoDsc) pour appliquer les paramètres à l’échelle.
- Nano Server ne peut pas être configuré pour utiliser un serveur proxy pour accéder à Internet.
- L’association de cartes réseau (plus précisément, l’équilibrage de charge et le basculement ou LBFO) n’est pas pris en charge, contrairement à SET (Switch Embedded Teaming).
- System Center Configuration Manager et System Center Data Protection Manager ne sont pas pris en charge.
- Les applets de commande BPA (Best Practices Analyzer) et l’intégration de BPA avec le Gestionnaire de serveur ne sont pas prises en charge.
- NANO Server ne prend pas en charge les adaptateurs de bus hôte virtuels (HBA).
- NANO Server n’a pas besoin d’être activé avec une clé de produit. Quand il fonctionne en tant qu’hôte Hyper-V, Nano Server ne prend pas en charge l’[Activation automatique de machine virtuelle](https://technet.microsoft.com/library/dn303421%28v=ws.11%29.aspx) (AVMA). Vous pouvez activer les machines virtuelles qui s’exécutent sur un hôte Nano Server à l’aide du [Service de gestion de clés](https://technet.microsoft.com/library/jj612867(v=ws.11).aspx) (KMS), avec une clé de licence en volume générique ou à l’aide de l’[activation basée sur Active Directory](https://technet.microsoft.com/library/dn502534(v=ws.11).aspx).
- La version de Windows PowerShell fournie avec Nano Server présente des différences importantes. Pour plus d’informations, voir [PowerShell sur Nano Server](PowerShell-on-Nano-Server.md).
- Nano Server est pris en charge uniquement sur le modèle de branche CBB (Current Branch for Business) : il n’existe aucune version de branche LTSB (Long Term Servicing Branch) pour Nano Server à ce stade. Pour plus d’informations, voir la sous-section suivante.

### <a name="current-branch-for-business"></a>Branche CBB
Nano Server est mis en service avec un modèle plus actif, appelé branche CBB, pour prendre en charge les clients qui adoptent une « cadence de cloud », avec des cycles de développement rapides. Dans ce modèle, les publications des mises à jour des fonctionnalités de Nano Server sont prévues deux à trois fois par an. Ce modèle nécessite la [Software Assurance](https://www.microsoft.com/en-us/licensing/licensing-programs/software-assurance-default.aspx) pour les serveurs Nano Server déployés et utilisés en production. Pour assurer la prise en charge, les administrateurs ne doivent pas utiliser une version CBB antérieure aux deux dernières publications. Toutefois, ces publications ne mettent pas automatiquement à jour les déploiements existants ; les administrateurs effectuent une installation manuelle d’une nouvelle version CBB à leur convenance. Pour obtenir des informations supplémentaires, voir [Windows Server 2016 new Current Branch for Business servicing option](https://blogs.technet.microsoft.com/windowsserver/2016/07/12/windows-server-2016-new-current-branch-for-business-servicing-option/).

Les options d’installation minimale et Serveur avec Expérience utilisateur sont toujours mises en service sur le [modèle LTSB (Long Term Servicing Branch)](https://support.microsoft.com/lifecycle#gp%2Fgp_msl_policy), avec 5 ans de support standard et 5 ans de support étendu.

## <a name="installation-scenarios"></a>Scénarios d’installation

### <a name="evaluation"></a>Évaluation
Vous pouvez obtenir une version d’évaluation, avec licence de 180jours, de Windows Server dans la page [Windows Server Evaluations (Évaluations de Windows Server)](https://www.microsoft.com/evalcenter/evaluate-windows-server-2016). Pour essayer Nano Server, choisissez **Nano Server |64-bit EXE option**, puis revenez à la rubrique [Démarrage rapide de Nano Server](Nano-Server-Quick-Start.md) ou [Déployer Nano Server](Deploy-Nano-Server.md) pour commencer.

### <a name="clean-installation"></a>Nouvelle installation
Comme vous installez Nano Server en configurant un disque dur virtuel, le mode de déploiement le plus rapide et le plus simple est une nouvelle installation.

- Pour effectuer rapidement un déploiement de base de Nano Server, en utilisant DHCP pour obtenir une adresse IP, consultez [Démarrage rapide de Nano Server](Nano-Server-Quick-Start.md). 
- Si vous êtes déjà familier avec les principes fondamentaux de Nano Server, les rubriques plus détaillées, en commençant par [Deploy Nano Server (Déployer Nano Server)](Deploy-Nano-Server.md), offrent un jeu complet d’instructions pour la personnalisation d’images, l’utilisation des domaines, l’installation de packages pour les rôles de serveur, d’autres fonctionnalités en ligne et hors connexion, et bien plus encore.

> [!IMPORTANT]  
> Une fois que l’exécution du programme d’installation est terminée et que vous avez installé tous les rôles et fonctionnalités de serveur dont vous avez besoin, recherchez et installez les mises à jour disponibles pour Windows Server 2016. Pour Nano Server, consultez la section « Gestion des mises à jour de Nano Server » de [Gérer Nano Server](Manage-Nano-Server.md).

### <a name="upgrade"></a>Mettre à niveau/Mise à niveau
Comme Nano Server est une nouveauté de Windows Server 2016, il n’existe aucune mise à niveau possible entre les anciennes versions de système d’exploitation et Nano Server.

### <a name="migration"></a>Migration
Comme Nano Server est une nouveauté de Windows Server 2016, il n’existe aucune migration possible entre les anciennes versions de système d’exploitation et Nano Server.
  
-------------------------------------
Si vous avez besoin d’une autre option d’installation, revenez à la [page principale de Windows Server 2016](windows-server-2016.md). 

  


 
