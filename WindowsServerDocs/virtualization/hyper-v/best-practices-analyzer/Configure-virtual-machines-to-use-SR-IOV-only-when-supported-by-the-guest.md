---
title: Configurer des machines virtuelles pour utiliser SR-IOV uniquement lorsqu’elles sont prises en charge par le système d’exploitation invité
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 33cf5b68-e43e-47ef-adbc-6b266c1d4dce
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 0634b10d1ffa81d875a7b90c9a8eadcddd52b4e7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80862012"
---
# <a name="configure-virtual-machines-to-use-sr-iov-only-when-supported-by-the-guest-operating-system"></a>Configurer des machines virtuelles pour utiliser SR-IOV uniquement lorsqu’elles sont prises en charge par le système d’exploitation invité

>S’applique à Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Avertissement|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
*Un ou plusieurs ordinateurs virtuels sont configurés pour utiliser la virtualisation d’e/s d’une racine unique (SR-IOV), mais le système d’exploitation invité ne prend pas en charge SR-IOV*  
  
## <a name="impact"></a>Impact  
*Les fonctions virtuelles SR-IOV ne sont pas allouées aux ordinateurs virtuels suivants :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>Résolution  
*Désactivez SR-IOV sur toutes les machines virtuelles qui exécutent des systèmes d’exploitation invités qui ne prennent pas en charge SR-IOV.*  
  
SR-IOV est pris en charge uniquement dans certains invités Windows 64 bits. Pour plus d’informations, consultez [compatibilité des fonctionnalités Hyper-V par génération et invité](../Hyper-V-feature-compatibility-by-generation-and-guest.md).  
  


