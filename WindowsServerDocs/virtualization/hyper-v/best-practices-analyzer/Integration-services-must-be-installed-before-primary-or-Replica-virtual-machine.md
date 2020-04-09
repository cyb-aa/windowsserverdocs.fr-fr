---
title: Les services d’intégration doivent être installés avant que les machines virtuelles principales ou de réplication puissent utiliser une autre adresse IP après un basculement
description: Version en ligne du texte de cette règle de Best Practices Analyzer, avec des liens vers des informations supplémentaires.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: a7fdd185-d6c8-4f58-9b58-2df5827bb056
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: d532d58d21b39963b41de969d83b720ed077e9c7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861902"
---
# <a name="integration-services-must-be-installed-before-primary-or-replica-virtual-machines-can-use-an-alternate-ip-address-after-a-failover"></a>Les services d’intégration doivent être installés avant que les machines virtuelles principales ou de réplication puissent utiliser une autre adresse IP après un basculement

>S’applique à Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Error|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
*Les machines virtuelles participant à la réplication peuvent être configurées pour utiliser une adresse IP spécifique en cas de basculement, mais uniquement si les services d’intégration sont installés dans le système d’exploitation invité de la machine virtuelle.*  
  
## <a name="impact"></a>Impact  
*En cas de basculement (planifié, non planifié ou de test), la machine virtuelle de réplication est mise en ligne à l’aide de la même adresse IP que la machine virtuelle principale. Cette configuration peut entraîner des problèmes de connectivité. Cela a un impact sur les machines virtuelles suivantes :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>Résolution  
*Utilisez connexion à un ordinateur virtuel pour installer les services d’intégration sur l’ordinateur virtuel.*  
  
À compter de Windows Server 2016, les services d’intégration pour les machines virtuelles Windows sont fournis par le biais de Windows Update. Assurez-vous que ces machines virtuelles sont configurées pour recevoir des mises à jour Windows afin d’obtenir la dernière version d’Integration Services. Le noyau Linux comprend désormais les services d’intégration Linux (LIS) et est mis à jour pour les nouvelles versions, mais les distributions Linux basées sur des noyaux plus anciens peuvent ne pas avoir les améliorations ou les correctifs les plus récents. Pour plus d’informations, consultez [machines virtuelles Linux et FreeBSD prises en charge pour Hyper-V sur Windows](../Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md).


