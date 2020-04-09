---
title: Utiliser toutes les fonctions virtuelles pour la mise en réseau lorsqu’elles sont disponibles
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: bf895484-6a0d-4aa4-9a42-9fac739e875d
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 0fab06ae21a4632df73b7a4d8b17b12665ffed98
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854212"
---
# <a name="use-all-virtual-functions-for-networking-when-they-are-available"></a>Utiliser toutes les fonctions virtuelles pour la mise en réseau lorsqu’elles sont disponibles

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
*Certaines fonctionnalités d’accélération matérielle ne sont pas utilisées*  
  
## <a name="impact"></a>Impact  
*Cette configuration peut entraîner une utilisation de l’UC globale plus élevée que nécessaire. Les performances de mise en réseau peuvent ne pas être optimales sur les ordinateurs virtuels suivants :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>Résolution  
*Envisagez de configurer la carte réseau virtuelle pour SR-IOV si le matériel physique prend en charge SR-IOV et si cette configuration n’est pas en conflit avec les fonctionnalités de mise en réseau requises par la machine virtuelle.*  
  


