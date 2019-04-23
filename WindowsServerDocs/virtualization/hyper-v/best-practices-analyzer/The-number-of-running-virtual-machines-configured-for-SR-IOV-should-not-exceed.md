---
title: Le nombre de machines virtuelles configurées pour SR-IOV en cours d’exécution ne doit pas dépasser le nombre de fonctions virtuelles disponibles pour les machines virtuelles
description: Version en ligne du texte pour cette règle de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 8bd4af5e-9e7d-4710-8950-39435a8bb373
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: e63df9283927437f9cfc62c052d83b07fe599b34
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829750"
---
# <a name="the-number-of-running-virtual-machines-configured-for-sr-iov-should-not-exceed-the-number-of-virtual-functions-available-to-the-virtual-machines"></a>Le nombre de machines virtuelles configurées pour SR-IOV en cours d’exécution ne doit pas dépasser le nombre de fonctions virtuelles disponibles pour les machines virtuelles

>S'applique à : Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Niveau de gravité**|Warning|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, italique indique le texte de l’interface utilisateur qui apparaît dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
*Fonctions virtuelles suffisamment ne sont pas disponibles pour le nombre d’ordinateurs virtuels configurés pour la virtualisation d’e/s de racine unique (SR-IOV).*  
  
## <a name="impact"></a>Impact  
*Performances de mise en réseau ne peut pas être optimale sur les ordinateurs virtuels suivants :*  
   
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>Résolution  
*Désactivez SR-IOV sur un ou plusieurs machines virtuelles qui ne nécessitent pas une fonction virtuelle SR-IOV.*  
  


