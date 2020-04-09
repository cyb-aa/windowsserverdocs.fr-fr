---
title: Le nombre d’ordinateurs virtuels en cours d’exécution configurés pour SR-IOV ne doit pas dépasser le nombre de fonctions virtuelles disponibles pour les machines virtuelles
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 8bd4af5e-9e7d-4710-8950-39435a8bb373
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 3c58e980471284c950f41551b02b92bf74aca7cc
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854612"
---
# <a name="the-number-of-running-virtual-machines-configured-for-sr-iov-should-not-exceed-the-number-of-virtual-functions-available-to-the-virtual-machines"></a>Le nombre d’ordinateurs virtuels en cours d’exécution configurés pour SR-IOV ne doit pas dépasser le nombre de fonctions virtuelles disponibles pour les machines virtuelles

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
*Il n’y a pas assez de fonctions virtuelles disponibles pour le nombre d’ordinateurs virtuels en cours d’exécution configurés pour la virtualisation d’e/s d’une racine unique (SR-IOV).*  
  
## <a name="impact"></a>Impact  
*Les performances de mise en réseau peuvent ne pas être optimales sur les ordinateurs virtuels suivants :*  
   
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>Résolution  
*Envisagez de désactiver SR-IOV sur une ou plusieurs machines virtuelles qui ne nécessitent pas de fonction virtuelle SR-IOV.*  
  


