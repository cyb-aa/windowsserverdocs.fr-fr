---
title: Guide de déploiement BranchCache
description: Cette rubrique fait partie du Guide de déploiement BranchCache pour Windows Server 2016, qui montre comment déployer BranchCache en mode de cache distribué et hébergé pour optimiser l’utilisation de la bande passante WAN dans les filiales.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 3830b356-36d3-44f9-a1d7-990ff3e57403
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 14eb9e5b4d5a28a64d3cfa0d27b5294ba7168da9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356728"
---
# <a name="branchcache-deployment-guide"></a>Guide de déploiement BranchCache

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Vous pouvez utiliser ce guide pour apprendre à déployer BranchCache dans Windows Server 2016.  
  
En plus de cette rubrique, ce guide contient les sections suivantes.  
  
-   [Choix d’une conception BranchCache](../../branchcache/plan/Choosing-a-BranchCache-Design.md)  
  
-   [Déployer BranchCache](../../branchcache/deploy/Deploy-BranchCache.md)  
  
## <a name="branchcache-deployment-overview"></a>Présentation du déploiement de BranchCache

BranchCache est une technologie d’optimisation de la bande passante du réseau étendu (WAN), incluse dans certaines éditions de Windows Server 2016, Windows Server @ no__t-0 2012 R2, Windows Server @ no__t-1 2012, Windows Server @ no__t-2 2008 R2 et le client Windows associé systèmes d’exploitation.  
  
Pour optimiser la bande passante du réseau étendu, BranchCache copie le contenu des serveurs de contenu de votre site principal et le met en cache dans les filiales, permettant ainsi aux ordinateurs clients des filiales d’accéder localement au contenu au lieu de passer par le réseau étendu.  
  
Dans les filiales, le contenu est mis en cache sur les serveurs qui exécutent la fonctionnalité BranchCache de Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 ou Windows Server 2008 R2, ou si aucun serveur n’est disponible dans la filiale, le contenu est CAC Hed sur les ordinateurs clients qui exécutent Windows 10 @ no__t-0, Windows @ no__t-1 8,1, Windows 8 ou Windows 7 @ no__t-2.  
  
Lorsqu’un ordinateur client demande et reçoit du contenu à partir du siège social ou du centre de données Cloud et que le contenu est mis en cache dans la filiale, les autres ordinateurs de la même filiale peuvent obtenir le contenu localement au lieu de contacter le serveur de contenu via le Liaison WAN.  
  
**Avantages du déploiement de BranchCache**  
  
BranchCache met en cache le contenu du fichier, du Web et de l’application au niveau des succursales, ce qui permet aux ordinateurs clients d’accéder aux données à l’aide du réseau local (LAN) plutôt que d’accéder au contenu via des connexions WAN lentes.  
  
BranchCache réduit le trafic WAN et le temps nécessaire pour que les utilisateurs des succursales ouvrent des fichiers sur le réseau.  BranchCache fournit toujours aux utilisateurs les données les plus récentes et protège la sécurité de votre contenu en chiffrant les caches sur le serveur de cache hébergé et sur les ordinateurs clients.  
  
### <a name="what-this-guide-provides"></a>Ce que ce guide contient  
Ce guide de déploiement vous permet de déployer BranchCache dans les modes suivants :  
  
-   Mode de cache distribué. Dans ce mode, les ordinateurs clients des succursales téléchargent du contenu à partir des serveurs de contenu dans le bureau principal ou le Cloud, puis le cache pour les autres ordinateurs de la même filiale. Le mode de cache distribué ne requiert pas de serveur dans la filiale.  
  
-   Mode de cache hébergé. Dans ce mode, les ordinateurs clients des succursales téléchargent du contenu à partir des serveurs de contenu du bureau principal ou du Cloud, et un serveur de cache hébergé récupère le contenu auprès des clients. Le serveur de cache hébergé met ensuite en cache le contenu pour d’autres ordinateurs clients.  
  
Ce guide fournit également des instructions sur le déploiement de trois types de serveurs de contenu. Les serveurs de contenu contiennent le contenu source téléchargé par les ordinateurs clients des filiales, et un ou plusieurs serveurs de contenu sont requis pour déployer BranchCache dans l’un ou l’autre mode. Les types de serveurs de contenu sont :  
  
-   **Serveurs de contenu basés sur un serveur Web**. Ces serveurs de contenu envoient le contenu aux ordinateurs clients BranchCache à l'aide des protocoles HTTP et HTTPS. Ces serveurs de contenu doivent exécuter les versions Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 ou Windows Server 2008 R2 qui prennent en charge BranchCache et sur lesquelles la fonctionnalité BranchCache est installée.  
  
-   **Serveurs d’applications basés sur bits**. Ces serveurs de contenu envoient du contenu aux ordinateurs clients BranchCache à l’aide du Service de transfert intelligent en arrière-plan (BITS). Ces serveurs de contenu doivent exécuter les versions Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 ou Windows Server 2008 R2 qui prennent en charge BranchCache et sur lesquelles la fonctionnalité BranchCache est installée.  
  
-   **Serveurs de contenu basés sur un serveur de fichiers**. Ces serveurs de contenu doivent exécuter des versions de Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 ou Windows Server 2008 R2 qui prennent en charge BranchCache et sur lesquelles le rôle de serveur services de fichiers est installé. De plus, le service de rôle **BranchCache pour fichiers réseau** du rôle serveur Services de fichiers doit être installé et configuré. Ces serveurs de contenu envoient le contenu aux ordinateurs clients BranchCache à l'aide du protocole SMB.  
  
Pour plus d’informations, consultez [versions de système d’exploitation pour BranchCache](https://technet.microsoft.com/windows-server-docs/networking/branchcache/branchcache#a-namebkmkosaoperating-system-versions-for-branchcache).  
  
### <a name="branchcache-deployment-requirements"></a>Conditions requises pour le déploiement de BranchCache

Vous trouverez ci-dessous la configuration requise pour le déploiement de BranchCache à l’aide de ce guide.  
  
-   Les **serveurs de contenu Web et de fichiers** doivent exécuter l’un des systèmes d’exploitation suivants pour fournir les fonctionnalités BranchCache : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 ou Windows Server 2008 R2. Les clients Windows 8 et versions ultérieures continuent de voir les avantages de BranchCache lors de l’accès aux serveurs de contenu qui exécutent Windows Server 2008 R2, mais ils ne peuvent pas utiliser les nouvelles technologies de segmentation et de hachage dans Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012.  
  
-   Les **ordinateurs clients** doivent exécuter Windows 10, Windows 8.1 ou Windows 8 pour utiliser le modèle de déploiement le plus récent, ainsi que les améliorations de segmentation et de hachage introduites avec Windows Server 2012.  
  
-   Les **serveurs de cache hébergé** doivent exécuter windows server 2016, windows server 2012 R2 ou windows server 2012 pour pouvoir utiliser les améliorations de déploiement et les fonctionnalités de mise à l’échelle décrites dans ce document.  Un ordinateur exécutant l’un de ces systèmes d’exploitation configuré en tant que serveur de cache hébergé peut continuer à servir les ordinateurs clients qui exécutent Windows 7, mais pour ce faire, il doit être équipé d’un certificat adapté au protocole TLS (Transport Layer Security). ), comme décrit dans le [Guide de déploiement](https://technet.microsoft.com/library/ee649232.aspx)de windows Server 2008 R2 et Windows 7 BranchCache.  
  
-   **Un domaine Active Directory** est nécessaire pour tirer parti de la détection automatique des stratégie de groupe et du cache hébergé, mais un domaine n’est pas requis pour utiliser BranchCache.  Vous pouvez configurer des ordinateurs individuels à l’aide de Windows PowerShell. En outre, il n’est pas nécessaire que vos contrôleurs de domaine exécutent Windows Server 2012 ou une version ultérieure pour utiliser les nouveaux paramètres de stratégie de groupe BranchCache. vous pouvez importer les modèles d’administration BranchCache sur les contrôleurs de domaine qui exécutent des systèmes d’exploitation antérieurs, ou vous pouvez créer des objets de stratégie de groupe à distance sur d’autres ordinateurs qui exécutent Windows 10, Windows Server 2016, Windows 8.1, Windows Server 2012 R2, Windows 8 ou Windows Server 2012.

-   Les **sites Active Directory** sont utilisés pour limiter l’étendue des serveurs de cache hébergé qui sont détectés automatiquement.  Pour détecter automatiquement un serveur de cache hébergé, les ordinateurs client et serveur doivent appartenir au même site. BranchCache est conçu pour avoir un impact minimal sur les clients et les serveurs et n’impose pas d’autres exigences matérielles en dehors de celles nécessaires pour exécuter leurs systèmes d’exploitation respectifs.  

**Documentation et historique de BranchCache**

BranchCache a été introduit pour la première fois dans Windows 7 @ no__t-0 et Windows Server @ no__t-1 2008 R2, et a été amélioré dans Windows Server 2012, Windows 8 et les systèmes d’exploitation ultérieurs.

> [!NOTE]
> Si vous déployez BranchCache dans des systèmes d’exploitation autres que Windows Server 2016, les ressources de documentation suivantes sont disponibles.
> 
> - Pour plus d’informations sur BranchCache dans Windows 8, Windows 8.1, Windows Server 2012 et Windows Server 2012 R2, consultez [vue d’ensemble de BranchCache](https://technet.microsoft.com/library/hh831696.aspx).  
> - Pour plus d’informations sur BranchCache dans Windows 7 et Windows Server 2008 R2, consultez [BranchCache pour Windows server 2008 R2](https://technet.microsoft.com/library/dd996634.aspx).  
  


