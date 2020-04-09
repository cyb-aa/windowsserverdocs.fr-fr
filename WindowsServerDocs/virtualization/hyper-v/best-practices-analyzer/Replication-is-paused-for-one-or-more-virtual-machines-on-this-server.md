---
title: La réplication est suspendue pour un ou plusieurs ordinateurs virtuels sur ce serveur
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: e1119a40-eda3-4058-8648-7df81cbc6c29
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 6974016dd564cf19d39a6d83b041020a4e327d2c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861812"
---
# <a name="replication-is-paused-for-one-or-more-virtual-machines-on-this-server"></a>La réplication est suspendue pour un ou plusieurs ordinateurs virtuels sur ce serveur

>S’applique à Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Avertissement|  
|**Catégorie**|Opérations|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
*La réplication est suspendue pour un ou plusieurs ordinateurs virtuels. Lorsque la machine virtuelle principale est suspendue, toutes les modifications qui se produisent sont accumulées et envoyées à la machine virtuelle de réplication une fois la réplication reprise.*  
  
## <a name="impact"></a>Impact  
*Tant que la réplication est suspendue, les modifications accumulées qui se produisent sur l’ordinateur virtuel principal consomment de l’espace disque disponible sur le serveur principal. Une fois la réplication reprise, il peut y avoir un grand nombre de trafic réseau vers le serveur de réplication. Cela a un impact sur les machines virtuelles suivantes :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>Résolution  
*Confirmez que la suspension de la réplication était prévue. Si la réplication a été suspendue pour gérer l’espace disque insuffisant ou la connectivité réseau, reprenez la réplication dès que ces problèmes sont résolus.*  
  


