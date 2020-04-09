---
title: Mémoire dynamique est activé, mais ne répond pas sur certaines machines virtuelles
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 91b7f50f-a071-4ab6-beb1-1b29f92f52b6
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 98bf61e6e132db8c8a16bf719410aefb433a1d99
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861982"
---
# <a name="dynamic-memory-is-enabled-but-not-responding-on-some-virtual-machines"></a>Mémoire dynamique est activé, mais ne répond pas sur certaines machines virtuelles

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
*Une ou plusieurs machines virtuelles rencontrent des problèmes avec le pilote requis pour Mémoire dynamique dans le système d’exploitation invité.*  
  
## <a name="impact"></a>Impact  
*Le système d’exploitation invité sur les ordinateurs virtuels suivants peut ne pas s’exécuter ou s’exécuter de manière non fiable, car Hyper-V ne peut pas ajuster la mémoire dynamiquement pour répondre aux modifications de la demande de mémoire. Cela a un impact sur les machines virtuelles suivantes :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>Résolution  
*Ce comportement est normal si l’ordinateur virtuel est en cours de démarrage. Si la machine virtuelle ne démarre pas, assurez-vous que les services d’intégration sont mis à niveau vers la dernière version et que le système d’exploitation invité prend en charge Mémoire dynamique.*  
  
À compter de Windows Server 2016, les services d’intégration sont fournis par le biais de Windows Update. Assurez-vous que les machines virtuelles sont configurées pour recevoir des mises à jour afin d’obtenir la dernière version d’Integration Services.  
  
Mémoire dynamique fonctionne avec des versions spécifiques des invités pris en charge. Consultez la page [vue d’ensemble d’Hyper-V mémoire dynamique](https://technet.microsoft.com/library/hh831766.aspx) pour les versions antérieures à windows server 2016 et Windows 10.  
  


