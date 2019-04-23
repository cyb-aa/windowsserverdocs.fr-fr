---
title: Guide de déploiement BranchCache
description: Cette rubrique fait partie de BranchCache déploiement Guide pour Windows Server 2016, qui montre comment déployer BranchCache en mode cache distribué et hébergé pour optimiser l’utilisation de la bande passante WAN dans les succursales
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 3830b356-36d3-44f9-a1d7-990ff3e57403
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9bccf69f0a913159a395fabc670a63e2c159bd91
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888180"
---
# <a name="branchcache-deployment-guide"></a>Guide de déploiement BranchCache

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser ce guide pour apprendre à déployer BranchCache dans Windows Server 2016.  
  
Outre cette rubrique, ce guide contient les sections suivantes.  
  
-   [Choix d’une conception BranchCache](../../branchcache/plan/Choosing-a-BranchCache-Design.md)  
  
-   [Déployer BranchCache](../../branchcache/deploy/Deploy-BranchCache.md)  
  
## <a name="branchcache-deployment-overview"></a>Vue d’ensemble du déploiement de BranchCache

BranchCache est une technologie d’optimisation WAN réseau (étendu WAN) la bande passante qui est incluse dans certaines éditions de Windows Server 2016, Windows Server&reg; 2012 R2, Windows Server&reg; 2012, Windows Server&reg; 2008 R2 et connexes Systèmes d’exploitation Windows.  
  
Pour optimiser la bande passante du réseau étendu, BranchCache copie le contenu des serveurs de contenu de votre site principal et le met en cache dans les filiales, permettant ainsi aux ordinateurs clients des filiales d’accéder localement au contenu au lieu de passer par le réseau étendu.  
  
Dans les succursales contenu est mis en cache sur les serveurs qui exécutent la fonctionnalité BranchCache de Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 ou Windows Server 2008 R2 - ou, si il y a aucun serveur disponible dans la succursale, contenue cac hange mis en cache sur les ordinateurs clients qui exécutent Windows 10&reg;, Windows&reg; 8.1, Windows 8 ou Windows 7&reg; .  
  
Lorsqu’un ordinateur client demande et reçoit le contenu à partir du centre de données office ou de cloud principal et que le contenu est mis en cache dans la succursale, les autres ordinateurs de la même filiale peuvent obtenir le contenu localement au lieu de contacter le serveur de contenu sur le Liaison de réseau étendu.  
  
**Avantages du déploiement de BranchCache**  
  
Fichier de caches de BranchCache, web et contenu de l’application aux emplacements des filiales, permettant aux ordinateurs client accéder aux données à l’aide du réseau local (LAN) au lieu d’accéder au contenu via des connexions WAN lentes.  
  
BranchCache réduit le trafic WAN et le temps nécessaire pour les utilisateurs des filiales ouvrir des fichiers sur le réseau.  BranchCache fournit toujours des utilisateurs avec les données les plus récentes, et qu’il protège la sécurité de votre contenu en chiffrant les caches sur le serveur de cache hébergé et sur les ordinateurs clients.  
  
### <a name="what-this-guide-provides"></a>Ce que ce guide contient  
Ce guide de déploiement vous permet de déployer BranchCache dans les modes suivants :  
  
-   Mode de cache distribué. Dans ce mode, les ordinateurs clients des succursales téléchargent du contenu depuis les serveurs de contenu dans le bureau principal ou le cloud et mettent ensuite en cache le contenu pour les autres ordinateurs dans la même filiale. Le mode de cache distribué ne requiert pas de serveur dans la filiale.  
  
-   Mode de cache hébergé. Dans ce mode, branch office client ordinateurs téléchargent le contenu à partir des serveurs de contenu dans le bureau principal ou le cloud et un serveur de cache hébergé récupère le contenu à partir des clients. Le serveur de cache hébergé puis les met en cache pour les autres ordinateurs clients.  
  
Ce guide fournit également des instructions sur la façon de déployer trois types de serveurs de contenu. Serveurs de contenu contiennent le contenu source qui est téléchargé par les ordinateurs clients des succursales et un ou plusieurs serveurs de contenu est nécessaire pour déployer BranchCache en mode. Les types de serveurs de contenu sont :  
  
-   **Serveurs de contenu basé sur le serveur Web**. Ces serveurs de contenu envoient le contenu aux ordinateurs clients BranchCache à l'aide des protocoles HTTP et HTTPS. Ces serveurs de contenu doivent être en cours d’exécution des versions Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 ou Windows Server 2008 R2 qui prennent en charge BranchCache et sur lequel la fonctionnalité BranchCache est installée.  
  
-   **Les serveurs d’applications BITS**. Ces serveurs de contenu envoient le contenu sur les ordinateurs clients BranchCache à l’aide du Service de transfert Intelligent en arrière-plan (BITS). Ces serveurs de contenu doivent être en cours d’exécution des versions Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 ou Windows Server 2008 R2 qui prennent en charge BranchCache et sur lequel la fonctionnalité BranchCache est installée.  
  
-   **Serveurs de contenu basé sur le serveur de fichiers**. Ces serveurs de contenu doivent être en cours d’exécution des versions Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 ou Windows Server 2008 R2 qui prennent en charge BranchCache et sur les Services qui, le fichier de rôle de serveur est installé. De plus, le service de rôle **BranchCache pour fichiers réseau** du rôle serveur Services de fichiers doit être installé et configuré. Ces serveurs de contenu envoient le contenu aux ordinateurs clients BranchCache à l'aide du protocole SMB.  
  
Pour plus d’informations, consultez [versions de système d’exploitation pour BranchCache](https://technet.microsoft.com/windows-server-docs/networking/branchcache/branchcache#a-namebkmkosaoperating-system-versions-for-branchcache).  
  
### <a name="branchcache-deployment-requirements"></a>Exigences de déploiement de BranchCache

Voici la configuration requise pour le déploiement de BranchCache à l’aide de ce guide.  
  
-   **Serveurs de contenu Web et le fichier** doit exécuter l’un des systèmes d’exploitation suivants pour fournir des fonctionnalités de BranchCache : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 ou Windows Server 2008 R2. Windows 8 et ultérieures clients continuent à voir les avantages de BranchCache lorsqu’ils accèdent à des serveurs de contenu qui exécutent Windows Server 2008 R2, mais ils ne peuvent pas s’utiliser de nouveau de segmentation et de hachage des technologies dans Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012.  
  
-   **Les ordinateurs clients** doivent exécuter Windows 10, Windows 8.1 ou Windows 8 s’utilisent le modèle de déploiement plus récente et la segmentation et de hachage améliorations qui ont été introduites avec Windows Server 2012.  
  
-   **Serveurs de cache hébergé** doit exécuter Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 pour utiliser les améliorations du déploiement et de mettre à l’échelle des fonctionnalités décrites dans ce document.  Un ordinateur qui exécute l’un de ces systèmes d’exploitation qui est configuré comme un serveur de cache hébergé peut continuer à servir des ordinateurs clients qui exécutent Windows 7, mais pour ce faire, il doit être équipé d’un certificat qui convient pour sécurité TLS (Transport Layer ), comme décrit dans le Windows Server 2008 R2 et Windows 7 [Guide de déploiement BranchCache](https://technet.microsoft.com/library/ee649232.aspx).  
  
-   **Un domaine Active Directory** est nécessaire pour tirer parti de la stratégie de groupe et la détection automatique de cache hébergé, mais un domaine n’est pas nécessaire pour utiliser BranchCache.  Vous pouvez configurer des ordinateurs individuels à l’aide de Windows PowerShell. En outre, il n’est pas nécessaire que vos contrôleurs de domaine exécutent Windows Server 2012 ou version ultérieure pour utiliser les nouveaux paramètres de stratégie de groupe BranchCache. Vous pouvez importer les modèles d’administration BranchCache sur les contrôleurs de domaine qui exécutent des systèmes d’exploitation antérieurs, ou vous pouvez créer des objets de stratégie de groupe à distance sur d’autres ordinateurs qui exécutent Windows 10, Windows Server 2016, Windows 8.1, Windows Server 2012 R2, Windows 8 ou Windows Server 2012.

-   **Les sites Active Directory** sont utilisés pour limiter l’étendue des serveurs de cache hébergé sont détectés automatiquement.  Pour découvrir automatiquement un serveur de cache hébergé, les ordinateurs client et serveur doivent appartenir au même site. BranchCache est conçu pour avoir un impact minimal sur les clients et serveurs et n’impose pas d’autre configuration matérielle requise au-delà de celles nécessaires à l’exécution de leurs systèmes d’exploitation respectifs.  

**Documentation et l’historique de BranchCache**

BranchCache a été introduite dans Windows 7&reg; et Windows Server&reg; 2008 R2 et a été amélioré dans Windows Server 2012, Windows 8 et les systèmes d’exploitation ultérieurs.

> [!NOTE]
> Si vous déployez BranchCache dans les systèmes d’exploitation autres que Windows Server 2016, les ressources de documentation suivantes sont disponibles.
> 
> - Pour plus d’informations sur BranchCache dans Windows 8, Windows 8.1, Windows Server 2012 et Windows Server 2012 R2, consultez [vue d’ensemble de BranchCache](https://technet.microsoft.com/library/hh831696.aspx).  
> - Pour plus d’informations sur BranchCache dans Windows 7 et Windows Server 2008 R2, consultez [BranchCache pour Windows Server 2008 R2](https://technet.microsoft.com/library/dd996634.aspx).  
  


