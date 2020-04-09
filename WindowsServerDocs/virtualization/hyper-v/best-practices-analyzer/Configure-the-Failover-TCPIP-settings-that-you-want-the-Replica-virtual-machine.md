---
title: Configurez les paramètres TCP/IP de basculement que vous souhaitez que l’ordinateur virtuel de réplication utilise en cas de basculement
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: d2db5fdedbe2f19c01b7dd172f18b6fec969e828
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80862052"
---
# <a name="configure-the-failover-tcpip-settings-that-you-want-the-replica-virtual-machine-to-use-in-the-event-of-a-failover"></a>Configurez les paramètres TCP/IP de basculement que vous souhaitez que l’ordinateur virtuel de réplication utilise en cas de basculement

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
*Les machines virtuelles de réplication configurées avec une adresse IP statique doivent être configurées pour utiliser une adresse IP différente de leur homologue de machine virtuelle principale en cas de basculement.*  
  
## <a name="impact"></a>Impact  
*Les clients qui utilisent la charge de travail prise en charge par l’ordinateur virtuel principal peuvent ne pas être en mesure de se connecter à la machine virtuelle de réplication après un basculement. En outre, l’adresse IP d’origine de l’ordinateur virtuel principal n’est pas valide dans la topologie de réseau de machines virtuelles de réplication. Cela a un impact sur les machines virtuelles suivantes :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>Résolution  
*Utilisez le Gestionnaire Hyper-V pour configurer l’adresse IP que l’ordinateur virtuel de réplication doit utiliser en cas de basculement.*  
  


