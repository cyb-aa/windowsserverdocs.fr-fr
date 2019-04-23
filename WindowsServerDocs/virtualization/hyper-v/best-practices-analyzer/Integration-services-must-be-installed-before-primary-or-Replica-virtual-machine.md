---
title: Services d’intégration doivent être installés avant le primaire ou de machines virtuelles de réplication peut utiliser une autre adresse IP après un basculement
description: Version en ligne du texte pour cette règle de Best Practices Analyzer, avec des liens vers plus d’informations.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: a7fdd185-d6c8-4f58-9b58-2df5827bb056
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 1ff8dbfd71655aee86ba7d0feac87ec2267a2171
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865510"
---
# <a name="integration-services-must-be-installed-before-primary-or-replica-virtual-machines-can-use-an-alternate-ip-address-after-a-failover"></a>Services d’intégration doivent être installés avant le primaire ou de machines virtuelles de réplication peut utiliser une autre adresse IP après un basculement

>S'applique à : Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Niveau de gravité**|Erreur|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, italique indique le texte de l’interface utilisateur qui apparaît dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
*Les machines virtuelles participant à la réplication peut être configuré pour utiliser une adresse IP spécifique en cas de basculement, mais uniquement si les services d’intégration sont installés dans le système d’exploitation invité de la machine virtuelle.*  
  
## <a name="impact"></a>Impact  
*Dans l’événement d’un basculement (planifié, non planifié ou test), la machine virtuelle de réplica est mis en ligne à l’aide de la même adresse IP en tant que la machine virtuelle principale. Cette configuration peut entraîner des problèmes de connectivité. Cela affecte les ordinateurs virtuels suivants :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>Résolution  
*Utiliser la connexion de Machine virtuelle pour installer integration services dans la machine virtuelle.*  
  
À compter de Windows Server 2016, les services d’intégration pour les machines virtuelles Windows sont remis via Windows Update. Vérifiez que ces machines virtuelles sont configurées pour recevoir des mises à jour de Windows pour obtenir la dernière version des services d’intégration. Maintenant, le noyau Linux inclut des services d’intégration Linux (LIS) et est mis à jour pour les nouvelles versions, mais les distributions Linux basées sur les anciens noyaux peut-être pas les dernières améliorations ou des correctifs. Pour plus d’informations, consultez [pris en charge de Linux et FreeBSD machines virtuelles pour Hyper-V sur Windows](../Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md).


