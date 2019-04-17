---
title: Guide de déploiement de BranchCache
description: Cette rubrique fait partie de la BranchCache déploiement Guide pour Windows Server2016, qui montre comment déployer BranchCache en mode de cache distribué et hébergé d’optimiser l’utilisation de la bande passante réseau étendu dans les filiales
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 3830b356-36d3-44f9-a1d7-990ff3e57403
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e7aa35260213a8a236b7c27cf74e36692438cd2a
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="branchcache-deployment-guide"></a>Guide de déploiement de BranchCache

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser ce guide pour apprendre à déployer BranchCache dans Windows Server2016.  
  
Outre cette rubrique, ce guide contient les sections suivantes.  
  
-   [Choix d’une conception BranchCache](../../branchcache/plan/Choosing-a-BranchCache-Design.md)  
  
-   [Déployer BranchCache](../../branchcache/deploy/Deploy-BranchCache.md)  
  
## <a name="branchcache-deployment-overview"></a>Vue d’ensemble du déploiement de BranchCache

BranchCache est une technologie d’optimisation étendu réseau (étendu WAN) la bande passante qui est incluse dans certaines éditions de Windows Server2016, Windows Server&reg; 2012R2, Windows Server&reg; 2012, Windows Server&reg; 2008R2 et les systèmes d’exploitation clients Windows connexes.  
  
Pour optimiser la bande passante réseau étendue, BranchCache copie le contenu de vos serveurs de contenu du siège social et met en cache dans les filiales, permettant ainsi aux clients les ordinateurs dans des filiales d’accéder au contenu localement plutôt que via le réseau étendu.  
  
Dans les filiales, le contenu est mis en cache sur les serveurs qui exécutent la fonctionnalité BranchCache de Windows Server2016, Windows Server2012R2, Windows Server2012 ou Windows Server2008R2 - ou, si aucun serveur ne sont disponibles dans la filiale, le contenu est mis en cache sur les ordinateurs clients qui exécutent Windows10&reg;, Windows&reg; 8.1, Windows8 ou Windows7&reg;.  
  
Une fois un ordinateur client demande et reçoit du contenu à partir du centre de données office ou cloud principal et le contenu est mis en cache dans la filiale, les autres ordinateurs de la même filiale peuvent obtenir le contenu localement plutôt que de contacter le serveur de contenu sur la liaison WAN.  
  
**Avantages du déploiement de BranchCache**  
  
Fichier de caches BranchCache, web et contenu de l’application à des emplacements de filiales, permettant aux ordinateurs client accéder aux données à l’aide du réseau local (LAN) au lieu d’accéder au contenu via des connexions de réseau étendues lentes.  
  
BranchCache réduit le trafic WAN et le temps requis pour les utilisateurs des filiales d’ouvrir des fichiers sur le réseau.  BranchCache fournit toujours les utilisateurs avec les données les plus récentes, et qu’il protège la sécurité de votre contenu en chiffrant les caches sur le serveur de cache hébergé et sur les ordinateurs clients.  
  
### <a name="what-this-guide-provides"></a>Ce que ce guide fournit  
Ce guide de déploiement vous permet de déployer BranchCache dans les modes suivants:  
  
-   Mode de cache distribué. Dans ce mode, les ordinateurs clients des succursales téléchargent du contenu à partir des serveurs de contenu dans le siège social ou du cloud et mettre en cache le contenu pour les autres ordinateurs dans la même filiale. Mode de cache distribué ne requiert pas d’un ordinateur de serveur dans la filiale.  
  
-   Mode de cache hébergé. Dans ce mode, branch office client ordinateurs téléchargent le contenu à partir de serveurs de contenu du siège social ou du cloud et un serveur de cache hébergé extrait le contenu à partir des clients. Puis, le serveur de cache hébergé met en cache le contenu pour les autres ordinateurs clients.  
  
Ce guide fournit également des instructions sur la façon de déployer les trois types de serveurs de contenu. Serveurs de contenu contiennent le contenu source qui est téléchargé par les ordinateurs clients des succursales et au moins un serveur de contenu est requis pour déployer BranchCache en mode. Les types de serveur de contenu sont:  
  
-   **Serveurs de contenu basé sur le serveur Web **. Ces serveurs de contenu envoient le contenu pour les ordinateurs clients BranchCache à l’aide des protocoles HTTP et HTTPS. Ces serveurs de contenu doivent exécuter Windows Server2016, Windows Server2012R2, Windows Server2012 ou Windows Server2008R2 versions qui prennent en charge BranchCache et sur lequel la fonctionnalité BranchCache est installée.  
  
-   **Serveurs d’applications BITS **. Ces serveurs de contenu envoient le contenu pour les ordinateurs clients BranchCache à l’aide du Service de transfert Intelligent en arrière-plan (BITS). Ces serveurs de contenu doivent exécuter Windows Server2016, Windows Server2012R2, Windows Server2012 ou Windows Server2008R2 versions qui prennent en charge BranchCache et sur lequel la fonctionnalité BranchCache est installée.  
  
-   **Serveurs de contenu basé sur le serveur de fichiers **. Ces serveurs de contenu doivent exécuter Windows Server2016, Windows Server2012R2, Windows Server2012 ou Windows Server2008R2 versions qui prennent en charge BranchCache et sur les Services que le fichier de rôle de serveur est installé. En outre, le **BranchCache pour fichiers réseau** service de rôle du rôle de serveur Services de fichiers doit être installé et configuré. Ces serveurs de contenu envoient le contenu pour les ordinateurs clients BranchCache à l’aide du protocole Server Message Block (SMB).  
  
Pour plus d’informations, voir [versions de système d’exploitation pour BranchCache ](https://technet.microsoft.com/en-us/windows-server-docs/networking/branchcache/branchcache#a-namebkmkosaoperating-system-versions-for-branchcache).  
  
### <a name="branchcache-deployment-requirements"></a>Exigences de déploiement de BranchCache

Voici la configuration requise pour le déploiement de BranchCache à l’aide de ce guide.  
  
-   **Serveurs de contenu Web et le fichier** doivent exécuter l’un des systèmes d’exploitation suivants pour fournir la fonctionnalité BranchCache: Windows Server2016, Windows Server2012R2, Windows Server2012 ou Windows Server2008R2. Windows8 et versions ultérieures clients continuent à voir les avantages de BranchCache lorsqu’ils accèdent à des serveurs de contenu qui exécutent Windows Server2008R2, mais ils ne peuvent pas rendre utilisation de la segmentation de nouveau et hachage des technologies dans Windows Server2016, Windows Server2012R2 et Windows Server2012.  
  
-   **Les ordinateurs clients** doit exécuter Windows10, Windows8.1 ou Windows8 pour utiliser le modèle de déploiement plus récent et la segmentation et de hachage améliorations qui ont été introduites avec Windows Server2012.  
  
-   **Serveurs de cache hébergé** doivent exécuter Windows Server2016, Windows Server2012R2 ou Windows Server2012 pour utiliser les améliorations de déploiement et d’adapter les fonctionnalités décrites dans ce document.  Un ordinateur qui exécute un de ces systèmes d’exploitation qui est configuré comme un serveur de cache hébergé peut continuer à traiter les ordinateurs clients qui exécutent Windows7, mais pour ce faire, il doit être équipé d’un certificat qui convient pour sécurité TLS (Transport Layer), comme décrit dans le Windows Server2008R2 et Windows7 [Guide de déploiement BranchCache ](https://technet.microsoft.com/en-us/library/ee649232.aspx).  
  
-   **Un domaine ActiveDirectory** est nécessaire pour tirer parti de la stratégie de groupe et la découverte automatique de cache hébergé, mais un domaine n’est pas nécessaire pour utiliser BranchCache.  Vous pouvez configurer des ordinateurs individuels à l’aide de Windows PowerShell. En outre, il n’est pas nécessaire que vos contrôleurs de domaine exécutent Windows Server2012 ou version ultérieure à utiliser les nouveaux paramètres de stratégie de groupe BranchCache. Vous pouvez importer les modèles d’administration BranchCache sur les contrôleurs de domaine qui exécutent des systèmes d’exploitation antérieurs, ou vous pouvez créer les objets de stratégie de groupe à distance sur d’autres ordinateurs qui exécutent Windows10, Windows Server2016, Windows8.1, Windows Server2012R2, Windows8 ou Windows Server2012.

-   **Les sites ActiveDirectory** sont utilisés pour limiter l’étendue des serveurs de cache hébergé qui sont détectés automatiquement.  Pour découvrir automatiquement un serveur de cache hébergé, les ordinateurs client et serveur doivent appartenir au même site. BranchCache est conçu pour avoir un impact minimal sur les clients et serveurs et n’impose pas d’autre configuration matérielle requise au-delà de celles nécessaires pour exécuter leurs systèmes d’exploitation respectifs.  

**Documentation et l’historique de BranchCache**

BranchCache a été introduite dans Windows7&reg; et Windows Server&reg; 2008R2 et a été améliorée dans Windows Server2012, Windows8 et les systèmes d’exploitation ultérieurs.

> [!NOTE]
> Si vous déployez BranchCache dans les systèmes d’exploitation autres que Windows Server2016, les ressources de documentation suivantes sont disponibles.
> 
> - Pour plus d’informations sur BranchCache dans Windows8, Windows8.1, Windows Server2012 et Windows Server2012R2, consultez [vue d’ensemble de BranchCache ](https://technet.microsoft.com/en-us/library/hh831696.aspx).  
> - Pour plus d’informations sur BranchCache dans Windows7 et Windows Server2008R2, consultez [BranchCache pour Windows Server2008R2](https://technet.microsoft.com/en-us/library/dd996634.aspx).  
  


