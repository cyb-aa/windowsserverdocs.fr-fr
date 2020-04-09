---
title: Configurer le serveur avec un nombre suffisant d’adresses MAC dynamiques
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: a2804519-9790-4006-80b6-e990a8f505fe
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 1fe7b34abc8be9777a491a08016627e7b2a66a2a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80862042"
---
# <a name="configure-the-server-with-a-sufficient-amount-of-dynamic-mac-addresses"></a>Configurer le serveur avec un nombre suffisant d’adresses MAC dynamiques

>S’applique à Windows Server 2016

*Cette rubrique vise à résoudre un problème spécifique identifié par une analyse Best Practices Analyzer. Vous devez appliquer les informations contenues dans cette rubrique uniquement aux ordinateurs sur lesquels l’Best Practices Analyzer Hyper-V s’exécutent et qui rencontrent le problème traité dans cette rubrique. Pour plus d’informations sur les meilleures pratiques et les analyses, consultez* [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Avertissement|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
  
*Le nombre d’adresses MAC dynamiques disponibles est faible.*  
  
## <a name="impact"></a>Impact  
  
*Si aucune adresse MAC dynamique n’est disponible, les machines virtuelles configurées pour utiliser une adresse MAC dynamique ne peuvent pas être démarrées.*  
  
## <a name="resolution"></a>Résolution  
  
*Utilisez le gestionnaire de commutateur virtuel pour afficher et étendre la plage d’adresses dynamiques.*  
  


