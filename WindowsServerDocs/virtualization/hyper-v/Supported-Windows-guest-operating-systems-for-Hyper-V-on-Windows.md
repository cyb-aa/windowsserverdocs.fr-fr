---
title: Systèmes d’exploitation d’invités Windows pris en charge pour Hyper-V sur Windows Server
description: Répertorie les systèmes d’exploitation Windows pris en charge pour une utilisation en tant qu’invité dans un ordinateur virtuel. Fournit également des liens vers des articles similaires pour les versions précédentes d’Hyper-V.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 06b35897-2192-48b7-8c2d-125c520b0786
author: lizap
ms.author: elizapo
ms.date: 01/08/2019
ms.openlocfilehash: 5f0e91f3202f09d340154b49408c56752a9de577
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874210"
---
# <a name="supported-windows-guest-operating-systems-for-hyper-v-on-windows-server"></a>Systèmes d’exploitation d’invités Windows pris en charge pour Hyper-V sur Windows Server

>S'applique à : Windows Server 2016, Windows Server 2019

Hyper-V prend en charge plusieurs versions de Windows Server, Windows et Linux distributions à exécuter sur des machines virtuelles, en tant que systèmes d’exploitation invités. Cet article décrit la prise en charge Windows Server et les systèmes d’exploitation invités Windows. Pour les distributions Linux et FreeBSD, consultez [pris en charge de Linux et FreeBSD machines virtuelles pour Hyper-V sur Windows](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md).  
    
Certains systèmes d’exploitation ont intégré les services d’intégration. D’autres nécessitent que vous installez ou mettez à niveau des services d’intégration comme une étape distincte après avoir configuré le système d’exploitation sur l’ordinateur virtuel. Pour plus d’informations, consultez les sections ci-dessous et [Integration Services](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/integration-services).  
  
## <a name="supported-windows-server-guest-operating-systems"></a>Systèmes d’exploitation invité Windows Server  

Voici les versions de Windows Server qui sont prises en charge en tant que systèmes d’exploitation invités pour Hyper-V dans Windows Server 2016 et Windows Server 2019. 
  
|Système d’exploitation invité (serveur)|Nombre maximal de processeurs virtuels|Services d’intégration|Notes|  
|-------------------------------------|----------------------------------------|------------------------|---------|  
|Windows Server 2019 |240 pour la génération 2 ;<br>64 pour la génération 1|Intégrée|| 
|Windows Server 2016 |240 pour la génération 2 ;<br>64 pour la génération 1|Intégrée|| 
|Windows Server 2012 R2 |64|Intégrée||  
|Windows Server 2012 |64|Intégrée||  
|Windows Server 2008 R2 avec Service Pack 1 (SP1)|64|Installez toutes les mises à jour Windows critiques après avoir configuré le système d’exploitation invité.|Éditions Datacenter, Entreprise, Standard et Web.|
|Windows Server 2008 avec Service Pack 2 (SP2)|8|Installez toutes les mises à jour Windows critiques après avoir configuré le système d’exploitation invité.|Éditions Datacenter, Entreprise, Standard et Web (32 bits et 64 bits).|  
  
## <a name="supported-windows-client-guest-operating-systems"></a>Systèmes de d’exploitation invité de client Windows pris en charge  

Voici les versions du client de Windows qui sont pris en charge en tant que systèmes d’exploitation invités pour Hyper-V dans Windows Server 2016 et Windows Server 2019.
  
|Système d’exploitation client (client)|Nombre maximal de processeurs virtuels|Services d’intégration|Notes|  
|-------------------------------------|----------------------------------------|------------------------|---------|  
|Windows 10|32|Intégrée||  
|Windows 8.1|32|Intégrée||  
|Windows 7 avec Service Pack 1 (SP1)|4|Après avoir configuré le système d’exploitation invité, mettez à niveau les services d’intégration.|Édition Intégrale, Édition Entreprise et Édition Professionnel (32 bits et 64 bits).|  
  
## <a name="guest-operating-system-support-on-other-versions-of-windows"></a>Prise en charge du système d’exploitation invité sur d’autres versions de Windows  

Le tableau suivant fournit des liens vers des informations sur les systèmes d’exploitation invités pris en charge pour Hyper-V sur d’autres versions de Windows.  
  
|Système d'exploitation de l'ordinateur hôte|Rubrique|  
|-------------------------|---------|  
|Windows 10|[Systèmes d’exploitation invités pris en charge pour Hyper-V Client dans Windows 10](https://docs.microsoft.com/virtualization/hyper-v-on-windows/about/supported-guest-os)|  
|Windows Server 2012 R2 et Windows 8.1|-   [Windows pris en charge les systèmes d’exploitation invités pour Hyper-V dans Windows Server 2012 R2 et Windows 8.1](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn792027(v=ws.11))<br />-   [Linux et les Machines virtuelles de FreeBSD sur Hyper-V](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)|  
|Windows Server 2012 et Windows 8|[Windows pris en charge les systèmes d’exploitation invités pour Hyper-V dans Windows Server 2012 et Windows 8](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn792028(v=ws.11))|  
|Windows Server 2008 et Windows Server 2008 R2|[Sur les Machines virtuelles et les systèmes d’exploitation invités](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc794868(v=ws.10))|  
  
## <a name="how-microsoft-provides-support-for-guest-operating-systems"></a>Comment Microsoft fournit la prise en charge pour les systèmes d’exploitation invités  

Microsoft assure la prise en charge des systèmes d’exploitation invités de la façon suivante :  
  
-   Les problèmes détectés dans les services d’intégration et systèmes d’exploitation Microsoft sont pris en charge par le support technique Microsoft.  
  
-   En ce qui concerne les problèmes dans d’autres systèmes d’exploitation ayant été certifiés par le fournisseur de système d’exploitation comme fonctionnant sur Hyper-V, la prise en charge est assurée par le fournisseur.  
  
-   En ce qui concerne les problèmes détectés dans d’autres systèmes d’exploitation, Microsoft soumet le problème à la communauté de support multifournisseur, [TASNet](https://www.tsanet.org/).  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Linux et les Machines virtuelles de FreeBSD sur Hyper-V](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)  
  
-   [Systèmes d’exploitation invités pris en charge pour Hyper-V Client dans Windows 10](https://docs.microsoft.com/virtualization/hyper-v-on-windows/about/supported-guest-os)  
  



